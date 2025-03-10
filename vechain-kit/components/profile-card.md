# Profile Card

## ProfileCard Component

The ProfileCard component provides a comprehensive display of user profile information, including avatar, domain, social links, and customization options.

<figure><img src="../../.gitbook/assets/image.png" alt="" width="322"><figcaption></figcaption></figure>

### Features

* Customizable header with generated background
* Avatar display with domain integration
* Social links integration (Email, Website, Twitter/X)
* Address display with copy functionality
* Edit and logout options for connected accounts
* Dark/Light mode support

### Props

```typescript
// ProfileCard Types
interface ProfileCardProps {
    // Required wallet address to display
    address: string;
    // Optional callback for edit button click
    onEditClick?: () => void;
    // Optional callback for logout button click
    onLogout?: () => void;
    // Toggle header visibility (default: true)
    showHeader?: boolean;
    // Toggle social links visibility (default: true)
    showLinks?: boolean;
    // Toggle description visibility (default: true)
    showDescription?: boolean;
    // Toggle display name visibility (default: true)
    showDisplayName?: boolean;
    // Toggle edit/logout buttons visibility (default: true)
    showEdit?: boolean;
}

// Internal types used by the component
interface TextRecords {
    display?: string;
    description?: string;
    email?: string;
    url?: string;
    'com.x'?: string;
}

interface WalletInfo {
    address: string;
    domain?: string;
    image?: string;
    isLoadingMetadata?: boolean;
    metadata?: TextRecords;
}
```

### Usage example

```typescript
// Example usage of ProfileCard component
import { ProfileCard } from '@vechain-kit/components';

const MyComponent = () => {
    // Example wallet address
    const walletAddress = "0x123...abc";

    return (
        <div style={{ maxWidth: '400px', margin: '0 auto' }}>
            {/* Basic usage */}
            <ProfileCard
                address={walletAddress}
            />

            {/* Full featured usage */}
            <ProfileCard
                address={walletAddress}
                showHeader={true}
                showLinks={true}
                showDescription={true}
                showDisplayName={true}
                showEdit={true}
            />
        </div>
    );
};

export default MyComponent;

/*
Note: The component will automatically:
- Resolve VET domains for the address
- Fetch and display avatar
- Load text records for social links
- Handle dark/light mode
- Manage connected account state
*/
```
