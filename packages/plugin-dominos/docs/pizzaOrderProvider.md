# pizzaOrderProvider

## Overview
The `pizzaOrderProvider` is a context provider that supplies real-time order status and contextual information to the AI system. It acts as a bridge between the order management system and the AI's decision-making process.

## Key Features

### Status Tracking
- Payment Status Monitoring
- Order Status Updates
- Store Availability Checking
- Next Action Requirements

### Context Generation
Provides comprehensive context including:
- Payment Status Details
- Order Summary
- Required Next Actions
- Store Status Information
- Order Progress Updates

### Integration Points
- Works with PizzaOrderManager
- Interfaces with Memory System
- Connects with Order Status System
- Links to Payment Processing

## Context Sections

### Payment Status
- Current payment state
- Payment requirement notifications
- Invalid payment alerts

### Order Status
- Current order state
- Processing status
- Confirmation requirements

### Store Status
- Store availability
- Delivery status
- Carryout options

### Next Actions
- Required customer actions
- Order completion steps
- Payment requirements

## Usage
The provider is automatically called by the AI system to:
- Understand current order context
- Determine next required actions
- Guide user interactions
- Make informed decisions

## Technical Implementation
- Implements Provider interface
- Uses async/await for data retrieval
- Maintains consistent context format
- Provides structured status information
