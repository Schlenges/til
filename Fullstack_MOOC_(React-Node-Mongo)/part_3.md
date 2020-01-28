Mongoose could be described as an object document mapper (ODM), and saving JavaScript objects as Mongo documents is straightforward with the library.

First we define the schema of a note that is stored in the noteSchema variable. The schema tells Mongoose how the note objects are to be stored in the database.
In the Note model definition, the first "Note" parameter is the singular name of the model. The name of the collection will be the lowercased plural notes, because the Mongoose convention is to automatically name collections as the plural (e.g. notes) when the schema refers to them in the singular (e.g. Note).
Document databases like Mongo are schemaless, meaning that the database itself does not care about the structure of the data that is stored in the database. It is possible to store documents with completely different fields in the same collection.
The idea behind Mongoose is that the data stored in the database is given a schema at the level of the application that defines the shape of the documents stored in any given collection.
```JavaScript
const noteSchema = new mongoose.Schema({
  content: String,
  date: Date,
  important: Boolean,
})

const Note = mongoose.model('Note', noteSchema)
```

Models are so-called constructor functions that create new JavaScript objects based on the provided parameters. Since the objects are created with the model's constructor function, they have all the properties of the model, which include methods for saving the object to the database.
```JavaScript
const note = new Note({
  content: 'Browser can execute only Javascript',
  date: new Date(),
  important: false,
})
```

Saving the object to the database happens with the appropriately named save method, that can be provided with an event handler with the then method. When the object is saved to the database, the event handler provided to then gets called. The event handler closes the database connection with the command mongoose.connection.close(). If the connection is not closed, the program will never finish its execution.

The objects are retrieved from the database with the find method of the Note model. The parameter of the method is an object expressing search conditions.


The error that is passed forwards is given to the next function as a parameter. If next was called without a parameter, then the execution would simply move onto the next route or middleware. If the next function is called with a parameter, then the execution will continue to the error handler middleware.

It's also important that the middleware for handling unsupported routes is next to the last middleware that is loaded into Express, just before the error handler.
Now the handling of unknown endpoints is ordered before the HTTP request handler. Since the unknown endpoint handler responds to all requests with 404 unknown endpoint, no routes or middleware will be called after the response has been sent by unknown endpoint middleware. The only exception to this is the error handler which needs to come at the very end, after the unknown endpoints handler.

{new: true} in findByIdAndUpdate will cause the event handler to be called with the new modified document instead of the original.


## Mongoose Validation
One smarter way of validating the format of the data before it is stored in the database, is to use the validation functionality available in Mongoose.
We can define specific validation rules for each field in the schema
The minlength and required validators are built-in and provided by Mongoose. The Mongoose custom validator functionality allows us to create new validators, if none of the built-in ones cover our needs.

## Heroku and env
The environment variables defined in dotenv will only be used when the backend is not in production mode, i.e. Heroku.
We defined the environment variables for development in file .env, but the environment variable that defines the database URL in production should be set to Heroku with the heroku config:set command.
heroku config:set MONGODB_URI=...