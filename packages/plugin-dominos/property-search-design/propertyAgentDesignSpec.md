# Property Search Agent Design Specification

## Design Goals
1. Create a conversational property search interface
2. Handle complex, natural language property queries
3. Maintain search context across interactions
4. Support iterative search refinement
5. Provide structured data for database queries
6. Enable semantic understanding of property attributes

## System Components

### 1. Search Session Management
```typescript
interface SearchSession {
    status: "ACTIVE" | "INACTIVE";
    lastQuery: string | null;
    results: PropertyResult[];
    filters: Record<string, any>;
}
```

### 2. Property Metadata Schema
```typescript
interface PropertyMetadata {
    rank: number;
    name: string;
    neighborhood: string;
    zoning: ZoningType;
    plotSize: PlotSize;
    buildingType: BuildingType;
    distances: {
        ocean: DistanceInfo;
        bay: DistanceInfo;
    };
    building: {
        floors: Range;
        height: Range;
    };
    plotArea: number;
}
```

### 3. Search Query Processing
Input format from LLM:
```json
{
    "searchText": string,
    "metadata": {
        "neighborhood": string[],
        "zoningTypes": string[],
        "plotSizes": string[],
        "buildingTypes": string[],
        "maxDistance": {
            "ocean": number | null,
            "bay": number | null
        },
        "rankRange": {
            "min": number | null,
            "max": number | null
        },
        "floorRange": {
            "min": number | null,
            "max": number | null
        }
    }
}
```

## Core Components

### 1. Initial Search Action
```typescript
export const startPropertySearch: Action = {
    name: "START_PROPERTY_SEARCH",
    description: "Initiates a property search session",
    similes: ["SEARCH_PROPERTIES", "FIND_PROPERTIES", "LOOK_FOR_PROPERTIES"],
    handler: async (runtime: IAgentRuntime, message: Memory, state: State) => {
        const searchManager = new PropertySearchManager(runtime);
        await searchManager.createSearchSession(message.userId, {
            status: "ACTIVE",
            lastQuery: null,
            results: [],
            filters: {}
        });
        return "I'm ready to help you search for properties. What kind of property are you looking for?";
    }
}
```

### 2. Search Manager
```typescript
export class PropertySearchManager {
    constructor(private runtime: IAgentRuntime) {}

    async getSearchSession(userId: string) {
        return await this.runtime.getMemory(userId + "_property_search");
    }

    async updateSearchResults(userId: string, results: PropertyResult[]) {
        const session = await this.getSearchSession(userId);
        session.results = results;
        await this.runtime.setMemory(userId + "_property_search", session);
    }

    async executeSearch(searchMetadata: SearchMetadata) {
        return await this.memorySystem.searchPropertiesByParams(searchMetadata);
    }
}
```

### 3. Context Provider
```typescript
export const propertySearchProvider: Provider = {
    get: async (runtime, message) => {
        const searchManager = new PropertySearchManager(runtime);
        const session = await searchManager.getSearchSession(message.userId);
        
        if (!session) return "";

        let context = `${message.userId} is currently searching for properties.\n`;
        // Add results and last query to context
        return context;
    }
}
```

## Action/Provider Flow

### 1. Search Initiation
- User triggers START_PROPERTY_SEARCH
- Session created
- Provider begins context injection

### 2. Query Processing
- User submits natural language query
- LLM converts to structured metadata
- Database search executed
- Results stored in session

### 3. Result Presentation
- Provider shows current results
- Maintains search context
- Enables refinement queries

### 4. Iterative Refinement
- User can modify search
- Previous context maintained
- Results updated incrementally

## Database Interface

```typescript
interface LandDatabaseAdapter {
    init(): Promise<void>;
    close(): Promise<void>;
    searchPropertiesByParams(params: SearchParams): Promise<PropertyResult[]>;
    storeProperty(metadata: PropertyMetadata): Promise<string>;
    getLandKnowledgeById(id: string): Promise<LandKnowledge | null>;
}
```

## Integration Notes

### 1. State Management
- Use runtime.getMemory/setMemory for persistence
- Maintain search session per user
- Store results for context

### 2. LLM Integration
- Use generateObjectV2 for query parsing
- Provide detailed system prompt
- Validate output with Zod schema

### 3. Error Handling
- Validate search parameters
- Handle missing data gracefully
- Provide meaningful error messages

### 4. Performance Considerations
- Cache frequent searches
- Limit result sets
- Optimize database queries

## Future Enhancements

1. Semantic similarity search
2. Property comparison features
3. Advanced filtering options
4. Search history management
5. Favorite properties tracking

## Testing Strategy

### 1. Unit Tests
- Test individual components (SearchManager, Provider)
- Validate query parsing
- Test state management

### 2. Integration Tests
- End-to-end search flows
- Database integration
- LLM query generation

### 3. Performance Tests
- Load testing search capabilities
- Memory usage monitoring
- Response time benchmarking
