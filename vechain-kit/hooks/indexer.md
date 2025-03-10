# Indexer

## Indexer Hooks

The hooks provide tools for interacting with the B3TR indexer service, offering sustainability and voting data:

### Sustainability Hooks

* `useSustainabilityActions`: Fetches sustainability actions with pagination for a user or app, returning detailed impact data including:
  * Environmental metrics (carbon, water, energy)
  * Waste metrics (mass, items, reduction)
  * Social impact (biodiversity, people)
  * Resource conservation (timber, plastic)
  * Educational impact (learning time)
  * Activity metrics (trees planted, calories burned)
  * Energy metrics (clean energy production)
  * Health metrics (sleep quality)

### Voting & Allocation Hooks

* `useRoundAppVotes`: Retrieves voting results for a specific round, including:
  * Number of voters
  * Total votes cast
  * App-specific voting data
  * Round statistics

### Usage Example

```typescript
// Example usage of Indexer hooks
import { 
    useSustainabilityActions,
    useRoundAppVotes 
} from '@indexer';

const ExampleComponent = () => {
    // Get sustainability actions with pagination
    const { 
        data: sustainabilityData,
        hasNextPage,
        fetchNextPage,
        isLoading: isLoadingActions 
    } = useSustainabilityActions({
        wallet: "0x...", // Optional: filter by wallet
        appId: "app_id", // Optional: filter by app
        direction: "desc", // Optional: sort direction
        limit: 10 // Optional: items per page
    });

    // Get voting results for a specific round
    const { 
        data: roundVotes,
        isLoading: isLoadingVotes 
    } = useRoundAppVotes({
        roundId: 1
    });

    // Handle loading more sustainability actions
    const handleLoadMore = () => {
        if (hasNextPage) {
            fetchNextPage();
        }
    };

    console.log(
        'Sustainability Actions:', sustainabilityData?.pages,
        'Has More Pages:', hasNextPage,
        'Round Votes:', roundVotes
    );

    return (
        // Your component JSX here
    );
};

export default ExampleComponent;

/*
Note: These hooks require:
- A valid indexer URL configuration
- Appropriate network settings
- Valid input parameters (wallet address, appId, or roundId)

Sustainability data includes:
- Environmental metrics (carbon, water, energy)
- Waste metrics (mass, items, reduction)
- Social impact (biodiversity, people)
- Resource conservation (timber, plastic)
- Educational impact (learning time)
- Activity metrics (trees planted, calories burned)
- Energy metrics (clean energy production)
- Health metrics (sleep quality)

Round votes data includes:
- Number of voters
- Total votes cast
- App-specific voting data
- Round statistics
*/
```
