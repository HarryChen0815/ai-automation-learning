# 1.2.4. Challenge


**The Task**: Use the @workspace command to find where the "API Gateway" defines its rate-limiting or routing rules.
**The Goal**: Locate the configuration file (e.g., microservices/api-gateway/src/config.ts) and ask: "How many requests per minute are allowed before the Gateway returns a 429 error?"
**Submission**: Post the file path and the rate limit value you found in our workshop channel.
**Professional Tip**: Discovery is not a one-time event. Use Slash Commands like /explainevery time you encounter a new microservice function to build a mental map of the system's "Quality DNA".


## @workspace find where the "API Gateway" defines its rate-limiting or routing rules, and tell me how many requests per minute are allowed before the API Gateway returns a 429 error?

# API Gateway Rate Limiting Configuration

Based on the codebase, the API Gateway defines its rate-limiting rules in microservices/api-gateway/src/index.ts.

## Rate Limiting Configuration

````typescript
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 1000, // Limit each IP to 1000 requests per windowMs
  message: 'Too many requests from this IP, please try again later.',
});
````

## Answer to Your Question

**File Path**: microservices/api-gateway/src/index.ts

**Rate Limit**: **1000 requests per 15 minutes**

**Requests per minute**: Approximately **67 requests per minute** (1000 ÷ 15)

**Error Response**: When this limit is exceeded, the gateway returns a **429 (Too Many Requests)** HTTP status code with the message: `"Too many requests from this IP, please try again later."`

## Routing Configuration

The API Gateway also defines routing rules in microservices/api-gateway/src/config.ts, which maps service paths to their respective URLs:

- `/api/products` → Product Service (3001)
- `/api/cart` → Cart Service (3002)
- `/api/orders` → Order Service (3003)
- `/api/recommendations` → AI Service (3004)
- `/api/users` → User Service (3005)