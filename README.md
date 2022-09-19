
# OpenJS Node.js Services Developer (JSNSD) certification Exercises & Quick Notes

To prepare JSNSD certification exam Linux foundation provides one Learning Course called as [Node.js Services Development (LFW212)](https://training.linuxfoundation.org/training/node-js-services-development-lfw212/). Here I have tried to cover the examples given in this course.

## Exercises & Labs

 01. [Creating a Web Server With Core Http Module](https://github.com/sudhir-j-sapkal/01_CreateWebServerWithHttp)
 02. [Creating a Web Server and Routes With Core Http Module](https://github.com/sudhir-j-sapkal/02_CreateWebServerAndRoutesWithHttp)
 03. [Create a Web Server with Express](https://github.com/sudhir-j-sapkal/03_CreateWebServerWithExpress)
 04. [Create a Web Server with Fastify](https://github.com/sudhir-j-sapkal/04_CreateWebServerWithFastify)
 05. [Labs 3.1 & 3.2](https://github.com/sudhir-j-sapkal/05_Lab3.1_3.2)
 06. [Serving Web Content Using Fastify](https://github.com/sudhir-j-sapkal/06_Serving_WebContent_Using_Fastify)
 07. [Creating RESTful JSON with Express](https://github.com/sudhir-j-sapkal/07_Creating_Restful_JSON_With_Express)
 08. [Lab 5.1](https://github.com/sudhir-j-sapkal/08_Lab_5.1)
 09. [Manipulating Data With Rest Services Using Express](https://github.com/sudhir-j-sapkal/09_ManipulatingDataWithRestServices_Using_Express)
 10. [Consuming And Agreegating Services](https://github.com/sudhir-j-sapkal/10_Consuming_And_Agreegating_Services)
 11. [Proxy Http Request With Single Route Multiple Proxy](https://github.com/sudhir-j-sapkal/11_Proxy_Http_Request)
 12. [Proxy Http Request With Single Origin Multiple Route](https://github.com/sudhir-j-sapkal/12_Proxy_Http_Request_Single_Origin_Multiroute)
 
 ## Quick Notes
 ### 1. How Not to Install Node
  ```bash
Often Node.js can be installed with a particular Operating System's official or unofficial package manager. 
For instance apt-get on Debian/Ubuntu, Brew on macOs, Chocolatey on Windows. 
It is strongly recommended against using this approach to install Node. 
Package managers tend to lag behind the faster Node.js release cycle. 
Additionally the placement of binary and config files and folders 
isn't standardized across OS package managers and can cause compatibility issues.

Another significant issue with installing Node.js via an OS package manager 
is that installing global modules with Node's module installer (npm) tends to require the 
use of sudo (a command which grants root privileges) on non-Windows systems. 
This is not an ideal setup for a developer machine and granting root privileges to the 
install process of third-party libraries is not a good security practice.
  ```
  ### 2. How to Use Express Generator 
  ```bash
  A tool called express-generator generates this structure with some other additions. 
  We'll use the generator in the next chapter, for now we're concentrating on the 
  bare bones to create the equivalent of our minimum viable web server that we implemented in the previous section.
 
   express my-express-service
  
  ```
  ### 3. Avoiding Parameter Pollution Attacks
  ```bash
  The parameter pollution exploits a bug that's often created by developers when handling query string parameters.
  Even when we know about the potential for the bug, it can still be easy to forget. 
  The main aim of such an attack is to cause a service to either crash or slow down by generating an exception in the service.
  
  In cases where a crash occurs, it will be because of an unhandled exception. 
  In cases where a slow down occurs it can be caused by generating an exception that's 
  generically handled and the error handling overhead (for instance, stack generation 
  for a new error object on every request) and then sending many requests to the server.
  Both of these are forms of Denial of Service attacks. 
  Preventing the attack from occurring in the first place requires an awareness of how query-string parsing works.
  
  The only way to avoid a parameter pollution attack is to ensure that any 
  code written for query-string parameters can run without error against both strings and arrays. 
  This can be as simple as a typeof check for string or conversely using Array.isArray method to 
  determine if the value is an array and then handling the value accordingly. 
  
  There's no right answer as to how to handle string value versus array values from parses query-strings, 
  it all depends on context. However one solution to the above buggy-code is the following:
  
  function convert (name) {
    var parts = name.split(' ');
    var last = parts.pop();
    var first = parts.shift();
    return {first: first, last: last};
   }
   router.get('/', (req, res, next) => {
     someAsynchronousOperation(() => {
       if (!req.query.name) {
         var err = new Error('Bad Request')
         err.status = 400
         next(err)
         return
       }
       if (Array.isArray(req.query.name)) {
         res.send(req.query.name.map(convert));
       } else {
         res.send(convert(req.query.name));
       }
     });
   });
   
   This solution responds to the cases where query-string values are arrays by responding with an array where each value is converted. 
   However, again, the way to solve this issue is entirely context dependent.
  ```
  ### 4. How to Validate Routes Using Fastify
  ```bash
  The recommended approach to route validation in Fastify is using the schema option which can be passed when declaring routes. 
  Fastify supports the JSONSchema format for declaring the rules for incoming (and also outgoing) data. 
  Not only is support of this common format useful as a standardized validation convention, 
  it's also used by Fastify to compile route specific serializers which speed up parsing time, 
  improving a services request-per-seconds performance. 
  Often the goals of performance and security compete, 
  in that performance can suffer due to security and vice versa. 
  However, using JSONSchema with Fastify yields gains for both.

  Code Example 1
  
  fastify.post('/', {
    schema: {
      body: {
        type: 'object',
        required: ['data'],
        additionalProperties: false,
        properties: {
          data: {
            type: 'object',
            required: ['brand', 'color'],
            additionalProperties: false,
            properties: {
              brand: {type: 'string'},
              color: {type: 'string'}
            }
          }
        }
      }
    }
  }, async (request, reply) => {
    const { data } = request.body
    const id = uid()
    await create(id, data)
    reply.code(201)
    return { id }
  })
  
  Code Example 2
  
  'use strict'
const { promisify } = require('util')
const { bicycle } = require('../../model')
const { uid } = bicycle
const read = promisify(bicycle.read)
const create = promisify(bicycle.create)
const update = promisify(bicycle.update)
const del = promisify(bicycle.del)

module.exports = async (fastify, opts) => {
  const { notFound } = fastify.httpErrors

  const bodySchema = {
    type: 'object',
    required: ['data'],
    additionalProperties: false,
    properties: {
      data: {
        type: 'object',
        required: ['brand', 'color'],
        additionalProperties: false,
        properties: {
          brand: {type: 'string'},
          color: {type: 'string'}
        }
      }
    }
  }

  fastify.post('/', {
    schema: {
      body: bodySchema
    }
  }, async (request, reply) => {
    const { data } = request.body
    const id = uid()
    await create(id, data)
    reply.code(201)
    return { id }
  })

  fastify.post('/:id/update', {
    schema: {
      body: bodySchema
    }
  }, async (request, reply) => {
    const { id } = request.params
    const { data } = request.body
    try {
      await update(id, data)
      reply.code(204)
    } catch (err) {
      if (err.message === 'not found') throw notFound()
      throw err
    }
  })

  fastify.get('/:id', async (request, reply) => {
    const { id } = request.params
    try {
      return await read(id)
    } catch (err) {
      if (err.message === 'not found') throw notFound()
      throw err
    }
  })

  fastify.put('/:id', {
    schema: {
      body: bodySchema
    }
  }, async (request, reply) => {
    const { id } = request.params
    const { data } = request.body
    try {
      await create(id, data)
      reply.code(201)
      return { }
    } catch (err) {
      if (err.message === 'resource exists') {
        await update(id, data)
        reply.code(204)
      } else {
        throw err
      }
    }
  })

  fastify.delete('/:id', async (request, reply) => {
    const { id } = request.params
    try {
      await del(id)
      reply.code(204)
    } catch (err) {
      if (err.message === 'not found') throw notFound()
      throw err
    }
  })
}
  ```
### 5. How to Validate Routes Using Express
```bash
Express does not offer any validation primitives or abstractions as a core part of the framework. 
There are no particular validation practices recommended in the frameworks' documentation. 
As a result, approaches to Express validation in the wild vary significantly. 
While validation libraries do exist - for an example see htt‌ps://express-validator.github.io/docs/ - there is no standard approach. 
It is even possible to use JSONSchema with Express via various middleware offerings but this is rarely seen in practice; 
possibly because the implementations available cause significant performance overhead. 
As a result the most common approach to validation in Express is to develop custom logic for the service as needed. 
This isn't exactly recommended, but when dealing with legacy services it's useful to understand this aspect of real-world legacy Express development.
```
