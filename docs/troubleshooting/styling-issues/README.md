# Styling Issues

VeChain Kit uses Chakra UI internally, which can cause styling conflicts with other CSS frameworks and custom styles.

### Common Problems

* CSS framework styles being overridden by VeChain Kit
* VeChain Kit components not rendering correctly due to missing Chakra setup
* Theme conflicts when your app also uses Chakra UI
* Unexpected styling on images, buttons, or form elements

### Quick Fixes

* **Install Chakra UI** as a peer dependency even if you don't use it directly
* **Use CSS layers** to control style precedence
* **Wrap your app** in ChakraProvider with ColorModeScript

### Issues in This Section

#### [Chakra Conflicts](./#chakra-conflicts)

Setting up Chakra UI properly and resolving conflicts when your app also uses Chakra.

#### [Chakra v3 host: useToken returns a snapshot](./#chakra-v3-host-usetoken-returns-a-snapshot-not-a-css-variable)

Why piping `useToken('colors', [...])` from a Chakra v3 host into the Kit's `theme` prop freezes colors at first render, and how to keep them reactive with `sys.token.var(...)`.

#### [CSS Framework Conflicts](./#css-framework-conflicts)

Using CSS layers to resolve conflicts with Tailwind, Bootstrap, and other CSS frameworks.

***

**Can't find your issue?** Search our [GitHub Issues](https://github.com/vechain/vechain-kit/issues) or ask on [Discord](https://discord.com/invite/vechain).
