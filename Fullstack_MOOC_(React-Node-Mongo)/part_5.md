## Local Storage
Values in the storage stay even when the page is rerendered. The storage is origin-specific so each web application has its own storage.
Values saved to the storage are DOMstrings, so we cannot save a JavaScript object as is. The object has to be first parsed to JSON with the method JSON.stringify. Correspondigly, when a JSON object is read from the local storage, it has to be parsed back to JavaScript with JSON.parse.

## props.children
Unlike the "normal" props we've seen before, children is automatically added by React and always exists. If a component is defined with an automatically closing /> tag then props.children is an empty array.

## ref
The createRef method is used to create a noteFormRef ref, that is assigned to the Togglable component containing the creation note form. The noteFormRef variable acts as a reference to the component.
The function that creates the component is wrapped inside of a forwardRef function call. This way the component can access the ref that is assigned to it.  
The component uses the useImperativeHandle hook to make its toggleVisibility function available outside of the component.
The useImperativeHandle function is a React hook, that is used for defining functions in a component which can be invoked from outside of the component.

# Testing
In addition to Jest, we also need another testing library that will help us render components for testing purposes. The best option for this used to be the enzyme library developed by Airbnb. Unfortunately, Enzyme does not support React hooks properly, so we will instead use **react-testing-library** which has seen rapid growth in popularity in recent times.

After the initial configuration, the test renders the component with the render method provided by the react-testing-library. Normally React components are rendered to the DOM. The render method we used renders the components in a format that is suitable for tests without rendering them to the DOM.  
render returns an object that has several properties. One of the properties is called container, and it contains all of the HTML rendered by the component.

Create-react-app configures tests to be run in watch mode by default, which means that the npm test command will not exit once the tests have finished, and will instead wait for changes to be made to the code. Once new changes to the code are saved, the tests are executed automatically after which Jest goes back to waiting for new changes to be made.  
If you want to run tests "normally", you can do so with the command: CI=true npm test

## searching for content in a component
The first way searches for a matching text from the entire HTML code rendered by the component:
```JavaScript
// method 1
expect(component.container).toHaveTextContent(
  'Component testing is done with react-testing-library'
)
```

The second way uses the getByText method of the object returned by the render method. The method returns the element that contains the given text. An exception occurs if no such element exists. For this reason, we would technically not need to specify any additional expectation:
```JavaScript
// method 2
const element = component.getByText(
  'Component testing is done with react-testing-library'
)
expect(element).toBeDefined()
```

The third way is to search for a specific element that is rendered by the component with the querySelector method that receives a CSS selector as its parameter:
```JavaScript
// method 3
const div = component.container.querySelector('.note')
expect(div).toHaveTextContent(
  'Component testing is done with react-testing-library'
)
```

## Debugging
The object returned by the render method has a debug method that can be used to print the HTML rendered by the component to the console.  
It is also possible to search for a smaller part of the component and print its HTML code. In order to do this, we need the prettyDOM method that can be imported from the @testing-library/dom package that is automatically installed with react-testing-library



## Setup
We'll use a small utility library for jest that helps making tests more readable: jest-dom adds new "matchers" for DOM specific assertions such as toBeVisible or toHaveFocus (import '@testing-library/jest-dom/extend-expect')  
We could repeat the same import in all of our test files. To prevent repetition and potentially forgetting the import we can use the import once in a single file. Let's create a new src/setupTests.js file for this configuration.  
If the configuration defined in the src/setupTests.js file is not used when the tests are run, it may help to add the following configuration to the package.json file:
```JavaScript
"jest": {
    ...
    "setupFiles": [
      "<rootDir>/src/setupTests.js"
    ],
    ...
  }
```

# Integration Test
Local storage is not available to our tests by default, as its functionality is provided by the browser and our tests are not running in the browser. It is quite easy to overcome this challenge by defining a mock that mimics the functionality of the local storage. There are many ways to accomplish this.
As our tests do not rely on any actual local storage functionality, we will write a very simple mock in the src/setupTests.js file:
```JavaScript
let savedItems = {}

const localStorageMock = {
  setItem: (key, item) => {
    savedItems[key] = item
  },
  getItem: (key) => savedItems[key],
  clear: () => {
    savedItems = {}
  }
}

Object.defineProperty(window, 'localStorage', { value: localStorageMock })
```

Our second challenge is that the application fetches its notes from the server. Fetching the notes happens immediately after the App component is created in the effect hook. The manual mock concept from Jest provides us with a good solution. With manual mocks, we can replace an entire module like noteService with an alternative module that can mock the functionality of the module e.g. by returning hardcoded data. 

The test starts by re-rendering the component ```component.rerender(<App />)```, this is done to ensure that all of the effects are executed. This command may no longer be necessary with newer versions of React.  
Since the action for fetching notes from the server is an asynchronous event, we use the waitForElement function for verifying that the App component renders all of the notes.

**test coverage command:**  
CI=true npm test -- --coverage


# Custom Hooks
React offers the option to create our own custom hooks. According to React, the primary purpose of custom hooks is to facilitate the reuse of the logic used in components.  
Building your own Hooks lets you extract component logic into reusable functions.  
Custom hooks are regular JavaScript functions that can use any other hooks, as long as they adhere to the rules of hooks. Additionally, the name of custom hooks must start with the word use

One natural place to save the custom hooks of your application is in the /src/hooks/index.js file
