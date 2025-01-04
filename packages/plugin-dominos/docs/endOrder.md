# endOrder Action

## Overview
The `endOrder` action cancels the current pizza order and cleans up associated order data. This action provides a way to abort an order that's in progress.

## Key Features
- Cancels active order
- Clears order data from memory
- Provides confirmation of cancellation

## Usage
This action can be triggered with various natural language commands:
- "Cancel my order"
- "End order"
- "Stop the order"
- "Finish order"
- "Complete order"

## Implementation Details
- Verifies existence of active order before cancellation
- Cleans up order data from PizzaOrderManager
- Returns confirmation message to user

## Error Handling
- Handles cases where no active order exists
- Ensures proper cleanup of order data
- Provides clear feedback on cancellation status

## Integration
Works with:
- PizzaOrderManager for order cleanup
- Memory management system for data cleanup

## Provider Integration
The `pizzaOrderProvider` assists the endOrder action by:
- Verifying order status before cancellation
- Clearing context after cancellation
- Updating store availability
- Resetting payment status
- Preparing for new orders

The provider ensures:
- Clean order termination
- Proper status updates
- Context cleanup
- System readiness for new orders
