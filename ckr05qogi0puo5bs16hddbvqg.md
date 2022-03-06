## Redis Crash Course

Redis is a type of database and it can be added to your production level application to make it more performant. I will cover the basics of Redis and show a real world example of Redis.

In this article, we'll discuss:

1. What is Redis?
2. Why the hype around Redis?
3. When to use Redis?
4. Redis Installation
5. Basic Redis Commands
   1. Lists
   2. Sets
   3. Hashes
6. Using Redis to make your website 30-40% faster
7. Using Redis as a primary database

So let's dive in.

## What is Redis?

Redis is an in-memory database with sub millisecond latency. Redis stands for **Re**mote **Di**ctionary **S**ervice. What makes redis so powerful is that, it stores the data in the memory and not in the slower disks. Every data point in the database is a key-value pair. The value can be any of the following fields:-

1. String - hello World
2. Bitmap - 0011001
3. Bitfield - {325}{655}{678}
4. Hash - {a: "hello", b:'world}
5. List - [ A > B > C ]
6. Set - { A, B, C}
7. Sorted set - {A: 1, B: 2, C: 3}
8. Geo-spatial - {A:(52, 2, 3)}
9. Hyperlog
10. Stream

## Why the hype around Redis?

Redis is so popular because of it's speed. Unlike a relational database where the data is stored in slower hard disks, redis stores the data in RAM. Due to the usage of RAM, Redis is volatile, meaning that when the system shuts down, your data is also lost. Hence, Redis is usually not used as a persistent database like MongoDb or PostgresSql and is instead used for caching. Now a days its more powerful as I will discuss below.

🌟 Super Fast data access :-  Redis takes milliseconds to access data as opposed to many hundreds of milliseconds using traditional methods.

## When to use Redis?

Redis is not a replacement for your database, Instead it is build on top of your traditional database. Any data that needed to be accessed frequently can be stored in Redis.

- When you need to access some data frequently
- When the data base query is long and takes a lot of time to execute

## Redis Installation

- **Ubuntu** - [from the official Ubuntu PPA](https://redis.io/download#from-the-official-ubuntu-ppa)

  ```shell
  $ sudo add-apt-repository ppa:redislabs/redis
  $ sudo apt-get update
  $ sudo apt-get install redis
  ```

- **Mac** - [use homebrew](https://gist.github.com/tomysmile/1b8a321e7c58499ef9f9441b2faa0aa8)

  ```bash
  brew update
  brew install redis
  ```

- **Windows** - use [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to install Redis on windows

## Basic Redis Commands

> Startup Commands

- `redis-server` -  To start Redis server use the command
- To use the Redis CLI open a new terminal and enter `redis-cli` and to close it use `quit` command

> Basic commands 

- `SET name your-name` - Setting a value 

- `GET name` - Get the above value 
- `DEL name` - Delete a key-value by 
- `EXISTS name` - To check if a key exist
- `KEYS *` - Get all keys 
- `flushall` - Clear all the data

> Expiration

- `ttl key-name` - Check how long a key has before being deleted automatically. if the result is `-1`  it means **ttl**(Time To Live) is not set and it wont expire.
- `expire key-name 10` - Set a **ttl** of 10 seconds.
- `setex name 10 your-name` - To set a **ttl** while setting a key value pair.

### Lists

Lists are useful when we want to implement a queue or stack. Like in case of a messenger app, where we cache some of the recent messages. 

- `lpush fruits apple` - Push an item into the left of a list. 
- `rpush fruits mango` - Push an item into the right of a list. 
- `lrange fruits 0 -1` - Get all the items from the list. The `-1` stands for the index at the end of the list.
- `LPOP fruits` - Removes the leftmost item in the list.
- `RPOP fruits` - Removes the rightmost item in the list.

### Sets

Sets are similar to lists.  What makes a Set different is that it will only store unique values.

- `SADD todo "read book"` - To add item into a set. (note: if we try to add "read book" again it won't be added as it is a duplicate)
- `SMEMBERS todo` - Show all the items in the todo set.
- `SREM todo "read book"` -  To remove item from set. 

### Hashes

Whereas `LIST`s and `SET`s in Redis hold sequences of items, Redis `HASH`es store a mapping of keys to values.

- `HSET person name John` - Here **name** is the key and **John** is the value.
- `HGET person name` - This returns the value associated with the key name and in this case it returns **John**.
- `HGETALL person` - Get all the info about person.
- `HDEL person name` - Remove the name property.
- `HEXISTS person name` - To check if the property exists.



## Using Redis to make your website 30-40% faster

**Install Redis**

```bash
npm i redis
```

**Start your Redis server**

```bash
redis-server
```

**Import the package and create an instance**

```js
// Import redis package
const Redis = require('redis')

// Create redis client, in case ofer development only
const redisClient = Redis.createClient()
// Incase of production pass your production instance url and use the below line
const redisClient = Redis.createClient({ url: "your-production-url"})
```

use the above `redisClient` instance to do all the commands I mentioned above. for example -

```js
redisClient.setex('photos', 3600, JSON.stringyfy(some-value-to-store))
```

**Before adding Redis Caching**

The code below takes about **480ms** to fetch a data of size 900kB.

```js
app.get("/photos", async(req, res) => {
    const albumId = req.query.albumId
    const { data } = await axios.get(
        "https://jsonplaceholder.typicode.com/photos"
        { params: { albumId }}
    )
})
```

**After adding Redis Caching**

The code below takes about **480ms** in the first fetch and in the recurrent fetch it takes only **37ms**. Now that is some serious performance gain.

```js
// Import redis package
const Redis = require('redis');

// Create redis client, in case ofer development only
const redisClient = Redis.createClient();
// Incase of production pass your production instance url and use the below line
const redisClient = Redis.createClient({ url: 'your-production-url' });

app.get('/photos', async (req, res) => {
  const albumId = req.query.albumId;
  redisClient.get('photos', async (error, photos) => {
    if (error) console.error(error);
    if (photos != null) {
      return res.json(JSON.parse(photos));
    } else {
      const { data } = await axios.get('https://jsonplaceholder.typicode.com/photos', {
        params: { albumId }
      });
      redisClient.setex('photos', 3600, JSON.stringyfy(data));
      res.json(data);
    }
  });
});

```

In the above example we first check if we have already cached photos in our Redis cache, In case it is cached, the cached value is returned, else the photos are fetched from the API.

## Using Redis as a primary Database

Redis is by nature super fast and doesn't need additional caching layers, but a necessary requirement of a database is to model complex relationships. Don't worry, Redis got you covered. Redis can be used a multi model database. It supports a variety of database paradigms with the help of various modules. Some of the most popular Redis modules are :-

1. RediSearch - Full-Text search over Redis.
2. RedisGraph - A graph database with a Cypher-based querying language using sparse adjacency matrices.
3. RedisBloom - Scalable Bloom filters.
4. RedisJson - A JSON data type for Redis.
5. RedisAI - A Redis module for serving tensors and executing deep learning graphs.
6. Neural-redis - Online trainable neural networks as Redis data types.
7. RedisTimeSeries - Time-series data structure for redis.

Checkout all the modules in [redis.io/modules]( https://redis.io/modules ). You can play around with these modules in [Redis Enterprise Cloud free tier](https://redislabs.com/try-free/?utm_source=youtube&utm_medium=social&utm_campaign=fireship-io).



## Not in a hurry? Read these 

1. [My favorite algorithm (and data structure): HyperLogLog](https://odino.org/my-favorite-data-structure-hyperloglog/)
2. [What is a message broker?](https://www.ibm.com/cloud/learn/message-brokers)

