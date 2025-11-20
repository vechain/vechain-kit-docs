# New in V2

### **Development Speed**

Experience faster development with reduced bundle sizes, facilitating smoother performance in local environments.

### **Customization Options**

Not only you can open specific flows of the vechain-kit modal in an isolated mode (eg: show only the "receive" or the "send" flow without allowing the user to browse/see other parts of the modal), but we also added customization options to it, so you can now set the colours and fonts of your brand and make the modals look more similar to your app.

Head over to the Customization section to explore all the available options.

### **Simplified Installation**

Benefit from a more streamlined installation process, minimizing troubleshooting time and effort.

### **Connex Deprecation in favour of SDK**

The new version replaces Connex with the SDK, allowing more options for developers and moving away from a deprecated library.

### **Improved Fee Delegation Handling**

No more need to set up a fee delegation service: with V2 we added out-of-the-box transaction sponsorship for social logged-in users, that will pay for the transactions with their own tokens (by using the Generic Delegator tech), allowing you to not spend any cent for those users.

We also improved the useSendTransaction() hook allowing you to decide to sponsor a single transaction based on your criteria.

Keep in mind, though, that even if you are not required to pay for user transactions anymore, you may still want to sponsor some transaction here and there for new users or to show them (mainly to social login users) how many fees a transaction will require.

Head over to the [fee-delegation.md](../social-login/fee-delegation.md "mention")section to learn more about this.

### **Server-Side Rendering Compatibility**

Ensured that the new version supports server-side rendering (SSR) to enhance performance and SEO benefits.

### **Improved Documentation**

Access comprehensive and detailed documentation designed to guide you through the transition with ease.

### **New Features**

Explore new functionalities like swap capabilities and advanced hooks and components, enriching the development toolkit.

{% hint style="danger" %}
**Breakign Changes**

V2 may have some breaking changes based on your V1 integration. Please read more in details what changed from V1 and how to migrate to version 2 in [the following section](../migrations/upgrade-vechain-kit-from-1.x-to-2.x/).
{% endhint %}

{% hint style="warning" %}
**Version 1 Deprecation**\
Support for version 1 will cease, and updates will target version 2, encouraging users to migrate to leverage new features.
{% endhint %}
