# 1.1.3. Challenge


To solidify your learning, complete this foundational task:

The Task: Use the @workspace command to find where the "Health Check" logic is implemented for the Cart Service.
The Goal: Identify the specific file path (e.g., microservices/cart-service/src/index.ts) and ask Copilot: @workspace Explain what this health check verifies.
Share Your Work: Push your workspace to GitHub and share the project link Below and in the #daily-challenge channel.

## @workspace where is the health check logic for the cart service?

The cart service just exposes a very simple `/api/health` handler in its router.

You can find it in:

- **routes.ts** – near the bottom of the file there’s:

```ts
/**
 * @swagger
 * /api/health:
 *   get:
 *     summary: Health check
 *     description: Check if the service is healthy
 *     tags: [Health]
 *     responses:
 *       200:
 *         description: Service is healthy
 */
router.get('/health', (req: Request, res: Response) => {
  res.json({ status: 'healthy', service: 'cart-service', timestamp: new Date().toISOString() });
});

export default router;
```

That’s the “health check logic” for the Cart Service.


## @workspace Explain what this health check verifies.

The `/health` handler in the cart service doesn’t hit a database or do any real diagnostics – it’s just a simple liveness endpoint.

In routes.ts you’ll find:

```ts
router.get('/health', (req: Request, res: Response) => {
  res.json({ status: 'healthy', service: 'cart-service', timestamp: new Date().toISOString() });
});
```

When the gateway (or a curl command) calls `GET /api/health` on that service it always returns a JSON object with:

* `status: 'healthy'`  
* `service: 'cart-service'`  
* a timestamp

So the health check simply verifies that the cart service process is up and able to respond to HTTP requests – it doesn’t perform any deeper checks.
