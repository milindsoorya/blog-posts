## Tips For Using Async/Await - Write Better JavaScript!


## Basic Async/Await

Things to note -
There are two parts to using `async/await` in your code.
First of all, we have the `async` keyword, which you put in front of a function declaration to turn it into an async function. An async function is a function that knows how to expect the possibility of the `await` keyword being used to invoke asynchronous code.

```jsx
const loadData = async () => {
  const url = "https://jsonplaceholder.typicode.com/todos/1";
  const res = await fetch(url);
  const data = await res.json();
  console.log(data);
};

loadData();
```

```jsx
// Console output
{
  completed: false,
  id: 1,
  title: "delectus aut autem",
  userId: 1
}
```

## Async/Await with error handling

We can handle errors using a try catch block.

```jsx
const loadData = async () => {
  try{
	  const url = "https://jsonplaceholder.typicode.com/todos/1";
	  const res = await fetch(url);
	  const data = await res.json();
	  console.log(data);
  } catch(err) {
    console.log(err)
  }
};

loadData();
```

Things to note - The above try-catch will only handle error while fetching data such as wrong syntax, wrong domain names, network error etc. For the situation in which we want to handle an error message from the API response code, we can use `res.ok`, It will give a Boolean with the value true if the response code is between 200 and 209.

```jsx
const loadData = async () => {
  try{
	  const url = "https://jsonplaceholder.typicode.com/todos/qwe1";
	  const res = await fetch(url);
	  if(res.ok){ 
	    const data = await res.json();
	    console.log(data);
	  } else {
	    console.log(res.status); // 404
	  }
  } catch(err) {
    console.log(err)
  }
};

loadData();

// OUTPUT
// 404
```

## Async function returns a promise

This is one of the traits of async functions — their return values are guaranteed to be converted to promises. To handle data returned from an `async` function we can use a `then` keyword to get the data. 

```jsx
const loadData = async () => {
  try{
    const url = "https://jsonplaceholder.typicode.com/todos/1";
    const res = await fetch(url);
    const data = await res.json();
    return data
  } catch(err) {
    console.log(err)
  }
};

const data = loadData().then(data => console.log(data));
```

**💡 PRO TIP :**
if you want to use an `async-await` to handle the returned data you can make use of an IIFE, but it is only available in Node 14.8 or higher.

```jsx
// use an async IIFE
(async () => {
  const data = await loadData();
  console.log(data);
})();
```

await only works inside async functions within regular JavaScript code, however it can be used on its own with [JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules).

## Promise.all()

`Promise.all()` comes in handy when we want to call multiple API's. Using a traditional `await` method we have to wait for each request to be completed before going to the next request. The problem occurs when each request takes some time to complete, this can easily add up and make the experience slower. Using `Promise.all()` we can call each of these API's in parallel. (it is an oversimplification, for more details checkout [this](https://anotherdev.xyz/promise-all-runs-in-parallel/#:~:text=all%20doesn't%20guarantee%20you,are%20done%20with%20their%20job.) amazing article).

```jsx
const loadData = async () => {
  try{
    const url1 = "https://jsonplaceholder.typicode.com/todos/1";
    const url2 = "https://jsonplaceholder.typicode.com/todos/2";
    const url3 = "https://jsonplaceholder.typicode.com/todos/3";
    const results = await Promise.all([
      fetch(url1),
      fetch(url2),
      fetch(url3)
    ]);
    const dataPromises = await results.map(result => result.json());
    const finalData = Promise.all(dataPromises);
    return finalData
  } catch(err) {
    console.log(err)
  }
};

const data = loadData().then(data => console.log(data));
```

```jsx
// Console output
[{
  completed: false,
  id: 1,
  title: "delectus aut autem",
  userId: 1
}, {
  completed: false,
  id: 2,
  title: "quis ut nam facilis et officia qui",
  userId: 1
}, {
  completed: false,
  id: 3,
  title: "fugiat veniam minus",
  userId: 1
}]
```
 