# How to monitor?

There are several ways to monitor a Node.js application. One approach is to use a monitoring tool like Prometheus, which is specifically designed to monitor and collect metrics from applications. Prometheus can be used to track key metrics like CPU and memory usage, request rates and latencies, and other operational and performance-related data.

## Prometheus

```js
const prometheus = require("prom-client");

// Create a new Prometheus registry
const registry = new prometheus.Registry();

// Create a new Prometheus counter
const counter = new prometheus.Counter({
  name: "http_requests_total",
  help: "The total number of HTTP requests made to the application",
  labelNames: ["method", "status"],
});

// Register the counter with the registry
registry.register(counter);

// Increment the counter when an HTTP request is made
app.use((req, res, next) => {
  counter.inc({ method: req.method, status: res.statusCode });
  next();
});

// Expose the metrics for Prometheus to scrape
app.get("/metrics", (req, res) => {
  res.set("Content-Type", registry.contentType);
  res.end(registry.metrics());
});
```

## Elasticsearch and Kibana (ELK)

Another approach is to use a logging and monitoring tool like Elasticsearch, Logstash, and Kibana (ELK), which can collect, store, and visualize your application logs. This can be useful for tracking and troubleshooting errors and other issues with your application.

```js
const winston = require("winston");
const elasticsearch = require("elasticsearch");

// Create a new Elasticsearch client
const client = new elasticsearch.Client({
  host: "localhost:9200",
  log: "trace",
});

// Create a new Winston logger
const logger = winston.createLogger({
  level: "info",
  format: winston.format.json(),
  transports: [
    new winston.transports.Console(),
    new winston.transports.Elasticsearch({
      client,
      index: "logstash",
      messageType: "log",
    }),
  ],
});

// Log an error when it occurs
app.use((err, req, res, next) => {
  logger.error(err.stack);
  next(err);
});
```
