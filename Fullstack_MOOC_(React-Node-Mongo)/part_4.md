## Express Router
A router object is an isolated instance of middleware and routes. You can think of it as a “mini-application,” capable only of performing middleware and routing functions. Every Express application has a built-in app router.  
The router is in fact a middleware, that can be used for defining "related routes" in a single place, that is typically placed in its own module.
The app.js file that creates the actual application, takes the router into use as shown below:
```JavaScript
const notesRouter = require('./controllers/notes')
app.use('/api/notes', notesRouter)
```

## Environments
The convention in Node is to define the execution mode of the application with the NODE_ENV environment variable. In our current application, we only load the environment variables defined in the .env file if the application is not in production mode.  
It is common practice to define separate modes for development and testing
```Json
  "scripts": {
    "start": "NODE_ENV=production node index.js",
    "watch": "NODE_ENV=development nodemon index.js",
    "test": "NODE_ENV=test jest --verbose --runInBand"
  }
```
We also added the runInBand option to the npm script that executes the tests. This option will prevent Jest from running tests in parallel.

There is a slight issue in the way that we have specified the mode of the application in our scripts: it will not work on Windows. We can correct this by installing the cross-env package with the command:  
npm install --save-dev cross-env

We can then achieve cross-platform compatibility by using the cross-env library in our npm scripts defined in package.json:
```JSON
{
  // ...
  "scripts": {
    "start": "cross-env NODE_ENV=production node index.js",
    "watch": "cross-env NODE_ENV=development nodemon index.js",
    // ...
    "test": "cross-env NODE_ENV=test jest --verbose --runInBand",
  },
  // ...
}
```

## Testing
It would be better to run our tests using a database that is installed and running in the developer's local machine. The optimal solution would be to have every test execution use its own separate database. This is "relatively simple" to achieve by running Mongo in-memory or by using Docker containers

**Supertest**  

The test imports the Express application from the app.js module and wraps it with the supertest function into a so-called superagent object. This object is assigned to the api variable and tests can use it for making HTTP requests to the backend.

The documentation for supertest says the following:
    if the server is not already listening for connections then it is bound to an ephemeral port for you so there is no need to keep track of ports.  
In other words, supertest takes care that the application being tested is started at the port that it uses internally.


The logger middleware that outputs information about the HTTP requests is obstructing the test execution output. It's a good idea to prevent logging when the application is in test mode. 


## async/await
With async/await the recommended way of dealing with exceptions is the old and familiar try/catch mechanism
```JavaScript
try {
  const savedNote = await note.save()  
  response.json(savedNote.toJSON())
} catch(exception) {   
    next(exception) 
}
```
The catch block simply calls the next function, which passes the request handling to the error handling middleware.

Despite our use of the async/await syntax, our solution does not work like we expected it to. The test execution begins before the database is initialized!  
The problem is that every iteration of the forEach loop generates its own asynchronous operation, and beforeEach won't wait for them to finish executing. In other words, the await commands defined inside of the forEach loop are not in the beforeEach function, but in separate functions that beforeEach will not wait for.  
One way of fixing this is to wait for all of the asynchronous operations to finish executing with the Promise.all method.  
The Promise.all method can be used for transforming an array of promises into a single promise, that will be fulfilled once every promise in the array passed to it as a parameter is resolved. The last line of code await Promise.all(promiseArray) waits that every promise for saving a note is finished, meaning that the database has been initialized.  
The returned values of each promise in the array can still be accessed when using the Promise.all method. If we wait for the promises to be resolved with the await syntax const results = await Promise.all(promiseArray), the operation will return an array that contains the resolved values for each promise in the promiseArray, and they appear in the same order as the promises in the array.  
Promise.all executes the promises it receives in parallel. If the promises need to be executed in a particular order, this will be problematic. In situations like this, the operations can be executed inside of a for...of block, that guarantees a specific execution order.
```JavaScript
beforeEach(async () => {
  await Note.deleteMany({})

  for (let note of helper.initialNotes) {
    let noteObject = new Note(note)
    await noteObject.save()
  }
})
```

# User Administration
Like with all document databases, we can use object id's in Mongo to reference documents in other collections. This is similar to using foreign keys in relational databases.

If we need a functionality similar to join queries, we will implement it in our application code by making multiple queries. In certain situations Mongoose can take care of joining and aggregating data, which gives the appearance of a join query. However, even in these situations Mongoose makes multiple queries to the database in the background.

The ids of the notes are stored within the user document as an array of Mongo ids. The definition is as follows:
```JavaScript
{
  type: mongoose.Schema.Types.ObjectId,
  ref: 'Note'
}
```
The type of the field is ObjectId that references note-style documents. Mongo does not inherently know that this is a field that references notes, the syntax is purely related to and defined by Mongoose.

In stark contrast to the conventions of relational databases, references are now stored in both documents: the note references the user who created it, and the user has an array of references to all of the notes created by them.

Mongoose does not have a built-in validator for checking the uniqueness of a field. We can find a ready-made solution for this from the mongoose-unique-validator npm package

As previously mentioned, document databases do not properly support join queries between collections, but the Mongoose library can do some of these joins for us. Mongoose accomplishes the join by doing multiple queries, which is different from join queries in relational databases which are transactional, meaning that the state of the database does not change during the time that the query is made. With join queries in Mongoose, nothing can guarantee that the state between the collections being joined is consistent, meaning that if we make a query that joins the user and notes collections, the state of the collections may change during the query.

The Mongoose join is done with the populate method.
The populate method is chained after the find method making the initial query. The parameter given to the populate method defines that the ids referencing note objects in the notes field of the user document will be replaced by the referenced note documents.  
It's important to understand that the database does not actually know that the ids stored in the user field of notes reference documents in the user collection.  
The functionality of the populate method of Mongoose is based on the fact that we have defined "types" to the references in the Mongoose schema with the ref option


## Token Authentication
If the password is correct, a token is created with the method jwt.sign. The token contains the username and the user id in a digitally signed form. The token has been digitally signed using a string from the environment variable SECRET as the secret. The digital signature ensures that only parties who know the secret can generate a valid token. The value for the environment variable must be set in the .env file. 

There are several ways of sending the token from the browser to the server. We will use the Authorization header. The header also tells which authentication schema is used. This can be necessary if the server offers multiple ways to authenticate. Identifying the schema tells the server how the attached credentials should be interpreted.
The Bearer schema is suitable to our needs. 

The helper function getTokenFrom isolates the token from the authorization header. The validity of the token is checked with jwt.verify. The method also decodes the token, or returns the Object which the token was based on. The object decoded from the token contains the username and id fields, which tells the server who made the request. 

Usernames, passwords and applications using token authentication must always be used over HTTPS. We could use a Node HTTPS server in our application instead of the HTTP server (it requires more configuration). On the other hand, the production version of our application is in Heroku, so our applications stays secure: Heroku routes all traffic between a browser and the Heroku server over HTTPS. 