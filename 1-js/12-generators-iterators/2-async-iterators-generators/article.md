
# Async iterators and generators

<<<<<<< HEAD
Asynchronous iterators allow to iterate over data that comes asynchronously, on-demand.
=======
Asynchronous iterators allow us to iterate over data that comes asynchronously, on-demand. Like, for instance, when we download something chunk-by-chunk over a network. And asynchronous generators make it even more convenient.
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc

For instance, when we download something chunk-by-chunk, or just expect events to come asynchronously and would like to iterate over them -- async iterators and generators may come in handy. Let's see a simple example first, to grasp the syntax, and then review a real-life use case.

## Async iterators

Asynchronous iterators are totally similar to regular iterators, with a few syntactic differences.

<<<<<<< HEAD
"Regular" iterable object from the chapter <info:iterable> look like this:
=======
A "regular" iterable object, as described in the chapter <info:iterable>, looks like this:
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc

```js run
let range = {
  from: 1,
  to: 5,

  // for..of calls this method once in the very beginning
*!*
  [Symbol.iterator]() {
*/!*
    // ...it returns the iterator object:
<<<<<<< HEAD
    // onward, for..of works only with that object, asking it for next values
=======
    // onward, for..of works only with that object,
    // asking it for next values using next()
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc
    return {
      current: this.from,
      last: this.to,

      // next() is called on each iteration by the for..of loop
*!*
      next() { // (2)
        // it should return the value as an object {done:.., value :...}
*/!*
        if (this.current <= this.last) {
          return { done: false, value: this.current++ };
        } else {
          return { done: true };
        }
      }
    };
  }
};

for(let value of range) {
  alert(value); // 1 then 2, then 3, then 4, then 5
}
```

If necessary, please refer to the [chapter about iterables](info:iterable) for details about regular iterators.

To make the object iterable asynchronously:
1. We need to use `Symbol.asyncIterator` instead of `Symbol.iterator`.
2. `next()` should return a promise.
3. To iterate over such an object, we should use a `for await (let item of iterable)` loop.

Let's make an iterable `range` object, like the one before, but now it will return values asynchronously, one per second:

```js run
let range = {
  from: 1,
  to: 5,

  // for await..of calls this method once in the very beginning
*!*
  [Symbol.asyncIterator]() { // (1)
*/!*
    // ...it returns the iterator object:
    // onward, for await..of works only with that object, asking it for next values
    return {
      current: this.from,
      last: this.to,

      // next() is called on each iteration by the for await..of loop
*!*
      async next() { // (2)
        // it should return the value as an object {done:.., value :...}
        // (automatically wrapped into a promise by async)
*/!*

*!*
        // can use await inside, do async stuff:
        await new Promise(resolve => setTimeout(resolve, 1000)); // (3)
*/!*

        if (this.current <= this.last) {
          return { done: false, value: this.current++ };
        } else {
          return { done: true };
        }
      }
    };
  }
};

(async () => {

*!*
  for await (let value of range) { // (4)
    alert(value); // 1,2,3,4,5
  }
*/!*

})()
```

As we can see, the components are similar to regular iterators:

1. To make an object asynchronously iterable, it must have a method `Symbol.asyncIterator` `(1)`.
<<<<<<< HEAD
2. It must return the object with `next()` method returning a promise `(2)`.
3. The `next()` method doesn't have to be `async`, it may be a regular method returning a promise, but `async` allows to use `await` inside. Here we just delay for a second `(3)`.
=======
2. This method must return the object with `next()` method returning a promise `(2)`.
3. The `next()` method doesn't have to be `async`, it may be a regular method returning a promise, but `async` allows us to use `await`, so that's convenient. Here we just delay for a second `(3)`.
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc
4. To iterate, we use `for await(let value of range)` `(4)`, namely add "await" after "for". It calls `range[Symbol.asyncIterator]()` once, and then its `next()` for values.

Here's a small cheatsheet:

|       | Iterators | Async iterators |
|-------|-----------|-----------------|
| Object method to provide iteraterable | `Symbol.iterator` | `Symbol.asyncIterator` |
| `next()` return value is              | any value         | `Promise`  |
| to loop, use                          | `for..of`         | `for await..of` |

<<<<<<< HEAD

````warn header="The spread operator doesn't work asynchronously"
=======
````warn header="The spread syntax `...` doesn't work asynchronously"
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc
Features that require regular, synchronous iterators, don't work with asynchronous ones.

For instance, a spread syntax won't work:
```js
alert( [...range] ); // Error, no Symbol.iterator
```

That's natural, as it expects to find `Symbol.iterator`, same as `for..of` without `await`.
````

## Async generators

JavaScript also provides generators, that are also iterable.

Let's recall a sequence generator from the chapter [](info:generators). It generates a sequence of values from `start` to `end` (could be anything else):

```js run
function* generateSequence(start, end) {
  for (let i = start; i <= end; i++) {
    yield i;
  }
}

for(let value of generateSequence(1, 5)) {
  alert(value); // 1, then 2, then 3, then 4, then 5
}
```


Normally, we can't use `await` in generators. All values must come synchronously: there's no place for delay in `for..of`.

But what if we need to use `await` in the generator body? To perform network requests, for instance.

No problem, just prepend it with `async`, like this:

```js run
*!*async*/!* function* generateSequence(start, end) {

  for (let i = start; i <= end; i++) {

*!*
    // yay, can use await!
    await new Promise(resolve => setTimeout(resolve, 1000));
*/!*

    yield i;
  }

}

(async () => {

  let generator = generateSequence(1, 5);
  for *!*await*/!* (let value of generator) {
    alert(value); // 1, then 2, then 3, then 4, then 5
  }

})();
```

Now we have an the async generator, iteratable with `for await...of`.

It's indeed very simple. We add the `async` keyword, and the generator now can use `await` inside of it, rely on promises and other async functions.

Technically, another difference of an async generator is that its `generator.next()` method is now asynchronous also, it returns promises.

Instead of `result = generator.next()` for a regular, non-async generator, values can be obtained like this:

```js
result = await generator.next(); // result = {value: ..., done: true/false}
```

## Iterables via async generators

When we'd like to make an object iterable, we should add `Symbol.iterator` to it.

```js
let range = {
  from: 1,
  to: 5,
*!*
  [Symbol.iterator]() { ...return object with next to make range iterable...  }
*/!*
}
```

A common practice for `Symbol.iterator` is to return a generator, rather than a plain object with `next` as in the example before.

Let's recall an example from the chapter [](info:generators):

```js run
let range = {
  from: 1,
  to: 5,

  *[Symbol.iterator]() { // a shorthand for [Symbol.iterator]: function*()
    for(let value = this.from; value <= this.to; value++) {
      yield value;
    }
  }
};

for(let value of range) {
  alert(value); // 1, then 2, then 3, then 4, then 5
}
```

Here a custom object `range` is iterable, and the generator `*[Symbol.iterator]` implements the logic for listing values.

If we'd like to add async actions into the generator, then we should replace `Symbol.iterator` with async `Symbol.asyncIterator`:

```js run
let range = {
  from: 1,
  to: 5,

*!*
  async *[Symbol.asyncIterator]() { // same as [Symbol.asyncIterator]: async function*()
*/!*
    for(let value = this.from; value <= this.to; value++) {

      // make a pause between values, wait for something  
      await new Promise(resolve => setTimeout(resolve, 1000));

      yield value;
    }
  }
};

(async () => {

  for *!*await*/!* (let value of range) {
    alert(value); // 1, then 2, then 3, then 4, then 5
  }

})();
```

Now values come with a delay of 1 second between them.

## Real-life example

So far we've seen simple examples, to gain basic understanding. Now let's review a real-life use case.

<<<<<<< HEAD
There are many online APIs that deliver paginated data. For instance, when we need a list of users, then we can fetch it page-by-page: a request returns a pre-defined count (e.g. 100 users), and provides an URL to the next page.
=======
There are many online services that deliver paginated data. For instance, when we need a list of users, a request returns a pre-defined count (e.g. 100 users) - "one page", and provides a URL to the next page.
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc

This pattern is very common. It's not about users, but just about anything. For instance, GitHub allows us to retrieve commits in the same, paginated fashion:

- We should make a request to URL in the form `https://api.github.com/repos/<repo>/commits`.
- It responds with a JSON of 30 commits, and also provides a link to the next page in the `Link` header.
- Then we can use that link for the next request, to get more commits, and so on.

<<<<<<< HEAD
What we'd like to have is an iterable source of commits, so that we could use it like this:
=======
But we'd like to have a simpler API: an iterable object with commits, so that we could go over them like this:
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc

```js
let repo = 'javascript-tutorial/en.javascript.info'; // GitHub repository to get commits from

for await (let commit of fetchCommits(repo)) {
  // process commit
}
```

<<<<<<< HEAD
<<<<<<< HEAD
We'd like `fetchCommits` to get commits for us, making requests whenever needed. And let it care about all pagination stuff, for us it'll be a simple `for await..of`.
=======
We'd like to make a function `fetchCommits(repo)` that gets commits for us, making requests whenever needed. And let it care about all pagination stuff, for us it'll be a simple `for await..of`.
>>>>>>> 852ee189170d9022f67ab6d387aeae76810b5923
=======
We'd like to make a function `fetchCommits(repo)` that gets commits for us, making requests whenever needed. And let it care about all pagination stuff. For us it'll be a simple `for await..of`.
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc

With async generators that's pretty easy to implement:

```js
async function* fetchCommits(repo) {
  let url = `https://api.github.com/repos/${repo}/commits`;

  while (url) {
    const response = await fetch(url, { // (1)
      headers: {'User-Agent': 'Our script'}, // github requires user-agent header
    });

    const body = await response.json(); // (2) parses response as JSON (array of commits)

    // (3) the URL of the next page is in the headers, extract it
    let nextPage = response.headers.get('Link').match(/<(.*?)>; rel="next"/);
    nextPage = nextPage?.[1];

    url = nextPage;

    for(let commit of body) { // (4) yield commits one by one, until the page ends
      yield commit;
    }
  }
}
```

<<<<<<< HEAD
1. We use the browser `fetch` method to download from a remote URL. It allows to supply authorization and other headers if needed, here GitHub requires `User-Agent`.
2. The fetch result is parsed as JSON, that's again a `fetch`-specific method.
3. We can get the next page URL from the `Link` header of the response. It has a special format, so we use a regexp for that. The next page URL may look like this: `https://api.github.com/repositories/93253246/commits?page=2`, it's generatd by GitHub itself.
4. Then we yield all commits received, and when they finish -- the next `while(url)` iteration will trigger, making one more request.
=======
1. We use the browser [fetch](info:fetch) method to download from a remote URL. It allows us to supply authorization and other headers if needed -- here GitHub requires `User-Agent`.
2. The fetch result is parsed as JSON. That's again a `fetch`-specific method.
3. We should get the next page URL from the `Link` header of the response. It has a special format, so we use a regexp for that. The next page URL may look like `https://api.github.com/repositories/93253246/commits?page=2`. It's generated by GitHub itself.
4. Then we yield all commits received, and when they finish, the next `while(url)` iteration will trigger, making one more request.
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc

An example of use (shows commit authors in console):

```js run
(async () => {

  let count = 0;

  for await (const commit of fetchCommits('javascript-tutorial/en.javascript.info')) {

    console.log(commit.author.login);

    if (++count == 100) { // let's stop at 100 commits
      break;
    }
  }

})();
```

That's just what we wanted. The internal pagination mechanics is invisible from the outside. For us it's just an async generator that returns commits.

## Summary

Regular iterators and generators work fine with the data that doesn't take time to generate.

When we expect the data to come asynchronously, with delays, their async counterparts can be used, and `for await..of` instead of `for..of`.

Syntax differences between async and regular iterators:

|       | Iterable | Async Iterable |
|-------|-----------|-----------------|
<<<<<<< HEAD
| Object method to provide iteraterable | `Symbol.iterator` | `Symbol.asyncIterator` |
| `next()` return value is              | any value         | `Promise`  |
=======
| Method to provide iterator | `Symbol.iterator` | `Symbol.asyncIterator` |
| `next()` return value is          | `{value:…, done: true/false}`         | `Promise` that resolves to `{value:…, done: true/false}`  |
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc

Syntax differences between async and regular generators:

|       | Generators | Async generators |
|-------|-----------|-----------------|
| Declaration | `function*` | `async function*` |
| `next()` return value is          | `{value:…, done: true/false}`         | `Promise` that resolves to `{value:…, done: true/false}`  |

In web-development we often meet streams of data, when it flows chunk-by-chunk. For instance, downloading or uploading a big file.

<<<<<<< HEAD
We could use async generators to process such data, but there's also another API called Streams, that may be more convenient, as it provides special interfaces to transform the data and to pass it from one stream to another (e.g. download from one place and immediately send elsewhere). But they are also more complex.

<<<<<<< HEAD
Streams API not a part of JavaScript language standard. Streams and async generators complement each other, both are great ways to handle async data flows.
=======
Streams API is not a part of JavaScript language standard.
>>>>>>> 852ee189170d9022f67ab6d387aeae76810b5923
=======
We can use async generators to process such data. It's also noteworthy that in some environments, like in browsers, there's also another API called Streams, that provides special interfaces to work with such streams, to transform the data and to pass it from one stream to another (e.g. download from one place and immediately send elsewhere).
>>>>>>> ae1171069c2e50b932d030264545e126138d5bdc
