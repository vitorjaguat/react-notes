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
        <div className='expense-item'>
          <div>March 28th 2021</div>
          <div className='expense-item__description'>
            <h2>Car Insurance</h2>
            <div className='expense-item__price'>$294.67</div>
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
    <div className='modal-backdrop'>
      <div className='modal'>{props.children}</div>
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
    <div className='modal-backdrop'>
      <div className='modal'>
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
    <div className='modal-backdrop'>
      <div className='modal'>
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
    <Card className='expense-item'>
      <ExpenseDate date={props.date} />
      <div className='expense-item__description'>
        <h2>{props.title}</h2>
        <div className='expense-item__price'>${props.amount}</div>
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
    <div className='App'>
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
    <Card className='expense-item'>
      <ExpenseDate date={props.date} />
      <div className='expense-item__description'>
        <h2>{title}</h2>
        <div className='expense-item__price'>${props.amount}</div>
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
      <div className='new-expense__controls'>
        <div className='new-expense__control'>
          <label>Title</label>
          <input type='text' onChange={titleChangeHandler} />
        </div>
        <div className='new-expense__control'>
          <label>Amount</label>
          <input
            type='number'
            min='0.01'
            step='0.01'
            onChange={amountChangeHandler}
          />
        </div>
        <div className='new-expense__control'>
          <label>Date</label>
          <input
            type='date'
            min='2019-01-01'
            max='2022-12-31'
            onChange={dateChangeHandler}
          />
        </div>
      </div>
      <div className='new-expense__actions'>
        <button type='submit'>Add Expense</button>
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
      <div className='new-expense__controls'>
        <div className='new-expense__control'>
          <label>Title</label>
          <input type='text' onChange={titleChangeHandler} />
        </div>
        <div className='new-expense__control'>
          <label>Amount</label>
          <input
            type='number'
            min='0.01'
            step='0.01'
            onChange={amountChangeHandler}
          />
        </div>
        <div className='new-expense__control'>
          <label>Date</label>
          <input
            type='date'
            min='2019-01-01'
            max='2022-12-31'
            onChange={dateChangeHandler}
          />
        </div>
      </div>
      <div className='new-expense__actions'>
        <button type='submit'>Add Expense</button>
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
    <div className='App'>
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
    <div className='App'>
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
    <div className='App'>
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
    <div className='App'>
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
<div className='modal-backdrop'>
  <div
    className='modal'
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
      <input type='text' onChange={goalInputChangeHandler} />
    </div>
    <Button type='submit'>Add Goal</Button>
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
        <input type='text' onChange={goalInputChangeHandler} />
      </FormControl>
      <Button type='submit'>Add Goal</Button>
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
        <input type='text' onChange={goalInputChangeHandler} />
      </FormControl>
      <Button type='submit'>Add Goal</Button>
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
    <form className='new-event-form'>
      <label>
        <span>Event Title:</span>
        <input type='text' onChange={(e) => setTitle(e.target.value)} />
      </label>
      <label>
        <span>Event Date:</span>
        <input type='date' onChange={(e) => setDate(e.target.value)} />
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
    <form className='new-event-form'>
      <label>
        <span>Event Title:</span>
        <input
          type='text'
          onChange={(e) => setTitle(e.target.value)}
          value={title}
        />
      </label>
      <label>
        <span>Event Date:</span>
        <input
          type='date'
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
    <form className='new-event-form' onSubmit={handleSubmit}>
      <label>
        <span>Event Title:</span>
        <input
          type='text'
          onChange={(e) => setTitle(e.target.value)}
          value={title}
        />
      </label>
      <label>
        <span>Event Date:</span>
        <input
          type='date'
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
    <div className='App'>
      <Title title='Events in Your Area' subtitle={subtitle} />

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
    <form className='new-event-form' onSubmit={handleSubmit}>
      <label>
        <span>Event Title:</span>
        <input
          type='text'
          onChange={(e) => setTitle(e.target.value)}
          value={title}
        />
      </label>
      <label>
        <span>Event Date:</span>
        <input
          type='date'
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
    <form className='new-event-form' onSubmit={handleSubmit}>
      <label>
        <span>Event Title:</span>
        <input type='text' ref={title} />
      </label>
      <label>
        <span>Event Date:</span>
        <input type='date' ref={date} />
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
    <div className='trip-list'>
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
    <div className='trip-list'>
      <h2>TripList</h2>
      <ul>
        {trips.map((trip, index) => (
          <li key={trip.id}>
            <h3>{trip.title}</h3>
            <p>{trip.price}</p>
          </li>
        ))}
      </ul>
      <div className='filters'>
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
    <div className='trip-list'>
      <h2>TripList</h2>
      <ul>
        {trips.map((trip, index) => (
          <li key={trip.id}>
            <h3>{trip.title}</h3>
            <p>{trip.price}</p>
          </li>
        ))}
      </ul>
      <div className='filters'>
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
    <div className='trip-list'>
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
      <div className='filters'>
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
    <div className='trip-list'>
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
      <div className='filters'>
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
        <label htmlFor='username'>Username</label>
        <input
          id='username'
          type='text'
          onChange={usernameChangeHandler}
          value={enteredUsername}
        />
        <label htmlFor='age'>Age</label>
        <input
          id='age'
          type='number'
          onChange={ageChangeHandler}
          value={enteredAge}
        />
        <Button type='submit'>Add User</Button>
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
          <label htmlFor='email'>E-Mail</label>
          <input
            type='email'
            id='email'
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
          <label htmlFor='password'>Password</label>
          <input
            type='password'
            id='password'
            value={enteredPassword}
            onChange={passwordChangeHandler}
            onBlur={validatePasswordHandler}
          />
        </div>
        <div className={classes.actions}>
          <Button type='submit' className={classes.btn} disabled={!formIsValid}>
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

**v6.4**

---

_repo react-max-20-router branch react-router6_4basic, Max lecture 291_

In react-router-dom@6.4, we can use **RouterProvider** component and **createBrowserRouter** function to define our main routes. By using the, we unlock a bunch of new features, like the **loader** prop.

The **loader** prop allows us to avoid using useEffect to load fetched data when we load a component. Fetched data is retrieved inside the component by **useLoaderData** hook. By using that, the page will only load when the data's already been loaded; so we don't have to worry about loading state.

In order to handle errors during fetching/loading data, we can add the prop **errorElement** to the route. We can also pass the errorElement prop to the parent route, then errors from its child routes will bubble-up to that errorElement and output it whenever needed. We can access the error object (and extract error.message) using the **useRouteError** hook in the ErrorPage component.

```js
//App.js
import {
  RouterProvider,
  Route,
  createBrowserRouter,
  createRoutesFromElements,
} from 'react-router-dom';

import BlogLayout from './pages/BlogLayout';
import BlogPostsPage, { loader as blogPostsLoader } from './pages/BlogPosts';
import NewPostPage from './pages/NewPost';
import PostDetailPage from './pages/PostDetail';
import RootLayout from './components/RootLayout';
import WelcomePage from './pages/Welcome';
import ErrorPage from './pages/Error';

const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<RootLayout />} errorElement={<ErrorPage />}>
      <Route index element={<WelcomePage />} />
      <Route path="/blog" element={<BlogLayout />}>
        <Route index element={<BlogPostsPage />} loader={blogPostsLoader} />
        <Route
          path=":id"
          element={<PostDetailPage />}
          loader={blogPostLoader}

        />
      </Route>
      <Route path="/blog/new" element={<NewPostPage />} />
    </Route>
  )
);

function App() {
  return <RouterProvider router={router} />;
}

export default App;

//RootLayout.js (use Outlet instead of props.children)
import { Outlet } from 'react-router-dom';
import MainNavigation from './MainNavigation';

function RootLayout() {
  return (
    <>
      <MainNavigation />
      <main>
        <Outlet />
      </main>
    </>
  );
}

export default RootLayout;

//BlogPosts.js (loading all posts)
import { useLoaderData } from 'react-router-dom';

import Posts from '../components/Posts';
import { getPosts } from '../util/api';

function BlogPostsPage() {
  const loaderData = useLoaderData();

  return (
    <>
      <h1>Our Blog Posts</h1>

      <Posts blogPosts={loaderData} />
    </>
  );
}

export default BlogPostsPage;

export function loader() {
  return getPosts();
}

//PostDetails.js
import { useLoaderData } from 'react-router-dom';
import BlogPost from '../components/BlogPost';
import { getPost } from '../util/api';

function PostDetailPage() {
  const postData = useLoaderData();

  return (
    <>
      <BlogPost title={postData.title} text={postData.body} />
    </>
  );
}

export default PostDetailPage;

export function loader({ params }) {
  const postId = params.id;
  return getPost(postId);
}

//Error.js (the errorElement of the parent route)
import { useRouteError } from 'react-router';

import MainNavigation from '../components/MainNavigation';

function ErrorPage() {
  const error = useRouteError();

  return (
    <>
      <MainNavigation />
      <main id="error-content">
        <h1>An error occurred!</h1>
        <p>{error.message}</p>
      </main>
    </>
  );
}

export default ErrorPage;

```

To submit forms in v6.4, we can use the **Form** component from react-router-dom. We will pass a method (get, post, put, delete) and an action (a path) to it.

```js
//App.jsx
import NewPostPage, { action as newPostAction } from './pages/NewPost';
etc etc
<Route path="/blog/new" element={<NewPostPage />} action={newPostAction} />
etc etc

//NewPostForm.jsx
etc etc
<Form className={classes.form} method="post" action="/blog/new">
etc etc input etc
</Form>

//NewPost.jsx
import { redirect, useNavigate } from 'react-router-dom';

import NewPostForm from '../components/NewPostForm';
import { savePost } from '../util/api';

function NewPostPage() {
  const navigate = useNavigate();

  function cancelHandler() {
    navigate('/blog');
  }

  return (
    <>
      <NewPostForm onCancel={cancelHandler} submitting={false} />
    </>
  );
}

export default NewPostPage;

export async function action({ request }) {
  const formData = await request.formData();
  const post = {
    title: formData.get('title'), //'title' is the 'name' defined in the input element inside the Form component
    body: formData.get('post-text'),
  };
  try {
    savePost(post);
  } catch (err) {
    if (err.status === 422) {
      //to be discussed
      throw err;
    }
    throw err;
  }
  return redirect('/blog');
}
```

By doing this, the error is handled by the errorElement in the main Route in App.js. Whenever an error is thrown by any component, the Errox.jsx function will be output. But, if we instead want to handle the error and show the error message on that same page (NewPost.jsx), we must:

1. return the error, instead of throwing it.
2. use the useActionData hook to get access to that err object.

```js
import { redirect, useNavigate, useActionData } from 'react-router-dom';

import NewPostForm from '../components/NewPostForm';
import { savePost } from '../util/api';

function NewPostPage() {
  const navigate = useNavigate();
  const data = useActionData();

  function cancelHandler() {
    navigate('/blog');
  }

  return (
    <>
      {data && data.status && <p>{data.message}</p>}
      <NewPostForm onCancel={cancelHandler} submitting={false} />
    </>
  );
}

export default NewPostPage;

export async function action({ request }) {
  const formData = await request.formData();
  const post = {
    title: formData.get('title'),
    body: formData.get('post-text'),
  };
  try {
    savePost(post);
  } catch (err) {
    if (err.status === 422) {
      //handling errors on this same page -> the err object will be "catched" by useActionData
      return err;
    }
    throw err;
  }
  return redirect('/blog');
}
```

In order to show 'pending' or 'loading' messages, or to disable a button while sending data to the back-end, we can use the **useNavigation** hook. This hook returns an object, which contains a property called 'state'. State can be either 'idle', 'loading' or 'submitting', and it will automatically listen if actions or loaders functions are working.

```js
//NewPostForm.jsx
etc etc
<button disabled={submitting}>
  {submitting ? 'Submitting...' : 'Create Post'}
</button>
etc etc

//NewPost.jsx
import {
  redirect,
  useNavigate,
  useActionData,
  useNavigation,
} from 'react-router-dom';

import NewPostForm from '../components/NewPostForm';
import { savePost } from '../util/api';

function NewPostPage() {
  const navigate = useNavigate();
  const data = useActionData();
  const navigation = useNavigation(); //calling useNavigation hook

  function cancelHandler() {
    navigate('/blog');
  }

  return (
    <>
      {data && data.status && <p>{data.message}</p>}
      <NewPostForm
        onCancel={cancelHandler}
        submitting={navigation.state === 'submitting'} //using the state property of navigation to conditionally show button text and to disable it while submitting
      />
    </>
  );
}
```

**v6.4 ADVANCED FEATURES**
_repo react-max-20-router, branch react-router6_4advanced, lecture 291 from 36min on_
Whenever you want to load a component that shows content that takes time to load, you can use the **defer** function and the **Await** and **Suspense** components to load the page with a 'loading' state, and then, as the content is loaded (from a slow server, for example), re-render the page and show all content.

```js
//DeferredBlogPosts.jsx
import { Suspense } from 'react';
import { useLoaderData, defer, Await } from 'react-router-dom';

import Posts from '../components/Posts';
import { getSlowPosts } from '../util/api';

function DeferredBlogPostsPage() {
  const loaderData = useLoaderData();

  return (
    <>
      <h1>Our Blog Posts</h1>
      <Suspense fallback={<p>Loading...</p>}>
        <Await
          resolve={loaderData.posts}
          errorElement={<p>Error loading blog posts.</p>}
        >
          {(loadedPosts) => <Posts blogPosts={loadedPosts} />}
          {/* render props:  the function will be executed once the resolve prop in Await is finished */}
        </Await>
      </Suspense>
    </>
  );
}

export default DeferredBlogPostsPage;

export async function loader() {
  return defer({ posts: getSlowPosts() });
}
```

If you want to send a form submit request and stay on the same page, you can use the **useFetch** hook to manually trigger a submit function in a form. useFetch also works for manually triggering a loader function.

---

**v6**

---

_repo react-max-20-router branch react-router6-example1_

Switch no longer exists. Instead, we wrap our routes in a Routes component.

The prop `exact` also don't exist anymore. React Router DOM v6 includes its feature by default on the app's routes.

The order of the Route components doesn't matter in v6: React will always match the best paths for each route.

new syntax to define routes:

```js
<Routes>
  <Route path='/welcome' element={<Welcome />} />

  <Route path='/products' element={<Products />} />

  <Route path='/products/:productId' element={<ProductDetail />} />
</Routes>
```

Links stay the same in v6.

NavLinks: activeClassName prop doesn't exist anymore. Instead, we have to manually pass a function with the navData object as a parameter:

```js
//v5
<NavLink activeClassName={classes.active} to="/products">
  Products
</NavLink>

//v6
<NavLink className={(navData) => (navData.isActive ? classes.active : '')} to="/products">
  Products
</NavLink>
```

_repo react-max-20-router branch react-router6-example2_
The Redirect component doesn't exist anymore. Instead, we can use the Navigate component.

```js
<Route path='/' element={<Navigate replace to='/welcome' />} />
```

In v6, Route components always have to be wrapped by Routes component, even in nested routes. Besides, in nested routes (routes that are defined outside the root route file), we set the path to the relative path.

```js
<Link to="new-user">New User</Link>
<Routes>
  <Route path="new-user" element={<p>Welcome, new user!</p>} />
</Routes>
```

Nested routes can also be defined directly using this kind of syntax:

```js
//App.js
<Route path='/welcome/*' element={<Welcome />}>
  <Route path='new-user' element={<p>Welcome, new user!</p>} />
</Route>;

//Welcome.js
const Welcome = () => {
  return (
    <section>
      <h1>The Welcome Page</h1>
      <Link to='new-user'>New User</Link>
      <Outlet />
    </section>
  );
};
```

Imperative or programmatic navigation is used inside of an useEffect hook or when some data finished fetching.

In v5, to make imperative navigation, we used to use useHistory and methods like .push('/path') and .replace('/path').
In v6, we now use the **useNavigate** hook.

```js
const navigate = useNavigate();
navigate('/path'); //the same as .push('/path')
navigate('/path', { replace: true }); //the same as .replace('/path')
navigate(-1); //go back to previous page
```

The Prompt component, used to prevent user from leaving page when filling a form, is no longer available in v6.

---

v5

---

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
    <Route path='/welcome/new-user'>
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
      <div className='centered'>
        <Link className='btn--flat' to={`/quotes/${params.quoteId}/comments`}>
          show comments
        </Link>
      </div>
    </Route>

    <Route path={`/quotes/${params.quoteId}/comments`}>
      <Comments />
      <div className='centered'>
        <Link className='btn--flat' to={`/quotes/${params.quoteId}`}>
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
<Route path='/' exact>
  <Redirect to='/welcome' />
</Route>
```

(users who try to reach '/' will be redirected to '/welcome')

### Not Found page

It is good practice to add a '_' route to redirect all non-assigned routes to a Not Found page. Just add a Route with the path assigned to '_':

```js
<Route path='*'>
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
    <div className='home'>
      <h2>Articles</h2>
      {isPending && <div>Loadind...</div>}
      {error && <div>{error}</div>}
      {articles &&
        articles.map((article) => (
          <div key={article.id} className='card'>
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
    <div className='searchbar'>
      <form onSubmit={handleSubmit}>
        <label htmlFor='search'>Search:</label>
        <input
          type='text'
          id='search'
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
      <h2 className='page-title'>Recipes including "{query}"</h2>
      {error && <p className='error'>{error}</p>}
      {isPending && <p className='loading'>Loading...</p>}
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

## React Router v6.4

_from Max course updates from 2023 Jan 16, section 20, lecture 266, repo react-max-20-router64_

### Defining routes

We generally define our routes in App.js, by storing the result of a `createBrowserRouter([{ path: 'xxx', element: <Xxx /> }, { path: 'yyy', element: <Yyy /> }])` function and passing it through the `router` prop in the returned `RouterProvider` component.

```js
//App.js
import { createBrowserRouter, RouterProvider } from 'react-router-dom';
import Home from './pages/Home';
import Products from './pages/Products';

const router = createBrowserRouter([
  { path: '/', element: <Home /> },
  { path: '/products', element: <Products /> },
]);

function App() {
  return <RouterProvider router={router} />;
}

export default App;
```

### Defining routes (alternative way)

There is an other way of defining routes, using `createRoutesfromElements(<jsx-code>)` function:

```js
// App.js
import {
  createBrowserRouter,
  createRoutesFromElements,
  RouterProvider,
  Route,
} from 'react-router-dom';
import Home from './pages/Home';
import Products from './pages/Products';

const routeDefinitions = createRoutesFromElements(
  <Route>
    <Route path='/' element={<Home />} />
    <Route path='/products' element={<Products />} />
  </Route>
);

const router = createBrowserRouter(routeDefinitions);

function App() {
  return <RouterProvider router={router} />;
}

export default App;
```

### Link component

Just like v5.

```js
// Home.js
import { Link } from 'react-router-dom';

export default function Home() {
  return (
    <>
      <h1>My Home Page</h1>
      <p>
        Go to <Link to='/products'>the list of products.</Link>
      </p>
    </>
  );
}
```

### Root Layout

We can add a layout to be rendered on all pages, 'wrapping' all pages' content. Inside the layout component, we also have to indicate _where_ the child components will be rendered, by using the built-in `Outlet` component.

```js
// App.js
import {
  createBrowserRouter,
  RouterProvider,
} from 'react-router-dom';
import Home from './pages/Home';
import Products from './pages/Products';
import RootLayout from './pages/Root';

const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    children: [
      { path: '/', element: <Home /> },
      { path: '/products', element: <Products /> },
    ],
  },
  ,
]);

function App() {
  return <RouterProvider router={router} />;
}

export default App;

// pages/Root.js
import { Outlet } from 'react-router-dom';
import MainNavigation from '../component/MainNavigation';
import classes from './Root.module.css';

export default function RootLayout() {
  return (
    <>
      <MainNavigation />
      <main className={classes.content}>
        <Outlet />
      </main>
    </>
  );
}

// components/MainNavigation.js
import { Link } from 'react-router-dom';
import classes from './MainNavigation.module.css';

export default function MainNavigation() {
  return (
    <header className={classes.header}>
      <nav>
        <ul className={classes.list}>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/products">Products</Link>
          </li>
        </ul>
      </nav>
    </header>
  );
}
```

### Nested layouts

We can add a nested layout inside of a root layout. This nested layout will render its children 'wrapped' in the element component of that path. Again, don't forget the Outlet component inside the nested layout component too.

```js
// App.js (branch part2, repo react-max-20-router64)
etc etc
const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    children: [
      { index: true, element: <Home /> },
      {
        path: 'events',
        element: <EventsRoot />,
        children: [
          { index: true, element: <Events /> },
          { path: ':eventId', element: <EventDetail /> },
          { path: 'new', element: <NewEvent /> },
          { path: ':eventId/edit', element: <EditEvent /> },
        ],
      },
    ],
  },
]);
etc etc
```

### Showing error pages with errorElement

We can define a callback component to be rendered whenever the user tries to reach a path that doesn't exist by defining an `errorElement` property in your router definitions.

```js
// App.js
etc etc
const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    errorElement: <ErrorPage />,
    children: [
      { path: '/', element: <Home /> },
      { path: '/products', element: <Products /> },
    ],
  },
  ,
]);
etc etc
```

### NavLink

NavLink component has special features like showing that a link is active (was clicked). Remember to use the `end` attribute to ensure that the path is exact. You can also define the styles inline (second NavLink in the example).

```js
// MainNavigation.module.css
.list a:hover,
.list a.active {
  text-decoration: underline;
}

// MainNavigation.js
import { NavLink } from 'react-router-dom';
import classes from './MainNavigation.module.css';

export default function MainNavigation() {
  return (
    <header className={classes.header}>
      <nav>
        <ul className={classes.list}>
          <li>
            <NavLink
              to="/"
              end
              className={({ isActive }) =>
                isActive ? classes.active : undefined
              }
            >
              Home
            </NavLink>
          </li>
          <li>
            <NavLink
              to="/products"
              // className={({ isActive }) =>
              //   isActive ? classes.active : undefined
              // }
              style={({ isActive }) => ({ textDecoration: isActive ? 'underline' : 'none' })}
            >
              Products
            </NavLink>
          </li>
        </ul>
      </nav>
    </header>
  );
}
```

### Navigating programmatically (redirect)

Instead of using useHistory, in 6.4 we use `useNavigate`. Just save it to a constant `const navigate = useNavigate()`, then call `navigate('/pathname')`.

### Dinamic paths

In the route definitions, we mark the dinamic path with :, eg, `path: '/products/:productId`. Then, in the details component, we extract the entered path by using `useParams` hook.

```js
// App.js
etc etc
const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    errorElement: <ErrorPage />,
    children: [
      { path: '/', element: <Home /> },
      { path: '/products', element: <Products /> },
      { path: '/products/:productId', element: <ProductDetail /> },
    ],
  },
  ,
]);
etc etc

// ProductDetail.js
import { useParams } from 'react-router-dom';

export default function ProductDetail() {
  const params = useParams();

  return (
    <>
      <div>ProductDetail</div>
      <p>{params.productId}</p>
    </>
  );
}

// Products.js
import { Link } from 'react-router-dom';

const PRODUCTS = [
  { id: 'p1', title: 'Product 1' },
  { id: 'p2', title: 'Product 2' },
  { id: 'p3', title: 'Product 3' },
];

export default function Products() {
  return (
    <>
      <h1>The Products Page</h1>
      <ul>
        {PRODUCTS.map((prod) => (
          <li key={prod.id}>
            <Link to={`/products/${prod.id}`}>{prod.title}</Link>
          </li>
        ))}
      </ul>
    </>
  );
}

```

### Absolute paths & Relative paths

In the path definitions in App.js, if the path begins with `/`, we call it an `absolute path`. When the root in our definitions is simply '/', we don't need to add the '/' in the beginning if our nested paths, we just need to write `path: 'path'`.

In Links, the relative path is added after the active path. The absolute path is added after the domain-name.

So, in the Products component, we can change the absolute path

```js
<Link to={`/products/${prod.id}`}>
```

to the relative path

```js
<Link to={prod.id}>
```

If we use the relative path '..', it goes back to the parent path in the path definition (that is not necessarily equal to .. when used in our CLI for example). To get a behavior that is always the same as in the CLI for '..', we can add the attribute `relative='path'` to the element:

```js
<Link to='..' relative='path'>
  Back
</Link>
```

### index route

In our routes definitions, if we set the property `index: true` of a route to true, then the assigned element will work as an 'index.html' or as a '/' path to that route.

```js
//App.js
etc etc
const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    errorElement: <ErrorPage />,
    children: [
      { index: true, element: <Home /> }, //path: ''
      { path: '/products', element: <Products /> },
      { path: '/products/:productId', element: <ProductDetail /> },
    ],
  },
]);
etc etc
```

### Data fetching with a loader()

_from now on, repo react-max-20-router64, branch part2_
In Router 6.4, we can define a loader function. The loader function will be called as soon as the user hits the path, even before (or simultaneously) the component itself is loaded.

```js
// App.js
etc etc
const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    children: [
      { index: true, element: <Home /> },
      {
        path: 'events',
        element: <EventsRoot />,
        children: [
          {
            index: true,
            element: <Events />,
            loader: async () => {
              const response = await fetch('http://localhost:8080/events');
              if (!response.ok) {
                // ...
              } else {
                const resData = await response.json();
                return resData.events;
              }
            },
          },
          { path: ':eventId', element: <EventDetail /> },
          { path: 'new', element: <NewEvent /> },
          { path: ':eventId/edit', element: <EditEvent /> },
        ],
      },
    ],
  },
]);
etc etc
```

The data that is returned from a loader function can be retrieved in the component, by using the `useLoaderData` hook.

The useLoaderData can also be called in components inside the component itself. In the example below, we could call it inside the EventsList component, then we don't have to pass the events object as a prop. That means: you can use useLoaderData in the element that's assigned to that route AND in all components that might be used inside that element.

```js
import { useLoaderData } from 'react-router-dom';

import EventsList from '../components/EventsList';

function EventsPage() {
  const events = useLoaderData();

  return (
    <>
      <EventsList events={events} />
    </>
  );
}

export default EventsPage;
```

- The usual workflow for loader functions is to export it from the component, then import it in App.js:

```js
//App.js
etc etc
import Events, { loader as eventsLoader } from './pages/Events.js';

const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    children: [
      { index: true, element: <Home /> },
      {
        path: 'events',
        element: <EventsRoot />,
        children: [
          {
            index: true,
            element: <Events />,
            loader: eventsLoader, //here
          },
          { path: ':eventId', element: <EventDetail /> },
          { path: 'new', element: <NewEvent /> },
          { path: ':eventId/edit', element: <EditEvent /> },
        ],
      },
    ],
  },
]);
etc etc
```

- The loader function can return responses from fetch, directly. The fetch() function returns a promise, and usually we would call .json() to get the actual data. But with Router6.4 we don't need it. We can return the responses, and the data will be available for consuming as a result of useLoaderData. So the code can stay like this:

```js
import { useLoaderData } from 'react-router-dom';

import EventsList from '../components/EventsList';

function EventsPage() {
  const data = useLoaderData();
  const events = data.events;

  return (
    <>
      <EventsList events={events} />
    </>
  );
}

export default EventsPage;

export async function loader() {
  const response = await fetch('http://localhost:8080/events');

  if (!response.ok) {
    // ...
  } else {
    // const resData = await response.json();
    // return resData.events;
    return response; //actually you don't need to call .json(), because the promise is resolved by Router itself when you call useLoaderData()!
  }
}
```

### Handling errors in loader functions

- Handling errors in loader functions (1). When !response.ok, we can returns an object with a "pseudostate", check for it and return a jsx with a custom message:

```js
import { useLoaderData } from 'react-router-dom';

import EventsList from '../components/EventsList';

function EventsPage() {
  const data = useLoaderData();

  if (data.isError) {
    return <p>{data.message}</p>;
  }

  const events = data.events;

  return (
    <>
      <EventsList events={events} />
    </>
  );
}

export default EventsPage;

export async function loader() {
  const response = await fetch('http://localhost:8080/events');

  if (!response.ok) {
    return { isError: true, message: 'Could not fetch events.' };
  } else {
    return response;
  }
}
```

- Handling errors in loader functions (2): alternatively, we can `throw` an error when !response.ok. When useLoaderData finds the thrown error, it will render the closest error element that is defined in the route definitions.

```js
import { useLoaderData } from 'react-router-dom';

import EventsList from '../components/EventsList';

function EventsPage() {
  const data = useLoaderData();

  // if (data.isError) {
  //   return <p>{data.message}</p>;
  // }

  const events = data.events;

  return (
    <>
      <EventsList events={events} />
    </>
  );
}

export default EventsPage;

export async function loader() {
  const response = await fetch('http://localhost:8080/events');

  if (!response.ok) {
    // return { isError: true, message: 'Could not fetch events.' };
    throw { message: 'Could not fetch events.' }; //once you throw something, the errorElement defined in your route definitions will be loaded. This custom message still wouldn't be shown (see below how to show it).
  } else {
    return response;
  }
}
```

- Handling errors in loader functions (3): maybe we want to have only one errorElement in our route definitions, but showing different messages depending on the status of that particular error. Then we can throw a Response() and, in that error page, deconstruct it to get the right message for each case.

```js
//Events.js
import { useLoaderData } from 'react-router-dom';

import EventsList from '../components/EventsList';

function EventsPage() {
  const data = useLoaderData();

  // if (data.isError) {
  //   return <p>{data.message}</p>;
  // }

  const events = data.events;

  return (
    <>
      <EventsList events={events} />
    </>
  );
}

export default EventsPage;

export async function loader() {
  const response = await fetch('http://localhost:8080/events');

  if (!response.ok) {
    throw new Response(
      JSON.stringify({ message: 'Could not fetch events. ' }),
      { status: 500 }
    );
  } else {
    return response;
  }
};

// Error.js
import { useRouteError } from 'react-router-dom';

import MainNavigation from '../components/MainNavigation';

export default function Error() {
  const error = useRouteError();

  let title = 'An error occurred!';
  let message = 'Something went wrong.';

  if (error.status === 500) {
    message = JSON.parse(error.data).message;
  }

  if (error.status === 404) {
    title = 'Not found!';
    message = 'Could not find resource or page';
  }

  return (
    <>
      <MainNavigation />
      <h1>{title}</h1>
      <p>{message}</p>
    </>
  );
}
```

- Handling errors in loader functions (4): instead of constructing a Response() object, we can use the `json()` function. It is a Router 6.4 feature that allows you to handle data with no need to use JSON.stringify or JSON.parse methods.

```js
// Events.js
import { useLoaderData, json } from 'react-router-dom';

import EventsList from '../components/EventsList';

function EventsPage() {
  const data = useLoaderData();

  const events = data.events;

  return (
    <>
      <EventsList events={events} />
    </>
  );
}

export default EventsPage;

export async function loader() {
  const response = await fetch('http://localhost:8080/events');

  if (!response.ok) {
    throw json({ message: 'Could not fetch events.' }, { status: 500 });
  } else {
    return response;
  }
}

// Error.js
import { useRouteError } from 'react-router-dom';

import MainNavigation from '../components/MainNavigation';

export default function Error() {
  const error = useRouteError();

  let title = 'An error occurred!';
  let message = 'Something went wrong.';

  if (error.status === 500) {
    message = error.data.message; //no need to parse the object because we used the json function in the loader!
  }

  if (error.status === 404) {
    title = 'Not found!';
    message = 'Could not find resource or page';
  }

  return (
    <>
      <MainNavigation />
      <h1>{title}</h1>
      <p>{message}</p>
    </>
  );
}
```

### Dynamic paths & loader()

The loader functions of a route accepts 2 parameters: `loader(request, params)`

- the first is a `request` parameter, that can be used, for axample, to get the url (request.url) and work with query strings;
- the second is a `params` parameter, that can be used to get the actual params used in that :dynamicValue path. Remember we cannot use any hooks like useParams outside of a component.

The rest of the workflow is the same of a static path with loader function.

```js
// EventDetails.js
import { json, useLoaderData } from 'react-router-dom';
import EventItem from '../components/EventItem';

export default function EventDetail() {
  // const params = useParams(); No need to use it because we're getting the params in the loader!
  const data = useLoaderData();

  return (
    <>
      <EventItem event={data.event} />
    </>
  );
}

export async function loader({ request, params }) {
  //request.url can access query parameters, for example;
  const id = params.eventId; //as with useParams, the name of the property will be the same as the :dynamicValue defined in the route definitions

  const response = await fetch('http://localhost:8080/events/' + id);

  if (!response.ok) {
    throw json(
      { message: 'Could now fetch details for selected event.' },
      { status: 500 }
    );
  } else {
    return response;
  }
}

// App.js
etc etc
const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    errorElement: <Error />,
    children: [
      { index: true, element: <Home /> },
      {
        path: 'events',
        element: <EventsRoot />,
        children: [
          {
            index: true,
            element: <Events />,
            loader: eventsLoader,
          },
          {
            path: ':eventId',
            element: <EventDetail />,
            loader: eventDetailLoader,
          },
          { path: 'new', element: <NewEvent /> },
          { path: ':eventId/edit', element: <EditEvent /> },
        ],
      },
    ],
  },
]);
etc etc
```

### useRouteLoaderData(): accessing data from other routes

We can get the data returned from a loader even outside of its own route, if instead of using useLoaderData, we use `useRouteLoaderData('route-id')`. We have to define an `id` as a property of the route that really has the loader, then pass it as a 'route-id' in useRouterLoaderData.

```js
//App.js (defining an id for the route)
etc etc
const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    errorElement: <Error />,
    children: [
      { index: true, element: <Home /> },
      {
        path: 'events',
        element: <EventsRoot />,
        children: [
          {
            index: true,
            element: <Events />,
            loader: eventsLoader,
          },
          {
            path: ':eventId',
            id: 'event-detail', //here
            loader: eventDetailLoader,
            children: [
              {
                index: true,
                element: <EventDetail />,
              },
              { path: 'edit', element: <EditEvent /> },
            ],
          },
          { path: 'new', element: <NewEvent /> },
        ],
      },
    ],
  },
]);
etc etc

// EditEvent.js
import { useRouteLoaderData } from 'react-router-dom';
import EventForm from '../components/EventForm';

export default function EditEvent() {
  const data = useRouteLoaderData('event-detail');

  return <EventForm event={data.event} />;
}
```

### action function (& Form component)

Just like loader functions, action functions can be exported from the component file, then imported to App.js, then we can point to it as the value of the action property in our route definitions.

Route actions are the "writes" to route loader "reads". They provide a way for apps to perform data mutations with simple HTML and HTTP semantics. Actions are called whenever the app sends a non-get submission ("post", "put", "patch", "delete") to your route.

As loaders, actions also have { request, params } parameter. Here, the `request` object can be used to parse data from the Form component by using `request.formData()`. From formData, we can extract the form entered values, by calling `get('nameOfInput')`.

Now, we just have to construct the data which will be sent as the `body` of the fetch request 'post' to the server.

```js
// NewEvent.js
import { json, redirect } from 'react-router-dom';
import EventForm from '../components/EventForm';

export default function NewEvent() {
  return <EventForm />;
}

export async function action({ request, params }) {
  const data = await request.formData(); //formData() is built-in into the request object.

  const eventData = {
    title: data.get('title'), //the arguments are the names of the inputs in that Form
    image: data.get('image'),
    date: data.get('date'),
    description: data.get('description'),
  };

  const response = await fetch('http://localhost:8080/events', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(eventData),
  });

  if (!response.ok) {
    throw json({ message: 'Could not save event.' }, { status: 500 });
  }

  return redirect('/events'); //we are not inside of a component, so we can't use useNavigate! Router 6.4 has the redirect('/path') function for these cases.
}

// EventForm.js
import { useNavigate, Form } from 'react-router-dom'; //importing the Form component

import classes from './EventForm.module.css';

function EventForm({ method, event }) {
  const navigate = useNavigate();
  function cancelHandler() {
    navigate('..');
  }

  return (
    <Form method="post" className={classes.form}> //using the Form component: add a method!
      <p>
        <label htmlFor="title">Title</label>
        <input
          id="title"
          type="text"
          name="title" //don't forget to add names to all inputs!
          required
          defaultValue={event ? event.title : ''}
        />
      </p>
      <p>
        <label htmlFor="image">Image</label>
        <input
          id="image"
          type="url"
          name="image"
          required
          defaultValue={event ? event.image : ''}
        />
      </p>
      <p>
        <label htmlFor="date">Date</label>
        <input
          id="date"
          type="date"
          name="date"
          required
          defaultValue={event ? event.date : ''}
        />
      </p>
      <p>
        <label htmlFor="description">Description</label>
        <textarea
          id="description"
          name="description"
          rows="5"
          required
          defaultValue={event ? event.description : ''}
        />
      </p>
      <div className={classes.actions}>
        <button type="button" onClick={cancelHandler}>
          Cancel
        </button>
        <button>Save</button>
      </div>
    </Form>
  );
}

export default EventForm;

//App.js
etc etc
const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    errorElement: <Error />,
    children: [
      { index: true, element: <Home /> },
      {
        path: 'events',
        element: <EventsRoot />,
        children: [
          {
            index: true,
            element: <Events />,
            loader: eventsLoader,
          },
          {
            path: ':eventId',
            id: 'event-detail',
            loader: eventDetailLoader,
            children: [
              {
                index: true,
                element: <EventDetail />,
              },
              { path: 'edit', element: <EditEvent /> },
            ],
          },
          { path: 'new', element: <NewEvent />, action: newEventAction }, //pointing the action function
        ],
      },
    ],
  },
]);
etc etc
```

### action function (2): programmatically submit

Besides a Form component, there is another way to trigger an action function. Form components usually send POST requests, but they also can be used to send DELETE or UPDATE requests. Here we will see how to send a DELETE request, but triggering the action without using a Form component. Instead, we'll use the `useSubmit` hook to programmatically trigger the action.

`useSubmit` will take 2 arguments:

- an object that contains the data (this data can be extracted in the action by using request.formData() and .get(), just as when we used Form component to trigger the action)
- an object that contain other options like method (delete, update, etc) and action (this property is used when we want to trigger an action that is defined in another path that is not the current path)

`useSubmit({ data: 'etc' }, { method: 'etc', action: '/path' })`

The information passed as parameters for useSubmit can be accessed in the action({ request }), request.formData(), request.method, and so on.

```js
// EventItem.js
import { Link, useSubmit } from 'react-router-dom';

import classes from './EventItem.module.css';

function EventItem({ event }) {
  const submit = useSubmit(); //calling useSubmit()

  function startDeleteHandler() {
    const proceed = window.confirm('Are you sure?'); //browser built-in function, returns a boolean.

    if (proceed) {
      submit(null, { method: 'delete' }); //programmatically triggering the action
    }
  }

  return (
    <article className={classes.event}>
      <img src={event.image} alt={event.title} />
      <h1>{event.title}</h1>
      <time>{event.date}</time>
      <p>{event.description}</p>
      <menu className={classes.actions}>
        <Link to="edit">Edit</Link>
        <button onClick={startDeleteHandler}>Delete</button> //the button that is triggering the action
      </menu>
    </article>
  );
}

export default EventItem;

// EventDetail.js
etc etc
export async function action({ params, request }) {
  const eventId = params.eventId;

  const response = await fetch('http://localhost:8080/events' + eventId, {
    // method: 'DELETE' // by hardcoding like this, you don't need to pass the method as an argument of useSubmit() (in EventItem.js)
    method: request.method,
  });

  if (!response.ok) {
    throw json({ message: 'Could not delete event.' }, { status: 500 });
  }
  return redirect('/events');
}

// App.js
etc
import EventDetail, {
  loader as eventDetailLoader,
  action as deleteEventAction,
} from './pages/EventDetail';
etc
const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    errorElement: <Error />,
    children: [
      { index: true, element: <Home /> },
      {
        path: 'events',
        element: <EventsRoot />,
        children: [
          {
            index: true,
            element: <Events />,
            loader: eventsLoader,
          },
          {
            path: ':eventId',
            id: 'event-detail',
            loader: eventDetailLoader,
            children: [
              {
                index: true,
                element: <EventDetail />,
                action: deleteEventAction,
              }, //defining our action for this path
              { path: 'edit', element: <EditEvent /> },
            ],
          },
          { path: 'new', element: <NewEvent />, action: newEventAction },
        ],
      },
    ],
  },
]);
etc
```

#### Updating the UI state based on the submission status

This is valid both for submissions sent via Form component or useSubmit.

We can use the useNavigation hook to conditionally showing a message (or disabling a button, for example) IF useNavigation().state is equal to 'submitting'. See more about the useNavigation hook below.

#### Reusing actions via request.method

We can use a same action function for two different purposes. For example, we can use a same action to fetch both 'POST' and 'PATCH' (update) fetch requests.

In our code, we will move the action to the EventForm component, and use this Form both to 'POST' and to 'PATCH' a request. We have to make the proper changes on the action and then, finally, define this action function as the action on both routes on App.js.

```js
// EventForm.js
import {
  useNavigate,
  Form,
  useNavigation,
  useActionData,
  json,
  redirect,
} from 'react-router-dom';

import classes from './EventForm.module.css';

function EventForm({ method, event }) {
  const data = useActionData();
  const navigate = useNavigate();
  const navigation = useNavigation();

  const isSubmitting = navigation.state === 'submitting';

  function cancelHandler() {
    navigate('..');
  }

  return (
    <Form method={method} className={classes.form}>
      //two pages are outputting this component: in NewEvent.js, the prop method
      is 'post', while in EditEvent.js the prop method is 'patch'
      {data && data.errors && (
        <ul>
          {Object.values(data.errors).map((err) => (
            <li key={err}>{err}</li>
          ))}
        </ul> //this comes from the validation (lecture 301)
      )}
      <p>
        <label htmlFor='title'>Title</label>
        <input
          id='title'
          type='text'
          name='title'
          required
          defaultValue={event ? event.title : ''}
        />
      </p>
      <p>
        <label htmlFor='image'>Image</label>
        <input
          id='image'
          type='url'
          name='image'
          required
          defaultValue={event ? event.image : ''}
        />
      </p>
      <p>
        <label htmlFor='date'>Date</label>
        <input
          id='date'
          type='date'
          name='date'
          required
          defaultValue={event ? event.date : ''}
        />
      </p>
      <p>
        <label htmlFor='description'>Description</label>
        <textarea
          id='description'
          name='description'
          rows='5'
          required
          defaultValue={event ? event.description : ''}
        />
      </p>
      <div className={classes.actions}>
        <button type='button' onClick={cancelHandler} disabled={isSubmitting}>
          Cancel
        </button>
        <button disabled={isSubmitting}>
          {isSubmitting ? 'Saving...' : 'Save'}
        </button>
      </div>
    </Form>
  );
}

export default EventForm;

export async function action({ request, params }) {
  const method = request.method; //extract the method from the request that has been sent
  const data = await request.formData();

  const eventData = {
    title: data.get('title'), //the arguments are the names of the inputs in that Form
    image: data.get('image'),
    date: data.get('date'),
    description: data.get('description'),
  };

  let url = 'http://localhost:8080/events/';

  if (method === 'PATCH') {
    url += params.eventId;
  } //concat the eventId if the method is 'PATCH'

  const response = await fetch(url, {
    method: method, //use request.method instead of hard-coding it
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(eventData),
  });

  if (response.status === 422) {
    return response;
  }

  if (!response.ok) {
    throw json({ message: 'Could not save event.' }, { status: 500 });
  }

  return redirect('/events');
}

//App.js
import { action as manipulateEventAction } from './components/EventForm';
etc;
const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    errorElement: <Error />,
    children: [
      { index: true, element: <Home /> },
      {
        path: 'events',
        element: <EventsRoot />,
        children: [
          {
            index: true,
            element: <Events />,
            loader: eventsLoader,
          },
          {
            path: ':eventId',
            id: 'event-detail',
            loader: eventDetailLoader,
            children: [
              {
                index: true,
                element: <EventDetail />,
                action: deleteEventAction,
              },
              {
                path: 'edit',
                element: <EditEvent />,
                action: manipulateEventAction,
              }, //here
            ],
          },
          { path: 'new', element: <NewEvent />, action: manipulateEventAction }, //here
        ],
      },
    ],
  },
]);
etc;
```

#### useFetcher(): sending an action to another route

We can trigger the action defined for another route using the ` action` prop in the Form component. But this would transition the user to that route, redirecting his navigation, automatically.

To avoid transitioning to another route when triggering that route's action from an 'outsider' route, we can use the `useFetcher` hook. This hook triggers the action behind the scenes, without transitioning the user to that action's route.

`const fetcher = useFetcher()` has several built-in methods and components, eg, `fetcher.Form`, `fetcher.data` (to get the data returned by the action), `fetcher.state` (either 'idle', 'loading' or 'submitting', just like in useNavigation).

```js
// NewsletterSignup.js
import { useEffect } from 'react';
import { useFetcher } from 'react-router-dom';
import classes from './NewsletterSignup.module.css';

function NewsletterSignup() {
  const fetcher = useFetcher();
  const { data, state } = fetcher;

  useEffect(() => {
    if (state === 'idle' && data && data.message) {
      window.alert(data.message);
    }
  }, [data, state]);

  return (
    <fetcher.Form
      action='/newsletter'
      method='post'
      className={classes.newsletter}
    >
      <input
        type='email'
        placeholder='Sign up for newsletter...'
        aria-label='Sign up for newsletter'
      />
      <button>Sign up</button>
    </fetcher.Form>
  );
}

export default NewsletterSignup;
```

### useNavigation

When the user clicks on a path that has a loader function, the page will be shown only after the loader function has completed and the components have been mounted. So, we no longer can have a isPending state to show a 'loading...' message while the data is being fetched.

In order to reflect the current navigation state in the UI, we can use `useNavigation`. When we call this hook, it returns an object with the `state` property. The state property can have 3 values: 'idle', 'loading' or 'submitting'. Now we can conditionally show some content if `navigation.state === 'loading'`. But this has to be rendered not in the page that will be "waiting", but in a component that is always visible, like the Root layout for example.

```js
// Root.js
import { Outlet, useNavigation } from 'react-router-dom';
import MainNavigation from '../components/MainNavigation';

export default function Root() {
  const navigation = useNavigation();

  return (
    <>
      <MainNavigation />
      <main>
        {navigation.state === 'loading' && <p>Loading...</p>}
        <Outlet />
      </main>
    </>
  );
}
```

### defer

`defer` is a function imported from react-router-dom. It is used when using loader or action functions, and allows us to show some content of the page before this functions are compoleted.

```js
import { Suspense } from 'react';
import { useLoaderData, json, defer, Await } from 'react-router-dom';

import EventsList from '../components/EventsList';

function EventsPage() {
  const { events } = useLoaderData();

  return (
    <Suspense fallback={<p style={{ textAlign: 'center' }}>Loading...</p>}>
      <Await resolve={events}>
        {(loadedEvents) => <EventsList events={loadedEvents} />}
      </Await>
    </Suspense>
  );
}

export default EventsPage;

async function loadEvents() {
  const response = await fetch('http://localhost:8080/events');

  if (!response.ok) {
    throw json({ message: 'Could not fetch events.' }, { status: 500 });
  } else {
    const resData = await response.json();
    return resData.events;
  }
}

export function loader() {
  return defer({
    events: loadEvents(),
  });
}
```

We can also use defer with 2 loaders, so that they begin to appear on the screen as soon as they are completed.

Here, besides the EventItem component, we are also outputting the EventList component, and they have independent defer properties:

```js
//EventDetail.js
import { Suspense } from 'react';
import {
  json,
  useRouteLoaderData,
  redirect,
  defer,
  Await,
} from 'react-router-dom';
import EventItem from '../components/EventItem';
import EventsList from '../components/EventsList';

export default function EventDetail() {
  const { event, events } = useRouteLoaderData('event-detail');

  return (
    <>
      <Suspense fallback={<p style={{ textAlign: 'center' }}>Loading...</p>}>
        <Await resolve={event}>
          {(loadedEvent) => <EventItem event={loadedEvent} />}
        </Await>
      </Suspense>
      <Suspense fallback={<p style={{ textAlign: 'center' }}>Loading...</p>}>
        <Await resolve={events}>
          {(loadedEvents) => <EventsList events={loadedEvents} />}
        </Await>
      </Suspense>
    </>
  );
}

async function loadEvent(id) {
  const response = await fetch('http://localhost:8080/events/' + id);

  if (!response.ok) {
    throw json(
      { message: 'Could now fetch details for selected event.' },
      { status: 500 }
    );
  } else {
    const resData = await response.json();
    return resData.event;
  }
}

async function loadEvents() {
  const response = await fetch('http://localhost:8080/events');

  if (!response.ok) {
    // return { isError: true, message: 'Could not fetch events.' };
    throw json({ message: 'Could not fetch events.' }, { status: 500 });
  } else {
    const resData = await response.json();
    return resData.events;
  }
}

export async function loader({ request, params }) {
  //request.url can access query parameters, for example;
  const id = params.eventId;

  return defer({
    event: await loadEvent(id),
    events: loadEvents(),
  });
}

export async function action({ params, request }) {
  const eventId = params.eventId;

  const response = await fetch('http://localhost:8080/events' + eventId, {
    // method: 'DELETE' // by hardcoding like this, you don't need to pass the method as an argument of useSubmit() (in EventItem.js)
    method: request.method,
  });

  if (!response.ok) {
    throw json({ message: 'Could not delete event.' }, { status: 500 });
  }
  return redirect('/events');
}
```

## React Router 6.4 (2): authentication issues

_repo react-max-21-auth-NEW, lecture 340 on_

### usesearchParams: query strings

Query strings (also called search params) are not declared on our routes definitions. Instead, they allow us to add information to our paths, that can be retrieved on the elements to change the content that will be output.

`useSeachParams()` returns an array with 2 variables: `searchParams` and `setSearchParams`. It can be used just as an useState, with the difference that it will change the browser's url.

For example, we can conditionally change the behavior of this Link, depending on the search params:

```js
import { Form, Link, useSearchParams } from 'react-router-dom';

import classes from './AuthForm.module.css';

function AuthForm() {
  const [searchParams] = useSearchParams();
  const isLogin = searchParams.get('mode') === 'login';

  return (
    <>
      <Form method='post' className={classes.form}>
        <h1>{isLogin ? 'Log in' : 'Create a new user'}</h1>
        <p>
          <label htmlFor='email'>Email</label>
          <input id='email' type='email' name='email' required />
        </p>
        <p>
          <label htmlFor='image'>Password</label>
          <input id='password' type='password' name='password' required />
        </p>
        <div className={classes.actions}>
          <Link to={`?mode=${isLogin ? 'signup' : 'login'}`}>
            {isLogin ? 'Create new user' : 'Login'}
          </Link>
          <button>Save</button>
        </div>
      </Form>
    </>
  );
}

export default AuthForm;
```

### Sending a POST request (signup)

1. Export an action function (can be exported from any file, here we're exporting from Authentication.js, which is the page that already has the element for the action's path).
2. Import that action into App.js, give it an alias and define it as an action in the '/auth' route.

Now, the action will be called when the Form is submitted. Form components automatically trigger the action of their own route. In he action function, the 'mode' (login or signup) will be extracted from the `request.url`. We create a `new URL` object from request.url, this object contains a property called `searchParams`, we can call `searchParams.get('mode')` to extract the ?mode= query string from that.

```js
//Authentication.js
import { json, redirect } from 'react-router-dom';
import AuthForm from '../components/AuthForm';

function AuthenticationPage() {
  return <AuthForm />;
}

export default AuthenticationPage;

export async function action({ request }) {
  //extracting query string to get the mode (this is important for THIS server):
  const searchParams = new URL(request.url).searchParams;
  const mode = searchParams.get('mode') || 'login';
  console.log(new URL(request.url));

  //handling situation where the user changed the query string ?mode=somethingElse:
  if (mode !== 'login' && mode !== 'signup') {
    throw json({ message: 'Unsupported mode.' }, { status: 422 });
  }

  const data = await request.formData();
  const authData = {
    email: data.get('email'),
    password: data.get('password'),
  };

  const response = await fetch('http://localhost:8080/' + mode, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(authData),
  });

  //handling response errors:
  if (response.status === 422 || response.status === 401) {
    return response;
  }
  if (!response.ok) {
    throw json({ message: 'Could not authenticate user.' }, { status: 500 });
  }

  //manage the token (soon)

  return redirect('/');
}

//App.js
etc etc
import AuthenticationPage, {
  action as authAction,
} from './pages/Authentication';
etc
{
  path: 'auth',
  element: <AuthenticationPage />,
  action: authAction,
},
etc etc
```

### Giving user feedback about authentication process (and outputting errors)

We can use the useActionData hook to get the response of the action. Here, our action will only give a response if there was an error; otherwise it will simply redirect the user to '/'.

To give feedback about the submission process, we will use useNavigation to conditionally disable the button and change its text from 'Save' to 'Submitting...' when navigation.state === 'submitting'.

```js
import {
  Form,
  Link,
  useSearchParams,
  useActionData,
  useNavigation, //here
} from 'react-router-dom';

import classes from './AuthForm.module.css';

function AuthForm() {
  const data = useActionData(); //here
  const navigation = useNavigation(); //here

  const [searchParams] = useSearchParams();
  const isLogin = searchParams.get('mode') === 'login';
  const isSubmitting = navigation.state === 'submitting'; //here

  return (
    <>
      <Form method='post' className={classes.form}>
        <h1>{isLogin ? 'Log in' : 'Create a new user'}</h1>
        {data && data.errors && (
          <ul>
            {Object.values(data.errors).map((err) => (
              <li key={err}>{err}</li>
            ))}
          </ul>
        )} //here
        {data && data.message && <p>{data.message}</p>} //here
        <p>
          <label htmlFor='email'>Email</label>
          <input id='email' type='email' name='email' required />
        </p>
        <p>
          <label htmlFor='image'>Password</label>
          <input id='password' type='password' name='password' required />
        </p>
        <div className={classes.actions}>
          <Link to={`?mode=${isLogin ? 'signup' : 'login'}`}>
            {isLogin ? 'Create new user' : 'Login'}
          </Link>
          <button disabled={isSubmitting}>
            {isSubmitting ? 'Submitting...' : 'Save'}
          </button> //here
        </div>
      </Form>
    </>
  );
}

export default AuthForm;
```

### Attaching Auth Tokens to outgoing requests

In the action that sends POST request to login/signup, we can store the token property of the response object (returned by the .fetch function) in localStorage.

Here, we created a helper function getAuthToken.js exported from /util/auth.js to get that token from the localStorage whenever it's necessary.

Now, in every action that needs to add an 'Authorization' header to be completed (this depends on our server settings), we can get the token from localStorage and pass it.

```js
// EventDetail.js
import { getAuthToken } from '../util/auth'; //here
etc etc
//this action deletes or updates the item:
export async function action({ params, request }) {
  const eventId = params.eventId;
  const token = getAuthToken(); //getting the token from localStorage using our helper function

  const response = await fetch('http://localhost:8080/events/' + eventId, {
    method: request.method,
    headers: {
      Authorization: 'Bearer ' + token, //attaching the token in the headers
    },
  });

  if (!response.ok) {
    throw json(
      { message: 'Could not delete event.' },
      {
        status: 500,
      }
    );
  }
  return redirect('/events');
}

// util/auth.js
export function getAuthToken() {
  const token = localStorage.getItem('token');
  return token;
}

```

### Implementing Logout

There are several ways of doing that. We can simply add a button on the MainNavigation with an onClick prop, create a function clickHandler inside this component that remove item 'token' from the localStorage and redirect the user to '/'.

Here we are going to do a more React Router specific way. We add a Form component wrapping that button on MainNavigation, with action='/logout' and method='post' (though it doesn't matter which method). Then export an action from new file /pages/Logout.js that removes item 'token' from localStorage and redirects the user to '/'. Then add a route with path: 'logout' and action: logoutAction, also import { action as logoutAction } from './pages/Logout' and that's it.

```js
//components/MainNavigation.js
etc
<li>
  <Form action='logout' method='post'>
    <button>Logout</button>
  </Form>
</li>
etc

//pages/Logout.js
import { redirect } from 'react-router-dom';

export function action() {
  localStorage.removeItem('token');
  return redirect('/');
}

//App.js
import { action as logoutAction } from './pages/Logout';
etc
{
  path: 'logout',
  action: logoutAction,
},
etc
```

### Conditionally updating UI depending on auth status (using loaders)

We could create an auth Context for that, but instead of that we will use loaders.

Just add a loader to the root route '/', that loader will only get the token from the localStorage. This loader will run whenever the root is reached, this happens when we login and also when we logout, so the returned value from the loader will always be up-to-date. We can use the returned value from that loader in any component, we just have to define an id: 'root' to the root route, and then call useRouteLoaderData('root') to get the returned value from that route's loader.

With this data, we can conditionally update our UI, depending on the user auth status.

```js
//util/auth.js
export function getAuthToken() {
  const token = localStorage.getItem('token');
  return token;
}

export function tokenLoader() {
  return getAuthToken();
} //this will simply get the token from localStorage

//App.js
etc
{
    path: '/',
    element: <RootLayout />,
    errorElement: <ErrorPage />,
    id: 'root', //add an id to be able to call useRouteLoaderData('root')
    loader: tokenLoader, //add the loader
    children: [ etc ]
}

//EventItem.js
import { Link, useSubmit, useRouteLoaderData } from 'react-router-dom';

import classes from './EventItem.module.css';

function EventItem({ event }) {
  const token = useRouteLoaderData('root'); //checking if there is a token
  const submit = useSubmit();

  function startDeleteHandler() {
    const proceed = window.confirm('Are you sure?');

    if (proceed) {
      submit(null, { method: 'delete' });
    }
  }

  return (
    <article className={classes.event}>
      <img src={event.image} alt={event.title} />
      <h1>{event.title}</h1>
      <time>{event.date}</time>
      <p>{event.description}</p>
      {token && (
        <menu className={classes.actions}>
          <Link to='edit'>Edit</Link>
          <button onClick={startDeleteHandler}>Delete</button>
        </menu>
      )} //conditionally updating UI
    </article>
  );
}

export default EventItem;
```

### Protecting routes (using loaders!)

We can protect our routes, and redirect the user if she's not logged in, using loaders. Loaders will be called before the user reaches the target element, so we can, inside the loader, check if the user has a token. If he hasn't, we simply redirect him to '/auth'; if he has, we return null and then he'll reach the target path. We can also return the token, so that we'll be able to use it inside that route by using useLoaderData. Instead of returning null, we could also return the token, then it would be available on that route if we use `useLoaderData()` inside of it.

```js
//util/auth.js
import { redirect } from 'react-router-dom';

export function getAuthToken() {
  const token = localStorage.getItem('token');
  return token;
}

export function tokenLoader() {
  return getAuthToken();
}

export function checkAuthLoader() {
  const token = getAuthToken();

  if (!token) {
    return redirect('/auth');
  }
  return null;
}

//App.js
import { checkAuthLoader } from './util/auth';
etc
{
  path: 'new',
  element: <NewEventPage />,
  action: manipulateEventAction,
  loader: checkAuthLoader,
},
etc
```

### Setting and managing token's expiration

To automatically logout users after a period of time (1h for example), we can set an expiration item in localStorage.

1. In Authentication.js, we have the action that logs in/ signs up. There, we must also store an expiration time inside localStorage.
2. In util/auth.js, we must create a helper function to get the expiration data, compare to now, and return the remaining duration of the token. We can also incorporate this function to getAuthToken, so that whenever we check if there is a token, we only return truthy if the token is still valid.
3. In our Root.js component, which is a component that loads for every path (because all paths in this projects are children of '/'), we set a useEffect to verify the token validity. This useEffect also handles an 'EXPIRED' value for the token, which is returned if the duration of the token is less than 0, by programmatically triggering the action 'logout'.

```js
// Authentication.js
etc;
//manage the token
const resData = await response.json();
const token = resData.token;
localStorage.setItem('token', token);
//token's expiration date
const expiration = new Date(); //now
expiration.setHours(expiration.getHours() + 1); //now + 1h
localStorage.setItem('expiration', expiration.toISOString());

return redirect('/');
etc;

//util/auth.js
etc;
export function getTokenDuration() {
  const storedExpirationDate = localStorage.getItem('expiration');
  const expirationDate = new Date(storedExpirationDate); //create a date object from that string
  const now = new Date(); //now
  const duration = expirationDate.getTime() - now.getTime(); //converts the date to timestamp and substract now
  //if the duration is a positive number, the token is still valid for that quantity of miliseconds
  return duration;
}

export function getAuthToken() {
  const token = localStorage.getItem('token');
  const tokenDuration = getTokenDuration();

  if (!token) {
    return null;
  }

  if (tokenDuration < 0) {
    return 'EXPIRED';
  }

  return token;
}
etc;

//Root.js
import { useEffect } from 'react';
import { Outlet, useSubmit, useLoaderData } from 'react-router-dom';

import MainNavigation from '../components/MainNavigation';
import { getTokenDuration } from '../util/auth';

function RootLayout() {
  const token = useLoaderData(); //getting the token
  const submit = useSubmit();

  useEffect(() => {
    if (!token) {
      return;
    }

    if (token === 'EXPIRED') {
      submit(null, { action: '/logout', method: 'post' });
      return;
    } //handling token value 'EXPIRED'

    const tokenDuration = getTokenDuration();

    const timeout = setTimeout(() => {
      submit(null, { action: '/logout', method: 'post' });
    }, tokenDuration); //setting a timeout in case the user stays on the same page for a long time

    return () => clearInterval(timeout);
  }, [token, submit]);

  return (
    <>
      <MainNavigation />
      <main>
        <Outlet />
      </main>
    </>
  );
}

export default RootLayout;
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
    <div className='navbar' style={{ background: context.color }}>
      <nav>
        <Link to='/' className='brand'>
          <h1>Cooking Ninja</h1>
        </Link>
        <SearchBar />
        <Link to='/create'>Create Recipe</Link>
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
    <div className='navbar' style={{ background: color }}>
      <nav>
        <Link to='/' className='brand'>
          <h1>Cooking Ninja</h1>
        </Link>
        <SearchBar />
        <Link to='/create'>Create Recipe</Link>
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
    <div className='navbar' style={{ background: color }}>
      <nav onClick={() => changeColor('pink')}>
        <Link to='/' className='brand'>
          <h1>Cooking Ninja</h1>
        </Link>
        <SearchBar />
        <Link to='/create'>Create Recipe</Link>
      </nav>
    </div>
  );
}
```

### Forwarding refs with useImperativeHandler and forwardRef

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

### React.memo(), useCallback() & useMemo()

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
    <div className='app'>
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
    <div className='home'>
      {error && <p className='error'>{error}</p>}
      {isPending && <p className='loading'>Loading...</p>}
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
      {error && <p className='error'>{error}</p>}
      {isPending && <p className='loading'>Loading...</p>}
      {recipe && (
        <>
          <h2 className='page-title'>{recipe.title}</h2>
          <p>Takes {recipe.cookingTime} to cook.</p>
          <ul>
            {recipe.ingredients.map((ing) => (
              <li key={ing}>{ing}</li>
            ))}
          </ul>
          <p className='method'>{recipe.method}</p>
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

_repo react-finance-tracker, NN section 14_

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
          type='email'
          onChange={(e) => setEmail(e.target.value)}
          value={email}
        />
      </label>
      <label>
        <span>password:</span>
        <input
          type='password'
          onChange={(e) => setPassword(e.target.value)}
          value={password}
        />
      </label>
      <label>
        <span>display name:</span>
        <input
          type='text'
          onChange={(e) => setDisplayName(e.target.value)}
          value={displayName}
        />
      </label>
      {!isPending && <button className='btn'>Signup</button>}
      {isPending && (
        <button className='btn' disabled>
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
          <Link to='/'>myMoney</Link>
        </li>
        <li>
          <Link to='/login'>Login</Link>
        </li>
        <li>
          <Link to='/signup'>Signup</Link>
        </li>
        <button className='btn' onClick={logout}>
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
          type='email'
          onChange={(e) => setEmail(e.target.value)}
          value={email}
        />
      </label>
      <label>
        <span>password:</span>
        <input
          type='password'
          onChange={(e) => setPassword(e.target.value)}
          value={password}
        />
      </label>
      {!isPending && <button className='btn'>Login</button>}
      {isPending && (
        <button className='btn' disabled>
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
          <Link to='/'>myMoney</Link>
        </li>

        {!user && (
          <>
            <li>
              <Link to='/login'>Login</Link>
            </li>
            <li>
              <Link to='/signup'>Signup</Link>
            </li>
          </>
        )}

        {user && (
          <>
            <li>hello, {user.displayName}</li>
            <li>
              <button className='btn' onClick={logout}>
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
    <div className='App'>
      {authIsReady && (
        <BrowserRouter>
          <Navbar />
          <Switch>
            <Route exact path='/'>
              {!user && <Redirect to='/login' />}
              {user && <Home />}
            </Route>
            <Route path='/login'>
              {user && <Redirect to='/' />}
              {!user && <Login />}
            </Route>
            <Route path='/signup'>
              {user && <Redirect to='/' />}
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
    <div className='App'>
      {authIsReady && (
        <BrowserRouter>
          <Navbar />
          <Routes>
            <Route
              path='/'
              element={user ? <Home /> : <Navigate to='/login' />}
            />
            <Route
              path='/login'
              element={!user ? <Login /> : <Navigate to='/' />}
            />
            <Route
              path='/signup'
              element={!user ? <Signup /> : <Navigate to='/' />}
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

## Firebase Auth REST Api

_Max section 22, repo react-max-22-auth_

Firebase Auth REST Api Docs: https://firebase.google.com/docs/reference/rest/auth

Besides using the methods contained in `firebase.auth()`, such as `.createUserWithEmailAndPassword(email, password)` and `.signInWithEmailAndPassword(email, password)`, we can also do such things by sending POST and GET requests to the REST Api.

`firebase.auth()` gives access to what we call `Firebase Auth SDK`. SDK stands for software development kit, also known as "devkit".

### Signup

The `returnSecureToken` property is important to get a tokenID (JSON web token) as part of the response. We will store the token in our auth-context to be able to More on JSON web token: https://jwt.io/introduction

The request should have this structure:

```js
fetch(
  `https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=${API_key}`, //this api-key is not a secret, but better keep it quiet.
  {
    method: 'POST',
    body: JSON.stringify({
      email: enteredEmail,
      password: enteredPassword,
      returnSecureToken: true, //always true
    }),
    headers: {
      'Content-Type': 'application/json',
    },
  }
).then((res) => {
  if (res.ok) {
    // redirect etc.
  } else {
    return res.json().then((data) => {
      //show an error modal etc.
      console.log(data);
    });
  }
});
```

### Signup and login (shared page)

It is convenient to write a single page either to signup or to login - we just have to add a button to change between these two logics.

```js
//AuthForm.js
import { useState, useRef, useContext } from 'react';
import { useHistory } from 'react-router-dom';

import AuthContext from '../../store/auth-context';
import API_key from '../../env';

import classes from './AuthForm.module.css';

const AuthForm = () => {
  const history = useHistory();
  const emailInputRef = useRef();
  const passwordInputRef = useRef();

  const authCtx = useContext(AuthContext);

  const [isLogin, setIsLogin] = useState(true);
  const [isLoading, setIsLoading] = useState(false);

  const switchAuthModeHandler = () => {
    setIsLogin((prevState) => !prevState);
  };

  const submitHandler = (event) => {
    event.preventDefault();

    const enteredEmail = emailInputRef.current.value;
    const enteredPassword = passwordInputRef.current.value;

    //optional: validation

    setIsLoading(true);
    let url;
    if (isLogin) {
      url = `https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key=${API_key}`;
    } else {
      url = `https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=
        ${API_key}`;
    } //just change the url depending on if login or signup, the other parameters of the fetch request are the same!

    fetch(url, {
      method: 'POST',
      body: JSON.stringify({
        email: enteredEmail,
        password: enteredPassword,
        returnSecureToken: true,
      }),
      headers: {
        'Content-Type': 'application/json',
      },
    })
      .then((res) => {
        setIsLoading(false);
        if (res.ok) {
          return res.json();
        } else { //handling errors
          return res.json().then((data) => {
            //show an error modal
            let errorMessage = 'Authentication failed!';
            if (data && data.error && data.error.message) {
              errorMessage = data.error.message;
            }

            throw new Error(errorMessage);
          });
        }
      })
      .then((data) => {
        authCtx.login(data.idToken); //storing the idToken for future requests (that might be protected, like change password or post content, for example)
        history.replace('/'); //redirecting
      })
      .catch((err) => {
        alert(err.message); //error feedback
      });
  };

  return (
    <section className={classes.auth}>
      <h1>{isLogin ? 'Login' : 'Sign Up'}</h1>
      <form onSubmit={submitHandler}>
        <div className={classes.control}>
          <label htmlFor="email">Your Email</label>
          <input type="email" id="email" ref={emailInputRef} required />
        </div>
        <div className={classes.control}>
          <label htmlFor="password">Your Password</label>
          <input
            type="password"
            id="password"
            ref={passwordInputRef}
            required
          />
        </div>
        <div className={classes.actions}>
          {!isLoading && (
            <button>{isLogin ? 'Login' : 'Create Account'}</button>
          )}
          {isLoading && <p>Sending request...</p>}
          <button
            type="button"
            className={classes.toggle}
            onClick={switchAuthModeHandler}
          >
            {isLogin ? 'Create new account' : 'Login with existing account'}
          </button>
        </div>
      </form>
    </section>
  );
};

export default AuthForm;

//auth-context.js
import React, { useState } from 'react';

const AuthContext = React.createContext({
  token: '',
  isLoggedIn: false,
  login: (token) => {},
  logout: () => {},
});

export const AuthContextProvider = (props) => {
  const [token, setToken] = useState(null);

  const userIsLoggedIn = !!token; //if token is truthy, it'll return true; if token is falsy, it'll return false.

  const loginHandler = (token) => {
    setToken(token);
  };

  const logoutHandler = () => {
    setToken(null);
  };

  const contextValue = {
    token: token,
    isLoggedIn: userIsLoggedIn,
    login: loginHandler,
    logout: logoutHandler,
  };

  return ( //this component will wrap the entire app in index.js (or App.js)
    <AuthContext.Provider value={contextValue}>
      {props.children}
    </AuthContext.Provider>
  );
};

export default AuthContext;

```

### Logout

Just tap authCtx and call the logout function that we've created (see auth-context.js above).

```js
import { useContext } from 'react';
import { Link } from 'react-router-dom';
import AuthContext from '../../store/auth-context';

import classes from './MainNavigation.module.css';

const MainNavigation = () => {
  const authCtx = useContext(AuthContext);

  const logoutHandler = () => {
    authCtx.logout();
    //optional: redirect user to '/'
  };

  return (
    <header className={classes.header}>
      <Link to='/'>
        <div className={classes.logo}>React Auth</div>
      </Link>
      <nav>
        <ul>
          {!authCtx.isLoggedIn && (
            <li>
              <Link to='/auth'>Login</Link>
            </li>
          )}
          {authCtx.isLoggedIn && (
            <li>
              <Link to='/profile'>Profile</Link>
            </li>
          )}
          {authCtx.isLoggedIn && (
            <li>
              <button onClick={logoutHandler}>Logout</button>
            </li>
          )}
        </ul>
      </nav>
    </header>
  );
};

export default MainNavigation;
```

### Protecting routes

It's a good practice to protect routes if user isn`t logged in, so that a malevolous user can't access them via url. This is made simply by adding conditionals to the Router in App.js.

```js
import { useContext } from 'react';
import { Switch, Route, Redirect } from 'react-router-dom';

import Layout from './components/Layout/Layout';
import UserProfile from './components/Profile/UserProfile';
import AuthPage from './pages/AuthPage';
import HomePage from './pages/HomePage';
import AuthContext from './store/auth-context';

function App() {
  const authCtx = useContext(AuthContext);

  return (
    <Layout>
      <Switch>
        <Route path='/' exact>
          <HomePage />
        </Route>
        {!authCtx.isLoggedIn && (
          <Route path='/auth'>
            <AuthPage />
          </Route>
        )}
        {authCtx.isLoggedIn && (
          <Route path='/profile'>
            <UserProfile />
          </Route>
        )}
        <Route path='*'>
          <Redirect to='/' />
        </Route>
      </Switch>
    </Layout>
  );
}

export default App;
```

### Persisting the user authentication status

To persist auth status data even if the user leaves/refreshes the page, we can use **localStorage** or **cookies**.

```js
import React, { useState } from 'react';

const AuthContext = React.createContext({
  token: '',
  isLoggedIn: false,
  login: (token) => {},
  logout: () => {},
});

export const AuthContextProvider = (props) => {
  const initialToken = localStorage.getItem('token'); //we don't need to use useEffect here; localStorage is a synchronous API, so it won't cause a loop.
  const [token, setToken] = useState(initialToken); //the initial value of the token state is the token that is stored in localStorage (this can be undefined too)

  const userIsLoggedIn = !!token;

  const loginHandler = (token) => {
    setToken(token);
    localStorage.setItem('token', token);
  };

  const logoutHandler = () => {
    setToken(null);
    localStorage.removeItem('token');
  };

  const contextValue = {
    token: token,
    isLoggedIn: userIsLoggedIn,
    login: loginHandler,
    logout: logoutHandler,
  };

  return (
    <AuthContext.Provider value={contextValue}>
      {props.children}
    </AuthContext.Provider>
  );
};

export default AuthContext;
```

### Auto-logout (setting an expiration time for the token)

It is very useful to automatically logout the user after a certain period of time after login/signup.
Most APIs (Firebase Auth REST Api included) include an expiresIn property in the response for a POST request.
Here we will use the default value of that property to logout the user after that time. By default, Firebase REST Api will return this expiresIn property in seconds, so we have to convert it to miliseconds and compare it to the current time using a helper function, calculateRemainingTime(expirationTime).

```js
//AuthForm.js
import { useState, useRef, useContext } from 'react';
import { useHistory } from 'react-router-dom';

import AuthContext from '../../store/auth-context';
import API_key from '../../env';

import classes from './AuthForm.module.css';

const AuthForm = () => {
  const history = useHistory();
  const emailInputRef = useRef();
  const passwordInputRef = useRef();

  const authCtx = useContext(AuthContext);

  const [isLogin, setIsLogin] = useState(true);
  const [isLoading, setIsLoading] = useState(false);

  const switchAuthModeHandler = () => {
    setIsLogin((prevState) => !prevState);
  };

  const submitHandler = (event) => {
    event.preventDefault();

    const enteredEmail = emailInputRef.current.value;
    const enteredPassword = passwordInputRef.current.value;

    //optional: validation

    setIsLoading(true);
    let url;
    if (isLogin) {
      url = `https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key=${API_key}`;
    } else {
      url = `https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=
        ${API_key}`;
    }

    fetch(url, {
      method: 'POST',
      body: JSON.stringify({
        email: enteredEmail,
        password: enteredPassword,
        returnSecureToken: true,
      }),
      headers: {
        'Content-Type': 'application/json',
      },
    })
      .then((res) => {
        setIsLoading(false);
        if (res.ok) {
          return res.json();
        } else {
          return res.json().then((data) => {
            //show an error modal
            let errorMessage = 'Authentication failed!';
            if (data && data.error && data.error.message) {
              errorMessage = data.error.message;
            }

            throw new Error(errorMessage);
          });
        }
      })
      .then((data) => {
        const expirationTime = new Date(
          new Date().getTime() + +data.expiresIn * 1000
        ); //creating the expirationTime (must convert expiresIn (which is in seconds) to miliseconds)

        authCtx.login(data.idToken, expirationTime.toISOString()); //pass the expirationTime as a string (because we are already creating a Date object in the login function in auth-context.js)
        history.replace('/');
      })
      .catch((err) => {
        alert(err.message);
      });
  };

  return (
    <section className={classes.auth}>
      <h1>{isLogin ? 'Login' : 'Sign Up'}</h1>
      <form onSubmit={submitHandler}>
        <div className={classes.control}>
          <label htmlFor="email">Your Email</label>
          <input type="email" id="email" ref={emailInputRef} required />
        </div>
        <div className={classes.control}>
          <label htmlFor="password">Your Password</label>
          <input
            type="password"
            id="password"
            ref={passwordInputRef}
            required
          />
        </div>
        <div className={classes.actions}>
          {!isLoading && (
            <button>{isLogin ? 'Login' : 'Create Account'}</button>
          )}
          {isLoading && <p>Sending request...</p>}
          <button
            type="button"
            className={classes.toggle}
            onClick={switchAuthModeHandler}
          >
            {isLogin ? 'Create new account' : 'Login with existing account'}
          </button>
        </div>
      </form>
    </section>
  );
};

export default AuthForm;

//auth-context.js
import React, { useState } from 'react';

const AuthContext = React.createContext({
  token: '',
  isLoggedIn: false,
  login: (token) => {},
  logout: () => {},
});

//helper function to calculate remaining time using stored expirationTime and current time
const calculateRemainingTime = (expirationTime) => {
  const currentTime = new Date().getTime(); //get the current timestamp in miliseconds
  const adjustedExpTime = new Date(expirationTime).getTime(); //convert expirationTime to a timestamp (ms)

  const remainingDuration = adjustedExpTime - currentTime;

  return remainingDuration;
};

export const AuthContextProvider = (props) => {
  const initialToken = localStorage.getItem('token'); //we don't need to use useEffect here; localStorage is a synchronous API, so it won't cause a loop.
  const [token, setToken] = useState(initialToken); //the initial value of the token state is the token that is stored in localStorage (this can be undefined too)

  const userIsLoggedIn = !!token; //if token is truthy, it'll return true; if token is falsy, it'll return false.

  const logoutHandler = () => {
    setToken(null);
    localStorage.removeItem('token');
  };

  const loginHandler = (token, expirationTime) => {
    setToken(token);
    localStorage.setItem('token', token);

    const remainingTime = calculateRemainingTime(expirationTime);

    setTimeout(logoutHandler, remainingTime);
  };

  const contextValue = {
    token: token,
    isLoggedIn: userIsLoggedIn,
    login: loginHandler,
    logout: logoutHandler,
  };

  return (
    <AuthContext.Provider value={contextValue}>
      {props.children}
    </AuthContext.Provider>
  );
};

export default AuthContext;

```

These two snippets from AuthForm.js and auth-context.js ...

```js
const expirationTime = new Date(new Date().getTime() + +data.expiresIn * 1000);
authCtx.login(data.idToken, expirationTime.toISOString());

const calculateRemainingTime = (expirationTime) => {
  const currentTime = new Date().getTime();
  const adjExpirationTime = new Date(expirationTime).getTime();
  const remainingDuration = adjExpirationTime - currentTime;
  return remainingDuration;
};
```

... can be replaced with these two (easy-to-read) lines:

```js
authCtx.login(data.idToken, Date.now() + data.expiresIn * 1000);

const calculateRemainingTime = (expirationTime) => expirationTime - Date.now();
```

`Date.now()` does the same as `new Date().getTime()`. It's even a little more precise (which is not relevant here), since it captures the time immediately, not only after creating a Date object first.

### Auto-login (if the token is still valid)

When the user refreshed the page, if the token is still valid, then he will be automatically logged in. This is done by storing the token and the expirationTime in localStorage, then checking if this token is still valid when the user comes back to the page.

```js
//auth-context.js
import React, { useState, useEffect, useCallback } from 'react';

let logoutTimer;

const AuthContext = React.createContext({
  token: '',
  isLoggedIn: false,
  login: (token) => {},
  logout: () => {},
});

const calculateRemainingTime = (expirationTime) => {
  const currentTime = new Date().getTime(); //get the current timestamp in miliseconds
  const adjustedExpTime = new Date(expirationTime).getTime(); //convert expirationTime to miliseconds

  const remainingDuration = adjustedExpTime - currentTime;

  return remainingDuration;
};

//helper function to check if the idToken is still valid:
const retrieveStoredToken = () => {
  const storedToken = localStorage.getItem('token');
  const storedExpirationDate = localStorage.getItem('expirationTime');

  const remainingTime = calculateRemainingTime(storedExpirationDate);

  if (remainingTime <= 6000) {
    localStorage.removeItem('token');
    localStorage.removeItem('expirationTime');
    return null;
  }

  return {
    token: storedToken,
    duration: remainingTime,
  };
};

export const AuthContextProvider = (props) => {
  const tokenData = retrieveStoredToken();
  let initialToken;
  if (tokenData) {
    initialToken = tokenData.token;
  }
  const [token, setToken] = useState(initialToken); //the initial value of the token state is the token that is stored in localStorage (this can be undefined if it's not valid anymore)

  const userIsLoggedIn = !!token;

  const logoutHandler = useCallback(() => {
    setToken(null);
    localStorage.removeItem('token');
    localStorage.removeItem('expirationTime');
    if (logoutTimer) {
      clearTimeout(logoutTimer);
    }
  }, []); //use useCallback to assure that this function will not cause a infinite loop when added as a dependency to useEffect (below)

  const loginHandler = (token, expirationTime) => {
    setToken(token);
    localStorage.setItem('token', token);
    localStorage.setItem('expirationTime', expirationTime);

    const remainingTime = calculateRemainingTime(expirationTime);

    logoutTimer = setTimeout(logoutHandler, remainingTime);
  };

  useEffect(() => {
    if (tokenData) {
      // console.log(tokenData.duration);
      logoutTimer = setTimeout(logoutHandler, tokenData.duration);
    }
  }, [tokenData, logoutHandler]); //using useEffect to get the stored token and expirationTime from the localStorage and recalculating remainingTime, every time the user is automatically logged in

  const contextValue = {
    token: token,
    isLoggedIn: userIsLoggedIn,
    login: loginHandler,
    logout: logoutHandler,
  };

  return (
    <AuthContext.Provider value={contextValue}>
      {props.children}
    </AuthContext.Provider>
  );
};

export default AuthContext;
```

#### IMPORTANT! Protect user's idToken from XSS attacks.

- About XSS attacks: https://academind.com/tutorials/xss-cross-site-scripting-attacks
- Neither localStorage, or session cookies, or http-only cookies are all subject to XSS atacks: https://academind.com/tutorials/localstorage-vs-cookies-xss
- Sanitize package (JS/Node.js) to efficiently prevent XSS injection before storing it in the server: https://www.npmjs.com/package/sanitize-html

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
   (NN lecture 144)

```js
//firebase.rules

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /transactions/{document=**} { //match any document inside the transactions collection
      // allow read, write;
      // allow read, create, delete, update;

      allow create: if request.auth != null;
      // if there is an auth token, user can create a new document. request.auth is valid for all projects;

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

## Deploying

### Adding Lazy Loading

When not using Lazy Loading, once our app is loaded, all the pages and components are downloaded, even if they are inside of routes that haven't been called yet.

Use the **lazy** function and the **Suspense** component (from 'react'), we can download some components on demand.

In React-Router 6.4, we must remember to pass the `meta` object to the loader function whenever implementing lazy-loading to a dynamic path.

Other tools for optimizing the building process and, after that, the production, are **React.memo()** and **useCallback** [here](#reactmemo-usecallback--usememo).

EXAMPLE REACT-ROUTER 5
_repo react-max-21-deploy_

```js
import React, { Suspense } from 'react';
import { Route, Switch, Redirect } from 'react-router-dom';

import AllQuotes from './pages/AllQuotes';
import Layout from './components/layout/Layout';
import LoadingSpinner from './components/UI/LoadingSpinner';

//lazy components:
const NewQuote = React.lazy(() => import('./pages/NewQuote'));
const QuoteDetail = React.lazy(() => import('./pages/QuoteDetail'));
const NotFound = React.lazy(() => import('./pages/NotFound'));

function App() {
  return (
    <Layout>
      // Suspense to show a loading spinner component while loading -> adding
      Suspense is mandatory when using lazy loading!
      <Suspense
        fallback={
          <div className='centered'>
            <LoadingSpinner />
          </div>
        }
      >
        <Switch>
          <Route path='/' exact>
            <Redirect to='/quotes' />
          </Route>
          <Route path='/quotes' exact>
            <AllQuotes />
          </Route>
          <Route path='/quotes/:quoteId'>
            <QuoteDetail />
          </Route>
          <Route path='/new-quote'>
            <NewQuote />
          </Route>
          <Route path='*'>
            <NotFound />
          </Route>
        </Switch>
      </Suspense>
    </Layout>
  );
}

export default App;
```

EXAMPLE REACT-ROUTER 6.4
_repo react-max-22-deploy-NEW_

```js
import { lazy, Suspense } from 'react';
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

// import BlogPage, { loader as postsLoader } from './pages/Blog';
import HomePage from './pages/Home';
// import PostPage, { loader as postLoader } from './pages/Post';
import RootLayout from './pages/Root';

const BlogPage = lazy(() => import('./pages/Blog'));
const PostPage = lazy(() => import('./pages/Post'));

const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    children: [
      {
        index: true,
        element: <HomePage />,
      },
      {
        path: 'posts',
        children: [
          {
            index: true,
            element: (
              <Suspense fallback={<p>Loading...</p>}>
                <BlogPage />
              </Suspense>
            ),
            loader: () =>
              import('./pages/Blog').then((module) => module.loader()),
          },
          {
            path: ':id',
            element: (
              <Suspense fallback={<p>Loading...</p>}>
                <PostPage />
              </Suspense>
            ),
            loader: (meta) =>
              import('./pages/Post').then((module) => module.loader(meta)), //don't forget to pass the meta object with the params when implementing lazyloading in dynamic paths!
          },
        ],
      },
    ],
  },
]);

function App() {
  return <RouterProvider router={router} />;
}

export default App;
```

### Deploying a React App to Github Pages

The npm package `gh-pages` enables us to deploy an app (created by using create-react-app) directly to Github Pages through bash commands.

Just follow these steps (after creating a create-react-app app via VSCode and setting a git repo for it):

1. Install the `gh-pages` npm package

Install the gh-pages npm package and designate it as a development dependency:

```js
   $ npm install gh-pages --save-dev
```

At this point, the gh-pages npm package is installed on your computer and the React app's dependence upon it is documented in the React app's package.json file.

2. Add a homepage property to the package.json file

Open the package.json file in a text editor.

```js
    $ code package.json
```

Add a homepage property in this format: `https://{username}.github.io/{repo-name}`

For a project site, that's the format. For a user site, the format is: https://{username}.github.io. You can read more about the homepage property in the "GitHub Pages" section of the create-react-app documentation.

```js
    {
      "name": "my-app",
      "version": "0.1.0",
      "homepage": "https://gitname.github.io/react-gh-pages",
      "private": true,
      etc etc
    }
```

At this point, the React app's package.json file includes a property named `homepage`.

3. Add deployment scripts to the package.json file

Open the package.json file in a text editor (if it isn't already open in one). Add a `predeploy` property and a `deploy` property to the scripts object:

```js
    "scripts": {
    +   "predeploy": "npm run build",
    +   "deploy": "gh-pages -d build",
        "start": "react-scripts start",
        "build": "react-scripts build",
      etc etc
    }
```

4. Add a "remote" that points to the GitHub repository

Add a "remote" to the local Git repository. You can do that by issuing a command in this format: `git remote add origin https://github.com/{username}/{repo-name}.git`

5. Push the React app to the GitHub repository

`npm run deploy`

That will cause the `predeploy` and `deploy` scripts defined in package.json to run.

Under the hood, the predeploy script will build a distributable version of the React app and store it in a folder named build. Then, the deploy script will push the contents of that folder to a new commit on the gh-pages branch of the GitHub repository, creating that branch if it doesn't already exist.

By default, the new commit on the gh-pages branch will have a commit message of "Updates". You can specify a custom commit message via the -m option, like this:

`npm run deploy -- -m "Your custom commit message"`

6. Configure GitHub Pages

In your web browser, navigate to the GitHub repository. Above the code browser, click on the tab labeled "Settings". In the sidebar, in the "Code and automation" section, click on "Pages".
Configure the "Build and deployment" settings like this:

Source: Deploy from a branch
Branch: gh-pages
Folder: / (root)

Click on the "Save" button

That's it! The React app has been deployed to GitHub Pages! 🚀

7. (Optional) Store the React app's source code on GitHub

In a previous step, the gh-pages npm package pushed the distributable version of the React app to a branch named gh-pages in the GitHub repository. However, the source code of the React app is not yet stored on GitHub.

In this step, I'll show you how you can store the source code of the React app on GitHub.

Commit the changes you made while you were following this tutorial, to the master branch of the local Git repository; then, push that branch up to the master branch of the GitHub repository.

```js
    $ git add .
    $ git commit -m "Configure React app for deployment to GitHub Pages"
    $ git push origin master
```

I recommend exploring the GitHub repository at this point. It will have two branches: master and gh-pages. The `master branch` will contain the React app's source code, while the `gh-pages branch` will contain the distributable version of the React app.

## Next.js

_repo react-max-23-nextjs, lecture 318_

What is Next.js?

- "The React framework for production"
- A **fullstack** framework for React: that means, it builds up on React, and it makes building large-scale React apps easier.

### Next.js' key-features:

1. Server-side rendering

In React apps, typically, the page is loaded on the fly: when a user clicks to open a post, for example, the data is loaded and the page is "mounted" on the fly. The Search Engine Crawlers are unable to be aware of our page's content.

Server-side rendering allows us to pre-render React pages and components on a server. thus solving the above mentioned problem. Another advantage of server-side rendering is that it avoids flickering and "loading..." states as we first load a page.

More about the difference between plain React and Next.js regarding SSR: https://www.imaginarycloud.com/blog/next-js-vs-react/

2. File-based Routing

With React Router, we define routes by using code. With Next.js, we define pages and routes with files and folders instead of code.

This implies less code, less work and highly understandable and readable structure.

3. Fullstack capabilities

- easily add backend (server-side) code to your apps
- storing data, getting data, authentication etc. can be added to your React projects

### Creating a Next.js app

`npx create-next-app`

This will give us a structure containing the folders node_modules, styles, public, pages, etc. The most important folder in a Next.js app is `pages`.

The pages folder contains an index.js file. This file exports a React component that will be rendered whenever the user reaches `our-domain.com/` path.

A file named `news.js` in the pages folder will be rendered whenever the user reaches `our-domain.com/news` path.

A folder created inside of the pages folder also will have its index.js rendered whenever the user reaches `our-domain.com/foldername` path. This approach is important if we want to create nested routes, like `our-domain.com/news/sport` for example.

This is how the routing works in a Next.js app.

### Dynamic pages (with parameters)

To create dynamic pages, that serve like a template to render different content, we will name that file using square brackets: `pages/news/[newsId].js`. This file will be rendered whenever reaches a path like `our-domain.com/news/something`.

To get the value of 'something', we will use a custom hook, which is built-in inside of `next/router`: `useRouter`.

```js
//[newsId].js
//ourdomain.com/news/something will render this page.

import { useRouter } from 'next/router';

function DetailPage() {
  const router = useRouter();

  const newsId = router.query.newsId;

  //send a request to the backend API
  // to fetch the news data with that Id

  return <h1>The Detail Page</h1>;
}

export default DetailPage;
```

### Linking between pages

In order to link between pages (inside a navbar for example), we can use the Link component imported from `next/link`.

While React's Link component takes a `to=` prop, the Next.js Link component takes a `href=` prop.

```js
<Link href='/news/bolsonaristas-cagam-na-cadeira-do-xandao'>
  Bolsonaristas cagam na cadeira do Xandão
</Link>
```

Navbar:
_repo react-max-23-nexjs2_

```js
//components/MainNavigation.js
import classes from './MainNavigation.module.css';
import Link from 'next/link';

function MainNavigation() {
  return (
    <header className={classes.header}>
      <div className={classes.logo}>React Meetups</div>
      <nav>
        <ul>
          <li>
            <Link href='/'>All Meetups</Link>
          </li>
          <li>
            <Link href='/new-meetup'>Add New Meetup</Link>
          </li>
        </ul>
      </nav>
    </header>
  );
}

export default MainNavigation;
```

### The \_app.js file

In Next.js apps, the `_app.js` file is a special file. It must be placed in the pages folder of our app. It serves like a root component, that will wrap every single page inside our app.

Inside \_app.js, `Component` and `pageProps` are standard objects, that will contain the components rendered inside of MyApp component, and the props of that component.

```js
import Layout from '../components/layout/Layout';
import '../styles/globals.css';

function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  );
}

export default MyApp;
```

### Programatic navigation (redirect)

The next/router hook `useRouter` has a method `push('/path')` that allows us to navigate programatically to that path, ie, redirect the user to that path.

```js
import { useRouter } from 'next/router';

etc etc
const router = useRouter();
router.push('/');
etc etc
```

### getStaticProps(): serving pre-rendered pages

Whenever we are creating a static website, the source code will appear without the data fetched from an API, if we use plain React. This is bad for search-engine-crawlers.

However, in Next.js, we have two forms of pre-rendering:

1. Static Site Generation (SSG): good for static websites
2. Server-side Rendering

In a static website, we can configure the app to serve pages that are pre-rendered via static generation. We do that by exporting the async function `getStaticProps()` in our pages.

The code that will run in that function (usually fetching data and handling this data) will only run during the `building process` of our pages, ie, this code won`t be stored in our client-side app.

`getStaticProps()` must return an object that contains a props object. That props object will be the props of that particular page.

Alternatively, a plain React app would use useEffect withou dependencies to fetch the data from a server. But this would result in an 'empty' source code, not good for search-engine purposes.

```js
export default function HomePage(props) {
  return <MeetupList meetups={props.meetups} />;
}

export async function getStaticProps() {
  //this code will only run on the build process
  //this code will never appear on the client-side

  //fetch data from an API

  return {
    props: {
      meetups: DUMMY_MEETUPS, //these properties are received by the page component as its own props
    },
    revalidate: 10, //this SSG-page will be re-generated every 10s on the server (if the page receives requests)
  };
}
```

**revalidate** property: without this property, the page is built as a SSG-page on the build process, but stays the same after that. If, for example, the API receives new POST requests, the website will remain outdated. The **revalidate** property, inside the object returned by `getStaticProps()`, solves this problem. The number set as its value is the period of time (in seconds) in which our page will be re-generated (inside the server), at least when this page receives new requests.

#### getStaticPaths(): working with params for SSG data fetching

When working with params (dynamic pages like \[itemId\].js), we can extract the requested param using the `context.params` property. In order to do that, we have to pass the context object to the getStaticProps() function:

```js
export async function getStaticProps(context) {
  //fetch data for a single meetup

  const { meetupId } = context.params; //requested param!

  return {
    props: {
      meetupData: {
        image: , //this data can be added after we fetch data and filter the response using params
        id: ,
        title: ,
        address: ,
        description:
      }
    }
  }
}
```

When working with params, besides `getStaticProps()` we also must export another function in our page component: `getStaticPaths()`.

getStaticPaths() must return an object with two properties:

- fallback: if false, the paths object contains all possible params and ids; if true or 'blocking', it can generate missing ones on the fly.
- paths: \[array\] contains one property:
  - params: {object} contains one property:
    - paramName: eg, meetupId, contains one property:
      - pathName: eg, 'm1', the path that identifies the data to be fetched.

(it's easier to see the example:)

```js
export async function getStaticPaths() {
  return {
    fallback: 'blocking', //if false, the paths object contains all possible params and ids; if true or 'blocking', it can generate missing ones on the fly.
    paths: [
      {
        params: {
          meetupId: 'm1',
        },
      },
      {
        params: {
          meetupId: 'm2',
        }, //later on, we're going to fetch all meetupIds from the server, so that we don't need to hard code every single one.
      },
    ],
  };
}
```

### getServerSideProps() and Server-side rendering (SSR)

If we can't get satisfied updating our pre-rendered page only when building or every X seconds, we can then export `getServerSideProps()`. This function will only run on the server every time a request for that page is made.

The disadvantage of getServerSideProps() is that it can take longer than getStaticProps(), as we have to wait for the server to generate the page everytime we load it.

getServerSideProps() also offers access to the `req` and `res` objects, which come as properties of the `context` parameter. This can be useful for authentication middlewares to handle session data, etc.

```js
export async function getServerSideProps(context) {
  const req = context.req;
  const res = context.res;

  //fetch data from an API, etc.

  return {
    props: {
      meetups: DUMMY_MEETUPS,
    },
  };
}
```

### API Routes

return to [here](#getstaticpaths-working-with-params-for-ssg-data-fetching) to complete and add link there

Next.js offers the possibility to add code to create a complete API, inside of our project, thus allowing us to build a fullstack application.

#### Creating a MongoAtlas db and fetching a POST request

1. Create an `api` folder inside of `pages` folder. All the code written here will not be served in the client-side, only in the server-side, thus it's possible to add credentials like api-keys here.
2. Create a .js file with a name that describe what will happen there. Eg, 'new-meetup.js'.
3. On the browser, set up a MongoDB Atlas cluster and click 'connect'.
4. npm install the official MongoDB driver via Terminal: `npm i mongo`.
5. Write the code in our /api/new-meetup.js file:

```js
// pages/api/new-meetup.js
// when a request is sent to /api/new-meetup, Next.js will trigger this function:

import { MongoClient } from 'mongodb';

async function handler(req, res) {
  if (req.method === 'POST') {
    const data = req.body; //getting the req.body, which will contain the form data (title, image, address, description)

    const { title, image, address, description } = data;

    //handle errors would be good!

    const client = await MongoClient.connect(
      'mongodb+srv://<username>:<password>@react-nextjs.2wgcrxv.mongodb.net/meetups-db?retryWrites=true&w=majority'
    ); //add username and password where needed; the name 'meetups-db' after 'mongodb.net/' is the name of our db, it'll be created on the fly as we use it.
    const db = client.db('meetups-db');

    const meetupsCollection = db.collection('meetups'); //this is the name if your collection inside 'meetups-db' database.

    const result = await meetupsCollection.insertOne(data); //inserting new document into the collection.

    console.log(result);

    client.close(); //closing the connection;

    res.status(201).json({ message: 'Meetup inserted!' }); // giving a response with a custom message.
  }
}

export default handler;
```

6. Write the code where you want to send the POST request:

```js
// pages/new-meetup/index.js
// our-domain.com/new-meetup/

import NewMeetupForm from '../../components/meetups/NewMeetupForm';

export default function NewMeetupPage() {
  async function addMeetupHandler(enteredMeetupData) {
    const response = await fetch('/api/new-meetup', {
      method: 'POST',
      body: JSON.stringify(enteredMeetupData),
      headers: {
        'Content-Type': 'application/json',
      },
    });

    const data = await response.json();

    console.log(data);
  }

  return <NewMeetupForm onAddMeetup={addMeetupHandler} />;
}
```

#### Fetching a GET request

**FIRST CASE: getting all documents from a collection**

1. For GET request, we usually don't need to create a new file on the `api` folder. That's because, if we are using functions that only run server-side (like `getStaticProps()`) in our page component, we can insert the backend code directly into that server-side rendering function. This code, as we said, will not be exposed client-side.

```js
// src/index.js
// will be shown when hitting 'our-domain.com/'
import { MongoClient } from 'mongodb';
import MeetupList from '../components/meetups/MeetupList';

export default function HomePage(props) {
  return <MeetupList meetups={props.meetups} />;
}

export async function getStaticProps() {
  //this code will never appear on the client-side

  //fetch data from API:
  //fetch('/api/meetups') //we could do it like this, but we don't need to create a new file!
  const client = await MongoClient.connect(
    'mongodb+srv://jaguat:2rfcofge@react-nextjs.2wgcrxv.mongodb.net/meetups-db?retryWrites=true&w=majority'
  );
  const db = client.db('meetups-db');
  const meetupsCollection = db.collection('meetups');

  const meetups = await meetupsCollection.find().toArray(); //get all documents in 'meetups' collection and transform them into an array;

  client.close();

  return {
    props: {
      meetups: meetups.map((meetup) => ({
        title: meetup.title,
        address: meetup.address,
        image: meetup.image,
        id: meetup._id.toString(), //convert the ObjectId() generated by MongoDB to a string;
      })),
    },
    revalidate: 10,
  };
}
```

**SECOND CASE: getting one document and preparing a dynamic-path rendering**

1. First, we need to generate our array of paths in `getStaticPaths()` on the \[meetupId\].js page dynamically. We can do it by getting an array of all \_id's from our documents in our MongoDB collection. Then, we map this array to create the array of paths that must be returned:

```js
// pages/[meetupId]/index.js
etc etc
export async function getStaticPaths() {
  const client = await MongoClient.connect(
    'mongodb+srv://jaguat:2rfcofge@react-nextjs.2wgcrxv.mongodb.net/meetups-db?retryWrites=true&w=majority'
  );
  const db = client.db('meetups-db');
  const meetupsCollection = db.collection('meetups');

  const meetups = await meetupsCollection.find({}, { _id: 1 }).toArray(); //get only the _id of each document and put them into an array.

  client.close()

  return {
    fallback: false, //if false, the paths object contains all possible params and ids; if true, it can generate missing ones on the fly.
    paths: meetups.map((meetup) => ({
      params: { meetupId: meetup._id.toString() },
    })) //generate our array of paths dynamically.
  };
}
etc etc
```

2. Fetch one single document from the database using getStaticProps() and pass it through props to the component.

```js
// pages/[meetupId]/index.js
import { MongoClient, ObjectId } from 'mongodb';
import MeetupDetail from '../../components/meetups/MeetupDetail';

export default function MeetupDetails(props) {
  return (
    <MeetupDetail
      image={props.meetupData.image}
      title={props.meetupData.title}
      address={props.meetupData.address}
      description={props.meetupData.description}
      id={props.meetupData.id}
    />
  );
}

export async function getStaticPaths() {
  see above
}

export async function getStaticProps(context) {
  // getting the param from the context object:
  const { meetupId } = context.params; //

  //fetch data for a single meetup:
  const client = await MongoClient.connect(
    'mongodb+srv://jaguat:2rfcofge@react-nextjs.2wgcrxv.mongodb.net/meetups-db?retryWrites=true&w=majority'
  );
  const db = client.db('meetups-db');
  const meetupsCollection = db.collection('meetups');

  const selectedMeetup = await meetupsCollection.findOne({
    _id: ObjectId(meetupId),
  }); // don't forget to import { ObjectId } from 'mongodb'!

  client.close();

  return {
    props: {
      meetupData: {
        id: selectedMeetup._id.toString(), //converting ObjectId to a string.
        title: selectedMeetup.title,
        address: selectedMeetup.address,
        description: selectedMeetup.description,
        image: selectedMeetup.image,
      },
    },
  };
}
```

### Adding head with title and description

HTML head tag contains metadata about our website, like title and description. This metadata will provide search engines with information about our website.

We can add this information using the Head component provided by 'next/head'.

```js
// /pages/index.js
import Head from 'next/head';
import MeetupList from '../components/meetups/MeetupList';

export default function HomePage(props) {
  return (
    <>
      <Head>
        <title>Meetups</title>
        <meta
          name="description"
          content="Your website to create meetups all around the world"
        />
      </Head>
      <MeetupList meetups={props.meetups} />
    </>
  );
}

etc etc
```

We can add the Head component in all page components. Alternatively, we can add a single Head component inside the \_app.js root component. Then, all pages will have the same metadata:

```js
// pages/_app.js
import Head from 'next/head';
import Layout from '../components/layout/Layout';
import '../styles/globals.css';

function MyApp({ Component, pageProps }) {
  return (
    <>
      <Head>
        <title>Meetups</title>
        <meta
          name='description'
          content='Your website to create meetups all around the world'
        />
      </Head>
      <Layout>
        <Component {...pageProps} />
      </Layout>
    </>
  );
}

export default MyApp;
```

We can also add dynamic values inside the Head component's tags. This is especially useful in dynamic pages (templates that use params), for example, pages that contain details about items on a list:

```js
import Head from 'next/head';
import MeetupDetail from '../../components/meetups/MeetupDetail';

export default function MeetupDetails(props) {
  return (
    <>
      <Head>
        <title>{props.meetupData.title}</title>
      </Head>
      <MeetupDetail
        image={props.meetupData.image}
        title={props.meetupData.title}
        address={props.meetupData.address}
        description={props.meetupData.description}
        id={props.meetupData.id}
      />
    </>
  );
}

etc etc
```

### Deploying Next.js projects

Next.js was created by the Vercel team, then this hosting service is optimized for hosting Next.js projects. We just have to import our GitHub repo to Vercel, and it will run build and build our compiled files for us.

As our project contains credentials, it is better to make the GitHub repo private, then link it to Vercel. About environmental variables, see below.

#### Configuring MongoDB Network Access

You will have to `allow access from anywhere` in MongoDB Network Access settings. This is important so that Vercel can access MongoDB to get new data and pre-render pages, store new data from forms, etc.

#### Updating dependencies

Sometimes our project was written with dependencies that are not uptodate to current Vercel standards, so maybe we should update them in our project before deployment, by running `npm install next@12 react@18 react-dom@18 --force`.

Sometimes you also have to follow these steps:

First remove all node moudle folder and packge-lock.json, then change the `package.json` (see below) and `npm i` again, then finally Vercel deploy will work:

```js
 {
  "name": "nextjs-course",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "mongodb": "^3.6.4",
    "next": "12.1.4",
    "react": "17.0.2",
    "react-dom": "17.0.2"
  }
}
```

#### Environment variables

In this section, Max hides the password by keeping the Git repository private. I wanted to make my repository public, so I added an environment variable `MY_ENVIRONMENT_VARIABLE` on the Vercel project page and set it to `<the MongoDB connect string>`. This value is a string, but you should NOT write it between '' in Vercel. Just the content itself.

Then I referred to process.env.`MY_ENVIRONMENT_VARIABLE` in the MongoClient.connect function. This way the string with the password wasn't exposed in the git repository.

To use the variable locally, I created a `.env` file and set `MY_ENVIRONMENT_VARIABLE='<the MongoDB connect string>'`. Restarted the dev server and it worked. Here you should use ''.

Also remember to add `.env` to `.gitignore`. Now we can push our code to GH as a public repo.

## React Animations

Frame Motion (by Net Ninja)
https://www.youtube.com/playlist?list=PL4cUxeGkcC9iHDnQfTHEVVceOEBsOf07i

React Spring (by Brad Traversy)
https://www.youtube.com/watch?v=S8yn3-WpVV8&ab_channel=TraversyMedia

```

```

## Links

Migrating from a React App to a Next.js app: https://nextjs.org/docs/migrating/from-create-react-app

## React App Pathway

\*based on NN section 17, repo

1. Get initial files (from Create React App or another tool).

2. "Clean-up" the initial files.

3. Define global css file, like `index.css` next to `index.js`.

4. Set-up Firebase: create a new project, create a Firestore Database and Authentication for email/password. Add a new web-app to your project. Copy the config object from that web-app settings.

5. Create a folder `firebase` inside of `src` with a file called `config.js`:

```js
// src/firestore/config.js
import firebase from 'firebase/app';
import 'firebase/firestore';
import 'firebase/auth';

//copy this object from your web-app settings in Firestore console:
const firebaseConfig = {
  apiKey: 'AIzaSyBoGzpEv3rXzBHMWrwIM_U2rzM9ansHGJA',
  authDomain: 'thedojosite-cf361.firebaseapp.com',
  projectId: 'thedojosite-cf361',
  storageBucket: 'thedojosite-cf361.appspot.com',
  messagingSenderId: '673918625748',
  appId: '1:673918625748:web:058c1c131b5bcaebacd841',
};

//initialize Firebase
firebase.initializeApp(firebaseConfig);

//initialize services
const projectFirestore = firebase.firestore();
const projectAuth = firebase.auth();

//timestamp
const timestamp = firebase.firestore.Timestamp;

export { projectFirestore, projectAuth, timestamp };
```

6. `npm i firebase` and initialize Firebase on the project `firebase init`. Enable Firebase, Hosting (1st option) and Storage. If error: go into Firebase project settings and set "Default GCP resource location". Just click on the pen and then "done".

What do you want to use as your public dire
ctory? (public) build

Configure as a single-page app (rewrite all urls to /index.html)? (y/N) y

Set up automatic builds and deploys with Gi
tHub? (y/N) N

(the rest goes with default, just hit Enter)

Now we have the files for rules, indexes and storage.rules in our project.

7. We can use the hooks and auth-context that we created in another project. They're inside this same repo :)

8. Enable Firestore Storage and update config.js to activate the service in our app:

```js
import firebase from 'firebase/app';
import 'firebase/firestore';
import 'firebase/auth';
import 'firebase/storage';

const firebaseConfig = {
  apiKey: 'AIzaSyBoGzpEv3rXzBHMWrwIM_U2rzM9ansHGJA',
  authDomain: 'thedojosite-cf361.firebaseapp.com',
  projectId: 'thedojosite-cf361',
  storageBucket: 'thedojosite-cf361.appspot.com',
  messagingSenderId: '673918625748',
  appId: '1:673918625748:web:058c1c131b5bcaebacd841',
};

//initialize Firebase
firebase.initializeApp(firebaseConfig);

//initialize services
const projectFirestore = firebase.firestore();
const projectAuth = firebase.auth();
const projectStorage = firebase.storage();

//timestamp
const timestamp = firebase.firestore.Timestamp;

export { projectFirestore, projectAuth, projectStorage, timestamp };
```

9. Update the `useSignup.js` hook to accept an image file object and update `photoURL` property on authenticated user:

```js
// /hooks/useSignup.js
import { useState, useEffect } from 'react';
import { projectAuth, projectStorage } from '../firebase/config';
import { useAuthContext } from './useAuthContext';

export const useSignup = () => {
  const [isCancelled, setIsCancelled] = useState(false);
  const [error, setError] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const { dispatch } = useAuthContext();

  const signup = async (email, password, displayName, thumbnail) => {
    setError(null);
    setIsPending(true);

    try {
      // signup
      const res = await projectAuth.createUserWithEmailAndPassword(
        email,
        password
      );

      if (!res) {
        throw new Error('Could not complete signup');
      }

      // update user thumbnail
      const uploadPath = `thumbnails/${res.user.uid}/${thumbnail.name}`; //create the path to upload
      const img = await projectStorage.ref(uploadPath).put(thumbnail); //uploading
      const imgURL = await img.ref.getDownloadURL(); //get the uploaded image URL

      // add display name to user
      await res.user.updateProfile({ displayName, photoURL: imgURL });

      // dispatch login action
      dispatch({ type: 'LOGIN', payload: res.user });

      if (!isCancelled) {
        setIsPending(false);
        setError(null);
      }
    } catch (err) {
      if (!isCancelled) {
        setError(err.message);
        setIsPending(false);
      }
    }
  };

  useEffect(() => {
    return () => setIsCancelled(true);
  }, []);

  return { signup, error, isPending };
};
```

```js
// /pages/signup/Signup.js
import { useState } from 'react';
import { useSignup } from '../../hooks/useSignup';
import './Signup.css';

export default function Signup() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [displayName, setDisplayName] = useState('');
  const [thumbnail, setThumbnail] = useState(null);
  const [thumbnailError, setTumbnailError] = useState(null);
  const { signup, isPending, error } = useSignup();

  // submit form data, including thumbnail:
  const handleSubmit = (e) => {
    e.preventDefault();
    signup(email, password, displayName, thumbnail);
  };

  // update 'thumbnail' state:
  const handleFileChange = (e) => {
    setThumbnail(null);
    let selected = e.target.files[0]; //this will be an array, but we just want one file.
    console.log(selected);

    if (!selected) {
      setTumbnailError('Please select a file');
      return;
    }
    if (!selected.type.includes('image')) {
      setTumbnailError('Selected file must be an image');
      return;
    }
    if (selected.size > 100000) {
      setTumbnailError('Image file size must be less than 100kB');
      return;
    }

    setTumbnailError(null);
    setThumbnail(selected);
    console.log('thumbnail updated');
  };

  return (
    <form className='auth-form' onSubmit={handleSubmit}>
      <h2>Sign up</h2>
      <label>
        <span>email:</span>
        <input
          type='email'
          required
          onChange={(e) => setEmail(e.target.value)}
          value={email}
        />
      </label>
      <label>
        <span>password:</span>
        <input
          type='password'
          required
          onChange={(e) => setPassword(e.target.value)}
          value={password}
        />
      </label>
      <label>
        <span>display name:</span>
        <input
          type='text'
          required
          onChange={(e) => setDisplayName(e.target.value)}
          value={displayName}
        />
      </label>
      <label>
        <span>profile thumbnail:</span>
        <input type='file' required onChange={handleFileChange} />
        {thumbnailError && <div className='error'>{thumbnailError}</div>}
      </label>
      {!isPending && <button className='btn'>sign up</button>}
      {isPending && (
        <button className='btn' disabled>
          loading
        </button>
      )}
      {error && <div className='error'>{error}</div>}
    </form>
  );
}
```

10. Create a collection `users` in Firestore DB. Later on, we will store project's data in that database, but for now we'll create a collection to store userdata, so that this data can ben easily found and used.

So, we have 3 services in Firestore that contain our users data:
a. Auth, stores user and built-in auth functions
b. Storage, stores thumbnail image
c. Database, stores displayName, online state and photoURL (path to Storage file)

```js
import { useState, useEffect } from 'react';
import {
  projectAuth,
  projectStorage,
  projectFirestore,
} from '../firebase/config';
import { useAuthContext } from './useAuthContext';

export const useSignup = () => {
  const [isCancelled, setIsCancelled] = useState(false);
  const [error, setError] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const { dispatch } = useAuthContext();

  const signup = async (email, password, displayName, thumbnail) => {
    setError(null);
    setIsPending(true);

    try {
      // signup
      const res = await projectAuth.createUserWithEmailAndPassword(
        email,
        password
      );

      if (!res) {
        throw new Error('Could not complete signup');
      }

      // update user thumbnail
      const uploadPath = `thumbnails/${res.user.uid}/${thumbnail.name}`; //create the path to upload
      const img = await projectStorage.ref(uploadPath).put(thumbnail); //uploading
      const imgURL = await img.ref.getDownloadURL(); //get the uploaded image URL

      // add display name to user
      await res.user.updateProfile({ displayName, photoURL: imgURL });

      // create a user document inside the 'users' collection in Firestore DB
      await projectFirestore.collection('users').doc(res.user.uid).set({
        online: true,
        displayName,
        photoURL: imgURL,
      });

      // dispatch login action
      dispatch({ type: 'LOGIN', payload: res.user });

      if (!isCancelled) {
        setIsPending(false);
        setError(null);
      }
    } catch (err) {
      if (!isCancelled) {
        setError(err.message);
        setIsPending(false);
      }
    }
  };

  useEffect(() => {
    return () => setIsCancelled(true);
  }, []);

  return { signup, error, isPending };
};
```

11. Implementing logout. First we need to update Navbar.js adding an onClick handler to the Logout button. Then, we have to update useLogout.js, so that the logout function also change the user's state online to false, inside the collection 'users' in DB.

```js
// Navbar.js
import { Link } from 'react-router-dom';
import { useLogout } from '../hooks/useLogout';
import './Navbar.css';
import Temple from '../assets/temple.svg';

export default function Navbar() {
  const { logout, isPending } = useLogout();

  return (
    <div className='navbar'>
      <ul>
        <li className='logo'>
          <img src={Temple} alt='Logo' />
          <span>The Dojo</span>
        </li>
        <li>
          <Link to='/login'>Login</Link>
        </li>
        <li>
          <Link to='/signup'>Signup</Link>
        </li>
        <li>
          {!isPending && (
            <button className='btn' onClick={logout}>
              Logout
            </button>
          )}
          {isPending && (
            <button className='btn' disabled>
              Logging out...
            </button>
          )}
        </li>
      </ul>
    </div>
  );
}
```

```js
// hooks/useLogout.js
import { useEffect, useState } from 'react';
import { projectAuth, projectFirestore } from '../firebase/config';
import { useAuthContext } from './useAuthContext';

export const useLogout = () => {
  const [isCancelled, setIsCancelled] = useState(false);
  const [error, setError] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const { dispatch, user } = useAuthContext();

  const logout = async () => {
    setError(null);
    setIsPending(true);

    try {
      // update online status on users collection in Firebase DB
      const { uid } = user;
      await projectFirestore
        .collection('users')
        .doc(uid)
        .update({ online: false });

      // sign the user out in Firebase Auth
      await projectAuth.signOut();

      // dispatch logout action
      dispatch({ type: 'LOGOUT' });

      // update state
      if (!isCancelled) {
        setIsPending(false);
        setError(null);
      }
    } catch (err) {
      if (!isCancelled) {
        setError(err.message);
        setIsPending(false);
      }
    }
  };

  useEffect(() => {
    return () => setIsCancelled(true);
  }, []);

  return { logout, error, isPending };
};
```

12. Basically the same as 11, but for Login:

```js
// Login.js
import { useState } from 'react';
import { useLogin } from '../../hooks/useLogin';
import './Login.css';

export default function Login() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const { login, isPending, error } = useLogin();

  const handleSubmit = (e) => {
    e.preventDefault();
    login(email, password);
  };

  return (
    <form className='auth-form' onSubmit={handleSubmit}>
      <h2>Login</h2>
      <label>
        <span>email:</span>
        <input
          type='email'
          required
          onChange={(e) => setEmail(e.target.value)}
          value={email}
        />
      </label>
      <label>
        <span>password:</span>
        <input
          type='password'
          required
          onChange={(e) => setPassword(e.target.value)}
          value={password}
        />
      </label>

      {!isPending && <button className='btn'>login</button>}
      {isPending && (
        <button className='btn' disabled>
          loading
        </button>
      )}
      {error && <div className='error'>{error}</div>}
    </form>
  );
}
```

```js
// hooks/useLogin.js
import { useState, useEffect } from 'react';
import { projectAuth, projectFirestore } from '../firebase/config';
import { useAuthContext } from './useAuthContext';

export const useLogin = () => {
  const [isCancelled, setIsCancelled] = useState(false);
  const [error, setError] = useState(null);
  const [isPending, setIsPending] = useState(false);
  const { dispatch } = useAuthContext();

  const login = async (email, password) => {
    setError(null);
    setIsPending(true);

    try {
      // login in Auth
      const res = await projectAuth.signInWithEmailAndPassword(email, password);

      // set online property on DB's user to true
      const { uid } = res.user;
      await projectFirestore
        .collection('users')
        .doc(uid)
        .update({ online: true });

      // dispatch login action
      dispatch({ type: 'LOGIN', payload: res.user });

      if (!isCancelled) {
        setIsPending(false);
        setError(null);
      }
    } catch (err) {
      if (!isCancelled) {
        setError(err.message);
        setIsPending(false);
      }
    }
  };

  useEffect(() => {
    return () => setIsCancelled(true);
  }, []);

  return { login, isPending, error };
};
```

13. Setting Route Guards, preventing unauthenticated users to reach Dashboard, for example:

```js
// app.js
import { BrowserRouter, Route, Switch, Redirect } from 'react-router-dom';
import { useAuthContext } from './hooks/useAuthContext';
import './App.css';
import Dashboard from './pages/dashboard/Dashboard';
import Create from './pages/create/Create';
import Login from './pages/login/Login';
import Signup from './pages/signup/Signup';
import Project from './pages/project/Project';
import Navbar from './components/Navbar';
import Sidebar from './components/Sidebar';

function App() {
  const { user, authIsReady } = useAuthContext();

  return (
    <div className='App'>
      {authIsReady && (
        <BrowserRouter>
          <Sidebar />
          <div className='container'>
            <Navbar />
            <Switch>
              <Route exact path='/'>
                {!user && <Redirect to='login' />}
                {user && <Dashboard />}
              </Route>
              <Route path='/create'>
                {!user && <Redirect to='login' />}
                {user && <Create />}
              </Route>
              <Route path='/projects/:id'>
                {!user && <Redirect to='login' />}
                {user && <Project />}
              </Route>
              <Route path='/login'>
                {user && <Redirect to='/' />}
                {!user && <Login />}
              </Route>
              <Route path='/signup'>
                {user && <Redirect to='/' />}
                {!user && <Signup />}
              </Route>
            </Switch>
          </div>
        </BrowserRouter>
      )}
    </div>
  );
}

export default App;
```

14. Now theta our routes are guarded, the user can't try to go the paths that are forbidden for who is not authenticated. So we can also conditionally show Login/Logout/Signup links on the Navbar, depending if the user is authenticated or not.

```js
//Navbar.js
import { Link } from 'react-router-dom';
import { useLogout } from '../hooks/useLogout';
import { useAuthContext } from '../hooks/useAuthContext';
import './Navbar.css';
import Temple from '../assets/temple.svg';

export default function Navbar() {
  const { logout, isPending } = useLogout();
  const { user } = useAuthContext();

  return (
    <div className='navbar'>
      <ul>
        <li className='logo'>
          <img src={Temple} alt='Logo' />
          <span>The Dojo</span>
        </li>

        {!user && (
          <>
            <li>
              <Link to='/login'>Login</Link>
            </li>
            <li>
              <Link to='/signup'>Signup</Link>
            </li>
          </>
        )}
        {user && (
          <li>
            {!isPending && (
              <button className='btn' onClick={logout}>
                Logout
              </button>
            )}
            {isPending && (
              <button className='btn' disabled>
                Logging out...
              </button>
            )}
          </li>
        )}
      </ul>
    </div>
  );
}
```

15. Showing avatar in the sidebar. The 'user' object, that we can grab by calling useAuthContext, contains a `photoURL` property, which contains the url for the thumbnail of our user. The thumbnails are stored in Storage. So, first we have to create a little Avatar.js component and add it to the sidebar. Then we have to set the sidebar to only show conditionally - otherwise we will get an error, because we will have no user object to get the photoURL.

```js
import { NavLink } from 'react-router-dom';
import { useAuthContext } from '../hooks/useAuthContext';
import './Sidebar.css';
import DashboardIcon from '../assets/dashboard_icon.svg';
import AddIcon from '../assets/add_icon.svg';
import Avatar from './Avatar';

export default function Sidebar() {
  const { user } = useAuthContext();

  return (
    <div className='sidebar'>
      <div className='sidebar-content'>
        <div className='user'>
          <Avatar src={user.photoURL} />
          <p>Hey, {user.displayName}</p>
        </div>

        <nav className='links'>
          <ul>
            <li>
              <NavLink exact to='/'>
                <img src={DashboardIcon} alt='dashboard' />
                <span>Dashboard</span>
              </NavLink>
            </li>
            <li>
              <NavLink to='/create'>
                <img src={AddIcon} alt='create new' />
                <span>New Project</span>
              </NavLink>
            </li>
          </ul>
        </nav>
      </div>
    </div>
  );
}
```

16. Create an OnlineUsers.js component to show all users. We will use the custom hook `useCollection` to get an array of all items on the `users` collection. We are also conditionally showing a green circle (a span styled with css) if the user is currently online.

```js
import { useCollection } from '../hooks/useCollection';
import Avatar from './Avatar';
import './OnlineUsers.css';

export default function OnlineUsers() {
  const { error, documents } = useCollection('users');

  return (
    <div className='user-list'>
      <h2>All Users</h2>
      {error && <div className='error'>{error}</div>}
      {documents &&
        documents.map((user) => (
          <div key={user.id} className='user-list-item'>
            {user.online && <span className='online-user'></span>}
            <span>{user.displayName}</span>
            <Avatar src={user.photoURL} />
          </div>
        ))}
    </div>
  );
}
```

17. Start creating a Create.js page, adding a form to create a new project.

```js
// pages/create/Create.js
import { useState } from 'react';
import './Create.css';

export default function Create() {
  const [name, setName] = useState('');
  const [details, setDetails] = useState('');
  const [dueDate, setDueDate] = useState('');
  const [category, setCategory] = useState('');
  const [assignedUsers, setAssignedUsers] = useState([]);

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(name, details, dueDate);
  };

  return (
    <div className='create-form'>
      <h2 className='page-title'>Create a new project</h2>
      <form onSubmit={handleSubmit}>
        <label>
          <span>Project name:</span>
          <input
            required
            type='text'
            onChange={(e) => setName(e.target.value)}
            value={name}
          />
        </label>
        <label>
          <span>Project details:</span>
          <textarea
            required
            type='text'
            onChange={(e) => setDetails(e.target.value)}
            value={details}
          ></textarea>
        </label>
        <label>
          <span>Set due date:</span>
          <input
            required
            type='date'
            onChange={(e) => setDueDate(e.target.value)}
            value={dueDate}
          />
        </label>

        <label>
          <span>Project category:</span>
          {/* category select here */}
        </label>
        <label>
          <span>Assign to:</span>
          {/* assignee select here */}
        </label>

        <button className='btn'>add project</button>
      </form>
    </div>
  );
}
```

18. Introducing the React Select package. It is a package that allows us to easily create select tags inside of our forms.
    I. `npm i react-select`
    II. `import { Select } from 'react-select'`
    III. create an array `categories` of objects (can be outside of the component if it won't change).
    IV. Build a Select component with a prop called `options` containing that array. Also set an `onChange` handler prop.

```js
// Create.js
import { useState } from 'react';
import Select from 'react-select';
import './Create.css';

const categories = [
  { value: 'development', label: 'Development' },
  { value: 'design', label: 'Design' },
  { value: 'sales', label: 'Sales' },
  { value: 'marketing', label: 'Marketing' },
];

export default function Create() {
  const [name, setName] = useState('');
  const [details, setDetails] = useState('');
  const [dueDate, setDueDate] = useState('');
  const [category, setCategory] = useState('');
  const [assignedUsers, setAssignedUsers] = useState([]);

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(name, details, dueDate, category.value);
  };

  return (
    <div className='create-form'>
      <h2 className='page-title'>Create a new project</h2>
      <form onSubmit={handleSubmit}>
        <label>
          <span>Project name:</span>
          <input
            required
            type='text'
            onChange={(e) => setName(e.target.value)}
            value={name}
          />
        </label>
        <label>
          <span>Project details:</span>
          <textarea
            required
            type='text'
            onChange={(e) => setDetails(e.target.value)}
            value={details}
          ></textarea>
        </label>
        <label>
          <span>Set due date:</span>
          <input
            required
            type='date'
            onChange={(e) => setDueDate(e.target.value)}
            value={dueDate}
          />
        </label>

        <label>
          <span>Project category:</span>
          <Select
            options={categories}
            onChange={(option) => setCategory(option)}
          />
        </label>
        <label>
          <span>Assign to:</span>
          {/* assignee select here */}
        </label>

        <button className='btn'>add project</button>
      </form>
    </div>
  );
}
```

19. In order to create the Select component for the assignedUsers state, first we have to get a list of all users by using `useCollection('users')`. Then we will map through this array, to create another array in the format `[{ label: XXY , value: XXY }, { label: XXX , value: XXX }]`. After passing this array as the value of the prop `options`, we will add the `isMulti` argument: this will allow the user to select multiple users.

```js
import { useEffect, useState } from 'react';
import Select from 'react-select';
import { useCollection } from '../../hooks/useCollection';
import './Create.css';

const categories = [
  { value: 'development', label: 'Development' },
  { value: 'design', label: 'Design' },
  { value: 'sales', label: 'Sales' },
  { value: 'marketing', label: 'Marketing' },
];

export default function Create() {
  const [name, setName] = useState('');
  const [details, setDetails] = useState('');
  const [dueDate, setDueDate] = useState('');
  const [category, setCategory] = useState('');
  const [assignedUsers, setAssignedUsers] = useState([]);

  // getting a list of all users to feed the assignees Select component:
  const [users, setUsers] = useState([]);
  const { documents } = useCollection('users');
  useEffect(() => {
    if (documents) {
      const options = documents.map((user) => {
        return { value: user, label: user.displayName };
      });
      setUsers(options);
    }
  }, [documents]); //useEffect will "listen" for changes in the documents array.

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(name, details, dueDate, category.value, assignedUsers);
  };

  return (
    <div className='create-form'>
      <h2 className='page-title'>Create a new project</h2>
      <form onSubmit={handleSubmit}>
        <label>
          <span>Project name:</span>
          <input
            required
            type='text'
            onChange={(e) => setName(e.target.value)}
            value={name}
          />
        </label>
        <label>
          <span>Project details:</span>
          <textarea
            required
            type='text'
            onChange={(e) => setDetails(e.target.value)}
            value={details}
          ></textarea>
        </label>
        <label>
          <span>Set due date:</span>
          <input
            required
            type='date'
            onChange={(e) => setDueDate(e.target.value)}
            value={dueDate}
          />
        </label>

        <label>
          <span>Project category:</span>
          <Select
            options={categories}
            onChange={(option) => setCategory(option)}
          />
        </label>
        <label>
          <span>Assign to:</span>
          <Select
            options={users}
            onChange={(option) => setAssignedUsers(option)}
            isMulti //allow selecting multiple users
          />
        </label>

        <button className='btn'>add project</button>
      </form>
    </div>
  );
}
```

20. Handling errors in the Create.js form. We will create a state called formError and make checks when the form is submitted.

```js
import { useEffect, useState } from 'react';
import Select from 'react-select';
import { useCollection } from '../../hooks/useCollection';
import './Create.css';

const categories = [
  { value: 'development', label: 'Development' },
  { value: 'design', label: 'Design' },
  { value: 'sales', label: 'Sales' },
  { value: 'marketing', label: 'Marketing' },
];

export default function Create() {
  const [name, setName] = useState('');
  const [details, setDetails] = useState('');
  const [dueDate, setDueDate] = useState('');
  const [category, setCategory] = useState('');
  const [assignedUsers, setAssignedUsers] = useState([]);
  const [formError, setFormError] = useState(null); //formError state

  const [users, setUsers] = useState([]);
  const { documents } = useCollection('users');
  useEffect(() => {
    if (documents) {
      const options = documents.map((user) => {
        return { value: user, label: user.displayName };
      });
      setUsers(options);
    }
  }, [documents]);

  const handleSubmit = (e) => {
    e.preventDefault();
    setFormError(null);

    if (!category) {
      setFormError('Please select a project category');
      return;
    } //checking if category is ''

    if (assignedUsers.length < 1) {
      setFormError('Please assign the project to at least 1 user');
      return;
    } //checking if assignedUsers is []

    console.log(name, details, dueDate, category.value, assignedUsers);
  };

  return (
    <div className='create-form'>
      <h2 className='page-title'>Create a new project</h2>
      <form onSubmit={handleSubmit}>
        <label>
          <span>Project name:</span>
          <input
            required
            type='text'
            onChange={(e) => setName(e.target.value)}
            value={name}
          />
        </label>
        <label>
          <span>Project details:</span>
          <textarea
            required
            type='text'
            onChange={(e) => setDetails(e.target.value)}
            value={details}
          ></textarea>
        </label>
        <label>
          <span>Set due date:</span>
          <input
            required
            type='date'
            onChange={(e) => setDueDate(e.target.value)}
            value={dueDate}
          />
        </label>
        <label>
          <span>Project category:</span>
          <Select
            options={categories}
            onChange={(option) => setCategory(option)}
          />
        </label>
        <label>
          <span>Assign to:</span>
          <Select
            options={users}
            onChange={(option) => setAssignedUsers(option)}
            isMulti
          />
        </label>
        <button className='btn'>add project</button>
        {formError && <p className='error'>{formError}</p>} //outputting the
        error
      </form>
    </div>
  );
}
```

21. Creating a project object and adding it to Firestore DB:

```js
//Create.js
import { useEffect, useState } from 'react';
import Select from 'react-select';
import { useCollection } from '../../hooks/useCollection';
import { useFirestore } from '../../hooks/useFirestore';
import { timestamp, projectFirestore } from '../../firebase/config';
import { useAuthContext } from '../../hooks/useAuthContext';
import { useHistory } from 'react-router-dom';
import './Create.css';

const categories = [
  { value: 'development', label: 'Development' },
  { value: 'design', label: 'Design' },
  { value: 'sales', label: 'Sales' },
  { value: 'marketing', label: 'Marketing' },
];

export default function Create() {
  const { user } = useAuthContext();
  const { addDocument, response } = useFirestore('projects');
  const history = useHistory();

  const [name, setName] = useState('');
  const [details, setDetails] = useState('');
  const [dueDate, setDueDate] = useState('');
  const [category, setCategory] = useState('');
  const [assignedUsers, setAssignedUsers] = useState([]);
  const [formError, setFormError] = useState(null);

  // getting a list of all users to feed the assignees Select component:
  const [users, setUsers] = useState([]);
  const { documents } = useCollection('users');
  useEffect(() => {
    if (documents) {
      const options = documents.map((user) => {
        return { value: user, label: user.displayName };
      });
      setUsers(options);
    }
  }, [documents]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    setFormError(null);

    // handling form errors:
    if (!category) {
      setFormError('Please select a project category');
      return;
    }

    if (assignedUsers.length < 1) {
      setFormError('Please assign the project to at least 1 user');
      return;
    }

    // building the 'project' object:
    const createdBy = {
      displayName: user.displayName,
      photoURL: user.photoURL,
      id: user.uid,
    };

    const assignedUsersList = assignedUsers.map((u) => {
      return {
        displayName: u.value.displayName,
        photoURL: u.value.photoURL,
        id: u.value.id,
      };
    });

    const project = {
      name,
      details,
      category: category.value,
      dueDate: timestamp.fromDate(new Date(dueDate)),
      commments: [],
      createdBy,
      assignedUsersList,
    };

    //adding the project as a document on the collection 'projects':
    // await projectFirestore.collection('projects').add(project);
    await addDocument(project);
    if (!response.error) {
      history.push('/');
    }

    console.log(project);
  };

  return (
    <div className='create-form'>
      <h2 className='page-title'>Create a new project</h2>
      <form onSubmit={handleSubmit}>
        <label>
          <span>Project name:</span>
          <input
            required
            type='text'
            onChange={(e) => setName(e.target.value)}
            value={name}
          />
        </label>
        <label>
          <span>Project details:</span>
          <textarea
            required
            type='text'
            onChange={(e) => setDetails(e.target.value)}
            value={details}
          ></textarea>
        </label>
        <label>
          <span>Set due date:</span>
          <input
            required
            type='date'
            onChange={(e) => setDueDate(e.target.value)}
            value={dueDate}
          />
        </label>

        <label>
          <span>Project category:</span>
          <Select
            options={categories}
            onChange={(option) => setCategory(option)}
          />
        </label>
        <label>
          <span>Assign to:</span>
          <Select
            options={users}
            onChange={(option) => setAssignedUsers(option)}
            isMulti
          />
        </label>

        <button className='btn'>add project</button>

        {formError && <p className='error'>{formError}</p>}
      </form>
    </div>
  );
}
```

22. Fetching the projects from DB and making a ProjectList comnponent to list them on dashboard. We'll fetch the projects from DB in Dashboard.js and then pass them through prop to ProjectList component.

```js
// Dashboard.js
import ProjectList from '../../components/ProjectList';
import { useCollection } from '../../hooks/useCollection';
import './Dashboard.css';

export default function Dashboard() {
  const { documents, error } = useCollection('projects');

  return (
    <div>
      <h2 className='page-title'>Dashboard</h2>
      {error && <p className='error'>{error}</p>}
      {documents && <ProjectList projects={documents} />}
    </div>
  );
}
```

```js
import { Link } from 'react-router-dom';
import Avatar from './Avatar';
import './ProjectList.css';

export default function ProjectList({ projects }) {
  return (
    <div className='project-list'>
      {projects.length === 0 && <p>No projects yet.</p>}
      {projects.map((project) => (
        <Link key={project.id} to={`/projects/${project.id}`}>
          <h4>{project.name}</h4>
          <p>Due by {project.dueDate.toDate().toDateString()}</p>
          <div className='assigned-to'>
            <ul>
              {project.assignedUsersList.map((user) => (
                <li key={user.photoURL}>
                  <Avatar src={user.photoURL} />
                </li>
              ))}
            </ul>
          </div>
        </Link>
      ))}
    </div>
  );
}
```

23. Create a `useDocument.js` hook to fetch a single document from a collection. With that, we will get the data to create /projects/:id pages.

```js
// useDocument.js
import { useEffect, useState } from 'react';
import { projectFirestore } from '../firebase/config';

export const useDocument = (collection, id) => {
  const [document, setDocument] = useState(null);
  const [error, setError] = useState(null);

  //realtime data for document
  useEffect(() => {
    const ref = projectFirestore.collection(collection).doc(id);

    const unsubscribe = ref.onSnapshot(
      (snapshot) => {
        //check is there is data inside snapshot:
        if (snapshot.data()) {
          setDocument({ ...snapshot.data(), id: snapshot.id });
          setError(null);
        } else {
          setError('No such document exists.');
        }
      },
      (err) => {
        console.log(err.message);
        setError('Failed to get document');
      }
    );

    return () => unsubscribe();
  }, [collection, id]);

  return { document, error };
};
```

24. Create a ProjectSummary component to be put inside of Project.js. Remember to leave space on the side to add comments, here is the css grid to divide the Project.js available space into this two areas:

```js
//project/Project.css
.project-details {
  display: grid;
  /* divide into 2 columns, one with width of 3fr, one with width of 2fr: */
  grid-template-columns: 3fr 2fr;
  align-items: start;
  grid-gap: 60px;
}
```

```js
// project/ProjectSummary.js
import Avatar from '../../components/Avatar';

export default function ProjectSummary({ project }) {
  return (
    <div className='project-summary'>
      <h2 className='page-title'>{project.name}</h2>
      <p className='due-date'>
        Project due by {project.dueDate.toDate().toDateString()}
      </p>
      <p className='details'>{project.details}</p>
      <h4>Project is assigned to:</h4>
      <div className='assigned-users'>
        {project.assignedUsersList.map((user) => (
          <div key={user.id}>
            <Avatar src={user.photoURL} alt={user.displayName} />
          </div>
        ))}
      </div>
    </div>
  );
}
```

25. Create a ProjectComments component. It will have a form to create new comment and also display all comments of this projects. We have to update our `useFirestore` hook, so that it can also update an existing document, thus adding a new comment to its 'comments' property.

```js
// useFirestore.js
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
      return { isPending: true, document: null, success: false, error: null };
    case 'ADDED_DOCUMENT':
      return {
        isPending: false,
        document: action.payload,
        success: true,
        error: null,
      };
    case 'DELETED_DOCUMENT':
      return { isPending: false, document: null, success: true, error: null };
    case 'UPDATED_DOCUMENT':
      return {
        isPending: false,
        document: action.payload,
        success: true,
        error: null,
      };
    case 'ERROR':
      return {
        isPending: false,
        document: null,
        success: false,
        error: action.payload,
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

  // only dispatch is not cancelled
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
      dispatchIfNotCancelled({ type: 'DELETED_DOCUMENT' });
    } catch (err) {
      dispatchIfNotCancelled({ type: 'ERROR', payload: 'could not delete' });
    }
  };

  // update a document
  const updateDocument = async (id, updates) => {
    dispatch({ type: 'IS_PENDING' });

    try {
      const updatedDocument = await ref.doc(id).update({
        updates,
      });
      dispatchIfNotCancelled({
        type: 'UPDATED_DOCUMENT',
        payload: updatedDocument,
      });
      return updatedDocument;
    } catch (err) {
      dispatchIfNotCancelled({ type: 'ERROR', payload: err.message });
      return null;
    }
  };

  useEffect(() => {
    return () => setIsCancelled(true);
  }, []);

  return { addDocument, deleteDocument, updateDocument, response };
};
```

```js
// ProjectComments.js
import { useState } from 'react';
import { timestamp } from '../../firebase/config';
import { useAuthContext } from '../../hooks/useAuthContext';
import { useFirestore } from '../../hooks/useFirestore';
import { v4 as uuidv4 } from 'uuid';
import Avatar from '../../components/Avatar';

export default function ProjectComments({ project }) {
  const { updateDocument, response } = useFirestore('projects');
  const [newComment, setNewComment] = useState();
  const { user } = useAuthContext();

  //add a new comment:
  const handleSubmit = async (e) => {
    e.preventDefault();

    const commentToAdd = {
      displayName: user.displayName,
      photoURL: user.photoURL,
      content: newComment,
      createdAt: timestamp.fromDate(new Date()),
      id: uuidv4(), //using uuid package to generate unique id
    };

    await updateDocument(project.id, {
      comments: [...project.comments, commentToAdd],
    });
    console.log(response);
    if (!response.error) {
      setNewComment('');
    }
  };

  return (
    <div className='project-comments'>
      <h4>Project Comments</h4>

      <ul>
        {project.comments.length > 0 &&
          project.comments.map((comment) => (
            <li key={comment.id}>
              <div className='comment-author'>
                <Avatar src={comment.photoURL} alt={comment.displayName} />
                <p>{comment.displayName}</p>
              </div>
              <div className='comment-date'>
                <p>date here</p>
              </div>
              <div className='comment-content'>
                <p>{comment.content}</p>
              </div>
            </li>
          ))}
      </ul>

      <form className='add-comment' onSubmit={handleSubmit}>
        <label>
          <span>Add new comment:</span>
          <textarea
            required
            onChange={(e) => setNewComment(e.target.value)}
            value={newComment}
          ></textarea>
        </label>
        <button className='btn'>Add Comment</button>
      </form>
    </div>
  );
}
```

26. Create a button on each Project to mark it as complete (in fact it will delete it from the DB). Conditionally show this button if the user is the user who created the project:

```js
// ProjectSummary.js
import { useHistory } from 'react-router-dom';
import { useFirestore } from '../../hooks/useFirestore';
import Avatar from '../../components/Avatar';
import { useAuthContext } from '../../hooks/useAuthContext';

export default function ProjectSummary({ project }) {
  const history = useHistory();
  const { deleteDocument } = useFirestore('projects');
  const { user } = useAuthContext();

  const handleClick = (e) => {
    deleteDocument(project.id);
    history.push('/');
  };

  return (
    <div>
      <div className='project-summary'>
        <h2 className='page-title'>{project.name}</h2>
        <p className='created-by'>by {project.createdBy.displayName}</p>
        <p className='due-date'>
          Project due by {project.dueDate.toDate().toDateString()}
        </p>
        <p className='details'>{project.details}</p>
        <h4>Project is assigned to:</h4>
        <div className='assigned-users'>
          {project.assignedUsersList.map((user) => (
            <div key={user.id}>
              <Avatar src={user.photoURL} alt={user.displayName} />
            </div>
          ))}
        </div>
      </div>
      {user.uid === project.createdBy.id && (
        <button className='btn' onClick={handleClick}>
          Mark as Complete
        </button>
      )}
    </div>
  );
}
```

27. Add a filter in the Dashboard to filter which projects will display on ProjectList.

```js
// Dashboard.js
import { useState } from 'react';
import ProjectList from '../../components/ProjectList';
import { useCollection } from '../../hooks/useCollection';
import './Dashboard.css';
import ProjectFilter from './ProjectFilter';
import { useAuthContext } from '../../hooks/useAuthContext';

export default function Dashboard() {
  const { documents, error } = useCollection('projects');
  const [currentFilter, setCurrentFilter] = useState('all');
  const { user } = useAuthContext();

  const changeFilter = (newFilter) => {
    setCurrentFilter(newFilter);
  };

  const projects = documents
    ? documents.filter((document) => {
        switch (currentFilter) {
          case 'all':
            return true;
          case 'mine':
            let assignedToMe = false;
            document.assignedUsersList.forEach((u) => {
              if (u.id === user.uid) {
                assignedToMe = true;
              }
            });
            return assignedToMe;
          case 'development':
          case 'design':
          case 'sales':
          case 'marketing':
            console.log(document.category, currentFilter);
            return document.category === currentFilter;
          default:
            return true;
        }
      })
    : null;

  return (
    <div>
      <h2 className='page-title'>Dashboard</h2>
      {error && <p className='error'>{error}</p>}
      {documents && (
        <ProjectFilter
          currentFilter={currentFilter}
          changeFilter={changeFilter}
        />
      )}
      {documents && <ProjectList projects={projects} />}
    </div>
  );
}
```

```js
//ProjectFilter.js
const filterList = [
  'all',
  'mine',
  'development',
  'design',
  'marketing',
  'sales',
];

export default function ProjectFilter({ currentFilter, changeFilter }) {
  const handleClick = (newFilter) => {
    changeFilter(newFilter);
  };

  return (
    <div className='project-filter'>
      <nav>
        <p>Filter by:</p>
        {filterList.map((f) => (
          <button
            key={f}
            onClick={() => handleClick(f)}
            className={currentFilter === f ? 'active' : ''}
          >
            {f}
          </button>
        ))}
      </nav>
    </div>
  );
}
```

28. Final touch: using `date-fns` package to show comment's dates in the format 'x minutes ago', '3 days ago', etc. `date-fns` is a date package that contains a function called `formatDistanceToNow` that does exactly that. Just pass 2 arguments: the date, and an options object { addSuffix: true } (this is to show words like "ago" etc).

```js
// ProjectComments.js
import { useState } from 'react';
import { timestamp } from '../../firebase/config';
import { useAuthContext } from '../../hooks/useAuthContext';
import { useFirestore } from '../../hooks/useFirestore';
import { v4 as uuidv4 } from 'uuid';
import Avatar from '../../components/Avatar';
import formatDistanceToNow from 'date-fns/formatDistanceToNow';

export default function ProjectComments({ project }) {
  const { updateDocument, response } = useFirestore('projects');
  const [newComment, setNewComment] = useState();
  const { user } = useAuthContext();

  //add a new comment:
  const handleSubmit = async (e) => {
    e.preventDefault();

    const commentToAdd = {
      displayName: user.displayName,
      photoURL: user.photoURL,
      content: newComment,
      createdAt: timestamp.fromDate(new Date()),
      id: uuidv4(), //using uuid package to generate unique id
    };

    await updateDocument(project.id, {
      comments: [...project.comments, commentToAdd],
    });
    console.log(response);
    if (!response.error) {
      setNewComment('');
    }
  };

  return (
    <div className='project-comments'>
      <h4>Project Comments</h4>

      <ul>
        {project.comments.length > 0 &&
          project.comments.map((comment) => (
            <li key={comment.id}>
              <div className='comment-author'>
                <Avatar src={comment.photoURL} alt={comment.displayName} />
                <p>{comment.displayName}</p>
              </div>
              <div className='comment-date'>
                <p>
                  {formatDistanceToNow(comment.createdAt.toDate(), {
                    addSuffix: true,
                  })}
                </p>
              </div>
              <div className='comment-content'>
                <p>{comment.content}</p>
              </div>
            </li>
          ))}
      </ul>

      <form className='add-comment' onSubmit={handleSubmit}>
        <label>
          <span>Add new comment:</span>
          <textarea
            required
            onChange={(e) => setNewComment(e.target.value)}
            value={newComment}
          ></textarea>
        </label>
        <button className='btn'>Add Comment</button>
      </form>
    </div>
  );
}
```

29. Add Firestore rules

```js
// firestore.rules
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{user_id} {
      allow read, create: if request.auth != null;
      allow update: if request.auth.uid == user_id;
    }
    match /projects/{project_id} {
      allow read, create, update: if request.auth != null;
      allow delete: if request.auth.uid == resource.data.createdBy.id;
    }
  }
}
// users collection
// - any authenticated user can read & create
// - only users who "own/created" a document can update it (user id's match): a user do it every time it logs in (the online status change to true)

// projects collection
// - any authenticated user can read, create & update a document
// - only users who "own/created" a document can delete it
```

29. Firestore Storage rules
    We can set the storage.rules so that only authenticated users can read or write on our cloud storage in Firebase.

To deploy: `firestore deploy --only firestore:rules`

```js
// storage.rules
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

30. Deploy the app.
    First, build: `npm run build`
    Then, deploy to Firebase: `firebase deploy`
