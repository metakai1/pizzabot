# PizzaOrderManager Analysis

## Overview
The `PizzaOrderManager` class is the central component of the Dominos plugin, implementing the `OrderManager` interface. It handles all aspects of pizza ordering, from menu configuration to order processing.

## Class Structure

### Configuration Properties

#### System State
```typescript
availability = {
    isStoreOpen: true,
    isDeliveryAvailable: true,
    isCarryoutAvailable: true,
};
```
Tracks store and service availability:
- Store operating status
- Delivery service status
- Carryout service status

#### Required Fields
```typescript
requiredFields = {
    requiresCustomerName: true,
    requiresAddress: true,
    requiresPayment: true,
    requiresPhone: true,
    requiresEmail: true,
};
```
Defines mandatory customer information:
- Name requirement
- Address requirement
- Payment requirement
- Contact information requirements

#### Payment Configuration
```typescript
paymentConfig = {
    acceptsCash: false,
    acceptsCredit: true,
    requiresCVV: true,
    requiresPostalCode: true,
    maxFailedAttempts: 3,
};
```
Manages payment options and requirements:
- Payment method acceptance
- Security requirements
- Failure handling limits

### Menu Configuration

#### Base Pricing
```typescript
private readonly menuConfig = {
    defaultProductCode: "PIZZA",
    basePrices: {
        [PizzaSize.SMALL]: 9.99,
        [PizzaSize.MEDIUM]: 11.99,
        [PizzaSize.LARGE]: 13.99,
        [PizzaSize.XLARGE]: 15.99,
    },
    // ...
};
```
Defines base pizza prices by size

#### Crust Pricing
```typescript
crustPrices: {
    [PizzaCrust.HAND_TOSSED]: 0,
    [PizzaCrust.THIN]: 0,
    [PizzaCrust.PAN]: 1.0,
    [PizzaCrust.GLUTEN_FREE]: 2.5,
    [PizzaCrust.BROOKLYN]: 1.5,
}
```
Manages crust option pricing:
- Standard crusts (no additional cost)
- Premium crusts (additional cost)

#### Topping System
```typescript
toppingPrices: {
    STANDARD: 1.5,
    PREMIUM: 2.5,
    SPECIALTY: 3.5,
},
toppingCategories: {
    STANDARD: ["PEPPERONI", "MUSHROOMS", ...],
    PREMIUM: ["ITALIAN_SAUSAGE", "BACON", ...],
    SPECIALTY: ["GRILLED_CHICKEN", "PHILLY_STEAK", ...],
}
```
Comprehensive topping management:
- Price tiers
- Categorization
- Available options

#### Special Combinations
```typescript
specialCombos: {
    MEAT_LOVERS: {
        name: "Meat Lovers",
        discount: 2.0,
        requiredToppings: ["PEPPERONI", "ITALIAN_SAUSAGE", "BACON", "HAM"],
    },
    // ...
}
```
Predefined pizza combinations:
- Named combinations
- Discount pricing
- Required toppings

#### Topping Compatibility
```typescript
incompatibleToppings: [
    ["ANCHOVIES", "CHICKEN"],
    ["PINEAPPLE", "ANCHOVIES"],
    ["ARTICHOKE_HEARTS", "GROUND_BEEF"],
]
```
Manages topping restrictions:
- Incompatible combinations
- Flavor pairing rules

## Core Functionality

### Store Management
```typescript
async getNearestStoreId(address: string): Promise<string>
```
Finds nearest available store:
- Queries nearby stores
- Checks store capabilities
- Validates delivery options
- Returns optimal store ID

### Order Management
```typescript
async getOrder(userId: UUID): Promise<Order | null>
async saveOrder(userId: UUID, order: Order): Promise<void>
```
Handles order persistence:
- Order retrieval
- Order storage
- User association

### Customer Management
```typescript
async getCustomer(userId: UUID): Promise<Customer | null>
async saveCustomer(userId: UUID, customer: Customer): Promise<void>
```
Manages customer data:
- Customer information storage
- Customer retrieval
- Data persistence

### Order Processing

#### Progress Tracking
```typescript
getNextRequiredAction(order: Order, customer: Customer): string
calculateOrderProgress(order: Order, customer: Customer): OrderProgress
```
Monitors order completion:
- Required action identification
- Progress calculation
- Status updates

#### Price Calculation
```typescript
calculatePizzaPrice(item: OrderItem): number
checkSpecialCombos(toppings: PizzaTopping[]): number
```
Handles pricing logic:
- Base price calculation
- Topping price addition
- Special combo discounts

#### Validation
```typescript
validateToppings(toppings: PizzaTopping[]): OrderError | null
validateCustomerInfo(customer: Customer): OrderError | null
validatePaymentMethod(payment: PaymentMethod): OrderError | null
```
Comprehensive validation:
- Topping compatibility
- Customer information
- Payment details

### Order Summary
```typescript
getOrderSummary(order: Order, customer: Customer): string
formatCurrency(amount: number): string
formatTopping(topping: PizzaTopping): string
```
Generates order information:
- Detailed summaries
- Formatted prices
- Topping descriptions

## Integration Points

### External Services
- Dominos API integration
- Store location services
- Payment processing

### Internal Systems
- Agent runtime integration
- Memory management
- State persistence

## Error Handling
- Comprehensive error types
- Detailed error messages
- Failure recovery

## Usage Flow
1. Store location
2. Order initialization
3. Customer validation
4. Payment processing
5. Order confirmation
6. Status tracking

## Best Practices
1. Type safety throughout
2. Comprehensive validation
3. Clear error messaging
4. Efficient data management
5. Modular design
