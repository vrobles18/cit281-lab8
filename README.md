# cit281-lab8

// #1 TODO: Declare fastify object from fastify, and execute
const fastify = require("fastify")();


// #2 TODO: Declare fetch object from node-fetch
const fetch = require('node-fetch');

fastify.get("/fotos", (request, reply) => {

    // 1) Get request
    // 2) Do something as a result of the request
    //  a*) Get the JSON data from the other server
    //  b) Process that data
    // 3) Provide a reply to the client
    //  a* If there were hiccups in getting the data, provide a different response.

    fetch('http://jsonplaceholder.typicode.com/photos')
    .then(response => {return response.json()})
    .then(jsonFromResponse => {  
        reply
        .code(200)
        .header("Content-Type", "text/json; charset=utf-8")
        .send({ error: "", statusCode: 200, photos: jsonFromResponse });
    })
        .catch(err => {
        reply
        .code(404)
        .header("Content-Type", "text/json; charset=utf-8")
        .send({ error: "", statusCode: 404, photos: [] });

    });

// #3 TODO:
  // Adapt the following code to attempt to retrieve
  // all photos from JSONPlaceholder site
  // using fetch, and handle returned Promise using:
  // - two .then() chain methods, return 200
  // - single .catch() chain method, return 404
});

fastify.get("/fotos/:id", (request, reply) => {
  // #4 TODO:
  // Adapt the following code to attempt to retrieve
  // a single photo from JSONPlaceholder site
  // using fetch, and handle returned Promise using:
  // - single .then() chain method, return 200
  // - single .catch() chain method, return 404
  // You may also try to use Object.keys() to 
  // ensure JSONPlaceholder returns an object with
  // properties. An empty object returned from 
  // JSONPlaceholder means that the passed photo ID
  // was invalid. Your server would also return
  // a 404 status code for an invalid Photo ID.

  // 1) Get info from request
  const { id = "" } = request.params; 
  // 2) Take action
  // 3) Reply to client 
  fetch(`http://jsonplaceholder.typicode.com/photos/${id}`)
    .then(response => {return response.json()})
    // 3) Reply to client
    .then(jsonFromResponse => {  
        reply
        .code(200)
        .header("Content-Type", "text/json; charset=utf-8")
        .send({ error: "", statusCode: 200, photos: jsonFromResponse });
    })
        .catch(err => {
        reply
        .code(404)
        .header("Content-Type", "text/json; charset=utf-8")
        .send({ error: "", statusCode: 404, photos: [] });

    });
  
});

// Start server and listen to requests using Fastify
const listenIP = "localhost";
const listenPort = 8080;
fastify.listen(listenPort, listenIP, (err, address) => {
  if (err) {
    console.log(err);
    process.exit(1);
  }
  console.log(`Server listening on ${address}`);
});
