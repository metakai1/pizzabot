# Types System Analysis

## Overview
The types system forms the foundation of the Dominos plugin, defining the core data structures and interfaces that enable type-safe pizza ordering operations. This document provides a detailed analysis of each component in the type system.

## Order Status System

### OrderStatus Enum
```typescript
enum OrderStatus {
    NEW = "NEW",
    AWAITING_CUSTOMER_INFO = "AWAITING_CUSTOMER_INFO",
    AWAITING_PAYMENT = "AWAITING_PAYMENT",
    AWAITING_CONFIRMATION = "AWAITING_CONFIRMATION",
    PROCESSING = "PROCESSING",
    CONFIRMED = "CONFIRMED",
    FAILED = "FAILED",
}
```

This enum represents the complete lifecycle of an order:
1. `NEW`: Initial state when order is created
2. `AWAITING_CUSTOMER_INFO`: Waiting for customer details
3. `AWAITING_PAYMENT`: Customer info received, needs payment
4. `AWAITING_CONFIRMATION`: Payment received, needs final confirmation
5. `PROCESSING`: Order is being processed
6. `CONFIRMED`: Order successfully placed
7. `FAILED`: Order failed at some point

## Payment System

### PaymentStatus Enum
```typescript
enum PaymentStatus {
    NOT_PROVIDED = "NOT_PROVIDED",
    INVALID = "INVALID",
    VALID = "VALID",
    ON_FILE = "ON_FILE",
    PROCESSED = "PROCESSED",
}
```

Tracks the state of payment:
- `NOT_PROVIDED`: No payment information received
- `INVALID`: Payment information is invalid
- `VALID`: Payment information validated but not processed
- `ON_FILE`: Payment information stored for customer
- `PROCESSED`: Payment successfully processed

### PaymentMethod Interface
```typescript
interface PaymentMethod {
    type: string;
    cardNumber?: string;
    expiryDate?: string;
    cvv?: string;
    postalCode?: string;
    isValid: boolean;
}
```

Defines payment information structure:
- Optional card details (for different payment types)
- Validation status tracking
- Postal code for address verification

## Customer System

### Customer Interface
```typescript
interface Customer {
    id?: string;
    name: string;
    phone: string;
    email: string;
    address: string;
    paymentMethod?: {
        cardNumber?: string;
        expiryDate?: string;
        cvv?: string;
        postalCode?: string;
    };
    isReturning: boolean;
}
```

Manages customer information:
- Basic contact details
- Delivery address
- Optional payment information
- Customer status tracking

## Pizza Configuration

### Size and Crust Enums
```typescript
enum PizzaSize {
    SMALL = "SMALL",
    MEDIUM = "MEDIUM",
    LARGE = "LARGE",
    XLARGE = "XLARGE",
}

enum PizzaCrust {
    HAND_TOSSED = "HAND_TOSSED",
    THIN = "THIN",
    PAN = "PAN",
    GLUTEN_FREE = "GLUTEN_FREE",
    BROOKLYN = "BROOKLYN",
}
```

Defines available pizza configurations:
- Four standard sizes
- Five crust options
- Type-safe selection

### Topping System
```typescript
enum ToppingPortion {
    LEFT = "LEFT",
    RIGHT = "RIGHT",
    ALL = "ALL",
}

interface PizzaTopping {
    code: string;
    portion: ToppingPortion;
    amount: number; // 1 for normal, 2 for extra
}
```

Manages topping customization:
- Portion control (half/whole pizza)
- Amount control (normal/extra)
- Topping identification via codes

## Order Management

### OrderItem Interface
```typescript
interface OrderItem {
    productCode: string;
    size: PizzaSize;
    crust: PizzaCrust;
    quantity: number;
    toppings: PizzaTopping[];
    specialInstructions?: string;
}
```

Represents individual items in an order:
- Product identification
- Size and crust selection
- Topping customization
- Quantity tracking
- Special instructions

### Order Interface
```typescript
interface Order {
    status: OrderStatus;
    paymentStatus: PaymentStatus;
    paymentMethod?: PaymentMethod;
    customer?: Customer;
    items?: OrderItem[];
}
```

Central order representation:
- Status tracking
- Payment information
- Customer details
- Order items

## Progress Tracking

### OrderProgress Interface
```typescript
interface OrderProgress {
    hasCustomerInfo: boolean;
    hasPaymentMethod: boolean;
    hasValidPayment: boolean;
    isConfirmed: boolean;
}
```

Tracks order completion status:
- Customer information status
- Payment method status
- Payment validation status
- Confirmation status

## Error Handling

### Error System
```typescript
enum ErrorType {
    PAYMENT_FAILED = "PAYMENT_FAILED",
    VALIDATION_FAILED = "VALIDATION_FAILED",
    CUSTOMER_NOT_FOUND = "CUSTOMER_NOT_FOUND",
    SYSTEM_ERROR = "SYSTEM_ERROR",
    NETWORK_ERROR = "NETWORK_ERROR",
}

interface OrderError {
    type: ErrorType;
    message: string;
    code: string;
    details?: any;
}
```

Comprehensive error handling:
- Categorized error types
- Detailed error information
- Optional additional details

## Store Management

### OrderManager Interface
```typescript
export interface OrderManager {
    storeId: string;
    availability: {
        isStoreOpen: boolean;
        isDeliveryAvailable: boolean;
        isCarryoutAvailable: boolean;
    };
    requiredFields: {
        requiresCustomerName: boolean;
        requiresAddress: boolean;
        requiresPayment: boolean;
        requiresPhone: boolean;
        requiresEmail: boolean;
    };
    paymentConfig: {
        acceptsCash: boolean;
        acceptsCredit: boolean;
        requiresCVV: boolean;
        requiresPostalCode: boolean;
        maxFailedAttempts: number;
    };
}
```

Store configuration and management:
- Store identification
- Availability tracking
- Required field configuration
- Payment configuration

## Event System

### OrderEvent Type
```typescript
type OrderEvent =
    | { type: "UPDATE_CUSTOMER_INFO"; payload: Partial<Customer> }
    | { type: "ADD_ITEM"; payload: OrderItem }
    | { type: "REMOVE_ITEM"; payload: string }
    | { type: "UPDATE_PAYMENT"; payload: PaymentMethod }
    | { type: "PROCESS_ORDER"; payload: Order }
    | { type: "HANDLE_ERROR"; payload: OrderError };
```

Event-based state management:
- Customer information updates
- Order modification events
- Payment updates
- Order processing
- Error handling

## Type System Usage

### In Code
The type system is used throughout the plugin to:
1. Ensure type safety
2. Provide IntelliSense support
3. Catch errors at compile time
4. Document code behavior
5. Enable proper error handling

### In Plugin Actions
Actions use these types to:
1. Validate input data
2. Transform order states
3. Handle errors appropriately
4. Maintain consistent data structure

### In State Management
The type system enables:
1. Predictable state transitions
2. Type-safe event handling
3. Consistent data flow
4. Reliable error handling
