---
uid: js-async
---

# Writing Javascript skills, the async way

One particularity of our current Javascript implementation is that your skill code runs in an async wrapper, something like:

```js
(async () => {

    [YOUR CODE]

})();
```

Because of this, we also provide promisified versions of libraries, like Axios and Request-Promise. Axios is a Promise-based HTTP client, and if you prefer the old Request library, Request-Promise is its Promise-based version. Here are some usage examples:

## Axios

```js
const axios = require('axios');
const key = await bot.secrets.read("ghtoken");
const options = {
  headers: {
    'User-Agent': 'Request-Promise',
    'Authorization': `token ${key}`
  }
};

await axios.get('https://api.github.com/user/repos', options)
  .then(response => {
    bot.reply(`User has ${response.data.length} repos`);
  })
  .catch(error => {
    // API call failed...
    bot.reply(error);
  });
```

Or with async/await

```js
const axios = require('axios');
const key = await bot.secrets.read("ghtoken");
const options = {
  headers: {
    'User-Agent': 'Request-Promise',
    'Authorization': `token ${key}`
  }
};

try {
  const response = await axios.get('https://api.github.com/user/repos', options)
  await bot.reply(response.data.length);
} catch (error) {
  await bot.reply(`Ooops, something went wrong ${error}`);
}
```

## Request-Promise

```js
const rp = require('request-promise');
const key = await bot.secrets.read("ghtoken");
const options = {
  headers: {
    'User-Agent': 'Request-Promise',
    'Authorization': `token ${key}`
  },
  json: true // Automatically parses the JSON string in the response
};

await rp('https://api.github.com/user/repos', options)
  .then(repos => {
    bot.reply(`User has ${repos.length} repos`);
  })
  .catch(error => {
    // API call failed...
    bot.reply(error);
  });
```

## Exception propagation

Note that your code is running inside a sandbox inside a container in an out-of-process language runner. Uncaught exceptions that get thrown from callbacks won't always be bubbled up to the bot console. If you get exceptions without line information, or if the bot console doesn't return any output, it's likely that an exception is getting thrown from inside a `then` or `catch` block. Make sure you have `try/catch` blocks on all your callback code to handle exceptions:

## Abbot API

Our API is also async/await, so when you want to have Abbot reply to the chat, you can `await bot.reply('hello world')` when not inside then handlers.