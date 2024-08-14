# Javascript

## Variables
### `var` and `let`
```javascript
var varA = "varA"; // Global Scope
let letA = "letA"; // Global Scope

function globalAccess() {
  console.log(varA); // Accessible anywhere
  console.log(letA); // Accessible anywhere
}
globalAccess();

function outerFunc() {
  if (1 < 2) {
    // Curly bracket defines a block
    var varB = "varB"; // Function Scope
    let letB = "letB"; // Block Scope
  }
  console.log(varB); // varB is accessible
  console.log(letB); // Error, letB is NOT accessible
}
outerFunc();
```

### Template string
embed string variable into template string
```javascript
var newStr = `${myStr} 123`;
```
## Functions
### Anonymous functions
#### Declaration
```javascript
function (params) {
  //function body
  return output;
}
```
or
```javascript
(params) => {
  //function body
  return output;
};
```
### Higher order functions

#### Functions as inputs

We have seen how functions are used in JavaScript in the previous unit. We consider some advanced usage of functions.

```js
function incr(x) {
  return x + 1;
}

function twice(f, x) {
  return f(f(x));
}

console.log(twice(incr, 10)); // 12
```

In the above we define a function `incr` which increments a numerical input by `1`. Then we define a function `twice`, which takes a function argument `f` and another argument `x`. We apply `f` to `x` to produce an intermediate result then applies `f` again to the intermediate result.

`twice` is known as a higher order function as it takes another function as its input argument. Using higher order functions allows us to reuse codes.

```js
function dbl(x) {
  return x * 2;
}

console.log(twice(dbl, 10)); // 40
```
### Functions as a returned value (Closure)

Look at the code below and try to answer the question in comment:

```js
function aFunc(a) {
  b = a * 5;
}
aFunc(2); // a=2, b=10

// Question: Is it possible to access or interact with the variables a and b after the funcion is invoked above?
```

The answer is no. However, there is a way achieve it.

We can define a function that returns another function.

```js
function part_sum(x) {
  return function (y) {
    return x + y;
  };
}

function twice(f, x) {
  return f(f(x));
}

var incr_too = part_sum(1);
console.log(twice(incr_too, 10)); // 12
```

In the above, we define a partial sum function, `part_sum` which takes an input `x` and returns an anonymous function that expects input `y`. The inner function returns the sum of `x` and `y`. `incr_too` is a reference to the anonymous function produced by `part_sum(1)` that expects `y` and returns `1 + y`.

In this case, `part_sum` is a higher order function.

#### Closure

A closure in JavaScript is an inner function having access to its outer function's state/variables. In the example above, the inner function is a _closure_ because the function is still having a reference to the variable `x` even after `part_sum(1)` has been invoked and returned. You can 'interact' with the variable `x` by invoking the `incr_too`.

### Functions as call-backs

_Function call-backs_, also known as continuations, turn out to be very useful for us to define operations that should be performed in sequence. For instance,

```js
var x = 1;
var y = incr(x);
var z = dbl(y);
console.log(z);
```

In the above we would like to increment `x` by `1` then multiply the intermediate result `y` by `2`. With call-backs, we could rewrite the above as

```js
function incr_with_callback(x, f) {
  var r = incr(x);
  f(r);
}

function dbl_with_callback(y, g) {
  var r = dbl(y);
  g(r);
}
var x = 1;
incr_with_callback(x, function (y) { // anonymous functions
  dbl_with_callback(y, function (z) {
    console.log(z);
  });
});
```

### Callback Queue

Things become more interesting in the presence of call-back functions associated with some UI elements. Recall one of our earlier examples,

```js
1: function handleButton1Click() {
2:   var textbox1 = document.getElementById("textbox1");
3:   var span1 = document.getElementById("span1");
4:   span1.innerHTML = textbox1.value;
5: }
6: function run() {
7:   var button1 = document.getElementById("button1");
8:   button1.addEventListener("click", handleButton1Click);
9: }
10: document.addEventListener("DOMContentLoaded", run);
```

| line num | call stack | event reg table           | callback queue |
| -------- | ---------- | ------------------------- | -------------- |
| 1        | \[main]    | {}                        | \[]            |
| 6        | \[main]    | {}                        | \[]            |
| 10       | \[main]    | {`DomContentLoaded : run} | \[]            |

When the HTML document is fully rendered, the browser triggers an `DomContentLoaded` event. At this point the callback function `run` associated with the event is enqueued to the callback queue. Since the call stack is empty and the callback queue is not, the JavaScript run-time dequeues `run` from the callback queue and creates a call frame in the call stack. It continues to move the program counter from lines 7-9.

### Promise

Promise is a builtin class in JavaScript, which allows us to build a sequence of asynchronous tasks without nesting call-backs.

A `Promise` object can be instantiated by passing in a function as argument. This function argument is called the **executor**. An executor function is a higher order function that takes two functions as arguments, commonly named as `resolve` and `reject`. `resolve` is applied to the result of the promise during normal execution, `reject` is applied when some error occurs. The function body of executor is executed synchronously while `resolve` and `reject` are **asynchronous and queued into microtask**. Look at the simple `Promise` example below which is not using `reject`, but only `resolve`.

```js
function asyncCounter() {
  var count = 0;
  return new Promise((resolve, reject) => {
    resolve(count);
  });
}

function incr(count) {
  console.log(count);
  return new Promise((resolve, reject) => {
    resolve(++count);
  });
}

let counter = asyncCounter();

counter.then(incr).then(incr).then(incr);

/*
output:
0
1
2
*/
```

Explanation:

- In the above we instantiate an asynchronous counter by calling the function `asyncCounter()`, which initializes a local variable `count` and return a promise object that immediately `resolve` with the variable.
- Do you notice that we use [closure](https://sutd50003.github.io/notes/l4_1_web_prog_common_js/#Closure) here?
- Next we invoke `.then` method of this counter (promise) three times. We can chain the invocation because, `.then` method takes a method and **returns a new promise**. In this case, we use the `incr` function as the argument of `.then`. In the `incr` function, we print the `count` variable and return the new promise while incrementing `count` by 1.
- **Fun-but-important fact**: you can simply write the return statement of `incr()` function as `return ++count;`. JavaScript intrisically turn the returned value as a promise object resolved with returned value. We use this approach sometimes in the notes.

In node.js runtime model, there are two callback queues:

1. The macro task queue, which is same as the callback queue that stores callback associated with events.
2. The micro task queue, which stores callbacks associated with promises.
Given that promises are special builtin of node.js run-time, they are treated "differently" when executed. When a promise is instantiated, its executor function is executed upto the `resolve(...)` (or `reject(...)`) statement. The call of `resolve(...)` (or `reject(...)`) is enqueued into the microtask queue. In an event loop cycle, when the call stack is empty, the run-time checks whether the microtask queue contains any item before checking the macrotask queue.
## Loops
##### For-in and For-of

We could rewrite the for-loop in the following

```javascript
for (let i in kvs) {
  sum = sum + kvs[i][1];
}
```

Notice that `i` is the keys of object `kvs`. `for-in` statement will loop through indices if the given iterable is an array.

Alternatively,

```javascript
for (let kv of kvs) {
  sum = sum + kv[1];
}
```

In this case `kv` denotes the values from the array `kvs`. `for-of` statement will loop through each item if the given iterable is an array.s

#### forEach and map

Given an array, we can use `.forEach(p)` to apply function `p` to each element in the array.

```js
kvs.forEach(function (kv) {
  sum = sum + kv[1];
});
```

In the above we pass anonymous function to add `kv[1]` to the `sum` variable. Note that the function provided is a function without return statement.

If we want to apply a function `f` to each element of the array to produce a new array, we use `.map(f)`. The function provided here has a return statement.

```javascript
var kvs2 = kvs.map(function (kv) {
  return [kv[0].toUpperCase(), kv[1]];
});
kvs2;
// [['APPLE', 100], ['ORANGE', 50], ['DURIAN', 200]]
```

Note that in JavaScript there is no tuple datatype.

## Classes
### Inheritance (Only available in ES6 onwards)

Since JavaScript version ES6, we can define subclasses using the `extends` keyword.

```javascript
class Square extends Rect {
  constructor(s) {
    super(s, s);
    this.side = s;
  }
}

var s = new Square(10);
console.log(s.area());
```

With inheritence objects instantiated from a subclass inherits methods and attributes belong to the base classes.

## Nodejs event loop
```
   ┌───────────────────────────┐
┌─>│           timers          │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │
│  └─────────────┬─────────────┘      ┌───────────────┐
│  ┌─────────────┴─────────────┐      │   incoming:   │
│  │           poll            │<─────┤  connections, │
│  └─────────────┬─────────────┘      │   data, etc.  │
│  ┌─────────────┴─────────────┐      └───────────────┘
│  │           check           │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │
   └───────────────────────────┘
```
Important points to note in this section are:

1. There are several commonly used APIs that allow NodeJS to execute instructions asynchronously such as `setTimeout()`, `setInterval()`, `setImmediate()`, and I/O related tasks (for example, `fs.writeFile()`, HTTP Request and Response, or `fetch()` )
2. The JS in browser's [callback queue](https://sutd50003.github.io/notes/l4_1_web_prog_common_js/#Callback%20Queue) that we know before is called **macrotask** queue in nodeJS runtime. Except `fetch()`, all the APIs mentioned in the previous points enqueue callbacks into macrotask.
3. Another type of callback queue are called **microtask**. Microtask has a higher priority than macrotask, meaning that all existing microtasks will be cleared before event-loop dequeue the next macrotask.
4. Callbacks that are enqueued into microtask are `resolve()` or `reject()` functions from `promise` and API `queueMicrotask()`.
5. Now we will discuss I/O APIs callbacks (the 2nd Phase, and 4th Phase), `fetch()`, and `promise` in the next sections. You can explore the other APIs on your own.

# React
Issues with the ExpressJS + AJAX approach
1. routes / controllers for view rendering and AJAX calls are intertwined
2. client side JS in the views are not so reusable and not so testable
![[Pasted image 20240813141907.png]]
front end web app detaches the view components from a monolith webapp.
### CORS
1. modify the router code at the backend app, i.e. `my_mysql_app`.
2. add the following statement before the `res.send()` is called `res.set("Access-Control-Allow-Origin", "http://localhost:3001");`
## React Components
`Component` is the building block of React app.
In `./src/Echo.js`
```js
function NewMessageBar({ message, onSubmitClick }) {
  return (
    <div>
      <input type="text" placeholder="" value={message}></input>
      <button onClick={onSubmitClick}> Submit </button>
    </div>
  );
}

function MessageList({ messages }) {
  let rows = [];
  for (let i in messages) {
    rows.push(
      <tr>
        <td>{messages[i].time}</td>
        <td>{messages[i].msg}</td>
      </tr>
    );
  }
  return (
    <table>
      <tbody>
        <tr>
          <th>Date Time</th>
          <th>Message</th>
        </tr>
        {rows}
      </tbody>
    </table>
  );
}

function Echo() {
  const messages = [
    { time: new Date().toDateString(), msg: "hello" },
    { time: new Date().toDateString(), msg: "bye" },
  ];
  return (
    <div>
      <NewMessageBar message="" onSubmitClick={() => {}} />
      <MessageList messages={messages} />
    </div>
  );
}

export default Echo;
```
In `App.js`
```js
import logo from "./logo.svg";
import "./App.css";
import Echo from "./Echo";

function App() {
  return (
    <div>
      <h1> Echo App </h1>
      <Echo />
    </div>
  );
}

export default App;
```

In the above version of `Echo.js`, we define two sub components, namely `NewMessageBar` and `MessageList`. Note that both functions take some objects as the arguments, these objects arguments are referred as `props` in React's terminologies. Refer [here](https://react.dev/learn/passing-props-to-a-component) for more details about `props` in react.

- `NewMessageBar()` takes an object with two attributes, `message` and `onSubmitClick`. `message` stores the text value in the text field and `onSubmitClick` is a callback when the `Submit` button is clicked. The function returns a text input field and a button `Submit`.
- `MessageList()` takes an object with an attribute, `messages` which is a list of message objects. It returns an HTML table whose rows are constructed by mapping each message in the list to a row of the table.
- `Echo()`, we hard code the data (which is supposed to be returned by the AJAX call to the backend, will be implemented later), and pass them to `MessageList` component during the component construction.
#### Props
As shown in the earlier example, props are the objects arguments being passed to the constructor of a React Component or function argument enclosed with curly brackets. Props carried data and information passed down from the use-site, (or the parent component).
## React Lifecycle
![[Pasted image 20240813144923.png]]
## React Hooks
### `useEffect`
**Summary:** `useEffect` is a hook for performing side effects in function components. It runs after every render by default, but you can control when it runs by specifying dependencies
**Usage:**
```js
useEffect(() => { // side effect logic 
}, [dependencies]);
```
You need to pass two arguments to `useEffect`:
1. A _setup function_ with setup code that connects to that system.
    - It should return a _cleanup function_ with cleanup code that disconnects from that system.
2. A list of dependencies including every value from your component used inside of those functions.
If the 2nd argument is an empty list `[]`. the call-back will only be triggered when the component is mounted.

### UseState
```js
function Echo() {
  const [msgTxt, setMsgTxt] = useState("");
  function handleSubmitClick() {
    alert("clicked " + msgTxt);
  }
  const messages = [
    { time: new Date().toDateString(), msg: "hello" },
    { time: new Date().toDateString(), msg: "bye" },
  ];
  return (
    <div>
      <NewMessageBar
        message={msgTxt}
        onMessageChange={setMsgTxt}
        onSubmitClick={handleSubmitClick}
      />
      <MessageList messages={messages} />
    </div>
  );
}
```
The `useState(initialState)` is one of React Hooks which allows you to have a getter and setter without writing a class. This function returns two values (in the previous example, the two values are captured in `msgTxt` and `setMsgTxt`)

# Software Testing
A test in a software system is an act to exercise software with test cases. A test have two goals
1. to find faults.
2. to show that the software behaves according to expectation (specification), i.e. to build confidence.

A test case is a formal documentation of a test. A test case consists of
- an identifier (often linked to the user case identifier).
- a description of the purpose and the use case description being tested.
- a precondition
- a set of inputs
- the expected output
- the expected postcondition
- the execution history (the log)
## Example 
```js
describe("FibSeq class test with setup and tear down", () =>{
    let fibSeq = null;4
    beforeEach(() => {
        fibSeq = new FibSeq();
    });
    test ("first fib num is 1 after reset", () => {
        const result = fibSeq.next();
        expect(result).toBe(1);
    });
    test ("second fib num is 1 after reset", () => {
        const result = fibSeq.next();
        expect(result).toBe(1);
    });
    afterEach(() => {
        fibSeq = null;
    })
})
```

Types of testing:
- Specification based testing
- Code based testing

## UI testing
There are two main objectives of conducting graphical user interface tests.
1. To ensure functional correctness.
2. To ensure visual correctness.
Functional Test Example:
```js
test('No message is rendered in empty MessageList', () => {
    const msgs = [];
    render(<MessageList messages={msgs} />);
    const table = screen.getByTestId("message-list");
    expect(table).toBeInTheDocument(); // the table must be rendered.
    expect(table.firstElementChild.children.length == 1); // only contains the header row.
});
```
```js
test('A message is rendered in a singleton MessageList', () => {
    const msgTxt = "hello";
    const msgTime = (new Date()).toString();
    const msg = { time : msgTime, msg: msgTxt };
    const msgs = [msg];
    render(<MessageList 
            messages={msgs} />);
    const table = screen.getByTestId("message-list");
    const row = screen.getByTestId(msgTime);
    expect(table).toBeInTheDocument(); // the table must be rendered.
    expect(row).toBeInTheDocument(); // the row must be rendered.
    expect(table.contains(row));
});
```
### UI-based end-to-end testing

An end-to-end (E2E) testing is to assess the built system's feature from the user's perspective. All the components in the system supporting that particular feature are actively tested, i.e. no mocking.  
An end-to-end test case is often derived directly from a use case.

To automate an end-to-end test, we can use test framework like Jest. For simple system feature like our Echo App. We could define our test as follows (similar to the `Echo.test.js` except that we don't require the mocking.)

```js
function getRandomInt(max) {
  return Math.floor(Math.random() * max);
}

test('End-to-end testing on App', async () => {
  const msgTxt = "test message" + getRandomInt(1000);
  render(<App />);
  const textbox = screen.getByLabelText('echo-message');
  const submitButton = screen.getByText(/submit/i); 
  fireEvent.change(textbox, {target: {value: msgTxt}});
  await userEvent.click(submitButton);
  expect(textbox.value).toBe(msgTxt);
  await waitFor( () => {
      const table = screen.getByTestId("message-list");
      fireEvent.change(textbox, {target: {value: ''}});
      const text = screen.getByText(msgTxt);
      expect(table).toBeInTheDocument(); // the table must be rendered.
      expect(text).toBeInTheDocument(); // the text must be rendered.
      expect(table.contains(text));
  }); 
})
```

## Code-based testing

### MCDC Coverage
MCDC stands for Modified Condition Decision Coverage.

MCDC requires
1. Each statement must be executed at least once.
2. Every program entry point and exit point must be invoked at least once.
3. All possible outcomes of every control statement are taken at least once.
4. Every nonconstant Boolean expression has been evaluated to both true and false outcomes. A boolean expression is constant iff it is a tautology, e.g. `a==a`, `a!=a` and `p && !p`
5. Every nonconstant condition in a Boolean expression has been evaluated to both true and false outcomes.
6. Every nonconstant condition in a Boolean expression has been shown to independently affect the outcomes (of the expression).

### Path-testing
To conduct path testing, a general approach is
- to generate enough test cases cover all paths with condensation (if possible).
- in case of complex condition expression (esp with dependent condition expressions), we should construct a decision table to check impossibilities and generate additional test cases to ensure condition coverage.
Number of unique paths: $V(G) = e - n + 2p$ where $e$ is the number of edges and $n$ is the number of nodes and $p$ denotes the number of connected components in $G$. Since we are dealing with one program at a time, the number of connected component is 1.

## Fuzzing
Fuzzing or fuzz testing is an automated software testing technique that involves providing invalid, unexpected, or random data as inputs to a computer program.
Fuzzing aims to identify test inputs which reveal exploitable vulnerabilities.
One downside for fuzzing is that it can't generate the expected outputs. This drawback is minor since for most of the fuzzing test cases, error should be expected outputs.
Besides test case generation automation, there are several extra reasons why fuzzing should be considered.
- A study found that one-quarter to one-third of all utilities on every UNIX system that the evaluators could get their hands on would crash in the presence of random input.
- A study that looked at 30 different Windows applications found that 21% of programs crashed and 24% hung when sent random mouse and keyboard input, and every single application crashed or hung when sent random Windows event messages.
- A study found that OS X applications, including Acrobat Reader, Apple Mail, Firefox, iChat, iTunes, MS Office, Opera, and Xcode, were even worse than the Windows ones.
### Mutation based fuzzing
One of the possible ways to overcome the limitation with the random testing is that we could consider generating fuzzy test cases based on known valid test inputs.
For instance, assuming that we note that one of the valid inputs to the test subject is the following string
```
ISTDisApillarInSUTDbutItsNameiSGoingToChange
```
One possible way to mutate it is to choose a character at a random position and replace it by ssomething else.
For instance, we randoml choose position (say 13) and replace the character `I` by character `Y`
```
ISTDisApillarYnSUTDbutItsNameiSGoingToChange
```
The above is consdered an mutant of the original input.
Here are some basic operations for mutation at some randomly chosen location.
- Flipping a bit/boolean/integer/character
- Trimming
- Swapping characters/bits
- Insert characters

# Fixing DB Race Condition
In a nutshell, SQL statements enclosed by `BEGIN TRANSACTION; ... COMMIT;` are to be executed by the datbase systems atomically and in isolation. Informally speaking, the enclosed statements should not be interleaved with other statements that cause a racing condition. In the event a racing condition is detected, a database exception is raised and the query statements are rejected. A retry or an abort is required.
```js
async function update(time, appendMsg) {
    let connection = null; 
    try {
        connection = await db.pool.getConnection();
        await connection.query(
             "SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE" 
        ); 
        await connection.query("START TRANSACTION");    
        const [rows, fieldDefs] = await connection.query(`
            SELECT * FROM ${tableName} where time = ?`, [time]
        );
        if (rows.length > 0) {
            var message = new Message(rows[0].msg, rows[0].time)
            await connection.query(`
                UPDATE ${tableName} SET msg = ? WHERE time = ?`, [message.msg + appendMsg, message.time]
            );
        }
        await connection.query("COMMIT");
    } catch (error) {
        if (connection) {
            await connection.query("ROLLBACK");
            await connection.release();
        }
        console.error("database connection failed. " + error);
        throw(error);
    } finally {
        if (connection) {
            await connection.release();
        }    
    }
}
```
1. we instantiate a specific connection object from the `db.pool` object. We replace all the use of `db.pool.query()` by `connection.query()`.
    > If we use `db.pool.query()` directly, each time a new connection is instantiated and there is no way to ensure the set of queries are enclosed in the same transaction block.
2. We then declare the transaction isolation level. `SERIALIZABLE` means any READ-WRITE operation pairs applied on the same table from different transactions are considered _in-conflict_. 
3. We enclose the critical section with the `START TRANSACTION` and `COMMIT`. In case of a conflict is detected, we rollback the transaction.