# React

## 0: Markdown

- **preview**: shift + command + v

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

We can also pass functions as props. This is particularly useful when we want to change the state of a parent component FROM a child component. In this situation, we have to pass the set state function from the parent to the child as a prop, then point to that prop wherever in out child component.

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

When we want the component to be added somewhere else in the DOM, and not on the original parent component's DOM, we can use the method ReactDOM.createPortal() to set another place on the DOM to place it.
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

Inside of a JSX parenthesis, we can add special props that begin with 'on', eg, 'onClick'.

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

## Forms and Events

NN section#7
