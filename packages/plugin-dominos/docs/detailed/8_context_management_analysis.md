# Context Management Analysis

## Overview
The context management system in the Dominos plugin is primarily handled by the `pizzaOrderProvider`, which acts as a bridge between the user interface, order management system, and the AI's decision-making process. This document analyzes how context flows through the system during different stages of the order process.

## Context Structure

### Base Context Template
```typescript
{
    PAYMENT_STATUS: {
        currentStatus: string,
        requirements?: string,
        errors?: string
    },
    REQUIRED_ACTIONS: {
        immediate: string,
        next: string
    },
    ORDER_STATUS: {
        summary: string,
        details: OrderDetails,
        nextAction: string
    },
    STORE_STATUS: {
        isOpen: boolean,
        deliveryAvailable: boolean,
        carryoutAvailable: boolean
    }
}
```

## Context Flow Analysis

### 1. Initial State (No Order)
```plaintext
No active pizza order. The customer needs to start a new order.
```

### 2. Order Creation Stage
```plaintext
=== PIZZA ORDER STATUS ===
Order #: New Order
Items:
- 1x LARGE HAND_TOSSED Pizza
  Toppings: None
  Price: $13.99

PAYMENT STATUS:
Current Status: NOT_PROVIDED
Payment information needed to complete order.

REQUIRED:
Please provide customer information to proceed.

NEXT REQUIRED ACTION:
Customer needs to provide delivery address and contact information.

STORE STATUS:
Store Open: Yes
Delivery Available: Yes
Carryout Available: Yes

ORDER STATUS:
Current Status: AWAITING_CUSTOMER_INFO
Waiting for customer information to proceed.
```

### 3. Customer Information Added
```plaintext
=== PIZZA ORDER STATUS ===
Order #: New Order
Customer: John Doe
Address: 123 Main St, Anytown, USA
Phone: (555) 123-4567
Items:
- 1x LARGE HAND_TOSSED Pizza
  Toppings: None
  Price: $13.99

PAYMENT STATUS:
Current Status: NOT_PROVIDED
Payment information needed to complete order.

REQUIRED:
Please provide credit card information to complete your order.

NEXT REQUIRED ACTION:
Payment information required to proceed.

STORE STATUS:
Store Open: Yes
Delivery Available: Yes
Carryout Available: Yes

ORDER STATUS:
Current Status: AWAITING_PAYMENT
Waiting for payment information.
```

### 4. Payment Added
```plaintext
=== PIZZA ORDER STATUS ===
Order #: New Order
Customer: John Doe
Address: 123 Main St, Anytown, USA
Phone: (555) 123-4567
Payment: Card ending in 1234
Items:
- 1x LARGE HAND_TOSSED Pizza
  Toppings: None
  Price: $13.99

PAYMENT STATUS:
Current Status: VALID
Payment information verified.

REQUIRED:
Please review your order and confirm to place it.

NEXT REQUIRED ACTION:
Order ready for confirmation.

STORE STATUS:
Store Open: Yes
Delivery Available: Yes
Carryout Available: Yes

ORDER STATUS:
Current Status: PROCESSING
Order is being processed but needs confirmation.
```

### 5. Order Confirmed
```plaintext
=== PIZZA ORDER STATUS ===
Order #: DOM12345
Customer: John Doe
Address: 123 Main St, Anytown, USA
Phone: (555) 123-4567
Payment: Card ending in 1234
Items:
- 1x LARGE HAND_TOSSED Pizza
  Toppings: None
  Price: $13.99

Total: $13.99
Estimated Delivery Time: 30 minutes

PAYMENT STATUS:
Current Status: PROCESSED
Payment successfully processed.

STORE STATUS:
Store Open: Yes
Delivery Available: Yes
Carryout Available: Yes

ORDER STATUS:
Current Status: CONFIRMED
Order is confirmed and being prepared.
```

## Context Management Implementation

### Provider Implementation
```typescript
export const pizzaOrderProvider: Provider = {
    get: async (runtime: IAgentRuntime, message: Memory) => {
        const orderManager = new PizzaOrderManager(runtime);
        const userId = message.userId;
        const order = await orderManager.getOrder(userId);
        const customer = await orderManager.getCustomer(userId);
        
        // Build context based on current state
        let context = "";
        
        // Add sections based on state
        context += buildPaymentStatus(order);
        context += buildRequiredActions(order);
        context += buildOrderSummary(order, customer);
        context += buildStoreStatus(orderManager);
        context += buildOrderStatus(order);
        
        return context;
    }
};
```

## Context Usage in Actions

### 1. startOrder Action
```typescript
// Context used for validation
const context = composeContext({
    state,
    template: extractionTemplate,
});

// Context influences order creation
const orderDetails = await generateObjectV2({
    runtime,
    context,
    modelClass: ModelClass.LARGE,
    schema: PizzaOrderSchema,
});
```

### 2. updateOrder Action
```typescript
// Context used for modification decisions
const context = composeContext({
    state,
    template: extractionTemplate,
    orderSummary: orderManager.getOrderSummary(order, customer),
});
```

### 3. confirmOrder Action
```typescript
// Context used for final validation
const context = await orderManager.getOrderSummary(order, customer);
```

## Context Flow Patterns

### 1. State-Based Context
```typescript
if (order.status === OrderStatus.AWAITING_PAYMENT) {
    context += "\nREQUIRED: Please provide credit card information...";
} else if (order.status === OrderStatus.PROCESSING) {
    context += "\nREQUIRED: Please review your order and confirm...";
}
```

### 2. Progress-Based Context
```typescript
context += "\nNEXT REQUIRED ACTION:\n";
context += orderManager.getNextRequiredAction(order, customer);
```

### 3. Error-Based Context
```typescript
if (order.paymentStatus === PaymentStatus.INVALID) {
    context += "Previous payment method was invalid...";
}
```

## Context Integration Points

### 1. With Order Manager
```typescript
// Order summary integration
context += orderManager.getOrderSummary(order, customer);

// Next action integration
context += orderManager.getNextRequiredAction(order, customer);
```

### 2. With Actions
```typescript
// Action validation using context
validate: async (runtime: IAgentRuntime, message: Memory) => {
    const context = await getOrderContext(runtime, message);
    return validateActionWithContext(context);
}
```

### 3. With AI System
```typescript
// Context used for decision making
const decision = await generateObjectV2({
    runtime,
    context,
    modelClass: ModelClass.LARGE,
    schema: ActionSchema,
});
```

## Context Management Best Practices

### 1. Consistency
- Maintain consistent section order
- Use standardized formatting
- Keep status messages clear

### 2. Relevance
- Include only necessary information
- Update context based on state
- Remove irrelevant details

### 3. Clarity
- Use clear section headers
- Provide explicit next steps
- Include error details when needed

## Context Usage Examples

### 1. Order Modification
```typescript
// Before modification
context = "Current order: 1x LARGE Pizza";

// After modification
context = "Updated order: 1x LARGE Pizza with EXTRA CHEESE";
```

### 2. Error Handling
```typescript
// Payment error
context = "Payment Status: INVALID\nPlease provide new payment method";

// Validation error
context = "Order Status: FAILED\nInvalid delivery address provided";
```

### 3. Progress Tracking
```typescript
// Initial state
context = "Next step: Provide delivery address";

// After address added
context = "Next step: Add payment information";
```

## Context-Driven Features

### 1. Smart Suggestions
```typescript
if (order.items.length === 0) {
    context += "\nSuggested: Would you like to try our special offers?";
}
```

### 2. Status Updates
```typescript
if (order.status === OrderStatus.CONFIRMED) {
    context += `\nDelivery ETA: ${order.estimatedWaitMinutes} minutes`;
}
```

### 3. Error Prevention
```typescript
if (incompatibleToppings) {
    context += "\nNote: Selected toppings may not complement each other";
}
```
