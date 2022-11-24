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

## useEffects, Reduce & ContextAPI

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

## React Route

Working on react-recipe-directory repo (NN section 11)

### BrowserRouter, Router, Link, NavLink, Switch components

Whenever we want to selectively output a component when clicking a link, instead of using anchor tags, we will use **Link** and **NavLink** components.
The paths of these Links and NavLinks must be surrounded by a **Router** component on the root file. The Router component must be surrounded by a **Switch** component to tell React to open one of those components selectively. The **exact** prop/attribute must be used to ensure the "/" path doesn't conflict with other paths that begin with "/".
The difference between Link and NavLink is that, using NavLink, it automatically adds classes like .active on our links, these classes can be easily manipulated through CSS.

```js
npm i react-route-dom

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
  const { mm } = useParams();
  const url = 'http://localhost:3000/articles/' + mm;
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

### Using queries

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
