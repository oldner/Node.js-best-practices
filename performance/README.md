# How to increase performance

In terms of optimization, there are several steps you can take to improve the performance of your application. For example, you can implement caching to store frequently accessed data in memory, which can greatly improve the speed at which your application can retrieve and serve data. You can also use tools like load balancers and horizontal scaling to distribute workloads across multiple servers, which can help improve the overall performance of your application.

In terms of when to use an in-memory cache, it can be helpful to store data in the cache when it is first read from the database, so that subsequent requests for the same data can be served quickly from the cache. This can help reduce the overall load on your database, which can improve the performance and scalability of your application.

There are several ways to implement caching in a Node.js application. One approach is to use an in-memory cache like Redis or Memcached, which are specifically designed to store and retrieve data quickly from memory. For example, you could use the Redis node.js client to connect to a Redis server and store data in memory like this:

## Redis

```js
const redis = require("redis");
const client = redis.createClient();
const mysql = require("mysql");
const connection = mysql.createConnection({
  host: "localhost",
  user: "user",
  password: "password",
  database: "database",
});

// Query the database to get a user's profile
connection.query("SELECT * FROM users WHERE id = 123", (err, results) => {
  if (err) {
    console.error(err);
  } else if (results.length > 0) {
    const user = results[0];

    // Set the user's profile in the cache
    client.set("user:123", JSON.stringify(user), (err, result) => {
      if (err) {
        console.error(err);
      } else {
        console.log(result); // 'OK'
      }
    });
  }
});

// Later, when the user's profile is needed again
client.get("user:123", (err, result) => {
  if (err) {
    console.error(err);
  } else {
    console.log(JSON.parse(result)); // { id: 123, name: 'Jane Doe', age: 35, email: 'jane.doe@example.com' }
  }
});
```

## Keep data with Javascript Map

Another approach is to use an in-memory cache like Node.js' built-in Map object, which allows you to store and retrieve data using key-value pairs. For example, you could use a Map object to store data in memory like this:

```js
const cache = new Map();

// Set a value in the cache
cache.set("key", "value");

// Get a value from the cache
const value = cache.get("key");
console.log(value); // 'value'
```

## Keep data with Memcached

You can also use Memcached in a similar way to store and quickly retrieve data from memory. Memcached is a high-performance, distributed memory caching system that can be used to improve the performance of web applications by reducing the need to access slower backing stores like databases.

```js
const memcached = require("memcached");
const client = new memcached("localhost:11211");

// Set a value in the cache
client.set(
  "user:123",
  {
    id: 123,
    name: "Jane Doe",
    age: 35,
    email: "jane.doe@example.com",
  },
  10,
  (err) => {
    if (err) {
      console.error(err);
    } else {
      console.log("Value set in cache");
    }
  }
);

// Get a value from the cache
client.get("user:123", (err, result) => {
  if (err) {
    console.error(err);
  } else {
    console.log(result); // { id: 123, name: 'Jane Doe', age: 35, email: 'jane.doe@example.com' }
  }
});
```
