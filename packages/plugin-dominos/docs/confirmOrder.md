# confirmOrder Action

## Overview
The `confirmOrder` action finalizes and places the pizza order with Dominos. It performs final validation, pricing calculation, and order submission.

## Key Features
- Validates order details with Dominos
- Calculates final pricing
- Places order with Dominos
- Updates order status
- Provides order confirmation details

## Prerequisites
The action requires:
- Complete customer information
- Valid payment information
- Unconfirmed order status

## Validation
Performs multiple validation steps:
1. Checks for existing order and customer
2. Verifies customer information completeness
3. Validates payment information
4. Performs final validation with Dominos API

## Order Processing
1. Validates order with Dominos
2. Calculates final pricing
3. Places order
4. Updates order status to CONFIRMED
5. Saves updated order information

## Response
Returns confirmation details including:
- Order number
- Estimated delivery time
- Order status

## Error Handling
- Validates prerequisites before processing
- Handles API errors during order placement
- Provides clear error messages

## Integration
Works with:
- PizzaOrderManager for order management
- Dominos API for order placement
- Order status tracking system

## Provider Integration
The `pizzaOrderProvider` is crucial for the confirmOrder action:
- Validates all required information
- Ensures payment status is valid
- Checks store availability
- Tracks order confirmation process
- Updates final order status

The provider manages:
- Final validation context
- Payment confirmation status
- Store readiness checks
- Order completion tracking
- Confirmation notifications
