# updateOrder Action

## Overview
The `updateOrder` action allows modification of an existing pizza order. It supports various types of updates including size changes, crust modifications, topping adjustments, and quantity updates.

## Modification Types
The action supports the following modification types:
- UPDATE_SIZE: Change pizza size
- UPDATE_CRUST: Change crust type
- ADD_TOPPING: Add new toppings
- REMOVE_TOPPING: Remove existing toppings
- ADD_PIZZA: Add another pizza to the order
- UPDATE_QUANTITY: Change pizza quantity
- UPDATE_INSTRUCTIONS: Modify special instructions

## Parameters
### ModificationSchema
- `type`: Type of modification (enum of supported modifications)
- `itemIndex`: Index of the pizza to modify
- `data`: Object containing modification details
  - `size`: New size (optional)
  - `crust`: New crust type (optional)
  - `topping`: Topping details (optional)
    - `code`: Topping identifier
    - `portion`: LEFT|RIGHT|ALL
    - `amount`: 1|2
  - `quantity`: New quantity (optional)
  - `specialInstructions`: New instructions (optional)
  - `newPizza`: Details for additional pizza (optional)

## Usage
This action is triggered when a user wants to modify an existing order. Example commands:
- "Change pizza size to large"
- "Add pepperoni to the first pizza"
- "Update quantity to 2"
- "Add another pizza to my order"

## Error Handling
- Validates modification parameters
- Ensures order exists before modifications
- Verifies item index is valid
- Provides clear feedback on modification status

## Provider Integration
The `pizzaOrderProvider` enhances the updateOrder action by:
- Maintaining real-time order status
- Tracking modification progress
- Providing context for AI decisions
- Validating store availability for changes
- Ensuring payment status is current

The provider supplies context about:
- Current order details
- Available modifications
- Store limitations
- Payment requirements
- Next required actions

## Integration
Works with:
- PizzaOrderManager for order updates
- Dominos API for applying changes
- LLM for understanding modification requests
