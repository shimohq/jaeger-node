# jaeger-node

[![Build Status](https://travis-ci.org/RisingStack/jaeger-node.svg?branch=master)](https://travis-ci.org/RisingStack/jaeger-node)  

Out of the box distributed tracing for [Node.js](https://nodejs.org) applications.

**WARNING: experimental library, do not use in production yet**  

**This library is a higher level wrapper around [opentracing-auto](https://github.com/RisingStack/opentracing-auto), that you should consider to use instead of this library.**

## Technologies

- [async_hooks](https://github.com/nodejs/node/blob/master/doc/api/async_hooks.md)
- [Jaeger](https://uber.github.io/jaeger/)
- [OpenTracing](http://opentracing.io/)

**Requirements**

- Node.js, >= v8
- Jaeger

## Getting started

```sh
npm install shimo-jaeger
```

```js
// must be in the first two lines of your application
const Tracer = require('shimo-jaeger')
const tracer = new Tracer({
  serviceName: 'my-server-2',
  options: {
    enables: ['koa', 'express', 'http']
  }
})

// rest of your code
const express = require('express')
// ...
```

**To start Jaeger and visit it's dashboard:**

```sh
docker run -d -p5775:5775/udp -p6831:6831/udp -p6832:6832/udp -p5778:5778 -p16686:16686 -p14268:14268 jaegertracing/all-in-one:latest && open http://localhost:16686
```

## Example

The example require a running MongoDB.  

```sh
npm run example
curl http://localhost:3000
open http://localhost:16686
```

![Jaeger Node.js tracing](https://user-images.githubusercontent.com/1764512/26843812-c3198758-4af1-11e7-8aa3-1da55d9e58b6.png)

## API

### new Tracer(args)

Create a new Tracer and instrument modules.

- `args.serviceName`: Name of your service
  - **required**
  - example: `'my-service-1'`
- `args.sampler`: Jaeger sampler, see [sampler docs](https://github.com/uber/jaeger-client-node/tree/master/src/samplers)
  - *optional*
  - default: `new jaeger.RateLimitingSampler(1)`
- `args.reporter`: Jaeger reporter, see [reporter docs](https://github.com/uber/jaeger-client-node/tree/master/src/reporters)
  - *optional*
  - default: `new jaeger.RemoteReporter(new UDPSender())`
- `args.options`: Jaeger options, see [docs](https://github.com/uber/jaeger-client-node#initialization)
  - *optional*
  - example: `{ tags: { gitHash: 'foobar', sender: { host: '192.168.1.x' }, enables: ['http', 'koa', 'express'] } }`

## Instrumentations

Check out [opentracing-auto instrumentations](https://github.com/RisingStack/opentracing-auto#instrumentations).

## EMSGSIZE and UDP buffer limits

Read more about it in the [Client Libraries](https://github.com/uber/jaeger/blob/master/docs/client_libraries.md#emsgsize-and-udp-buffer-limits) documentation.
