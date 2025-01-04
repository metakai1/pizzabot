# PizzaOrderManager

## Overview
The `PizzaOrderManager` class is the core component responsible for managing pizza orders in the Dominos plugin. It implements the `OrderManager` interface and handles all aspects of order management, from store availability to payment processing.

## Key Components

### Store Management
- Manages store ID and availability
- Tracks store status (open/closed)
- Handles delivery and carryout availability

### Configuration Management
#### Required Fields
- Customer Name
- Address
- Payment Information
- Phone Number
- Email Address

#### Payment Configuration
- Credit Card Support
- CVV Requirements
- Postal Code Validation
- Failed Attempt Tracking
- Cash Payment Options

#### Menu Configuration
- Default Product Codes
- Base Pricing Structure
- Size Options
- Crust Types
- Topping Management

## Core Functionality

### Order Management
- Creates new orders
- Updates existing orders
- Validates order details
- Manages order status
- Handles order confirmation

### Customer Management
- Stores customer information
- Validates customer details
- Updates customer profiles
- Manages customer preferences

### Payment Processing
- Handles payment validation
- Processes payments
- Manages payment status
- Tracks payment errors

### Store Integration
- Connects with nearby stores
- Validates store availability
- Manages store-specific menus
- Handles store communications

## Error Handling
- Manages order errors
- Handles payment failures
- Processes validation errors
- Provides error feedback

## Integration Points
- Dominos API Integration
- Payment Gateway Integration
- Customer Data Management
- Store Location Services

## Provider Integration
The PizzaOrderManager works closely with the `pizzaOrderProvider` to:

### Context Management
- Provide real-time order status
- Supply customer information
- Track payment progress
- Monitor store availability

### Status Updates
- Update order progress
- Track payment status
- Monitor store conditions
- Manage customer data

### Decision Support
- Guide AI responses
- Determine next actions
- Validate operations
- Track order flow

### Data Flow
- Supply order details
- Provide customer context
- Update payment information
- Track store status

The provider acts as a bridge between the PizzaOrderManager and the AI system, ensuring all components have access to current order state and context.

## Usage
The PizzaOrderManager is used by various actions in the plugin:
- startOrder
- updateOrder
- confirmOrder
- endOrder
- updateCustomer
