# React

## 0: Markdown

- **preview**: shift + command + v
- **same page links**: [here](#place-2)

## 1: Settings

### Recommended VS Code Extensions

- Prettier
- Material Icon Theme (optional): change the look of the file icons

## 2: JavaScript Refresher

### Arrow Functions

```js
function myFnc() {
    ...
}

const myFnc = () => {
    ...
}

const multiply = number => {
    return number * 2;
}

const multiply = number => number * 2;
```

### Exports & Imports (modules)

```js
<person.js>
const person = {
    name: 'Max'
}

export default person;

<utility.js>
export const clean = () => {...}
export const baseData = 10;

<app.js>
import person from './person.js';
import prs from './person.js';

import { baseData, clean } from './utility.js';
import { baseData as baseD } from './utility.js';
```

### Classes

```js
//Properties and methods:
class Person {
    name = 'Max';
    call = () => {...}
}

//Instanciating the class:
const myPerson = new Person();
myPerson.call();
console.log(myPerson.name);

//Inheritance:
class Woman extends Person {
    constructor() {
        super();
        this.gender = 'F';
    }
}
```

Modern sintax:

```js
//ES6
constructor() {
    this.myProperty = 'value';
}
myMethod() {...}

//ES7
myProperty = 'value';
myMethod = () => {...};
```

Example ES7:

```js
class Human {
  gender = 'male';

  printGender = () => {
    console.log(this.gender);
  };
}

class Person extends Human {
  name = 'Max';
  gender = 'female';

  printMyName = () => {
    console.log(this.name);
  };
}

const person = new Person();
person.printMyName();
//'Max'
person.printGender();
//'female'
```

### Spread & Rest Operators

#### Spread

```js
const newArray = [...oldArray, 1, 2];
const newObject = { ...oldObject, newProp: 5 };
```

If the oldObject also has a property called newProd, the old version is overwritten by the newly added one.

#### Rest

```js
function sortArgs(...args) {
  return args.sort();
}

const filter = (...args) => {
  return args.filter((el) => el === 1);
};
console.log(filter(1, 2, 3));
//[1]
```

### Destructuring

```js
[a,b] = ['Hello', 'Max'];
console.log(a) //'Hello'
console.log(b) //'Max'

const numbers = [1, 2, 3];
[num1, , num3] = numbers;
console.log(num3) //3

{name} = {name: 'Max', age: 28};
console.log(name) //'Max'
console.log(age) //undefined
```

### Reference and Primitive Types

Primitive types: numbers, strings, null, undefined, booleans
Reference types: objects, arrays

```js
const number = 1;
const num2 = number;
//makes a real copy of 1

const person = {
  name: 'Max',
};
const secondPerson = person;
//doesn't make a real copy, just copies the same reference as the first object and assigns to the same place in memory.
person.name = 'Manu';
console.log(secondPerson.name);
//'Manu'

//to really copy an object or array, you have to use the ...spread operator!
const thirdPerson = { ...person };
person.name = 'Manuel';
console.log(thirdPerson.name);
//'Manu'
```

### Array Functions

```js
const number = [1, 2, 3];
const doubleNumArray = numbers.map((num) => {
  return num * 2;
});
console.log(doubleNumArray);
//[2, 4, 6]
```

map(), filter(), reduce(), sort(), etc.

## Section 3: React Basics & Working with components

Why components?

- Reausability: don't repeat yourself (DRY principle)
- Separation of Concerns: dont't do too many things in one and the same place (function)
  - Split big chunks of code into multiple smaller functions

---

How is a component built?

> HTML + (CSS) + JS => React

Declarative approach

> Define the desired target state(s) and let React figure out the actual JS DOM instructions. On the other hand, Vanilla JS has an imperative approach.

28. Creating a new React Project

```
cdnpx create-react-app project-name
cd project-name
npm start
```

Or, if you downloaded an existent React project:

```
npm install
npm audit fix
npm audit fix --force
npm start
```

On the newly created project's folder, there will be an **index.js** file. It is the first JS file to be loaded as we visit the webpage. It contains as follows:

```js
import ReactDOM from 'react-dom/client';

import './index.css';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

The folder also contains an **index.html** file. This is the only html file that this single-page-application will have.
In this file, we see a div:

```
<div id="root"></div>
```

App.js:

```js
function App() {
  return (
    <div>
      <h2>Let's get started!</h2>
    </div>
  );
}

export default App;
```

The special sintax that's being used in these files is called JSX.

29. Introducing JSX

    > JSX stands for JavaScript XML (Extensible Markup Language). The JSX sintax we write on development stage will later be transformed by React into browser-readable code, which we can see by accessing the Source panel in the browser's Devtools.

30. Building a first custom component

        1. Create a folder called 'components' inside of 'src' folder
        2. Create a file called 'ExpenseItem.js' inside of 'components' folder
        3. Write a function on the file:

    ```js
    function ExpenseItem() {
      return <h2>Expense Item!</h2>;
    }

    export default ExpenseItem;
    ```

        4.Import that function into App.js:

    ```js
    import ExpenseItem from './components/ExpenseItem';

    function App() {
      return (
        <div>
          <h2>Let's get started!</h2>
          <ExpenseItem></ExpenseItem>
        </div>
      );
    }

    export default App;
    ```

        More complex, with CSS:

    ```js
    import './ExpenseItem.css';

    function ExpenseItem() {
      return (
        <div className="expense-item">
          <div>March 28th 2021</div>
          <div className="expense-item__description">
            <h2>Car Insurance</h2>
            <div className="expense-item__price">$294.67</div>
          </div>
        </div>
      );
    }

    export default ExpenseItem;
    ```

    ### React fragments

    The HTML elements returned by a React component function must be inside of a single and only root element - for example, a div.
    See Max's examples and under the hood explanation [here](#wrapper-component)
    Alternatively, the elements can be pu inside an empty HTML tag:

    ```
    <>
    other elements
    </>
    ```

    This is just the abbreviated form of creating a fragment. Whenever we are passing props through this root element, we have to use the complete form of React.Fragment:

    ````js
    {showEvents &&
        events.map((event, index) => (
          <React.Fragment key={event.id}>
            <h2>
              {index} - {event.title}
            </h2>
            <button onClick={() => handleClick(event.id)}>delete event</button>
          </React.Fragment>
        ))}
        ```

    ````

### Props

Props (properties) allow us to reuse the same component, "feeding" with different content.
In App.js, we set "attributes" to the html tags that are being imported from the component .js. Then, in ExpenseItem.js, we should use (props) as a parameter for the ExpenseItem(props) function. "props" is now the name of an object, and the names of the attibutes we defined in App.js are the names of the properties, whose values are the values of these same attributes.

Props can only be passed from parent to child. If we want to pass them from grandparent to grandchild, we must first pass them from grandparent to parent.

To pass data, state, etc. from child to parent (bottom-up), we need to create a function on the parent, pass this function through props to the child, and then call this function wherever is suitable in the child. This is also called **to lift the state up**.

```js
////App.js
import ExpenseItem from './components/ExpenseItem';

function App() {
  const expenses = [
    {
      id: 'e1',
      title: 'Toilet Paper',
      amount: 94.12,
      date: new Date(2020, 7, 14),
    },
    { id: 'e2', title: 'New TV', amount: 799.49, date: new Date(2021, 2, 12) },
    {
      id: 'e3',
      title: 'Car Insurance',
      amount: 294.67,
      date: new Date(2021, 2, 28),
    },
    {
      id: 'e4',
      title: 'New Desk (Wooden)',
      amount: 450,
      date: new Date(2021, 5, 12),
    },
  ];

  return (
    <div>
      <h2>Let's get started!</h2>
      <ExpenseItem
        title={expenses[0].title}
        amount={expenses[0].amount}
        date={expenses[0].date}
      ></ExpenseItem>
      <ExpenseItem
        title={expenses[1].title}
        amount={expenses[1].amount}
        date={expenses[1].date}
      ></ExpenseItem>
      <ExpenseItem
        title={expenses[2].title}
        amount={expenses[2].amount}
        date={expenses[2].date}
      ></ExpenseItem>
    </div>
  );
}

export default App;

////components/ExpenseItem.js
import './ExpenseItem.css';

function ExpenseItem(props) {
  const expenseDate = new Date(2021, 2, 28);
  const expenseTitle = 'Car Insurance';
  const expenseAmount = 294.67;

  return (
    <div className="expense-item">
      <div>{props.date.toISOString()}</div>
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```

### Compositions

We have a composition when a component is called inside of another component (nested components). For example, inside of Expenses we call ExpenseItem, and inside of that we are calling ExpenseDate.

### props.children

This is a special kind of props that is used to display whatever you include between the opening and closing tags when invoking a component inside of another component.

```js
//Card.js
import './Card.css';

const Card = (props) => {
  const classes = 'card ' + props.className;
  return <div className={classes}>{props.children}</div>;
};

export default Card;

//ExpenseItem.js
import ExpenseDate from './ExpenseDate';
import './ExpenseItem.css';
import Card from '../UI/Card';

const ExpenseItem = (props) => {
  return (
    <Card className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
    </Card>
  );
};

export default ExpenseItem;
```

NN

```js
//App.js
<Modal>
  <h2>10% OFF Coupon Code!</h2>
  <p>Use the code NINJA10 at the checkout.</p>
</Modal>;

//Modal.js
export default function Modal(props) {
  return (
    <div className="modal-backdrop">
      <div className="modal">{props.children}</div>
    </div>
  );
}
```

### Functions as props

We can also pass functions as props. This is particularly useful when we want to change the state of a parent component FROM a child component, or to pass data from child to perent (see max58). In this situation, we have to pass the set state function from the parent to the child as a prop, then point to that prop wherever in out child component.

```js
//App.js
const [showModal, setShowModal] = useState(true);

const handleClose = () => {
  setShowModal(false);
};

{
  showModal && (
    <Modal handleClose={handleClose}>
      <h2>Terms and Conditions</h2>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Odio sunt
        quaerat nam, eos ex unde beatae alias voluptates nobis cupiditate? Lorem
        ipsum dolor sit amet, consectetur adipisicing elit. Sequi, repellat.
      </p>
    </Modal>
  );
}

//Modal.js
export default function Modal(props) {
  return (
    <div className="modal-backdrop">
      <div className="modal">
        {props.children}
        <button onClick={props.handleClose}>close</button>
      </div>
    </div>
  );
}
```

### React Portal

More examples and usage details [here](#react-portals).
When we want the component to be added somewhere else in the DOM, and not on the original parent component's DOM, we can use the method ReactDOM.createPortal() to set another place on the DOM to place it.

This is specially useful, for example, when adding a Modal on the page, as modal should be placed right under the beginning of the body element on DOM, or on the end of the body element - and not inside other element. Semantically, it will make more sense.

ReactDOM.cretePortal() accepts 2 arguments:
A) the first is the HTML/JSX code that will be inserted;
B) the second is a common DOM element that will receive this piece of code as an append.

```js
import ReactDOM from 'react-dom';
import './Modal.css';

export default function Modal(props) {
  return ReactDOM.createPortal(
    <div className="modal-backdrop">
      <div className="modal">
        {props.children}
        <button onClick={props.handleClose}>close</button>
      </div>
    </div>,
    document.body
  );
}
```

---

## React State and Working with Events

### Listening to Events

Inside of a JSX parenthesis, we can add special attributes that begin with 'on', eg, 'onClick'.

```js
<button
  onClick={() => {
    console.log('Clicked');
  }}
>
  Click Me!
</button>
```

Defining a named function:

```js
const ExpenseItem = (props) => {
  const clickHandler = () => {
    console.log('Clicked!!!');
  };

  return (
    <Card className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
      <button onClick={clickHandler}>Click Me</button>
    </Card>
  );
};
```

### useState

State is used to update data from a component when this component is reloaded due to a click or other event.

NN

```js
import './App.css';
import { useState } from 'react';

function App() {
  const [name, setName] = useState('mario');

  const handleClick = () => {
    setName('luigi');
    console.log(name);
  };

  return (
    <div className="App">
      <h1>My name is {name}</h1>
      <button onClick={handleClick}>Change name</button>
    </div>
  );
}

export default App;
```

Max:

```js
import React, { useState } from 'react';

import ExpenseDate from './ExpenseDate';
import './ExpenseItem.css';
import Card from '../UI/Card';

const ExpenseItem = (props) => {
  const [title, setTitle] = useState(props.title);

  const clickHandler = () => {
    setTitle('Updated!');
    console.log('Clicked!!!');
  };

  return (
    <Card className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
      <button onClick={clickHandler}>Click Me</button>
    </Card>
  );
};

export default ExpenseItem;
```

### multiple useState (getting input values from a form)

There are two ways of doing it:

```js
import React, { useState } from 'react';
import './ExpenseForm.css';

const ExpenseForm = () => {
  const [enteredTitle, setEnteredTitle] = useState('');
  const [enteredAmount, setEnteredAmount] = useState('');
  const [enteredDate, setEnteredDate] = useState('');

  const titleChangeHandler = (event) => {
    setEnteredTitle(event.target.value);
  };

  const amountChangeHandler = (event) => {
    setEnteredAmount(event.target.value);
  };

  const dateChangeHandler = (event) => {
    setEnteredDate(event.target.value);
  };

  console.log(enteredTitle, enteredAmount, enteredDate);

  return (
    <form>
      <div className="new-expense__controls">
        <div className="new-expense__control">
          <label>Title</label>
          <input type="text" onChange={titleChangeHandler} />
        </div>
        <div className="new-expense__control">
          <label>Amount</label>
          <input
            type="number"
            min="0.01"
            step="0.01"
            onChange={amountChangeHandler}
          />
        </div>
        <div className="new-expense__control">
          <label>Date</label>
          <input
            type="date"
            min="2019-01-01"
            max="2022-12-31"
            onChange={dateChangeHandler}
          />
        </div>
      </div>
      <div className="new-expense__actions">
        <button type="submit">Add Expense</button>
      </div>
    </form>
  );
};

export default ExpenseForm;
```

Or:

```js
import React, { useState } from 'react';
import './ExpenseForm.css';

const ExpenseForm = () => {
  //   const [enteredTitle, setEnteredTitle] = useState('');
  //   const [enteredAmount, setEnteredAmount] = useState('');
  //   const [enteredDate, setEnteredDate] = useState('');
  const [userInput, setUserInput] = useState({
    enteredTitle: '',
    enteredAmount: '',
    enteredDate: '',
  });

  const titleChangeHandler = (event) => {
    setUserInput({
      ...userInput,
      enteredTitle: event.target.value,
    });
  };

  const amountChangeHandler = (event) => {
    // setEnteredAmount(event.target.value);
    setUserInput({
      ...userInput,
      enteredAmount: event.target.value,
    });
  };

  const dateChangeHandler = (event) => {
    // setEnteredDate(event.target.value);
    setUserInput({
      ...userInput,
      enteredDate: event.target.value,
    });
  };

  console.log(userInput);

  return (
    <form>
      <div className="new-expense__controls">
        <div className="new-expense__control">
          <label>Title</label>
          <input type="text" onChange={titleChangeHandler} />
        </div>
        <div className="new-expense__control">
          <label>Amount</label>
          <input
            type="number"
            min="0.01"
            step="0.01"
            onChange={amountChangeHandler}
          />
        </div>
        <div className="new-expense__control">
          <label>Date</label>
          <input
            type="date"
            min="2019-01-01"
            max="2022-12-31"
            onChange={dateChangeHandler}
          />
        </div>
      </div>
      <div className="new-expense__actions">
        <button type="submit">Add Expense</button>
      </div>
    </form>
  );
};

export default ExpenseForm;
```

### outputting lists

Whenever outputting a list of items, we must provide each item an unique "key" attribute/prop. This will be useful if later we want to delete or update a particular item.

NN

```js
import './App.css';
import { useState } from 'react';

function App() {
  const [name, setName] = useState('mario');
  const [events, setEvents] = useState([
    { title: "mario's birthday bash", id: 1 },
    { title: "bowser's live stream", id: 2 },
    { title: 'race on moo moo farm', id: 3 },
  ]);

  const handleClick = () => {
    setName('luigi');
    console.log(name);
  };

  return (
    <div className="App">
      <h1>My name is {name}</h1>
      <button onClick={handleClick}>Change name</button>
      {events.map((event, index) => (
        <div key={event.id}>
          <h2>
            {index} - {event.title}
          </h2>
        </div>
      ))}
    </div>
  );
}

export default App;
```

### deleting one item on the list

NN

```js
import './App.css';
import { useState } from 'react';

function App() {
  const [events, setEvents] = useState([
    { title: "mario's birthday bash", id: 1 },
    { title: "bowser's live stream", id: 2 },
    { title: 'race on moo moo farm', id: 3 },
  ]);

  const handleClick = (id) => {
    setEvents(
      events.filter((event) => {
        return id !== event.id;
        //when returning true, the item will be maintained, when returning false, the item will be omitted.
      })
    );
    console.log(id);
  };

  return (
    <div className="App">
      {events.map((event, index) => (
        <div key={event.id}>
          <h2>
            {index} - {event.title}
          </h2>
          <button onClick={() => handleClick(event.id)}>delete event</button>
        </div>
      ))}
    </div>
  );
}

export default App;
```

When the current state depends on the previous state's id to be updated, it is good practice to rename the previous state's argument on the callback of the setEvents function.

The previous state is not declared anywhere by us, it is something that React keeps track of states during the render cycles of components and it automatically gives us the previous state snapshot if we decide to go with the functional form of the state updating function where we get this prevState as an argument.

prevEvents is a positional argument, anything you put in that place will refer to the previous state. When you have a state-setting function like setEvents(zorgs), React knows to set your state to whatever value is inside those parentheses, which in this case is 'zorgs'. But when we have an arrow function inside those parenthesis?

setEvents( (zorgs) => { return blah blah blah } )

...React will take whatever the argument of that inner function is to mean "the previous state." So even though you haven't declared "zorgs" anywhere, it doesn't matter because whatever is inside the inner arrow function parenthesis will represent the previous state.

In general it's a good practice to use the functional approach of the state updating function if the new state depends on the previous state. With this approach it's guaranteed by React that you always get the latest state automatically.

Otherwise you might get undesired results since setState is executed asynchronously. So:

```js
import './App.css';
import { useState } from 'react';

function App() {
  const [events, setEvents] = useState([
    { title: "mario's birthday bash", id: 1 },
    { title: "bowser's live stream", id: 2 },
    { title: 'race on moo moo farm', id: 3 },
  ]);

  const handleClick = (id) => {
    setEvents((prevEvents) => {
      return prevEvents.filter((event) => {
        return id !== event.id;
      });
    });
    console.log(id);
  };

  return (
    <div className="App">
      {events.map((event, index) => (
        <div key={event.id}>
          <h2>
            {index} - {event.title}
          </h2>
          <button onClick={() => handleClick(event.id)}>delete event</button>
        </div>
      ))}
    </div>
  );
}

export default App;
```

### Conditional templates

Another use for state in a conponent is to conditionally output parts of the template based on the state.

```js
import './App.css';
import { useState } from 'react';

function App() {
  const [showEvents, setShowEvents] = useState(true);
  const [events, setEvents] = useState([
    { title: "mario's birthday bash", id: 1 },
    { title: "bowser's live stream", id: 2 },
    { title: 'race on moo moo farm', id: 3 },
  ]);

  console.log(showEvents);

  const handleClick = (id) => {
    setEvents((prevEvents) => {
      return prevEvents.filter((event) => {
        return id !== event.id;
      });
    });
    console.log(id);
  };

  return (
    <div className="App">
      //only show this button when showEvents is true:
      {showEvents && (
        <div>
          <button onClick={() => setShowEvents(false)}>hide events</button>
        </div>
      )}
      //only show this button when showEvents is false:
      {!showEvents && (
        <div>
          <button onClick={() => setShowEvents(true)}>show events</button>
        </div>
      )}
      //only run this logic when showEvents is true:
      {showEvents &&
        events.map((event, index) => (
          <div key={event.id}>
            <h2>
              {index} - {event.title}
            </h2>
            <button onClick={() => handleClick(event.id)}>delete event</button>
          </div>
        ))}
    </div>
  );
}

export default App;
```

### Styling components (CSS)

NN section#6

#### Global stylesheets

Global stylesheets will style the **index.js** file. This is the js file that creates the **root component** from App.js inside the id root in the DOM (index.html). So all styles formatted here will refer to classes and elements of all React component in that project. **index.css** must be imported only into the **index.js** file.

#### Component stylesheets

These are stylesheets that are specific to a particular component. They have to be imported into that specific component.
**Take care!** After being compiled, component stylesheets will be added to the html head, so they will also be global. This can cause unpredicted results if we define the same selectors on more than one stylesheet on our project.
The way to resolve this problem is to add a **root class** inside components. For example, add **modal** class (via className) in our Modal.js component, so that we can then style our h3 without affecting all h3 of the project.

#### Inline styles

Just add a **style** attribute to the JSX/html element. Put one {} to show that it's a JS value, and another {} to open and close a JS object. The CSS properties/values are key/value pairs of that object. The properties' names must be camelCased whenever needed.

```js
<div className="modal-backdrop">
  <div
    className="modal"
    style={{
      border: '$px solid',
      borderColor: '#ff4500',
      textAlign: 'center',
    }}
  >
    {children}
    <button onClick={handleClick}>close</button>
  </div>
</div>
```

#### Dynamic inline styles

We can add conditional values to inline styles, by using a ternary operator. Don't forget to pass the prop of the value that will be evaluated during the process (in this case, it's the isSalesModal prop).

```js
//App.js
***<Modal handleClose={handleClose} isSalesModal={true}>
  <h2>Terms and Conditions</h2>
  <p>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Odio sunt quaerat
    nam, eos ex unde beatae alias voluptates nobis cupiditate? Lorem ipsum dolor
    sit amet, consectetur adipisicing elit. Sequi, repellat.
  </p>
</Modal>

//Modal.js
export default function Modal({ children, handleClose, ***isSalesModal }) {
  return ReactDOM.createPortal(
    <div className="modal-backdrop">
      <div
        className="modal"
        style={{
          border: '4px solid',
          ***borderColor: isSalesModal ? '#ff4500' : '#555',
          textAlign: 'center',
        }}
      >
        {children}
        <button onClick={handleClose}>close</button>
      </div>
    </div>,
    document.body
  );
}
```

#### Conditional CSS classes

We can also apply conditional classes using ternary operator.

```js
export default function Modal({ children, handleClose, isSalesModal }) {
  return ReactDOM.createPortal(
    <div className="modal-backdrop">
      <div
        className="modal"
        style={{
          border: '4px solid',
          borderColor: isSalesModal ? '#ff4500' : '#555',
          textAlign: 'center',
        }}
      >
        {children}
        <button
          onClick={handleClose}
          className={isSalesModal ? 'sales-btn' : ''}
        >
          close
        </button>
      </div>
    </div>,
    document.body
  );
}

//Modal.css
.modal .sales-btn {
  border: 4px solid #333;
  font-size: 18px;
  text-transform: uppercase;
}
```

One more example by Max (#75). Here, we use backticks to inject one more class, conditionally:

```js
return (
  <form onSubmit={formSubmitHandler}>
    <div className={`form-control ${!isValid ? 'invalid' : ''}`}>
      <label>Course Goal</label>
      <input type="text" onChange={goalInputChangeHandler} />
    </div>
    <Button type="submit">Add Goal</Button>
  </form>
);
```

#### CSS modules

CSS modules will scope the css .class and #id selectors only to the specific js file into which they are imported and used. The element selectors WON'T be scoped when used without any reference to a .class or #id selector.
There is a special way to import CSS modules into the components:

```js
//EventList.module.css
.card {
  border: 1px;
  box-shadow: 4px 4px 5px rgba(0, 0, 0, 0.05);
  padding: 10px;
  max-width: 400px;
  margin: 20px auto;
  border-radius: 4px;
}

//EventList.js
import styles from './EventList.module.css';

export default function EventList(props) {
  return (
    <div>
      {props.events.map((event, index) => (
        <div key={event.id} className={styles.card}>
          <h2>
            {index} - {event.title}
          </h2>
          <button onClick={() => props.handleClick(event.id)}>
            delete event
          </button>
        </div>
      ))}
    </div>
  );
}
```

### Styled components

**styled-components** is a package that creates React components which receive styles in a more direct syntax.
Installation: `npm install --save styled-components`

This example shows that, by using styled-components, there is no need to pass props anymore, because styled-components will track them all for us.

```js
////Button.js
// import React from 'react';
// import './Button.css';
import styled from 'styled-components';

const Button = styled.button`
  font: inherit;
  padding: 0.5rem 1.5rem;
  border: 1px solid #8b005d;
  color: white;
  background: #8b005d;
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.26);
  cursor: pointer;

  &:focus {
    outline: none;
  }

  &:hover,
  &:active {
    background: #ac0e77;
    border-color: #ac0e77;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.26);
  }
`;

// const Button = props => {
//   return (
//     <button type={props.type} className="button" onClick={props.onClick}>
//       {props.children}
//     </button>
//   );
// };

export default Button;

////
```

#### Adding dynamic classes inside a styled-component

Here we are creating a styled-component inside the same file that will call this component. We are also dynamically setting the 'invalid' class to it.

```js
import React, { useState } from 'react';

import Button from '../../UI/Button/Button';
import './CourseInput.css';

import styled from 'styled-components';

const FormControl = styled.div`
  margin: 0.5rem 0;

  & label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
  }

  & input {
    display: block;
    width: 100%;
    border: 1px solid #ccc;
    font: inherit;
    line-height: 1.5rem;
    padding: 0 0.25rem;
  }

  & input:focus {
    outline: none;
    background: #fad0ec;
    border-color: #8b005d;
  }

  &.invalid input {
    border-color: red;
    background: #ffd7d7;
  }

  &.invalid label {
    color: red;
  }
`;

const CourseInput = (props) => {
  const [enteredValue, setEnteredValue] = useState('');
  const [isValid, setIsValid] = useState(true);

  const goalInputChangeHandler = (event) => {
    if (event.target.value.trim().length > 0) {
      setIsValid(true);
    }
    setEnteredValue(event.target.value);
  };

  const formSubmitHandler = (event) => {
    event.preventDefault();
    if (enteredValue.trim().length === 0) {
      setIsValid(false);
      return;
    }
    props.onAddGoal(enteredValue);
  };

  return (
    <form onSubmit={formSubmitHandler}>
      <FormControl className={!isValid && 'invalid'}>
        <label>Course Goal</label>
        <input type="text" onChange={goalInputChangeHandler} />
      </FormControl>
      <Button type="submit">Add Goal</Button>
    </form>
  );
};

export default CourseInput;
```

Another way of dynamically changing styles with styled-components is to pass a prop (in this case, 'invalid') and then, in the styles, pass a function:

```js
import React, { useState } from 'react';

import Button from '../../UI/Button/Button';
import './CourseInput.css';

import styled from 'styled-components';

const FormControl = styled.div`
  margin: 0.5rem 0;

  & label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
    ***color: ${(props) => (props.invalid ? 'red' : 'black')}
  }

  & input {
    display: block;
    width: 100%;
    ***border: 1px solid ${(props) => (props.invalid ? 'red' : '#ccc')};
    ***background: ${(props) => (props.invalid ? '#ffd7d7' : 'transparent')}
    font: inherit;
    line-height: 1.5rem;
    padding: 0 0.25rem;
  }

  & input:focus {
    outline: none;
    background: #fad0ec;
    border-color: #8b005d;
  }
`;

const CourseInput = (props) => {
  const [enteredValue, setEnteredValue] = useState('');
  const [isValid, setIsValid] = useState(true);

  const goalInputChangeHandler = (event) => {
    if (event.target.value.trim().length > 0) {
      setIsValid(true);
    }
    setEnteredValue(event.target.value);
  };

  const formSubmitHandler = (event) => {
    event.preventDefault();
    if (enteredValue.trim().length === 0) {
      setIsValid(false);
      return;
    }
    props.onAddGoal(enteredValue);
  };

  return (
    <form onSubmit={formSubmitHandler}>
      ***
      <FormControl invalid={!isValid}>
        <label>Course Goal</label>
        <input type="text" onChange={goalInputChangeHandler} />
      </FormControl>
      <Button type="submit">Add Goal</Button>
    </form>
  );
};

export default CourseInput;
```

#### Styled-components and media queries

It's pretty easy to add media queries when using styled-components:

```js
//Button.js
import styled from 'styled-components';

const Button = styled.button`
  width: 100%;
  font: inherit;
  padding: 0.5rem 1.5rem;
  border: 1px solid #8b005d;
  color: white;
  background: #8b005d;
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.26);
  cursor: pointer;

  @media (min-width: 768px) {
    width: auto;
    color: 'green';
  }

  &:focus {
    outline: none;
  }

  &:hover,
  &:active {
    background: #ac0e77;
    border-color: #ac0e77;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.26);
  }
`;

export default Button;
```

This will set the button's width to auto and the button color to green when the screen width is below 768px.

## Forms and Events

NN section#7

### The onChange event

**onChange** event should be added into the input element. This event's value will be an anonymous function with the (e) object as an argument, the function sets the state of title and date by using setTitle and setDate. Remember to define states for title and date to store the input values.

```js
import { useState } from 'react';
import './NewEventForm.css';

export default function NewEventForm() {
  const [title, setTitle] = useState('');
  const [date, setDate] = useState('');

  return (
    <form className="new-event-form">
      <label>
        <span>Event Title:</span>
        <input type="text" onChange={(e) => setTitle(e.target.value)} />
      </label>
      <label>
        <span>Event Date:</span>
        <input type="date" onChange={(e) => setDate(e.target.value)} />
      </label>
      <button>Submit</button>
      <p>
        title: {title}, date: {date}
      </p>
    </form>
  );
}
```

### Controlled inputs

When we control an input value from outside of it, this is a controlled input. For example, when we reset an input to empty using a setState function, we are controlling it.

In this example, the resetForm function resets the states of title and date, and the input value is also reset to empty.

This is also called **two way binding**. See MAX lecture#57.

```js
export default function NewEventForm() {
  const [title, setTitle] = useState('');
  const [date, setDate] = useState('');

  const resetForm = () => {
    setTitle('');
    setDate('');
  };

  return (
    <form className="new-event-form">
      <label>
        <span>Event Title:</span>
        <input
          type="text"
          onChange={(e) => setTitle(e.target.value)}
          value={title}
        />
      </label>
      <label>
        <span>Event Date:</span>
        <input
          type="date"
          onChange={(e) => setDate(e.target.value)}
          value={date}
        />
      </label>
      <button>Submit</button>
      <p>
        title: {title}, date: {date}
      </p>
      <p onClick={resetForm}>reset the form</p>
    </form>
  );
}
```

### Submitting forms (onSubmit event)

The **onSubmit** event must be added into the **form** element, not into the **submit** element.
Don't forget to call e.preventDefault() in the handleSubmit function to prevent the browser's default behavior to form submission.

```js
export default function NewEventForm() {
  const [title, setTitle] = useState('');
  const [date, setDate] = useState('');

  const resetForm = () => {
    setTitle('');
    setDate('');
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    const event = {
      title: title,
      date: date,
      id: Math.floor(Math.random() * 10000),
    };
    console.log(event);
    resetForm();
  };

  return (
    <form className="new-event-form" onSubmit={handleSubmit}>
      <label>
        <span>Event Title:</span>
        <input
          type="text"
          onChange={(e) => setTitle(e.target.value)}
          value={title}
        />
      </label>
      <label>
        <span>Event Date:</span>
        <input
          type="date"
          onChange={(e) => setDate(e.target.value)}
          value={date}
        />
      </label>
      <button>Submit</button>
    </form>
  );
}
```

### Adding events to the Events List

To add events to the events array in the App.js file, we first have to make the handleSubmit function create a **event** object. Then, in the App.js we create a function called **addEvent**, then include this function as a prop of the NewEventForm component. Then we can call the addEvent function inside NewEventForm.js.

```js
//App.js
function App() {
  const [showModal, setShowModal] = useState(false);
  const [showEvents, setShowEvents] = useState(true);
  const [events, setEvents] = useState([
    { title: "mario's birthday bash", id: 1 },
    { title: "bowser's live stream", id: 2 },
    { title: 'race on moo moo farm', id: 3 },
  ]);

  const addEvent = (event) => {
    setEvents((prevEvents) => {
      return [...prevEvents, event];
    });
    setShowModal(false);
  };

  const handleClick = (id) => {
    setEvents((prevEvents) => {
      return prevEvents.filter((event) => {
        return id !== event.id;
      });
    });
    console.log(id);
  };

  const subtitle = 'All the latest events in Marioland';

  return (
    <div className="App">
      <Title title="Events in Your Area" subtitle={subtitle} />

      {showEvents && (
        <div>
          <button onClick={() => setShowEvents(false)}>hide events</button>
        </div>
      )}
      {!showEvents && (
        <div>
          <button onClick={() => setShowEvents(true)}>show events</button>
        </div>
      )}
      {showEvents && <EventList events={events} handleClick={handleClick} />}

      {showModal && (
        <Modal isSalesModal={true}>
          <NewEventForm addEvent={addEvent} />
        </Modal>
      )}

      <br />
      <div>
        <button onClick={() => setShowModal(true)}>Add New Event</button>
      </div>
    </div>
  );
}

//NewEventForm.js
export default function NewEventForm({ addEvent }) {
  const [title, setTitle] = useState('');
  const [date, setDate] = useState('');

  const resetForm = () => {
    setTitle('');
    setDate('');
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    const event = {
      title: title,
      date: date,
      id: Math.floor(Math.random() * 10000),
    };
    console.log(event);
    addEvent(event);
    resetForm();
  };

  return (
    <form className="new-event-form" onSubmit={handleSubmit}>
      <label>
        <span>Event Title:</span>
        <input
          type="text"
          onChange={(e) => setTitle(e.target.value)}
          value={title}
        />
      </label>
      <label>
        <span>Event Date:</span>
        <input
          type="date"
          onChange={(e) => setDate(e.target.value)}
          value={date}
        />
      </label>
      <button>Submit</button>
    </form>
  );
}
```

### useRef hook

**useRef** is a hook that is pretty similar to useState, but has some differences.
When we call `const ourRef = useRef(0);`, ourRef's value will be an **object**. This object has a property called **current**, that stores the current value of ourRef.

Every element in the DOM has an attribute called ref which we can set our ref to.

When the reference value is changed, it is updated without the need to refresh or re-render. However in useState, the component must render again to update the state or its value.

Refs are useful when getting user input, DOM element properties and storing constantly updating values. However, if you are storing component related info or use methods in components states are the best option. Let's now "translate" our form input using useRef:

```js
import { useRef } from 'react';
import './NewEventForm.css';

export default function NewEventForm({ addEvent }) {
  const title = useRef();
  const date = useRef();

  const resetForm = () => {
    title.current.value = '';
    date.current.value = '';
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    const event = {
      title: title.current.value,
      date: date.current.value,
      id: Math.floor(Math.random() * 10000),
    };
    addEvent(event);
    resetForm();
  };

  return (
    <form className="new-event-form" onSubmit={handleSubmit}>
      <label>
        <span>Event Title:</span>
        <input type="text" ref={title} />
      </label>
      <label>
        <span>Event Date:</span>
        <input type="date" ref={date} />
      </label>
      <button>Submit</button>
    </form>
  );
}
```

## Fetching data & useEffect

NN section#8
See [below](#fetching-data-from-api), Max examples of HTTP-requests.
See [below](#fetching-data-from-a-database), NN examples inside of React Route.

### useEffect

When fetching data from an API, we must use the useEffect hook, so that the data will run just once - ie, when the page is loaded.

The useEffect hook takes two arguments, the first is a callback function, the second is what we call **dependency array**. This array can be empty.

```js
import { useState, useEffect } from 'react';
import './TripList.css';

export default function TripList() {
  const [trips, setTrips] = useState([]);

  useEffect(() => {
    fetch('http://localhost:3000/trips')
      .then((response) => response.json())
      .then((json) => setTrips(json));
  }, []);

  console.log(trips);

  return (
    <div className="trip-list">
      <h2>TripList</h2>
      <ul>
        {trips.map((trip, index) => (
          <li key={trip.id}>
            <h3>{trip.title}</h3>
            <p>{trip.price}</p>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### The useEffect dependency array

Each time that the values on the dependencies array change, the component will be re-rendered. If the dependencies array is empty, then the component will be loaded just once.

```js
export default function TripList() {
  const [trips, setTrips] = useState([]);
  const [url, setUrl] = useState('http://localhost:3000/trips');

  const fetchTrips = async () => {
    const response = await fetch(url);
    const json = await response.json();
    setTrips(json);
  };

  useEffect(() => {
    fetch(url)
      .then((response) => response.json())
      .then((json) => setTrips(json));
  }, [url]);

  console.log(trips);

  return (
    <div className="trip-list">
      <h2>TripList</h2>
      <ul>
        {trips.map((trip, index) => (
          <li key={trip.id}>
            <h3>{trip.title}</h3>
            <p>{trip.price}</p>
          </li>
        ))}
      </ul>
      <div className="filters">
        <button
          onClick={() => setUrl('http://localhost:3000/trips?loc=europe')}
        >
          European Trips
        </button>
        <button onClick={() => setUrl('http://localhost:3000/trips')}>
          All Trips
        </button>
      </div>
    </div>
  );
}
```

### useCallback for Function Dependencies

Whenever one of our useEffect dependencies is a function, we must use useCallback, so that useEffect doesn't create an infinite loop as it runs.

```js
import { useState, useEffect, useCallback } from 'react';
import './TripList.css';

export default function TripList() {
  const [trips, setTrips] = useState([]);
  const [url, setUrl] = useState('http://localhost:3000/trips');

  const fetchTrips = useCallback(async () => {
    const response = await fetch(url);
    const json = await response.json();
    setTrips(json);
  }, [url]);

  useEffect(() => {
    fetchTrips();
  }, [fetchTrips]);

  console.log(trips);

  return (
    <div className="trip-list">
      <h2>TripList</h2>
      <ul>
        {trips.map((trip, index) => (
          <li key={trip.id}>
            <h3>{trip.title}</h3>
            <p>{trip.price}</p>
          </li>
        ))}
      </ul>
      <div className="filters">
        <button
          onClick={() => setUrl('http://localhost:3000/trips?loc=europe')}
        >
          European Trips
        </button>
        <button onClick={() => setUrl('http://localhost:3000/trips')}>
          All Trips
        </button>
      </div>
    </div>
  );
}
```

### Creating a custom React Hook

We can create a custom hook to fetch data from the API, so that everytime we need to fetch data, we don't need to repeat the same code again.

Usually, custom hooks can be stored in separated JS files in a separated folder called 'hooks'.

See more about Custom Hooks [here](#custom-hooks)

```js
// hooks/useFetch.js
import { useState, useEffect } from 'react';

export const useFetch = (url) => {
  const [data, setData] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      const res = await fetch(url);
      const json = await res.json();
      setData(json);
    };

    fetchData();
  }, [url]);

  return { data: data };
};

// components/TripList.js
import { useState, useEffect } from 'react';

export const useFetch = (url) => {
  const [data, setData] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      const res = await fetch(url);
      const json = await res.json();
      setData(json);
    };

    fetchData();
  }, [url]);

  return { data: data };
};
```

### Adding a loading/pending state

Just review.
We can add a new isPending state into our useFetch hook. Then return the state off the hook and use it inside our TripList component.

```js
//hooks/useFetch.js
import { useState, useEffect } from 'react';

export const useFetch = (url) => {
  const [data, setData] = useState(null);
  const [isPending, setIsPending] = useState(false);

  useEffect(() => {
    const fetchData = async () => {
      setIsPending(true);

      const res = await fetch(url);
      const json = await res.json();

      setIsPending(false);
      setData(json);
    };

    fetchData();
  }, [url]);

  return { data, isPending };
};

//components/TripList.js
export default function TripList() {
  const [url, setUrl] = useState('http://localhost:3000/trips');
  const { data: trips, isPending } = useFetch(url);

  return (
    <div className="trip-list">
      <h2>TripList</h2>
      {isPending && <div>Loading trips...</div>}
      <ul>
        {trips &&
          trips.map((trip, index) => (
            <li key={trip.id}>
              <h3>{trip.title}</h3>
              <p>{trip.price}</p>
            </li>
          ))}
      </ul>
      <div className="filters">
        <button
          onClick={() => setUrl('http://localhost:3000/trips?loc=europe')}
        >
          European Trips
        </button>
        <button onClick={() => setUrl('http://localhost:3000/trips')}>
          All Trips
        </button>
      </div>
    </div>
  );
}
```

### Handling errors!

NN lecture#54
In order to handle errors, we must define a state on useFetch called error, setError.
Then we add a try/catch block to catch errors in the fetching process. Because the will always be a response, even if it's an error message, we can add a **guard clause** to create a new Error(), that will be catched.

```js
// hooks/useFetch.js
import { useState, useEffect } from 'react';

export const useFetch = (url) => {
  const [data, setData] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      setIsPending(true);

      try {
        const res = await fetch(url);
        if (!res.ok) {
          throw new Error(res.statusText);
        }
        const json = await res.json();

        setIsPending(false);
        setData(json);
        setError(null);
      } catch (err) {
        setIsPending(false);
        setError('Could not fetch the data');
        console.log(err.message);
      }
    };

    fetchData();
  }, [url]);

  return { data, isPending, error };
};

// components/TripList.js
import { useState } from 'react';
import { useFetch } from '../hooks/useFetch';
import './TripList.css';

export default function TripList() {
  const [url, setUrl] = useState('http://localhost:3000/trips');
  const { data: trips, isPending, error } = useFetch(url);

  return (
    <div className="trip-list">
      <h2>TripList</h2>
      {isPending && <div>Loading trips...</div>}
      {error && <div>{error}</div>}
      <ul>
        {trips &&
          trips.map((trip, index) => (
            <li key={trip.id}>
              <h3>{trip.title}</h3>
              <p>{trip.price}</p>
            </li>
          ))}
      </ul>
      <div className="filters">
        <button
          onClick={() => setUrl('http://localhost:3000/trips?loc=europe')}
        >
          European Trips
        </button>
        <button onClick={() => setUrl('http://localhost:3000/trips')}>
          All Trips
        </button>
      </div>
    </div>
  );
}
```

### Clean-up function (aborting fetch requests)

1. Create a new AbortController() inside of the useEffect callback function
   `const controller = new AbortController()`
2. The fetch method accepts two arguments: a url string and an object of options. Inside that object, we pass a signal property
   `const res = await fetch(url, { signal: controller.signal })`
   This associates the fetch request with the AbortController.
3. At the final of the callback function of the useEffect, return an abort()
   `return () => { controller.abort() }`
4. This will throw an error, that can be catched inside of the catch block
   `if (err.name === 'AbortError') {
   console.log('the fetch was aborted);
   }

## Fragments, Portals & Refs

Max section#9

### Wrapper component

As we know, it's not allowed to return more than one element inside of a JSX component function.
In order to avoid having to nest JSX content inside of a div, we can create a Wrapper component that just returns **props.children**.
Typically, we don't need to create a Wrapper component. This is just to explain how the **<React.Fragment>** element work under the hood.

```js
// components/Helpers/Wrapper.js
// (there is no need to import React, because we are not using JSX in here!)
const Wrapper = (props) => {
  return props.children;
};

export default Wrapper;

//components/Users/AddUser.js
return (
  <Wrapper>
    {error && (
      <ErrorModal
        onConfirm={errorHandler}
        title={error.title}
        message={error.message}
      />
    )}
    <Card className={classes.input}>
      <form onSubmit={addUserHandler}>
        <label htmlFor="username">Username</label>
        <input
          id="username"
          type="text"
          onChange={usernameChangeHandler}
          value={enteredUsername}
        />
        <label htmlFor="age">Age</label>
        <input
          id="age"
          type="number"
          onChange={ageChangeHandler}
          value={enteredAge}
        />
        <Button type="submit">Add User</Button>
      </form>
    </Card>
  </Wrapper>
);
```

### React Portals

Specially useful for customizing the exact place on the DOM structure where we want to put components or parts of components. Eg: modals, sidebars, navbars, etc.

```js
// public/index.html
    <div id="backdrop-root"></div>
    <div id="overlay-root"></div>
    <div id="root"></div>

// components/UI/ErrorModal.js
import React from 'react';
import Card from './Card';
import Button from './Button';
import classes from './ErrorModal.module.css';
import ReactDOM from 'react-dom';

const Backdrop = (props) => {
  return <div className={classes.backdrop} onClick={props.onConfirm} />;
};

const ModalOverlay = (props) => {
  return (
    <Card className={classes.modal}>
      <header className={classes.header}>
        <h2>{props.title}</h2>
      </header>
      <div className={classes.content}>
        <p>{props.message}</p>
      </div>
      <footer className={classes.actions}>
        <Button onClick={props.onConfirm}>Okay</Button>
      </footer>
    </Card>
  );
};

const ErrorModal = (props) => {
  return (
    <React.Fragment>
      {ReactDOM.createPortal(
        <Backdrop onConfirm={props.onConfirm} />,
        document.getElementById('backdrop-root')
      )}
      {ReactDOM.createPortal(
        <ModalOverlay
          title={props.title}
          message={props.message}
          onConfirm={props.onConfirm}
        />,
        document.getElementById('overlay-root')
      )}
    </React.Fragment>
  );
};

export default ErrorModal;
```

## useEffect, Reduce & ContextAPI

### What are side effects?

Max section 10
Up to now, we learned that React's main task are to render the UI and re-render its components whenever the states or props change.

Anything that is out of this scope, we should call **side-effects**. For example, store data in browser storage, sending http requests, set and manage timers, etc.

These tasks must happen outside of the normal component evaluation and render cycle - especially since they might block/delay rendering.

### useEffect

#### useEffect behaviors:

**1)** useEffect hook without mentioning any dependency array like.. useEffect(someCallbackFuction); runs for every render of the functional component in which its included.. (useless)
**2)** useEffect hook with an **empty dependency array** like this.. useEffect(callbackFunc , [] ) is executed only for the the initial render of the the functional component. And then it will not run in the further renders of the same functional Component..
**3)** useEffect hook with **some dependencies** inside the dependency array like this.. useEffect(callbackFunc , [ dependency ] ); will run for the initial render as well as when the render happen due to change in dependencies mentioned in the dependency array...

#### Case 2) Empty dependency array

_Max lecture 113, repo/react-max-section10_

Here, once the user logs in, we set an item in localStorage. Then, the next time the user renders the app, the isLoggedIn state changes to true, without causing infinite loops. If no useEffect was used, once the localStorage is read, the state would change and the component would render again, causing infinite loop.

```js
//App.js
import React, { useState, useEffect } from 'react';

import Login from './components/Login/Login';
import Home from './components/Home/Home';
import MainHeader from './components/MainHeader/MainHeader';

function App() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  useEffect(() => {
    const storedUserLoggedInInformation = localStorage.getItem('isLoggedIn');

    if (storedUserLoggedInInformation === '1') {
      setIsLoggedIn(true);
    }
  }, []);

  const loginHandler = (email, password) => {
    localStorage.setItem('isLoggedIn', '1');
    setIsLoggedIn(true);
  };

  const logoutHandler = () => {
    localStorage.removeItem('isLoggedIn');
    setIsLoggedIn(false);
  };

  return (
    <React.Fragment>
      <MainHeader isAuthenticated={isLoggedIn} onLogout={logoutHandler} />
      <main>
        {!isLoggedIn && <Login onLogin={loginHandler} />}
        {isLoggedIn && <Home onLogout={logoutHandler} />}
      </main>
    </React.Fragment>
  );
}

export default App;
```

#### Case 3) useEffect with dependencies (and clean up function)

_Max lecture 113, repo/react-max-section10_

Here, every time the input changes, enteredEmail and enteredPassword will change, so the useEffect will be called.

To avoid calling it at every key-press, a Timeout was set, so that the function will wait 500ms to be called. Everytime the user presses a key before the 500ms has passed, the clean-up function will execute, cleaning the anterior Timeout. The current Timeout, however, will continue to run, and if the user stops typing for more than 500ms, then the current useEffect callback function will run.

`Clean up function runs AFTER the new render but BEFORE the 'new' effects are applied.`

So in essence, the previous effect is cleaned up first before executing the next effect. This is mainly done for performance optimization so that the rendering process does not suffer any delay.

```js
//Login.js
import React, { useState, useEffect } from 'react';

import Card from '../UI/Card/Card';
import classes from './Login.module.css';
import Button from '../UI/Button/Button';

const Login = (props) => {
  const [enteredEmail, setEnteredEmail] = useState('');
  const [emailIsValid, setEmailIsValid] = useState();
  const [enteredPassword, setEnteredPassword] = useState('');
  const [passwordIsValid, setPasswordIsValid] = useState();
  const [formIsValid, setFormIsValid] = useState(false);

  useEffect(() => {
    const identifier = setTimeout(() => {
      console.log('Checking form validity!');
      setFormIsValid(
        enteredEmail.includes('@') && enteredPassword.trim().length > 6
      );
    }, 500);

    return () => {
      console.log('CLEANUP');
      clearTimeout(identifier);
    };
  }, [enteredEmail, enteredPassword]);

  const emailChangeHandler = (event) => {
    setEnteredEmail(event.target.value);
  };

  const passwordChangeHandler = (event) => {
    setEnteredPassword(event.target.value);

    setFormIsValid(
      event.target.value.trim().length > 6 && enteredEmail.includes('@')
    );
  };

  const validateEmailHandler = () => {
    setEmailIsValid(enteredEmail.includes('@'));
  };

  const validatePasswordHandler = () => {
    setPasswordIsValid(enteredPassword.trim().length > 6);
  };

  const submitHandler = (event) => {
    event.preventDefault();
    props.onLogin(enteredEmail, enteredPassword);
  };

  return (
    <Card className={classes.login}>
      <form onSubmit={submitHandler}>
        <div
          className={`${classes.control} ${
            emailIsValid === false ? classes.invalid : ''
          }`}
        >
          <label htmlFor="email">E-Mail</label>
          <input
            type="email"
            id="email"
            value={enteredEmail}
            onChange={emailChangeHandler}
            onBlur={validateEmailHandler}
          />
        </div>
        <div
          className={`${classes.control} ${
            passwordIsValid === false ? classes.invalid : ''
          }`}
        >
          <label htmlFor="password">Password</label>
          <input
            type="password"
            id="password"
            value={enteredPassword}
            onChange={passwordChangeHandler}
            onBlur={validatePasswordHandler}
          />
        </div>
        <div className={classes.actions}>
          <Button type="submit" className={classes.btn} disabled={!formIsValid}>
            Login
          </Button>
        </div>
      </form>
    </Card>
  );
};

export default Login;
```

### useReducer

Sometimes, you have ++more complex state++ - for example if you got ++multiple states++, ++multiple ways of changing it++ or ++dependencies++ to other states.

useState() then often becomes hard or error-prone to use - it's easy to write bad or buggy code in such scenarios.

**useReducer()** can be used as a replacement for useState() if you need more powerful state management.
**COMPLETAR**

## React Router

Working on react-recipe-directory repo (NN section 11)

### BrowserRouter, Router, Link, NavLink, Switch components

Whenever we want to selectively output a component when clicking a link, instead of using anchor tags, we will use **Link** and **NavLink** components.

**NavLink** is the same as **Link**, but it has more features. We can add an **activeClassName** attribute to NavLink, it will add this class on that element whenever it's active (whenever we're ON THAT path). Eg:
`<NavLink activeClassName={classes.active} to="/products">`

The paths of these Links and NavLinks must be surrounded by a **Router** component on the root file. The Router component must be surrounded by a **Switch** component to tell React to open one of those components selectively. The **exact** prop/attribute must be used to ensure the "/" path doesn't conflict with other paths that begin with "/".
The difference between Link and NavLink is that, using NavLink, it automatically adds classes like .active on our links, these classes can be easily manipulated through CSS.

```js
npm i react-router-dom

//App.js
import './App.css';
import { BrowserRouter, Route, Switch, Link, NavLink } from 'react-router-dom';

//page components:
import Home from './pages/Home';
import Contact from './pages/Contact';
import About from './pages/About';

function App() {
  return (
    <div className="App">
      <BrowserRouter>
        <nav>
          <h1>My Articles</h1>
          <NavLink exact to="/">
            Home
          </NavLink>
          <NavLink to="/about">About</NavLink>
          <NavLink to="/contact">Contact</NavLink>
        </nav>

        <Switch>
          <Route exact path="/">
            <Home />
          </Route>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/contact">
            <Contact />
          </Route>
        </Switch>
      </BrowserRouter>
    </div>
  );
}

export default App;

//App.css
.App {
  max-width: 960px;
  margin: 0 auto;
}
nav {
  display: flex;
  margin: 0 auto 60px;
  gap: 10px;
  align-items: center;
}
nav h1 {
  margin-right: auto;
}
nav a {
  color: #333;
  padding: 4px 10px;
}
nav a.active {
  color: white;
  background: #333;
  text-decoration: none;
}
```

`the difference between <a> and <Link>... inside of a react-app, both should work. but the anchor tag will send a request to a new html page, while the <Link> will treat the app as a single-page application with a client-side router, thus won't send a new http request.`

### Nested routes

We can create new routes inside of a page component that is already inside of a route.
We just need to import Route from 'react-router-dom', then add Route element with a path that already contains the path of the page we're in.
For example, to nest another route on /welcome: `<Route path='welcome/new-user'>`
Nested routed can be used for output a new component, or to conditionally show another component or piece of code on that same page.
In this example below, `<p>Welcome, new user!</p>` will only be shown if the path is `/welcome/new-user`. Users who go to `/welcome` will only see the h1:

```js
import { Route } from 'react-router-dom';

export default function Welcome() {
  return;
  <section>
    <h1>the welcome page</h1>
    <Route path="/welcome/new-user">
      <p>Welcome, new user!</p>
    </Route>
  </section>;
}
```

EXAMPLE 2: show/hide comments:

```js
return (
  <Fragment>
    <HighlightedQuote text={quote.text} author={quote.author} />

    <Route path={`/quotes/${params.quoteId}`} exact>
      <div className="centered">
        <Link className="btn--flat" to={`/quotes/${params.quoteId}/comments`}>
          show comments
        </Link>
      </div>
    </Route>

    <Route path={`/quotes/${params.quoteId}/comments`}>
      <Comments />
      <div className="centered">
        <Link className="btn--flat" to={`/quotes/${params.quoteId}`}>
          hide comments
        </Link>
      </div>
    </Route>
  </Fragment>
);
```

Alternatively, we can use **useRouteMatch**, it is a 'react-router-dom' hook that returns an object with keys like path (ie, the url in the form '/page/:param'), url, params.

### Redirect

We can redirect the user to another url by using the Redirect component.
For example, in App.js:

```js
<Route path="/" exact>
  <Redirect to="/welcome" />
</Route>
```

(users who try to reach '/' will be redirected to '/welcome')

### Not Found page

It is good practice to add a '_' route to redirect all non-assigned routes to a Not Found page. Just add a Route with the path assigned to '_':

```js
<Route path="*">
  <NotFound />
</Route>
```

### Fetching data from a database

To fetch data from a database, we will be using the useFetch() custom hook that we already made. We just have to import it and, inside the function, deconstruct the variables { data, isPending, error }.
Now we can output the titles and authors of the articles in a list on Home.js:

```js
import { useFetch } from '../hooks/useFetch';
import './Home.css';

export default function Home() {
  const {
    data: articles,
    isPending,
    error,
  } = useFetch('http://localhost:3000/articles');

  return (
    <div className="home">
      <h2>Articles</h2>
      {isPending && <div>Loadind...</div>}
      {error && <div>{error}</div>}
      {articles &&
        articles.map((article) => (
          <div key={article.id} className="card">
            <h3>{article.title}</h3>
            <p>{article.author}</p>
          </div>
        ))}
    </div>
  );
}
```

### Route parameters

We can add parameters to a "to" prop on the Link component element on Home.js. This parameter can be dynamic:

```js
import { useFetch } from '../hooks/useFetch';
import './Home.css';
import { Link } from 'react-route-dom'

export default function Home() {
  const {
    data: articles,
    isPending,
    error,
  } = useFetch('http://localhost:3000/articles');

  return (
    <div className="home">
      <h2>Articles</h2>
      {isPending && <div>Loadind...</div>}
      {error && <div>{error}</div>}
      {articles &&
        articles.map((article) => (
          <div key={article.id} className="card">
            <h3>{article.title}</h3>
            <p>{article.author}</p>
            <Link to={`/articles/${article.id}`}>
          </div>
        ))}
    </div>
  );
}

//We also need to add this Link path's to the Router on App.js. HERE is the place to use :id, where ":" indicates that it is a mutable parameter:
function App() {
  return (
    <div className="App">
      <BrowserRouter>
        <nav>
          <h1>My Articles</h1>
          <NavLink exact to="/">
            Home
          </NavLink>
          <NavLink to="/about">About</NavLink>
          <NavLink to="/contact">Contact</NavLink>
        </nav>

        <Switch>
          <Route exact path="/">
            <Home />
          </Route>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/contact">
            <Contact />
          </Route>
          <Route path="/articles/:id">
            <Article />
          </Route>
        </Switch>
      </BrowserRouter>
    </div>
  );
}
```

### useParams hook

This parameter return an object. This object has one property that is called just like the name we have put after the ":" in the Router.

```js
import { useParams } from 'react-router-dom';
import { useFetch } from '../hooks/useFetch';

export default function Article() {
  const { id } = useParams();
  const url = 'http://localhost:3000/articles/' + id;
  const { data: article, isPending, error } = useFetch(url);

  return (
    <div>
      {isPending && <div>Loading...</div>}
      {error && <div>{error}</div>}
      {article && (
        <div>
          <h2>{article.title}</h2>
          <p>By {article.author}</p>
          <p>{article.body}</p>
        </div>
      )}
    </div>
  );
}
```

### POST request

In order to send POST request, we need to update our useFetch() custom hook, so that it can handle options and add a header with the request body.

```js
//useFetch.js (optimized to GET and POST):
import { useState, useEffect } from 'react';

export const useFetch = (url, method = 'GET') => {
  const [data, setData] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const [error, setError] = useState(null);
  const [options, setOptions] = useState(null);

  const postData = (postData) => {
    setOptions({
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(postData),
    });
  };

  useEffect(() => {
    const controller = new AbortController();

    const fetchData = async (fetchOptions) => {
      setIsPending(true);

      try {
        const res = await fetch(url, {
          ...fetchOptions,
          signal: controller.signal,
        });
        if (!res.ok) {
          throw new Error(res.statusText);
        }
        const data = await res.json();

        setIsPending(false);
        setData(data);
        setError(null);
      } catch (err) {
        if (err.name === 'AbortError') {
          console.log('the fetch was aborted');
        } else {
          setIsPending(false);
          setError('Could not fetch data');
        }
      }
    };

    if (method === 'GET') {
      fetchData();
    }
    if (method === 'POST' && options) {
      fetchData(options);
    }

    return () => {
      controller.abort();
    };
  }, [url, options, method]);

  return { data, isPending, error, postData };
};
```

To use the useFetch custom hook to send a POST request, we have to pass the data we want to POST as arguments to the postData() function:

```js
//Create.js
const { postData, data, error } = useFetch(
  'http://localhost:3000/recipes',
  'POST'
);

const handleSubmit = (e) => {
  e.preventDefault();
  postData({
    title,
    ingredients,
    method,
    cookingTime: cookingTime + ' minutes',
  });
};
```

### Programmatic (imperative) navigation

_Max sc20 lecture 283, repo react-max-20-router_

Sometimes, we want to trigger navigation acts (go backwards, forwards, redirect, etc) when the user does something.

In React 5, we use the **useHistory** hook. It returns an object that contains methods like **push()** (which receives a path and redirects the user to that path, WITHOUT the option to go back via browser) and **replace()** (which receives a path and redirects the user to that path, WITHOUT the option to go back via browser).

### Prompt component: preventing user from leaving the page

The Prompt component must be imported from 'react-router-dom'. It accepts the props:

- when={something} _adds a condition for the prompt to be displayed when user leaves page_
- message=(location => 'messageString') _a message to display on the alert prompt_

### Query parameters

_What is the difference between regular route parameters ('/article/:id') and query parameters?_ Regular route parameters are mandatory, ie, the user will reach that route only if there is something in the place of :id. Query parameters are optional, the user will reach that route if there are no queries.

First we need to take the input value and "redirect" the user to the /search page with that ?q= query parameter.

```js
//SearchBar.js
import { useState } from 'react';
import { useHistory } from 'react-router-dom';
import './SearchBar.css';

export default function SearchBar() {
  const [term, setTerm] = useState('');
  const history = useHistory();

  const handleSubmit = (e) => {
    e.preventDefault();
    history.push(`/search?q=${term}`);
  };

  return (
    <div className="searchbar">
      <form onSubmit={handleSubmit}>
        <label htmlFor="search">Search:</label>
        <input
          type="text"
          id="search"
          onChange={(e) => setTerm(e.target.value)}
          required
        />
      </form>
    </div>
  );
}
```

Second, we need to extract that query string, fetch the data from db using that query string and then display the results:

```js
import { useLocation } from 'react-router-dom';
import RecipeList from '../../components/RecipeList';
import { useFetch } from '../../hooks/useFetch';
import './Search.css';

export default function Search() {
  const queryString = useLocation().search;
  const queryParams = new URLSearchParams(queryString);
  const query = queryParams.get('q');
  console.log(query);

  const url = 'http://localhost:3000/recipes?q=' + query;
  const { error, isPending, data } = useFetch(url);

  return (
    <div>
      <h2 className="page-title">Recipes including "{query}"</h2>
      {error && <p className="error">{error}</p>}
      {isPending && <p className="loading">Loading...</p>}
      {data && <RecipeList recipes={data} />}
    </div>
  );
}
```

Queries EXAMPLE 2: ADDING SORT ITEMS on a list
_Max, lecture 285 repo react-max-20-router_
Here we have a list of quotes, and we want to add a sort by ascending order button.

1. Add the button, import and call useHistory and change the url as the user clicks the button.

```js
import { Fragment } from 'react';
import { useHistory } from 'react-router-dom';

import QuoteItem from './QuoteItem';
import classes from './QuoteList.module.css';

const QuoteList = (props) => {
  const history = useHistory();
  const changeSortingHandler = () => {
    history.push('/quotes?sort=' + 'asc');
  };

  return (
    <Fragment>
      <div className={classes.sorting}>
        <button onClick={changeSortingHandler}>Sort Ascending</button>
      </div>

      <ul className={classes.list}>
        {props.quotes.map((quote) => (
          <QuoteItem
            key={quote.id}
            id={quote.id}
            author={quote.author}
            text={quote.text}
          />
        ))}
      </ul>
    </Fragment>
  );
};

export default QuoteList;
```

2. Import and call **useLocation**, it returns an objects with the 'search' property, the value is the query ?key=value.

`const location = useLocation()`

3. Use the **URLSearchParams** interface to get an object with the key and value of the string in 'location.search'.

`const queryParams = new URLSearchParams(location.search)`

4. Check if the value of the 'sort' key in queryParams is 'asc', by creating a boolean variable. URLSearchParams.get() returns the first value associated with the given search parameter.

`const isSortingAscending = queryParams.get('sort') === 'asc'`

5. Dinamically set the button's text and the history.push path to 'ascending' or 'descending'.

```js
const changeSortingHandler = () => {
  history.push('/quotes?sort=' + (isSortingAscending ? 'desc' : 'asc'));
};
```

```js
<button onClick={changeSortingHandler}>
  Sort {isSortingAscending ? 'Descending' : 'Ascending'}
</button>
```

6. Define a sortQuotes function (can be a helper function outside of the component function).

```js
const sortQuotes = (quotes, ascending) => {
  return quotes.sort((quoteA, quoteB) => {
    if (ascending) {
      return quoteA.id > quoteB.id ? 1 : -1;
    } else {
      return quoteA.id < quoteB.id ? 1 : -1;
    }
  });
};
```

7. Call sortQuotes(props.quotes, isSortingAscending) inside of the component. Don't forget to substitute props.quotes for sortedQuotes inside of the JSX list.

```js
const sortedQuotes = sortQuotes(props.quotes, isSortingAscending);
```

8. Complete code:

```js
//QuoteList.js
import { Fragment } from 'react';
import { useHistory, useLocation } from 'react-router-dom';

import QuoteItem from './QuoteItem';
import classes from './QuoteList.module.css';

const sortQuotes = (quotes, ascending) => {
  return quotes.sort((quoteA, quoteB) => {
    if (ascending) {
      return quoteA.id > quoteB.id ? 1 : -1;
    } else {
      return quoteA.id < quoteB.id ? 1 : -1;
    }
  });
};

const QuoteList = (props) => {
  const history = useHistory();
  const location = useLocation();

  const queryParams = new URLSearchParams(location.search);

  const isSortingAscending = queryParams.get('sort') === 'asc';

  const sortedQuotes = sortQuotes(props.quotes, isSortingAscending);

  const changeSortingHandler = () => {
    history.push('/quotes?sort=' + (isSortingAscending ? 'desc' : 'asc'));
  };

  return (
    <Fragment>
      <div className={classes.sorting}>
        <button onClick={changeSortingHandler}>
          Sort {isSortingAscending ? 'Descending' : 'Ascending'}
        </button>
      </div>

      <ul className={classes.list}>
        {sortedQuotes.map((quote) => (
          <QuoteItem
            key={quote.id}
            id={quote.id}
            author={quote.author}
            text={quote.text}
          />
        ))}
      </ul>
    </Fragment>
  );
};

export default QuoteList;
```

## React Context & Reducers

Still working on react-recipe-directory repo
NN section 12

### Prop drilling

When a prop has to be passed through several levels of a project's tree just to be available to a grand-grand-grand-grandson component, this is called **prop drilling**. It is ok to have that situation in small apps, but in complex apps this can be avoided.

### Context API

The solution to prop drilling is the **Context API**. It creates what is called as **global states**, which are available on all components that are included in the ContextProvider.
That allows us, for example, to create global authentications and dark mode.

### Creating a ContextProvider

This example will show how to create a context for theme (dark/light mode).

First we create a ThemeContext component in a new /context folder:

```js
// /context/ThemeContext.js
import { createContext } from 'react';

export const ThemeContext = createContext();
```

Second, in index.js, we wrap our App component with this Context's Provider:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { ThemeContext } from './context/ThemeContext';

ReactDOM.render(
  <React.StrictMode>
    <ThemeContext.Provider value={{ color: 'blue' }}>
      <App />
    </ThemeContext.Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

Now the 'value' prop will be available in all components.
But usually we don't want that global prop to be hard-coded, instad we want to be able to change it over time.
In order to do that, we will create a new component inside of ThemeContext.js, that will return the ThemeContext.Provider component and its children. Inside of this newly created ThemeProvider component, we can add the logic that will mutate the desired global prop as we want:

```js
// context/ThemeComponent.js
import { createContext } from 'react';

export const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  // custom logic here!

  return (
    <ThemeContext.Provider value={{ color: 'blue' }}>
      {children}
    </ThemeContext.Provider>
  );
}

//index.js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { ThemeProvider } from './context/ThemeContext';

ReactDOM.render(
  <React.StrictMode>
    <ThemeProvider>
      <App />
    </ThemeProvider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

### Context limitations

- React Context is NOT optimized for high frequency changes. For that, we should use Redux.
- React Context also shouldn't be used to replace ALL component communications and props. Components should still be configurable via props and short "prop chains" might not need any replacement.

### useContext hook

In order to access the Context and change the global props, we have to use a hook called **useContext**.
Calling useContext and using ThemeContext as an argument will return an object. The properties defined as props of ThemeContext.Provider will be transmitted as properties of this object.

For example, in NavBar.js:

```js
import { Link } from 'react-router-dom';
import { useContext } from 'react';
import SearchBar from './SearchBar';
import { ThemeContext } from '../context/ThemeContext';

//styles
import './Navbar.css';

export default function Navbar() {
  const context = useContext(ThemeContext);
  console.log(context);

  return (
    <div className="navbar" style={{ background: context.color }}>
      <nav>
        <Link to="/" className="brand">
          <h1>Cooking Ninja</h1>
        </Link>
        <SearchBar />
        <Link to="/create">Create Recipe</Link>
      </nav>
    </div>
  );
}
```

### Creating a useTheme custom hook

This custom hook will easily access the context. Additionally, it will throw an error if anything goes wrong (eg, is the context is being called outside of its scope).

```js
// useTheme.js
import { useContext } from 'react';
import { ThemeContext } from '../context/ThemeContext';

export const useTheme = () => {
  const context = useContext(ThemeContext);

  if (context === undefined) {
    throw new Error('useTheme() must be used inside a ThemeProvider');
  }

  return context;
};

// Navbar.js
import { Link } from 'react-router-dom';
import SearchBar from './SearchBar';
import { useTheme } from '../hooks/useTheme';

//styles
import './Navbar.css';

export default function Navbar() {
  const { color } = useTheme();

  return (
    <div className="navbar" style={{ background: color }}>
      <nav>
        <Link to="/" className="brand">
          <h1>Cooking Ninja</h1>
        </Link>
        <SearchBar />
        <Link to="/create">Create Recipe</Link>
      </nav>
    </div>
  );
}
```

### useReducer hook

Explanations from Max, lecture 116, repo react-max-section10
Examples from NN, repo react-recipe-directory

The function of useReducer is similar to useState, but it adds more complexity on it. For example, it can take care of several properties.

The structure of useReducer is also different: `const [state, dispatch] = useReducer(choicesReducer, { properties: values })`

useReducer structure (Max):
`const [state, dispatchFn] = useReducer(reducerFn, initialState, initFn)`
_state_: the latest state snapshot, used in the component re-render/re-evaluation cycle;
_dispatchFn_: a function that can be used to dispatch a new _action_ (ie, trigger an update of the state). This action will be consumed by the first argument in the useReducer function (_reducerFn_);
_reducerFn_: `(prevState, action) => newState` A function that is triggered automatically once an action is dispatched (via dispatchFn()). It receives the latest state snapshot and should return the new updated state. This function will be called by React and can be defined outside of the component.
_initialState_: the initial state, just as in useState(initialState);
_initFn_: (optional) A function to set the initial state programatically.

The **dispatch** function provides options for the **choicesReducer** function and calls it with these options. `type` and `payload` are properties of the `action` object, that is returned by the dispatch function.

The **choiceReducer** function can be named as we want. It takes the `state` object and the `action` object as parameters and has a switch operator that allows to choose what is returned, accordingly to the action.type that is being provided by dispatch. The switch block will return a new object, with the state object's properties spread, and one property that can be overwritten by the value provided in the `payload` property of the action object.

```js
// context/ThemeContext.js
import { createContext, useReducer } from 'react';

export const ThemeContext = createContext();

const themeReducer = (state, action) => {
  switch (action.type) {
    case 'CHANGE_COLOR':
      return { ...state, color: action.payload };
    default:
      return state;
  }
};

export function ThemeProvider({ children }) {
  const [state, dispatch] = useReducer(themeReducer, {
    color: 'blue',
  });

  const changeColor = (color) => {
    dispatch({ type: 'CHANGE_COLOR', payload: color });
  };

  return (
    <ThemeContext.Provider value={{ ...state, changeColor }}>
      {children}
    </ThemeContext.Provider>
  );
}

// components/Navbar.js
import { Link } from 'react-router-dom';
import SearchBar from './SearchBar';
import { useTheme } from '../hooks/useTheme';

//styles
import './Navbar.css';

export default function Navbar() {
  const { color, changeColor } = useTheme();

  return (
    <div className="navbar" style={{ background: color }}>
      <nav onClick={() => changeColor('pink')}>
        <Link to="/" className="brand">
          <h1>Cooking Ninja</h1>
        </Link>
        <SearchBar />
        <Link to="/create">Create Recipe</Link>
      </nav>
    </div>
  );
}
```

## Forwarding refs with useImperativeHandler and forwardRef

This topic is not so well explained in Max lectures 128, 129, repo react-max-section10.
`Still have to study this point!`

```js
//Login.js
import React, {
  useState,
  useEffect,
  useReducer,
  useContext,
  useRef,
} from 'react';

import Card from '../UI/Card/Card';
import classes from './Login.module.css';
import Button from '../UI/Button/Button';
import AuthContext from '../../store/auth-context';
import Input from '../UI/Input/Input';

const emailReducer = (state, action) => {
  if (action.type === 'USER_INPUT') {
    return { value: action.val, isValid: action.val.includes('@') };
  }
  if (action.type === 'INPUT_BLUR') {
    return { value: state.value, isValid: state.value.includes('@') };
  }
  return { value: '', isValid: false };
};

const passwordReducer = (state, action) => {
  if (action.type === 'USER_INPUT') {
    return { value: action.val, isValid: action.val.trim().length > 6 };
  }
  if (action.type === 'INPUT_BLUR') {
    return { value: state.value, isValid: state.value.trim().length > 6 };
  }
  return { value: '', isValid: false };
};

const Login = (props) => {
  // const [enteredEmail, setEnteredEmail] = useState('');
  // const [emailIsValid, setEmailIsValid] = useState();
  // const [enteredPassword, setEnteredPassword] = useState('');
  // const [passwordIsValid, setPasswordIsValid] = useState();
  const [formIsValid, setFormIsValid] = useState(false);

  const [emailState, dispatchEmail] = useReducer(emailReducer, {
    value: '',
    isValid: null,
  });

  const [passwordState, dispatchPassword] = useReducer(passwordReducer, {
    value: '',
    isValid: null,
  });

  const authCtx = useContext(AuthContext);

  const emailInputRef = useRef();
  const passwordInputRef = useRef();

  useEffect(() => {
    console.log('EFFECT RUNNING');

    return () => {
      console.log('EFFECT CLEANUP');
    };
  }, []);

  const { isValid: emailIsValid } = emailState;
  const { isValid: passwordIsValid } = passwordState;

  useEffect(() => {
    const identifier = setTimeout(() => {
      console.log('Checking form validity');
      setFormIsValid(emailIsValid && passwordIsValid);
    }, 500);

    return () => {
      console.log('CLEANUP');
      clearTimeout(identifier);
    };
  }, [emailIsValid, passwordIsValid]);

  const emailChangeHandler = (event) => {
    dispatchEmail({ type: 'USER_INPUT', val: event.target.value });

    // setFormIsValid(
    //   event.target.value.includes('@') && passwordState.value.trim().length > 6
    // );
  };

  const passwordChangeHandler = (event) => {
    dispatchPassword({ type: 'USER_INPUT', val: event.target.value });

    // setFormIsValid(event.target.value.trim().length > 6 && emailState.isValid);
  };

  const validateEmailHandler = () => {
    dispatchEmail({ type: 'INPUT_BLUR' });
  };

  const validatePasswordHandler = () => {
    dispatchPassword({ type: 'INPUT_BLUR' });
  };

  const submitHandler = (event) => {
    event.preventDefault();
    if (formIsValid) {
      authCtx.onLogin(emailState.value, passwordState.value);
    } else if (!emailIsValid) {
      emailInputRef.current.focus();
    } else {
      passwordInputRef.current.focus();
    }
  };

  return (
    <Card className={classes.login}>
      <form onSubmit={submitHandler}>
        <Input
          ref={emailInputRef}
          id="email"
          label="E-mail"
          type="email"
          isValid={emailIsValid}
          value={emailState.value}
          onChange={emailChangeHandler}
          onBlur={validateEmailHandler}
        />
        <Input
          ref={passwordInputRef}
          id="password"
          label="Password"
          type="password"
          isValid={passwordIsValid}
          value={passwordState.value}
          onChange={passwordChangeHandler}
          onBlur={validatePasswordHandler}
        />
        <div className={classes.actions}>
          <Button type="submit" className={classes.btn}>
            Login
          </Button>
        </div>
      </form>
    </Card>
  );
};

export default Login;


// Input.js
import React, { useRef, useImperativeHandle } from 'react';

import classes from './Input.module.css';

const Input = React.forwardRef((props, ref) => {
  const inputRef = useRef();

  const activate = () => {
    inputRef.current.focus();
  };

  useImperativeHandle(ref, () => {
    return {
      focus: activate,
    };
  });

  return (
    <div
      className={`${classes.control} ${
        props.isValid === false ? classes.invalid : ''
      }`}
    >
      <label htmlFor={props.id}>{props.label}</label>
      <input
        ref={inputRef}
        type={props.type}
        id={props.id}
        value={props.value}
        onChange={props.onChange}
        onBlur={props.onBlur}
      />
    </div>
  );
});

export default Input;
```

## React.memo(), useCallback() & useMemo()

Important reminder: React handles the reevaluation of components. ReactDOM handles the rendering of DOM. Components are reevaluated every time a state changes. But the DOM is not always re-rendered - it only happens when something in the DOM changes, and it happens after a comparison between the previous DOM and the current DOM, so that only the changes will actually count.

So, component reevaluation is not the same as re-render of DOM.

### React.memo()

Every time a component is re-evaluated, all its children components will also be re-evaluated - even if there is no change of props, states or context that affects that child.

For small apps, the is ok. But for big apps, we can prevent this behavior by using **React.memo()**. By doing so, the child components of the wrapped component also won't be re-evaluated. A re-evaluation will only be made if a prop/state/context that these components are using has changed.

```js
import React from 'react';

const DemoOutput = (props) => {
  console.log('DemoOutput running');
  return <p>{props.show ? 'This is new!' : ''}</p>;
};

export default React.memo(DemoOutput);
```

### useCalback() hook

React.memo() works well when the props passed to the wrapped component is a primitive type. But, with functions, objects and arrays (which are reference types), it doesn't work that well. This is because reference types cannot be compared the same way that primitive types do.

`false === false // true`
`[1, 2, 3] === [1, 2, 3] // false`

**useCallback()** is a hook that allows us to 'save' a reference type in React memory, so that the function isn't re-created every time the component is re-evaluated.

Besides the function itself, useCallback also receives a second argument: it is an array of dependencies, equal to useEffect (all the variables that comes from the outside of the function, except for set's). Whenever any of the dependencies' value change, the function will then be recreated, updating this value and working perfectly.

```js
function App() {
  const [showParagraph, setShowParagraph] = useState(false);

  console.log('APP RUNNING');

  const toggleParagraphHandler = useCallback(() => {
    setShowParagraph((prevShowParagraph) => !prevShowParagraph);
  }, []);

  return (
    <div className="app">
      <h1>Hi there!</h1>
      <DemoOutput show={showParagraph} />
      <Button onClick={toggleParagraphHandler}>Toggle Paragraph</Button>
    </div>
  );
}

export default App;
```

### useMemo() hook

This is just like useCallback(), but useMemo is used for storing arrays, objects and arrays and objects that are being modified by methods (like sort, filter, etc). Ie, it is used to store data that won't change over time, so that the data is not re-contructed everytime the app is re-evaluated.

```js
const sortedList = useMemo(() => {
  return [2, 1, 5, 6].sort((a, b) => a - b);
}, []);
```

## Class-based components

repo react-max-section13

Besides building components by using functions, we also can create components by constructing JS Classes. Not too long ago, we HAD to work with classes when building React components, so it's good to learn it because out in the wild we can see this kind of code.

_Functional component:_

```js
function Product(props) {
  return <h2>A Product!</h2>;
}
```

Components are regular JS functions which return renderable results (typically JSX).

_Class-based component:_

```js
class Product extends Component {
  render() {
    return <h2>A Product!</h2>;
  }
}
```

Components can also be defined as JS classes where a **render()** method defines the output to be rendered.

Traditionally (before React v16.8), you had to use Class-based components to manage states and side-effects. React 16.8 introduced React Hooks for Functional Components. Class-based components CAN'T use hooks.

### Simple Class-based component (no states)

```js
import classes from './User.module.css';
import { Component } from 'react';

class User extends Component {
  render() {
    return <li className={classes.user}>{this.props.name}</li>;
  }
}

// const User = (props) => {
//   return <li className={classes.user}>{props.name}</li>;
// };

export default User;
```

### Class-based component with states

```js
import { Component } from 'react';

import User from './User';
import classes from './Users.module.css';

const DUMMY_USERS = [
  { id: 'u1', name: 'Max' },
  { id: 'u2', name: 'Manuel' },
  { id: 'u3', name: 'Julie' },
];

class Users extends Component {
  constructor() {
    super();
    this.state = {
      showUsers: true,
    };
  }

  toggleUsersHandler() {
    this.setState((curState) => {
      return { showUsers: !curState.showUsers };
    });
  }

  render() {
    const usersList = (
      <ul>
        {DUMMY_USERS.map((user) => (
          <User key={user.id} name={user.name} />
        ))}
      </ul>
    );

    return (
      <div className={classes.users}>
        <button onClick={this.toggleUsersHandler.bind(this)}>
          {this.state.showUsers ? 'Hide' : 'Show'} Users
        </button>
        {this.state.showUsers && usersList}
      </div>
    );
  }
}

// const Users = () => {
//   const [showUsers, setShowUsers] = useState(true);

//   const toggleUsersHandler = () => {
//     setShowUsers((curState) => !curState);
//   };

//   const usersList = (
//     <ul>
//       {DUMMY_USERS.map((user) => (
//         <User key={user.id} name={user.name} />
//       ))}
//     </ul>
//   );

//   return (
//     <div className={classes.users}>
//       <button onClick={toggleUsersHandler}>
//         {showUsers ? 'Hide' : 'Show'} Users
//       </button>
//       {showUsers && usersList}
//     </div>
//   );
// };

export default Users;
```

### Lifecycle methods

These are built-in methods in Component class, just as render(). They allow us to handle side-effects in CB-components.

`componentDidMount()` is called once component mounted (was evaluated and rendered). Equivalent to useEffect(..., []) hook, with an empty dependency array.<br>
`componentDidUpdate()` is called once component is updated (was re-evaluated and re-rendered). Equivalent to useEffect(..., [ someValue ]), with some variable in the dependency array.<br>
`componentWillUnmount()` is called right before component is unmounted (removed from the DOM). Equivalent to the clean-up function in useEffect.

### Class-based components & Context

It is also possible to define a context object using CB-components. See Max lecture 168.

### Error boundaries

Up to now, there is still no way of defining Error Boundaries using Functional Components. We still have to build a CB-component to easily handle errors throughout our application.

For that, we create a CB-component that has a componentDidCatch(error) method - this will be our ErrorBoundary component. Then, we wrap our prone-to-throw-error component inside `<ErrorBoundary></ErrorBoundary>`. It will work as a try/catch on Vanilla JS.

```js
// Users.js (intentionally created an error when no user is found)
componentDidUpdate() {
    if (this.props.users.length === 0) {
      throw new Error('No users provided!');
    }
  }

// ErrorBoundary.js
import { Component, Fragment } from 'react';

class ErrorBoundary extends Component {
  constructor() {
    super();
    this.state = { hasError: false };
    this.errorMsg = '';
  }

  componentDidCatch(error) {
    console.log(error.message);
    this.setState({ hasError: true });
    this.errorMsg = error.message;
  }

  render() {
    if (this.state.hasError) {
      return (
        <>
          <p>Something went wrong!</p>
          <p>{this.errorMsg}</p>
        </>
      );
    }
    return this.props.children;
  }
}

export default ErrorBoundary;

// UserFinder.js (this is the component where Users.js is called)
render() {
    return (
      <Fragment>
        <div className={classes.finder}>
          <input type="search" onChange={this.searchChangeHandler.bind(this)} />
        </div>
        <ErrorBoundary>
          <Users users={this.state.filteredUsers} />
        </ErrorBoundary>
      </Fragment>
    );
  }
```

## Fetching data from API

Max, section 14; repo react-max-section14

### Sending a GET request

Max, section 14, lecture 176-179. Send a GET request for Star Wars API, adds state for controlling the output of a loading-message, adds state for handling/showing errors:

```js
import React, { useState } from 'react';

import MoviesList from './components/MoviesList';
import './App.css';

function App() {
  const [movies, setMovies] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  async function fetchMoviesHandler() {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch('https://swapi.dev/api/films/');

      if (!response.ok) {
        throw new Error('Something went wrong!');
      }
      const data = await response.json();

      const transformedMovies = data.results.map((movieData) => {
        return {
          id: movieData.episode_id,
          title: movieData.title,
          openingText: movieData.opening_crawl,
          releaseDate: movieData.release_date,
        };
      });
      setMovies(transformedMovies);
    } catch (error) {
      setError(error.message);
    }
    setIsLoading(false);
  }

  let content = <p>Found no movies</p>;

  if (movies.length > 0) {
    content = <MoviesList movies={movies} />;
  }

  if (error) {
    content = <p>{error}</p>;
  }

  if (isLoading) {
    content = <p>Loading...</p>;
  }

  return (
    <React.Fragment>
      <section>
        <button onClick={fetchMoviesHandler}>Fetch Movies</button>
      </section>
      <section>{content}</section>
    </React.Fragment>
  );
}

export default App;
```

### Sending a POST request

First, we've set up a Firebase database (called 'react-http'), which also comes with a ready-made backend. Now it's ready to receive get and post requests.

This is the structure of a POST request:

```js
async function addMovieHandler(movie) {
  const response = await fetch(
    'https://react-http-3ff60-default-rtdb.firebaseio.com/movies.json',
    {
      method: 'POST',
      body: JSON.stringify(movie),
      headers: {
        'Content-Type': 'application/json',
      },
    }
  );
  const data = await response.json();
  console.log(data);
}
```

The entire component (see repo react-max-section14):

```js
import React, { useState, useEffect, useCallback } from 'react';

import MoviesList from './components/MoviesList';
import AddMovie from './components/AddMovie';
import './App.css';

function App() {
  const [movies, setMovies] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  const fetchMoviesHandler = useCallback(async () => {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch(
        'https://react-http-3ff60-default-rtdb.firebaseio.com/movies.json'
      );
      if (!response.ok) {
        throw new Error('Something went wrong!');
      }

      const data = await response.json();

      const loadedMovies = [];

      for (const key in data) {
        loadedMovies.push({
          id: key,
          title: data[key].title,
          openingText: data[key].openingText,
          releaseDate: data[key].releaseDate,
        });
      }

      setMovies(loadedMovies);
    } catch (error) {
      setError(error.message);
    }
    setIsLoading(false);
  }, []);

  useEffect(() => {
    fetchMoviesHandler();
  }, [fetchMoviesHandler]);

  async function addMovieHandler(movie) {
    const response = await fetch(
      'https://react-http-3ff60-default-rtdb.firebaseio.com/movies.json',
      {
        method: 'POST',
        body: JSON.stringify(movie),
        headers: {
          'Content-Type': 'application/json',
        },
      }
    );
    const data = await response.json();
    console.log(data);
  }

  let content = <p>Found no movies.</p>;

  if (movies.length > 0) {
    content = <MoviesList movies={movies} />;
  }

  if (error) {
    content = <p>{error}</p>;
  }

  if (isLoading) {
    content = <p>Loading...</p>;
  }

  return (
    <React.Fragment>
      <section>
        <AddMovie onAddMovie={addMovieHandler} />
      </section>
      <section>
        <button onClick={fetchMoviesHandler}>Fetch Movies</button>
      </section>
      <section>{content}</section>
    </React.Fragment>
  );
}

export default App;
```

## Custom hooks

Custom hooks are reusable functions that can contain stateful logic. Ie, unlike other "regular" functions, custom hooks can use other React hooks and React state.
repo react-max-section15

We can return values from a custom hook, just as in any other function:

```js
// hooks/use-counter.js
import { useState, useEffect } from 'react';

const useCounter = () => {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCounter((prevCounter) => prevCounter + 1);
    }, 1000);

    return () => clearInterval(interval);
  }, []);

  return counter;
};

export default useCounter;

// components/ForwardCounter.js
import Card from './Card';
import useCounter from '../hooks/use-counter';

const ForwardCounter = () => {
  const counter = useCounter();

  return <Card>{counter}</Card>;
};

export default ForwardCounter;
```

### Adding parameters to custom hooks

We can change the behavior of custom hooks, according to parameters. For example, we can make the useCounter hook count backwards if it is called with an argument of 'false':

```js
// hooks/use-counter.js
import { useState, useEffect } from 'react';

const useCounter = (forwards = true) => {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      if (forwards) {
        setCounter((prevCounter) => prevCounter + 1);
      } else {
        setCounter((prevCounter) => prevCounter - 1);
      }
    }, 1000);

    return () => clearInterval(interval);
  }, [forwards]);

  return counter;
};

export default useCounter;

// components/BackwardCounter.js
import { useState, useEffect } from 'react';

import Card from './Card';
import useCounter from '../hooks/use-counter';

const BackwardCounter = () => {
  const counter = useCounter(false);

  return <Card>{counter}</Card>;
};

export default BackwardCounter;
```

### Form validation custom hook

max section 16, repo react-max-section16

When validating a form, validation can take 3 steps:

1. validate whenever the user inputs something, letter by letter (onChange);
2. validate whenever the user removes the focus from the input (onBlur);
3. validate when the user submits the form (onSubmit);

The 3 steps are equally important.
We've built this **useInput** custom hook to manage all of them. The hook returns/manages 3 states and 3 functions, so that we don't need to handle states in our component to validate forms. Here is how it works:

```js
// hooks/use-input.js
import { useState } from 'react';

const useInput = (validateValue) => {
  const [enteredValue, setEnteredValue] = useState('');
  const [isTouched, setIsTouched] = useState(false);

  const valueIsValid = validateValue(enteredValue);
  const hasError = !valueIsValid && isTouched;

  const valueChangeHandler = (event) => {
    setEnteredValue(event.target.value);
  };

  const inputBlurHandler = () => {
    setIsTouched(true);
  };

  const reset = () => {
    setEnteredValue('');
    setIsTouched(false);
  };

  return {
    value: enteredValue,
    isValid: valueIsValid,
    hasError,
    valueChangeHandler,
    inputBlurHandler,
    reset,
  };
};

export default useInput;

// components/BasicForm.js
import useInput from '../hooks/use-input';

const BasicForm = (props) => {
  const {
    value: enteredFirstName,
    isValid: firstNameIsValid,
    hasError: firstNameHasError,
    valueChangeHandler: firstNameChangeHandler,
    inputBlurHandler: firstNameBlurHandler,
    reset: firstNameReset,
  } = useInput((value) => value.trim().length > 0);

  const {
    value: enteredLastName,
    isValid: lastNameIsValid,
    hasError: lastNameHasError,
    valueChangeHandler: lastNameChangeHandler,
    inputBlurHandler: lastNameBlurHandler,
    reset: lastNameReset,
  } = useInput((value) => value.trim().length > 0);

  const {
    value: enteredEmail,
    isValid: emailIsValid,
    hasError: emailHasError,
    valueChangeHandler: emailChangeHandler,
    inputBlurHandler: emailBlurHandler,
    reset: emailReset,
  } = useInput((value) => value.includes('@'));

  let formIsValid = false;
  if (firstNameIsValid && lastNameIsValid && emailIsValid) {
    formIsValid = true;
  }

  const formSubmissionHandler = (e) => {
    e.preventDefault();

    //extra-validation (the user could enable the button via Devtools)
    if (!formIsValid) {
      return;
    }

    console.log(enteredFirstName, enteredLastName, enteredEmail);
    firstNameReset();
    lastNameReset();
    emailReset();
  };

  const firstNameClass = firstNameHasError
    ? 'form-control invalid'
    : 'form-control';

  const lastNameClass = lastNameHasError
    ? 'form-control invalid'
    : 'form-control';

  const emailClass = emailHasError ? 'form-control invalid' : 'form-control';

  return (
    <form onSubmit={formSubmissionHandler}>
      <div className="control-group">
        <div className={firstNameClass}>
          <label htmlFor="firstname">First Name</label>
          <input
            type="text"
            id="firstname"
            value={enteredFirstName}
            onChange={firstNameChangeHandler}
            onBlur={firstNameBlurHandler}
          />
          {firstNameHasError && (
            <p className="error-text">Please enter a valid first name.</p>
          )}
        </div>
        <div className={lastNameClass}>
          <label htmlFor="lastname">Last Name</label>
          <input
            type="text"
            id="lastname"
            onChange={lastNameChangeHandler}
            onBlur={lastNameBlurHandler}
            value={enteredLastName}
          />
          {lastNameHasError && (
            <p className="error-text">Please enter a valid last name.</p>
          )}
        </div>
      </div>
      <div className={emailClass}>
        <label htmlFor="email">E-Mail Address</label>
        <input
          type="text"
          id="email"
          onChange={emailChangeHandler}
          onBlur={emailBlurHandler}
          value={enteredEmail}
        />
        {emailHasError && (
          <p className="error-text">Please enter a valid e-mail.</p>
        )}
      </div>
      <div className="form-actions">
        <button disabled={!formIsValid}>Submit</button>
      </div>
    </form>
  );
};

export default BasicForm;
```

Other possibility when working with forms and React is to make use of libraries like **Formik** and **React Hook Form**.

## Firebase Firestore (database)

Firebase is a Backend-as-a-Service (Baas). It provides developers with a variety of tools and services to help them develop their apps.

Firebase Firestore is categorized as a NoSQL database program, which stores data in JSON-like documents.

Some of its key-features are: authentication, realtime database, cloud hosting, test lab and notifications.

More Firestore (timestamp, custom hook) [here](#timestamp)

### Firestore Databases

NN, section 13, repo react-recipe-directory

Firestore is the newest database service provided by Firebase. The old one is called Realtime Database and still is available.

### Connecting to Firebase

1. `npm install firebase@8.5`
2. Get the configuration object, that includes a project id, etc. In order to do that, we need to register a front-end web-app for this project. We can have different apps connected to a same database - for example, web, Android and IOS versions for the same application.
3. Create a `config.js` file in your src folder.

```js
// src/firebase/config.js
import firebase from 'firebase/app';
import 'firebase/firestore';

const firebaseConfig = {
  apiKey: 'AIzaSyBQzAksGcSAbgR9i9a0WNJFdSi33EJKBLQ',
  authDomain: 'react-recipe-directory-d6466.firebaseapp.com',
  projectId: 'react-recipe-directory-d6466',
  storageBucket: 'react-recipe-directory-d6466.appspot.com',
  messagingSenderId: '118388807863',
  appId: '1:118388807863:web:af0ed816296554ad649532',
};

//init firebase
firebase.initializeApp(firebaseConfig);

//init services
const projectFirestore = firebase.firestore();

export { projectFirestore };
```

4. And that's it! Now the front and back-end of our project are connected through the exported object.

### Fetching a Firestore collection

Now we can use **projectFirestore** to access and fetch the data from our database. In that, we will call the method `.collection('collectionName')`, then the method `.get()`, which returns a promise that can be handled using `.then()`.
By calling the `data()` method that's inside of each object in the `docs` object in the snapshot, we get access to an object with the recipes properties.

`(Tip: console.log(snapshot) to understand its structure and the properties and functions it provides!)`

```js
//Home.js
import { projectFirestore } from '../../firebase/config';
import { useEffect, useState } from 'react';
import RecipeList from '../../components/RecipeList';
import './Home.css';

export default function Home() {
  const [data, setData] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const [error, setError] = useState(false);

  useEffect(() => {
    setIsPending(true);

    projectFirestore
      .collection('recipes')
      .get()
      .then((snapshot) => {
        // console.log(snapshot);
        if (snapshot.empty) {
          setError('No recipes to load');
          setIsPending(false);
        } else {
          let results = [];
          snapshot.docs.forEach((doc) => {
            results.push({ id: doc.id, ...doc.data() });
          });
          setData(results);
          setIsPending(false);
        }
      })
      .catch((err) => {
        setError(err.message);
        setIsPending(false);
      });
  }, []);

  return (
    <div className="home">
      {error && <p className="error">{error}</p>}
      {isPending && <p className="loading">Loading...</p>}
      {data && <RecipeList recipes={data} />}
    </div>
  );
}
```

### Fetching a Firestore document

On our Home.js page, each recipe has a link to recipe page with details on that recipe. This link has the route **/recipes/:id**.

Now, on Recipe.js, we're goint to extract that **id** from the address using useParams(): `const { id } = useParams();`.

Then we can use this id to fetch data from that specific **document** inside the 'recipes' collection in our Firestore database.

`Tip: console.log(doc) to understand the structure of this object and the properties and functions it provides!`

```js
import { projectFirestore } from '../../firebase/config';
import { useEffect, useState } from 'react';
import { useParams } from 'react-router-dom';
import { useTheme } from '../../hooks/useTheme';

//styles
import './Recipe.css';

export default function Recipe() {
  const { id } = useParams();
  const { mode } = useTheme();

  const [recipe, setRecipe] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const [error, setError] = useState(false);

  useEffect(() => {
    setIsPending(true);

    projectFirestore
      .collection('recipes')
      .doc(id)
      .get()
      .then((doc) => {
        // console.log(doc);
        if (doc.exists) {
          setIsPending(false);
          setRecipe(doc.data());
        } else {
          setIsPending(false);
          setError('Could not find this recipe');
        }
      });
  }, [id]);

  return (
    <div className={`recipe ${mode}`}>
      {error && <p className="error">{error}</p>}
      {isPending && <p className="loading">Loading...</p>}
      {recipe && (
        <>
          <h2 className="page-title">{recipe.title}</h2>
          <p>Takes {recipe.cookingTime} to cook.</p>
          <ul>
            {recipe.ingredients.map((ing) => (
              <li key={ing}>{ing}</li>
            ))}
          </ul>
          <p className="method">{recipe.method}</p>
        </>
      )}
    </div>
  );
}
```

### Making POST requests to Firetore

In order to make a POST request to Firestore, we just need to call `.collection('collectionName')` and then `.add(docToBePosted)`. It is good to wrap this in a try/catch block.

```js
//Create.js
import { projectFirestore } from '../../firebase/config';

etc etc

const handleSubmit = async (e) => {
    e.preventDefault();
    const doc = {
      title,
      ingredients,
      method,
      cookingTime: cookingTime + ' minutes',
    };

    try {
      await projectFirestore.collection('recipes').add(doc);
      history.push('/');
    } catch (err) {
      console.log(err);
    }
  };

  etc etc
```

### Deleting a doc from a Firetore collection

For that, we select the doc by calling `projectFirestore.collection('recipes').doc(id)` and delete it by calling `.delete()`.

```js
import { projectFirestore } from '../../firebase/config';

etc etc

const handleClick = (id) => {
  projectFirestore.collection('recipes').doc(id).delete();
};

etc etc
```

### Real-time collection data

After deleting the doc from the database, the page keeps showing it - that's because it has not been re-rendered and didn't fetch the data again.

In order to fix that, in Home.js (that's where the GET request for the collection list of docs is being made), we substitute `.get().then()` for `.onSnapshot()`. The onSnapshot method takes 2 arguments: the first is a function to handle the snapshot (eg, extracting all docs to create an array of objects); the second is a function to handle errors.

When using the onSnapshot method, we must add a **clean-up function** to useEffect. Otherwise, once we leave the page, the app will keep receiving new snapshots triggered by that method. The clean-up function will be executed as soon as the component unmounts.

Regularly, the onSnapshot method returns a unsubscribe function. We save it to a variable called `unsub` and call `unsub()` in our clean-up function.

```js
//Home.js
import { projectFirestore } from '../../firebase/config';

etc etc

useEffect(() => {
    setIsPending(true);

    const unsub = projectFirestore.collection('recipes').onSnapshot(
      (snapshot) => {
        if (snapshot.empty) {
          setError('No recipes to load');
          setIsPending(false);
        } else {
          let results = [];
          snapshot.docs.forEach((doc) => {
            results.push({ id: doc.id, ...doc.data() });
          });
          setData(results);
          setIsPending(false);
        }
      },
      (err) => {
        setError(err.message);
        setIsPending(false);
      }
    );

    return () => unsub();
  }, []);

  etc etc
```

### Updating documents in Firestore

To update a document, we refer to that particular document and update it by calling `projectFirestore.collection('recipes').doc(id).update({ oldProperty: newValue })`.

```js
// Recipe.js
const handleUpdate = () => {
  projectFirestore
    .collection('recipes')
    .doc(id)
    .update({
      title: `${recipe.title} UPDATED`,
    });
};
```

### Real-time document data

Same as when we deleted an item from a list, now we updated a particular document, but we have to refresh in order to fetch it again from the database, because the component wasn't re-rendered.

Same as [before](#real-time-collection-data), we're going to use the `onSnapshot()` method.

Same as before also, we have to add a clean-up function to unsubscribe to onSnapshot, otherwise the app would keep listening to changes on the collection even if we leave the page.

```js
//Recipe.js
useEffect(() => {
  setIsPending(true);

  const unsub = projectFirestore
    .collection('recipes')
    .doc(id)
    .onSnapshot((doc) => {
      // console.log(doc);
      if (doc.exists) {
        setIsPending(false);
        setRecipe(doc.data());
      } else {
        setIsPending(false);
        setError('Could not find this recipe');
      }
    });

  return () => unsub();
}, [id]);
```

## Firebase Authentication

### Connecting Firebase Auth

After installing `npm install firebase@8.5` and creating a config.js file (see [here](#connecting-to-firebase)), we have to enable e-mail authentication on Firebase website.

We have also to add Firebase Auth to our config.js file, like this:

```js
import firebase from 'firebase/app';
import 'firebase/firestore';
import 'firebase/auth';

const firebaseConfig = {
  apiKey: 'AIzaSyD5NU6********FA5RxWkaM_PSbp6Ug',
  authDomain: 'react-finance-tracker-64f25.firebaseapp.com',
  projectId: 'react-finance-tracker-64f25',
  storageBucket: 'react-finance-tracker-64f25.appspot.com',
  messagingSenderId: '1098***72131',
  appId: '1:10********131:web:ab66984c8090b3bd7353b6',
};

//init firebase
firebase.initializeApp(firebaseConfig);

//init services
const projectFirestore = firebase.firestore();
const projectAuth = firebase.auth();

export { projectFirestore, projectAuth };
```

### Creating a signup custom hook

We can create a custom hook to create a new user with the e-mail/password provided in the signup form. We should use the object **projectAuth** in order to do that.

```js
import { useState } from 'react';
import { projectAuth } from '../firebase/config';

export const useSignup = () => {
  const [error, setError] = useState(null);
  const [isPending, setIsPending] = useState(false);

  const signup = async (email, password, displayName) => {
    setError(null);
    setIsPending(true);

    try {
      //signup user
      const res = await projectAuth.createUserWithEmailAndPassword(
        email,
        password
      );
      console.log(res.user);

      if (!res) {
        throw new Error('Could not complete signup');
      }

      //add displayName to user
      await res.user.updateProfile({ displayName: displayName });
      setIsPending(false);
      setError(null);
    } catch (err) {
      console.log(err.message);
      setError(err.message);
      setIsPending(false);
    }
  };

  return { error, isPending, signup };
};
```

Using the hook in the Signup.js component:

```js
//Signup.js
import { useSignup } from '../../hooks/useSignup';

etc etc

const { signup, isPending, error } = useSignup();

  const handleSubmit = (e) => {
    e.preventDefault();
    signup(email, password, displayName);
  };

  etc etc
```

Complete code:

```js
//Signup.js
import { useState } from 'react';
import { useSignup } from '../../hooks/useSignup';
import styles from './Signup.module.css';

export default function Signup() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [displayName, setDisplayName] = useState('');
  const { signup, isPending, error } = useSignup();

  const handleSubmit = (e) => {
    e.preventDefault();
    signup(email, password, displayName);
  };

  return (
    <form onSubmit={handleSubmit} className={styles['signup-form']}>
      <h2>Signup</h2>
      <label>
        <span>email:</span>
        <input
          type="email"
          onChange={(e) => setEmail(e.target.value)}
          value={email}
        />
      </label>
      <label>
        <span>password:</span>
        <input
          type="password"
          onChange={(e) => setPassword(e.target.value)}
          value={password}
        />
      </label>
      <label>
        <span>display name:</span>
        <input
          type="text"
          onChange={(e) => setDisplayName(e.target.value)}
          value={displayName}
        />
      </label>
      {!isPending && <button className="btn">Signup</button>}
      {isPending && (
        <button className="btn" disabled>
          loading
        </button>
      )}
      {error && <p>{error}</p>}
    </form>
  );
}
```

### Creating an Auth Context

In order to share the situation of authentication throughout all of the pages on the app, we need to create a Context and wrap the App component in the ContextProvider, so that login info is available in all the components in our app.

1. Creating the Context and ContextProvider:

```js
// context/AuthContext.js
import { createContext, useReducer } from 'react';

export const AuthContext = createContext();

export const authReducer = (state, action) => {
  switch (action.type) {
    default:
      return state;
  }
};

export const AuthContextProvider = ({ children }) => {
  const [state, dispatch] = useReducer(authReducer, {
    user: null,
  });

  return (
    <AuthContext.Provider value={{ ...state, dispatch }}>
      {children}
    </AuthContext.Provider>
  );
};
```

2. Wrapping the entire App inside the ContextProvider:

```js
// index.js
import { createContext, useReducer } from 'react';

export const AuthContext = createContext();

export const authReducer = (state, action) => {
  switch (action.type) {
    case 'LOGIN':
      return { ...state, user: action.payload };
    //we are exporting the dispatch function (as a value in the AuthContext.Provider component in index.js), so that we can dispatch actions from other components and custom hooks.
    default:
      return state;
  }
};

export const AuthContextProvider = ({ children }) => {
  const [state, dispatch] = useReducer(authReducer, {
    user: null,
  });

  return (
    <AuthContext.Provider value={{ ...state, dispatch }}>
      //we are exporting the dispatch function(as a value in the
      AuthContext.Provider component in index.js), so that we can dispatch
      actions from other components and custom hooks.
      {children}
    </AuthContext.Provider>
  );
};
```

3. Creating a useAuthContext to consume the context:

```js
// hooks/useAuthContext.js
import { AuthContext } from '../context/AuthContext';
import { useContext } from 'react';

export const useAuthContext = () => {
  const context = useContext(AuthContext);

  if (!context) {
    throw Error('useAuthContext must be used inside an AuthContextProvider');
  }

  return context;
};
```

### Dispatching a Login action

We are exporting the dispatch function (as a value in the AuthContext.Provider component in index.js), so that we can dispatch actions from other components and custom hooks.

So, we're going to dispatch a type LOGIN action with the payload equal to the new user object, from the useSignup custom hook:

```js
// hooks/useSignup.js
import { useState, useEffect } from 'react';
import { projectAuth } from '../firebase/config';
import { useAuthContext } from './useAuthContext'; //importing useAuthContext, which exports the 'context' object, which contains the state of the context AND the dispatch fnc (see index.js)

export const useSignup = () => {
  const [isCancelled, setIsCancelled] = useState(false);
  const [error, setError] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const { dispatch } = useAuthContext(); //deconstructing dispatch fnc

  const signup = async (email, password, displayName) => {
    setError(null);
    setIsPending(true);

    try {
      //signup user
      const res = await projectAuth.createUserWithEmailAndPassword(
        email,
        password
      );

      if (!res) {
        throw new Error('Could not complete signup');
      }

      //add displayName to user
      await res.user.updateProfile({ displayName: displayName });

      //dispatch login action ******
      dispatch({ type: 'LOGIN', payload: res.user });

      //update states (will not run if the component unmounts)
      if (!isCancelled) {
        setIsPending(false);
        setError(null);
      }
    } catch (err) {
      if (!isCancelled) {
        console.log(err.message);
        setError(err.message);
        setIsPending(false);
      }
    }
  };

  useEffect(() => {
    return setIsCancelled(true);
  }, []);

  return { error, isPending, signup };
};
```

### Creating and using a useLogout custom hook

1. Creating useLogout:

```js
import { useState, useEffect } from 'react';
import { projectAuth } from '../firebase/config';
import { useAuthContext } from './useAuthContext';

export const useLogout = () => {
  const [isCancelled, setIsCancelled] = useState(false);
  const [error, setError] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const { dispatch } = useAuthContext();

  const logout = async () => {
    setError(null);
    setIsPending(true);

    //sign the user out
    try {
      await projectAuth.signOut();

      //dispatch logout action
      dispatch({ type: 'LOGOUT' });

      //update states
      if (!isCancelled) {
        setIsPending(false);
        setError(null);
      }
    } catch (err) {
      console.log(err.message);
      if (!isCancelled) {
        setError(err.message);
        setIsPending(false);
      }
    }
  };

  useEffect(() => {
    return setIsCancelled(true);
  }, []);

  return { logout, error, isPending };
};
```

2. Using useLogout in the Navbar component:

```js
import { Link } from 'react-router-dom';
import { useLogout } from '../hooks/useLogout'; //importing
import styles from './Navbar.module.css';

export default function Navbar() {
  const { logout } = useLogout(); //extracting the logout fnc

  return (
    <nav className={styles.navbar}>
      <ul>
        <li className={styles.title}>
          <Link to="/">myMoney</Link>
        </li>
        <li>
          <Link to="/login">Login</Link>
        </li>
        <li>
          <Link to="/signup">Signup</Link>
        </li>
        <button className="btn" onClick={logout}>
          Logout
        </button>//using the logout fnc
      </ul>
    </nav>
  );
}
```

### IMPORTANT! Adding cleanup functions to our hooks

Whenever a hook or other function is able to make a fetch request to a backend, it's important to make a clean-up function to cancel the request if the component is unmonted. Otherwise, if the component is unmounted before the promise is fulfilled, there will be an error.
_This update has already been made in useSignup and useLogout (above), as well as in the repo._

### Creating and using a useLogin hook

This hook is pretty much the same as useLogout and useSignup:

```js
// hooks/useLogin.js
import { useState, useEffect } from 'react';
import { projectAuth } from '../firebase/config';
import { useAuthContext } from './useAuthContext';

export const useLogin = () => {
  const [isCancelled, setIsCancelled] = useState(false);
  const [error, setError] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const { dispatch } = useAuthContext();

  const login = async (email, password) => {
    setError(null);
    setIsPending(true);

    //sign the user in
    try {
      const res = await projectAuth.signInWithEmailAndPassword(email, password);

      //dispatch login action
      dispatch({ type: 'LOGIN', payload: res.user });

      //update states (will not run if the component is unmounted)
      if (!isCancelled) {
        setIsPending(false);
        setError(null);
      }
    } catch (err) {
      if (!isCancelled) {
        console.log(err.message);
        setError(err.message);
        setIsPending(false);
      }
    }
  };

  useEffect(() => {
    return setIsCancelled(true);
  }, []);

  return { login, error, isPending };
};

// pages/login/Login.js
import { useState } from 'react';
import { useLogin } from '../../hooks/useLogin';
import styles from './Login.module.css';

export default function Login() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const { login, error, isPending } = useLogin();

  const handleSubmit = (e) => {
    e.preventDefault();
    login(email, password);
  };

  return (
    <form onSubmit={handleSubmit} className={styles['login-form']}>
      <h2>Login</h2>
      <label>
        <span>email:</span>
        <input
          type="email"
          onChange={(e) => setEmail(e.target.value)}
          value={email}
        />
      </label>
      <label>
        <span>password:</span>
        <input
          type="password"
          onChange={(e) => setPassword(e.target.value)}
          value={password}
        />
      </label>
      {!isPending && <button className="btn">Login</button>}
      {isPending && (
        <button className="btn" disabled>
          loading
        </button>
      )}
      {error && <p>{error}</p>}
    </form>
  );
}
```

### Conditionally showing user content

In our auth context object, we have access to the user property, whose value is an object with displayName, email and other info. Then we can conditionally show content depending if the user is/isn't logged in.

1. import useAuthContext (or use useContext directly);
2. deconstruct the user property from the context object.
3. conditionally show content if the the user property is truthy.

```js
import { Link } from 'react-router-dom';
import { useLogout } from '../hooks/useLogout';
import { useAuthContext } from '../hooks/useAuthContext';
import styles from './Navbar.module.css';

export default function Navbar() {
  const { logout } = useLogout();
  const { user } = useAuthContext();

  return (
    <nav className={styles.navbar}>
      <ul>
        <li className={styles.title}>
          <Link to="/">myMoney</Link>
        </li>

        {!user && (
          <>
            <li>
              <Link to="/login">Login</Link>
            </li>
            <li>
              <Link to="/signup">Signup</Link>
            </li>
          </>
        )}

        {user && (
          <>
            <li>hello, {user.displayName}</li>
            <li>
              <button className="btn" onClick={logout}>
                Logout
              </button>
            </li>
          </>
        )}
      </ul>
    </nav>
  );
}
```

### Checking if the user is logged-in with onAuthStateChanged()

Up to now, if the user reloads the page, our front-end will state that he is logged out - even if he is currently logged in according to Firebase Auth.
To prevent this kind of disarrangement, we can call **projectAuth.onAuthStateChanged()** whenever the page is first loaded. For that, we can use useEffect. If there is a logged user, we will dispatch a type 'AUTH_IS_READY' action, which will set both the user and the **authIsReady** property in our auth-context.
We need also to **unsubcribe** to this method, otherwise this function will run everytime the logged user changes.

```js
import { createContext, useReducer } from 'react';
import { useEffect } from 'react';
import { projectAuth } from '../firebase/config';

export const AuthContext = createContext();

export const authReducer = (state, action) => {
  switch (action.type) {
    case 'LOGIN':
      return { ...state, user: action.payload };
    case 'LOGOUT':
      return { ...state, user: null };
    case 'AUTH_IS_READY':
      return { ...state, user: action.payload, authIsReady: true };
    default:
      return state;
  }
};

export const AuthContextProvider = ({ children }) => {
  const [state, dispatch] = useReducer(authReducer, {
    user: null,
    authIsReady: false,
  });

  useEffect(() => {
    const unsub = projectAuth.onAuthStateChanged((user) => {
      dispatch({ type: 'AUTH_IS_READY', payload: user });
      unsub();
    });
  }, []);

  console.log('AuthContext state:', state);

  return (
    <AuthContext.Provider value={{ ...state, dispatch }}>
      {children}
    </AuthContext.Provider>
  );
};
```

Now, in our App.js, we can conditionally show the entire app when the authIsReady property of the context is truthy:

```js
etc etc

function App() {
  const { authIsReady } = useAuthContext();

  return (
    <div className="App">
      {authIsReady && (
        <BrowserRouter>
          <Navbar />
          <Switch>
            <Route exact path="/">
              <Home />
            </Route>
            <Route path="/login">
              <Login />
            </Route>
            <Route path="/signup">
              <Signup />
            </Route>
          </Switch>
        </BrowserRouter>
      )}
    </div>
  );
}

etc etc
```

### Route Guarding with Redirect

If the user is not logged in, we have to protect the routes that he cannot access. We use the Redirect component, that is a built-in component inside of 'react'.

```js
import { BrowserRouter, Route, Switch, Redirect } from 'react-router-dom';
import { useAuthContext } from './hooks/useAuthContext';
import Home from './pages/home/Home';
import Login from './pages/login/Login';
import Signup from './pages/signup/Signup';
import Navbar from './components/Navbar';

function App() {
  const { authIsReady, user } = useAuthContext();

  return (
    <div className="App">
      {authIsReady && (
        <BrowserRouter>
          <Navbar />
          <Switch>
            <Route exact path="/">
              {!user && <Redirect to="/login" />}
              {user && <Home />}
            </Route>
            <Route path="/login">
              {user && <Redirect to="/" />}
              {!user && <Login />}
            </Route>
            <Route path="/signup">
              {user && <Redirect to="/" />}
              {!user && <Signup />}
            </Route>
          </Switch>
        </BrowserRouter>
      )}
    </div>
  );
}

export default App;
```

In React 6^, there is no Redirect component, instead we can use the Navigate component, something like this:

```js
import { BrowserRouter, Route, Switch, Navigate } from 'react-router-dom';
import { useAuthContext } from './hooks/useAuthContext';
import Home from './pages/home/Home';
import Login from './pages/login/Login';
import Signup from './pages/signup/Signup';
import Navbar from './components/Navbar';

function App() {
  const { authIsReady, user } = useAuthContext();

  return (
    <div className="App">
      {authIsReady && (
        <BrowserRouter>
          <Navbar />
          <Routes>
            <Route
              path="/"
              element={user ? <Home /> : <Navigate to="/login" />}
            />
            <Route
              path="/login"
              element={!user ? <Login /> : <Navigate to="/" />}
            />
            <Route
              path="/signup"
              element={!user ? <Signup /> : <Navigate to="/" />}
            />
          </Routes>
        </BrowserRouter>
      )}
    </div>
  );
}

export default App;
```

### Firestore Timestamp

Timestamp is the way by which Firestore stores dates. Sometimes we need a property that has a timestamp as its value, so that we can know the exact date and time when this document was created.

In order to be able to create timestamps, first we need to adjust our config file:

```js
// firebase/config.js
import firebase from 'firebase/app';
import 'firebase/firestore';
import 'firebase/auth';

const firebaseConfig = {
  apiKey: 'AIzaSyD5NU6NTRsnt4dg2kFA5RxWkaM_PSbp6Ug',
  authDomain: 'react-finance-tracker-64f25.firebaseapp.com',
  projectId: 'react-finance-tracker-64f25',
  storageBucket: 'react-finance-tracker-64f25.appspot.com',
  messagingSenderId: '1098735872131',
  appId: '1:1098735872131:web:ab66984c8090b3bd7353b6',
};

//init firebase
firebase.initializeApp(firebaseConfig);

//init services
const projectFirestore = firebase.firestore();
const projectAuth = firebase.auth();

//timestamp ***
const timestamp = firebase.firestore.Timestamp;

export { projectFirestore, projectAuth, timestamp };
```

### Creating a useFirestore custom hook

This custom hook will allow us to add and delete documents to a Firestore collection.

```js
import { useReducer, useEffect, useState } from 'react';
import { projectFirestore, timestamp } from '../firebase/config';

let initialState = {
  document: null,
  isPending: false,
  error: null,
  success: null,
};

const firestoreReducer = (state, action) => {
  switch (action.type) {
    case 'IS_PENDING':
      return { success: false, isPending: true, error: null, document: null };
    case 'ERROR':
      return {
        success: false,
        isPending: false,
        error: action.payload,
        document: null,
      };
    case 'ADDED_DOCUMENT':
      return {
        success: true,
        isPending: false,
        error: null,
        document: action.payload,
      };
    case 'DELETED_DOCUMENT':
      return {
        isPending: false,
        document: null,
        success: true,
        error: null,
      };
    default:
      return state;
  }
};

export const useFirestore = (collection) => {
  const [response, dispatch] = useReducer(firestoreReducer, initialState);
  const [isCancelled, setIsCancelled] = useState(false);

  // collection ref
  const ref = projectFirestore.collection(collection);

  // only dispatch if not cancelled
  const dispatchIfNotCancelled = (action) => {
    if (!isCancelled) {
      dispatch(action);
    }
  };

  // add a document
  const addDocument = async (doc) => {
    dispatch({ type: 'IS_PENDING' });

    try {
      const createdAt = timestamp.fromDate(new Date());
      const addedDocument = await ref.add({ ...doc, createdAt });
      dispatchIfNotCancelled({
        type: 'ADDED_DOCUMENT',
        payload: addedDocument,
      });
    } catch (err) {
      dispatchIfNotCancelled({ type: 'ERROR', payload: err.message });
    }
  };

  // delete a document
  const deleteDocument = async (id) => {
    dispatch({ type: 'IS_PENDING' });

    try {
      await ref.doc(id).delete();
      dispatchIfNotCancelled({
        type: 'DELETED_DOCUMENT',
      });
    } catch (err) {
      dispatchIfNotCancelled({ type: 'ERROR', payload: 'could not delete' });
    }
  };

  useEffect(() => {
    return () => setIsCancelled(true);
  }, []);

  return { addDocument, deleteDocument, response };
};
```

### Creating a useCollection hook to fetch collection data (all docs)

_see final version of this hook [here](#usecollection-final-version)_

This custom hook uses the method `projectFirestore.collection('collectionName').onSnapshot()` to get access to a snapshot of that collection. The data is extracted from each document of that collection by using the `.data()` method. More on onSnapshot [here](#real-time-collection-data).

We call it inside of a useEffect having collection as a dependency, so that every time the collection changes, the function is called again.

The onSnapshot method returns an **unsubcribe** function, which we can use inside a clean-up function to stop listening to changes on that collection when the component unmounts.

```js
import { useState, useEffect } from 'react';
import { projectFirestore } from '../firebase/config';

export const useCollection = (collection) => {
  const [documents, setDocuments] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    let ref = projectFirestore.collection(collection);

    const unsubscribe = ref.onSnapshot(
      (snapshot) => {
        let results = [];
        snapshot.docs.forEach((doc) => {
          results.push({ ...doc.data(), id: doc.id });
        });

        //update state
        setDocuments(results);
        setError(null);
      },
      (error) => {
        console.log(error);
        setError('could not fetch the data');
      }
    );

    //unsubscribe on unmount
    return () => unsubscribe();
  }, [collection]);

  return { documents, error };
};
```

To use this hook, we pass a collection and receive a documents array, which can be iterated to generate a list. In our project, the documents array is being loaded in the Home.js components, and passed to TransactionList.js as a prop (transactions).

### Firestore queries

Up to now, our transaction list is not filtering transaction by user, so we are always seeing all transaction on every user homepage.

We do that by using Firestore queries, which are passed with the `.where('property', 'logicalOperator', 'value')` method.

For example:

```js
let ref = db.collection('cities');
let refWithQuery = ref.where('capital', '==', 'true');
//this query will filter all cities that are capitals
```

So, we have to update our useCollection hook and also our Home.js component, to pass the query:
_see final version of this hook [here](#usecollection-final-version)_

```js
//useCollection.js

import { useEffect, useState, useRef } from 'react';
import { projectFirestore } from '../firebase/config';

export const useCollection = (collection, _query) => {
  const [documents, setDocuments] = useState(null);
  const [error, setError] = useState(null);

  //_query is an array (reference type), so it is "different" on every function call -> so we have to use useRef to "freeze" it
  const query = useRef(_query).current;

  useEffect(() => {
    let ref = projectFirestore.collection(collection);

    if (query) {
      ref = ref.where(...query);
    }

    const unsubscribe = ref.onSnapshot(
      (snapshot) => {
        let results = [];
        snapshot.docs.forEach((doc) => {
          console.log(doc);
          results.push({ ...doc.data(), id: doc.id });
        });

        // update state
        setDocuments(results);
        setError(null);
      },
      (error) => {
        console.log(error);
        setError('could not fetch the data');
      }
    );

    // unsubscribe on unmount
    return () => unsubscribe();
  }, [collection, query]);

  return { documents, error };
};

//Home.js
etc etc

const { user } = useAuthContext();
const { documents, error } = useCollection('transactions', ['uid', '==', user.uid]);

etc etc
```

### Ordering queries with .orderBy()

_see final version of this hook [here](#usecollection-final-version)_

By default, a query retrieves all documents that satisfy the query in ascending order by document ID. You can specify the sort order for your data using orderBy(), and you can limit the number of documents retrieved using limit().

The .orderBy() method is similar to the .where() method, but it allows us to order a query request in descending order. It accepts 2 arguments, the first is the property which will be evaluated, the second is 'desc' or 'asc'.

In our project, we added a property called 'createdAt' in each of our document, in the moment when we created them. So we can pass this property as an argument to order the documents that are returned by our request.

Ex.: `db.collection('collectionName').where('year', '>', '2020').orderBy('amount', 'desc')`

### useCollection FINAL VERSION!

So, the final version of our useCollection hook is:

```js
// useCollection.js
import { useEffect, useState, useRef } from 'react';
import { projectFirestore } from '../firebase/config';

export const useCollection = (collection, _query, _orderBy) => {
  const [documents, setDocuments] = useState(null);
  const [error, setError] = useState(null);

  //_query is an array (reference type), so it is "different" on every function call -> so we have to use useRef to "freeze" it
  const query = useRef(_query).current;
  const orderBy = useRef(_orderBy).current;

  useEffect(() => {
    let ref = projectFirestore.collection(collection);

    if (query) {
      ref = ref.where(...query);
    }
    if (orderBy) {
      ref = ref.orderBy(...orderBy);
    }

    const unsubscribe = ref.onSnapshot(
      (snapshot) => {
        let results = [];
        snapshot.docs.forEach((doc) => {
          console.log(doc);
          results.push({ ...doc.data(), id: doc.id });
        });

        // update state
        setDocuments(results);
        setError(null);
      },
      (error) => {
        console.log(error);
        setError('could not fetch the data');
      }
    );

    // unsubscribe on unmount
    return () => unsubscribe();
  }, [collection, query, orderBy]);

  return { documents, error };
};
```

And here is an example of its usage:

```js
// Home.js
import { useAuthContext } from '../../hooks/useAuthContext';
import { useCollection } from '../../hooks/useCollection';

// styles
import styles from './Home.module.css';

// components
import TransactionForm from './TransactionForm';
import TransactionList from './TransactionList';

export default function Home() {
  const { user } = useAuthContext();
  const { documents, error } = useCollection(
    'transactions',
    ['uid', '==', user.uid],
    ['createdAt', 'desc']
  );

  return (
    <div className={styles.container}>
      <div className={styles.content}>
        {error && <p>{error}</p>}
        {documents && <TransactionList transactions={documents} />}
      </div>
      <div className={styles.sidebar}>
        <TransactionForm uid={user.uid} />
      </div>
    </div>
  );
}
```

## Firestore Rules

Before deploying our app in production-mode, we have to change Firestore Rules to guarantee the security of our data.

We can edit and publich the rules directly on the Firebase website, but is preferable to to it by using the Firebase CLI.

1. Install Firebase Tools CLI globally:
   `npm install -g firebase-tools`
   mac/linux: `sudo npm install -g firebase-tools`

2. Login:
   `firebase login`

3. On the projects directory:
   `firebase init`

4. Follow steps: NN lecture 143

5. Firebase init will create a bunch of files inside our project's directory. One of these is `firebase.rules`, which we will edit now:

```js
//firebase.rules

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /transactions/{document=**} { //match any document inside the transactions collection
      // allow read, write;
      // allow read, create, delete, update;

      allow create: if request.auth != null;
      // if there is an auth token, user can create a new document

      allow read, delete: if request.auth.uid == resource.data.uid;
      //if the uid of the request matches the uid of the resource we want to access
    }
  }
}
```

6. Deploy our Firestore rules to Firebase:
   `firebase deploy --only firestore`

## Deploying

1. `npm run build`
2. `firebase deploy`
   To update, it's the same process.

## Redux

_repo react-max-section18-redux, section 18, lecture 225_

Redux is a _state management system_ for cross-component or app-wide state.

Disadvantages of React Context:

- complex setup/management: for example, when managing several states in a context or when nesting several context-providers
- performance: for changes that happen more frequently, performance might be a disadvantage

### Redux: core concepts

- central data (state) store
- components "subscribe" to the store
- components NEVER directly manipulate the store
- _reducer functions_ can manipulate the store. Reducer functions are functions that receive an input and _transform_ it into another thing.
- components dispatch _actions_, that are forwarded to the reducer function, that spits out the new data into the central store

### Redux: basic syntax

Similar to useReducer, Redux also takes a reducerFunction as parameter, and this reducerFunction also takes the state and an action as parameters:
`https://www.youtube.com/watch?v=HKU24nY8Hsc&list=PL4cUxeGkcC9ij8CfkAY2RAGb-tmkNwQHG&index=34`

```js
import { legacy_createStore as createStore } from 'redux'; //import createStore

const initState = {
  todos: [],
  posts: []
} //initial state

function myreducer(state = initState, action) {
  if(action.type === 'ADD_TODO') {
    return {
      ...state,
      todos: [...state.todos, action.todo]
    }
  }
} //reducer function with options (if/switch)

const store = createStore(myreducer); //the global store object

const todoAction = { type: 'ADD_TODO', todo: 'buy milk'}; //setting an action

store.dispatch(todoAction) //dispatching the action

store.subscribe() => {
  console.log('state updated');
  console.log(store.getState()); //
} //subscribing to changes on the global store

```

_The reducerFunction..._ should be a pure function (same input leads to same output, ie, there must be no side-effects inside of that function).
_The reducerFunction..._ receives 2 parameters: the old _state_ and a dispatched _action_.
_The reducerFunction..._ must return a new state object.

### Step by step

1. Setting the global store:

```js
import { legacy_createStore as createStore } from 'redux';

const counterReducer = (state = { counter: 0 }, action) => {
  if (action.type === 'increment') {
    return { counter: state.counter + 1 };
  }
  if (action.type === 'decrement') {
    return { counter: state.counter - 1 };
  }
  return state;
};

const store = createStore(counterReducer);

export default store;
```

2. Providing the store:
   We can provide the store in App.js or any other file to wrap its child components inside the scope of that store.

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import { Provider } from 'react-redux';

import './index.css';
import App from './App';
import store from './store';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

3. Using data in components:
   First import the hook _useSelector_ from 'react-redux'. This hook makes data from the state object available throughout the app. It receives a function that "selects" a property of the current state. It also subscribes to that store and, if the component is unmounted, it unsubscribes to it.

```js
import { useSelector } from 'react-redux'; //importing
import classes from './Counter.module.css';

const Counter = () => {
  const counter = useSelector((state) => state.counter); //"selecting"

  const toggleCounterHandler = () => {};

  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      <div className={classes.value}>{counter}</div> //using
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};

export default Counter;
```

4. Dispatching actions from inside components
   To dispatch actions from inside components, we will use another 'react-redux' hook, _useDispatch_.

```js
import { useSelector, useDispatch } from 'react-redux'; //importing
import classes from './Counter.module.css';

const Counter = () => {
  const dispatch = useDispatch(); //saving
  const counter = useSelector((state) => state.counter);

  const incrementHandler = () => {
    dispatch({ type: 'increment' }); //calling
  };

  const decrementHandler = () => {
    dispatch({ type: 'decrement' }); //calling
  };

  const toggleCounterHandler = () => {};

  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      <div className={classes.value}>{counter}</div>
      <div>
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={decrementHandler}>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};

export default Counter;
```

4. 1. Redux with Class-based components
      We can also use Redux with CB-components. For that, we need to use the _connect_ function:

```js
import { useSelector, useDispatch, connect } from 'react-redux'; //importing
import classes from './Counter.module.css';
import { Component } from 'react';

class Counter extends Component {
  incrementHandler() {
    this.props.increment();
  }

  decrementHandler() {
    this.props.decrement();
  }

  toggleCounterHandler() {}

  render() {
    return (
      <main className={classes.counter}>
        <h1>Redux Counter</h1>
        <div className={classes.value}>{this.props.counter}</div>
        <div>
          <button onClick={this.incrementHandler.bind(this)}>Increment</button>
          <button onClick={this.decrementHandler.bind(this)}>Decrement</button>
        </div>
        <button onClick={this.toggleCounterHandler}>Toggle Counter</button>
      </main>
    );
  }
}

//we have to create these 2 functions to pass as arguments in the connect function:
const mapStateToProps = (state) => {
  return {
    counter: state.counter,
  };
};

const mapDispatchToProps = (dispatch) => {
  return {
    increment: () => dispatch({ type: 'increment' }),
    decrement: () => dispatch({ type: 'decrement' }),
  };
};

//connect() returns a function, which will be called having our Counter component as an argument:
export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

### REDUX TOOLKIT

_repo react-max-section18-redux, section 18, lecture 240_

Redux Toolkit simplifies the writing and setting-up of Redux. It is the way to go, especially if we work with deeply nested state objects with a large number of properties.

`npm i @reduxjs/toolkit`

This set-up of the store object and reducer function creates, behind the scenes, methods that can be executed inside the component:

```js
//////// store/index.js
// import { legacy_createStore as createStore } from 'redux';
import { createSlice, configureStore } from '@reduxjs/toolkit';

const initialState = {
  counter: 0,
  showCounter: false,
};

const counterSlice = createSlice({
  name: 'counter',
  initialState: initialState,
  reducers: {
    increment(state) {
      state.counter++;
      //properties can be updated like this, toolkit will prevent from causing side-effects on the original object, creating a new object for us.
    },
    decrement(state) {
      state.counter--;
    },
    increase(state, action) {
      state.counter += action.payload;
    },
    toggleCounter(state) {
      state.showCounter = !state.showCounter;
    },
  },
});

// const counterReducer = (state = initialState, action) => {
//   if (action.type === 'increment') {
//     return { ...state, counter: state.counter + 1 };
//   }
//   if (action.type === 'increase') {
//     return { ...state, counter: state.counter + action.amount };
//   }
//   if (action.type === 'decrement') {
//     return { ...state, counter: state.counter - 1 };
//   }
//   if (action.type === 'toggle') {
//     return { ...state, showCounter: !state.showCounter };
//   }
//   return state;
// };

const store = configureStore({
  reducer: counterSlice.reducer,
  //'reducer' will "merge" all reducer functions into one big function, with cases.
});

export const counterActions = counterSlice.actions;

export default store;


////// components/Counter.js
import { useSelector, useDispatch } from 'react-redux';
import { counterActions } from '../store';
import classes from './Counter.module.css';

const Counter = () => {
  const dispatch = useDispatch();
  const counter = useSelector((state) => state.counter);
  const show = useSelector((state) => state.showCounter);

  const incrementHandler = () => {
    // dispatch({ type: 'increment' });
    dispatch(counterActions.increment());
  };

  const increaseHandler = () => {
    // dispatch({ type: 'increase', amount: 5 });
    dispatch(counterActions.increase(5));
    //{ type: SOME_UNIQUE_IDENTIFIER, payload: 5}  the name of property is 'payload'!
  };

  const decrementHandler = () => {
    // dispatch({ type: 'decrement' });
    dispatch(counterActions.decrement());
  };

  const toggleCounterHandler = () => {
    // dispatch({ type: 'toggle' });
    dispatch(counterActions.toggleCounter());
  };

  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      {show && <div className={classes.value}>{counter}</div>}
      <div>
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={increaseHandler}>Increase by 5</button>
        <button onClick={decrementHandler}>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};

export default Counter;
```

### Multiple slices

When we want to manage multiple states that belong to different features of the app, it's good that we work with more than one slice. We can put everyting inside of a single slice, but this is not a good approach. Remember the _separation of concerns_ principle in programming.

When working with multiple slices, we still have only one **store**.

We must import **useDispatch** into the files where we want to dispatch actions.
We must import **useSelector** into the files where we need to check states.

We can put each slice in a separated file.

```js
/////// store/index.js
import { createSlice, configureStore } from '@reduxjs/toolkit';

const initialCounterState = {
  counter: 0,
  showCounter: false,
};

const counterSlice = createSlice({
  name: 'counter',
  initialState: initialCounterState,
  reducers: {
    increment(state) {
      state.counter++;
    },
    decrement(state) {
      state.counter--;
    },
    increase(state, action) {
      state.counter += action.payload;
    },
    toggleCounter(state) {
      state.showCounter = !state.showCounter;
    },
  },
});

const initialAuthState = {
  isAuthenticated: false,
};

//new slice:
const authSlice = createSlice({
  name: 'authentication',
  initialState: initialAuthState,
  reducers: {
    login(state) {
      state.isAuthenticated = true;
    },
    logout(state) {
      state.isAuthenticated = false;
    },
  },
});

const store = configureStore({
  reducer: { counter: counterSlice.reducer, auth: authSlice.reducer },
  //'reducer' will "merge" all reducer functions into one big function, with cases.
});

export const counterActions = counterSlice.actions;
export const authActions = authSlice.actions;

export default store;


///////Header.js
import { useSelector, useDispatch } from 'react-redux';
import { authActions } from '../store';

import classes from './Header.module.css';

const Header = () => {
  const isAuth = useSelector((state) => state.auth.isAuthenticated);
  const dispatch = useDispatch();

  const logoutHandler = () => {
    dispatch(authActions.logout());
  };

  return (
    <header className={classes.header}>
      <h1>Redux Auth</h1>

      {isAuth && (
        <nav>
          <ul>
            <li>
              <a href="/">My Products</a>
            </li>
            <li>
              <a href="/">My Sales</a>
            </li>
            <li>
              <button onClick={logoutHandler}>Logout</button>
            </li>
          </ul>
        </nav>
      )}
    </header>
  );
};

export default Header;
```

### Async tasks, side-effects & Redux

_here begins repo react-max-19-advanced-redux_
_max section19 lecture 249_

Reducers must be pure, side-effect free, synchronous functions.

They should only receive an old state and an action as input, and output a new state. Nothing more.

If we want to store the Redux output in a server (sending a PUT or POST request), we cannot do it inside of Redux reducers.

So, where should side-effects and async tasks be executed? There are 2 answers for that question:

- inside the components (eg, using useEffect)
- inside the action creators (thunks)

#### Redux & async tasks 1: performing the async tasks using useEffects inside of components

We can choose a component, eg App.js, use useSelector to listen to changes made by Redux on the state, then use useEffect to send a POST or PUT request to the backend.

```js
import { useSelector } from 'react-redux';
import { useEffect } from 'react';
import Cart from './components/Cart/Cart';
import Layout from './components/Layout/Layout';
import Products from './components/Shop/Products';

function App() {
  const showCart = useSelector((state) => state.ui.cartIsVisible);
  const cart = useSelector((state) => state.cart);

  useEffect(() => {
    fetch('https://react-http-3ff60-default-rtdb.firebaseio.com/cart.json', {
      method: 'PUT',
      body: JSON.stringify(cart),
    });
  }, [cart]);

  return (
    <Layout>
      {showCart && <Cart />}
      <Products />
    </Layout>
  );
}

export default App;
```

The only problem with this solution is that useEffect will execute when our app starts, replacing the data in Firebase collection with an empty array (which is the Redux cart state initialState). This problem can be solved this way:

```js
import { useSelector } from 'react-redux';
import { useEffect } from 'react';
import Cart from './components/Cart/Cart';
import Layout from './components/Layout/Layout';
import Products from './components/Shop/Products';

let isInitial = true; // solving!

function App() {
  const showCart = useSelector((state) => state.ui.cartIsVisible);
  const cart = useSelector((state) => state.cart);

  useEffect(() => {
    if (isInitial) {
      isInitial = false;
      return;
    } // solved! now it won't run at start.

    fetch('https://react-http-3ff60-default-rtdb.firebaseio.com/cart.json', {
      method: 'PUT',
      body: JSON.stringify(cart),
    });
  }, [cart]);

  return (
    <Layout>
      {showCart && <Cart />}
      <Products />
    </Layout>
  );
}

export default App;
```

_Complete code contains error catching and notifications component. See repo react-max-19-advanced-redux App2.js_

#### Redux & async tasks 2: performing the async tasks using thunks

Max section 19, lecture 259^
**What is "thunk"?**

- a function that delays an action until later.
- an action creator function that does NOT return the action itself but another function which eventually returns the action.

### Redux DevTools

It is a browser extension that allows us to get a comprehensive outlook of all actions being performed throughout the app's workflow.

We can time-travel (jump) to a specific action, and see what the app looks like when that action just has been performed.

## localStorage

https://javascript.plainenglish.io/creating-a-persistent-cart-in-react-f287ed4b4df0

In this tutorial, Francisco Sainz explains how to use localStorage to implement a persistent shopping cart in ReactJS.

### Benefits of using LocalStorage

As you may be wondering, local storage is used to store key information about a user when he is logged in, which ensures that he has access to a service or functionality without needing to request permission from a server or other every time the client makes a request.

In other words, you have instant access to this memory as long as it exists, so you don’t have to worry about async functions or anything like that.

This wouldn’t be a cool tutorial without some code, so let’s get into it. However, before we start, let me explain how localStorage actually works in practice.

It’s important to understand that anything you store inside this local storage is saved as a string, which is basically in JSON format.

### localStorage methods

#### localStorage.setItem(key: string, value: string)

Notice that to set (save). an item we need to do it as a key value pair, with both consisting of strings. This is important, because as I said before, localStorage will only save strings properly.

To set the value we want — the cart array — we can simply call **JSON.stringify(cart)** and it will be converted to a string representation.

#### localStorage.getItem(key: string)

To get an item, we simply input the key that we used to set the item previously, this too as a string.

Because the value is a string, we call **JSON.parse(cartString)** to convert it to whatever it was before it was stored — an array, in our case.

#### localStorage.removeItem(key: string)

Lastly, to remove a value, we simply pass its key and it will be forever erased from your browser’s local storage, living only in your dreams.

### Implementing a cart using localStorage

We will start by creating an array that will hold our cart’s state by using the useState hook and setting its initial value to an empty array.

```js
const App = () => {
  let [cart, setCart] = useState([]);

  let localCart = localStorage.getItem('cart');

  const addItem = (item) => {};
  const updateItem = (itemID, amount) => {};
  const removeItem = (itemID) => {};

  //this is called on component mount
  useEffect(() => {
    //turn it into js
    localCart = JSON.parse(localCart);
    //load persisted cart into state if it exists
    if (localCart) setCart(localCart);
  }, []); //the empty array ensures useEffect only runs once

  return <div></div>;
};
```

That was easy. You can also see that I added some empty functions that we will use to implement the cart’s logic too.

You can also see that I implemented useEffect, which is a hook that can be run as a functional component version of componentDidMount or componentWillMount. This is done because if the user refreshes the page, the local state will be lost, so we need to load the cart inside localStorage — if it exists — to restore it to the app’s state.

#### Adding a new item

There are two conditions we must account for when adding an item to the cart.

- Adding an item that does not yet exist inside the cart
- Adding an item that already exists inside the cart.

First of all, we need to check to see if the item exists in the array, and if it doesn’t, just add it, but if it does, we need to update the existing item’s quantity.

Finally, we need to save the updated cart to localStorage in case our user is getting trigger happy with the refresh button.

```js
cons addItem = (item) => {

  //create a copy of our cart state, avoid overwritting existing state
  let cartCopy = [...cart];

  //assuming we have an ID field in our item
  let {ID} = item;

  //look for item in cart array
  let existingItem = cartCopy.find(cartItem => cartItem.ID == ID);

  //if item already exists
  if (existingItem) {
      existingItem.quantity += item.quantity //update item
  } else { //if item doesn't exist, simply add it
    cartCopy.push(item)
  }

  //update app state
  setCart(cartCopy)

  //make cart a string and store in local space
  let stringCart = JSON.stringify(cartCopy);
  localStorage.setItem("cart", stringCart)

}
```

#### Updating an Item

To update an item, our function will receive two parameters, the itemID, and the amount to add or subtract.

The amount can be negative, meaning that we are subtracting x quality from an existing item, or adding them if positive.

We can assume the function will always be called on an item that exists in the list, but its always a good idea to make your code work whatever happens, so we will address that scenario either way.

We need to worry about two things here:

- What happens if an item doesn’t exists
- What happens if the final quantity is equal or less than 0

We can simply use find on the array like we did before, and check if it exists. If it doesn’t, we simply return to stop the function.

In case it does exist, we can update and validate the resulting quantity of the affected item.

We do this by simply updating the quantity and check if the result is ≤ 0. If it is, we must delete the item from the cart.

```js
//editItem.js
const editItem = (itemID, amount) => {
  let cartCopy = [...cart];

  //find if item exists, just in case
  let existentItem = cartCopy.find((item) => item.ID == itemID);

  //if it doesnt exist simply return
  if (!existentItem) return;

  //continue and update quantity
  existentItem.quantity += amount;

  //validate result
  if (existentItem.quantity <= 0) {
    //remove item  by filtering it from cart array
    cartCopy = cartCopy.filter((item) => item.ID != itemID);
  }

  //again, update state and localState
  setCart(cartCopy);

  let cartString = JSON.stringify(cartCopy);
  localStorage.setItem('cart', cartString);
};
```

We can remove an item shown above by filtering it from the array. Again, we must update the localState with our resulting state.

```js
//remove.js
const removeItem = (itemID) => {
  //create cartCopy
  let cartCopy = [...cart];

  cartCopy = cartCopy.filter((item) => item.ID != itemID);

  //update state and local
  setCart(cartCopy);

  let cartString = JSON.stringify(cartCopy);
  localStorage.setItem('cart', cartString);
};
```

Our functionality is finally done. Our cart will now be persisted inside localStorage even if the user refreshes or closes the browser.

Additionally, you can reset the cart from memory by calling `localStorage.removeItem(“cart”)` if your user checks out or completes a purchase.
