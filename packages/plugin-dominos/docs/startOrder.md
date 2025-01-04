# startOrder Action

## Overview
The `startOrder` action initiates a new pizza order in the Dominos plugin. It's responsible for creating a fresh order instance and validating that there isn't an existing active order.

## Key Features
- Checks for existing orders to prevent duplicates
- Extracts order details from natural language using LLM
- Validates order parameters
- Creates a new order instance

## Parameters
The action extracts the following parameters from user input:
- `size`: Pizza size (SMALL|MEDIUM|LARGE|XLARGE)
- `crust`: Crust type (HAND_TOSSED|THIN|PAN|GLUTEN_FREE|BROOKLYN)
- `toppings`: Array of toppings with portion and amount
- `quantity`: Number of pizzas
- `specialInstructions`: Any special preparation instructions

## Default Values
If information is missing, the following defaults are used:
- Size: MEDIUM
- Crust: HAND_TOSSED
- Toppings: None
- Quantity: 1

## Usage
This action is triggered when a user wants to start a new pizza order. It can be invoked with natural language commands like:
- "Start a new order"
- "Begin order"
- "Create order"
- "New order"

## Error Handling
- Prevents creation of multiple active orders
- Validates order parameters before creation
- Provides clear feedback if order creation fails

## Integration
Works closely with:
- PizzaOrderManager for order management
- Dominos API for order creation
- LLM for natural language processing

## Provider Integration
The `pizzaOrderProvider` supports the startOrder action by:
- Confirming no active order exists
- Providing initial order context
- Setting up status tracking
- Establishing payment context
- Preparing store availability information

The provider helps validate that:
- No existing order is in progress
- The store is open and available
- Delivery/carryout options are available
