# 1.3.4. Challenge


**The Task**: Target the Order Service logic in microservices/order-service/src/service.ts.

**The Goal**: Use Template 1: Test Case Generation from your library to generate a manual test plan for the createOrder function.

**The Constraint**: You must include a "Data Integrity" section in your prompt that checks if the order total matches the sum of cart items.

**Submission**: Copy your engineered prompt and the resulting "Edge Case" table into the LMS portal.

**Professional Tip**: Using templates doesn't just make you faster; it ensures that every member of the QA team produces artifacts of the same high professional standard.


## Engineered Prompt

### ROLE
As a Senior QA Engineer specializing in microservices testing, I focus on ensuring robust API behavior, data integrity, and edge case handling in distributed systems. My expertise includes manual test case design for order processing logic, boundary value analysis, and validation of business rules in e-commerce applications.

### CONTEXT
- Application: MyBasket Lite
- Target Logic: `createOrder` in microservices/order-service/src/service.ts
- Endpoints: POST /api/orders (assumed based on service function)

### TASK
Generate a comprehensive set of manual test cases for the createOrder function.

### CONSTRAINTS
1. Include Positive (happy path), Negative (invalid inputs), and Edge cases (boundary conditions).
2. Include a "Data Integrity" section that checks if the order total matches the sum of cart items (price * quantity).
3. Format as a Markdown table with: Test ID, Description, Steps, and Expected Result.


## Result
| Test Case ID | Description | Input Data | Expected Result | Actual Result |
|---|---|---|---|---|
| 2.1 | Single item with quantity 1 | `items: [{ price: 10.00, quantity: 1 }]` | `totalAmount: 10.00` | |
| 2.2 | Large quantity on single item | `items: [{ price: 5.00, quantity: 999 }]` | `totalAmount: 4995.00` | |
| 2.3 | Multiple items with quantity 1 each | `items: [{ price: 20.00, qty: 1 }, { price: 30.00, qty: 1 }]` | `totalAmount: 50.00` | |
| 2.4 | Decimal prices requiring rounding | `items: [{ price: 10.33, quantity: 3 }]` | `totalAmount: 30.99` (Math.round applied) | |
| 2.5 | Prices with cents that create repeating decimals | `items: [{ price: 0.10, quantity: 3 }]` | `totalAmount: 0.30` | |
| 2.6 | Very high price items | `items: [{ price: 99999.99, quantity: 2 }]` | `totalAmount: 199999.98` | |
| 2.7 | Mixed item quantities and prices | `items: [{ price: 12.34, qty: 5 }, { price: 56.78, qty: 2 }, { price: 9.01, qty: 3 }]` | `totalAmount: 237.77` | |
| 2.8 | Zero quantity item (edge validation) | `items: [{ price: 50.00, quantity: 0 }]` | Should be rejected or `totalAmount: 0.00` | |
| 2.9 | Identical items in same order | `items: [{ id: "prod1", price: 25.00, qty: 2 }, { id: "prod1", price: 25.00, qty: 1 }]` | Both included; `totalAmount: 75.00` | |
| 2.10 | Maximum rounding scenario | `items: [{ price: 0.015, quantity: 1 }]` | Verify rounding logic applied correctly | |
| 2.11 | Order with minimum viable address data | Minimal address fields | Order created successfully | |
| 2.12 | Order with complete address data | Full address with optional fields | Order created successfully | |