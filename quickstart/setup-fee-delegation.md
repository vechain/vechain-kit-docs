# Setup Fee Delegation

Fee delegation is **mandatory** if you want to use this kit with social login.&#x20;

You can learn more about what Fee Delegation is and what it is used for [in this section](../social-login/fee-delegation.md).

To allow your app to use the fee delegation service, you will need to pass a FEE\_DELEGATION\_URL argument to the VeChainKit Provider.

### FEE\_DELEGATION\_URL

You need to set this parameter in your `VeChainKitProvider` wrapper.

You need one per environment.

To obtain one, you can deploy your own custom solution or use [vechain.energy](https://vechain.energy/).

## Option 1: Create your own

You can deploy this as a microservice (through Lambda, Cloudflare, etc.) or as an endpoint of your backend.\


```typescript
import { Address, HDKey, Transaction, Secp256k1, Hex } from '@vechain/sdk-core';

// the default signer is a solo node seeded account
const DEFAULT_SIGNER = 'denial kitchen pet squirrel other broom bar gas better priority spoil cross'

export async function onRequestPost({ request, env }): Promise<Response> {
    const body = await request.json()
    console.log('Incoming request', body);

    const signerWallet = HDKey.fromMnemonic((env.SIGNER_MNEMONIC ?? DEFAULT_SIGNER).split(' '), HDKey.VET_DERIVATION_PATH).deriveChild(0);
    if (!signerWallet.publicKey || !signerWallet.privateKey) { throw new Error('Could not load signing wallet') }

    const signerAddress = Address.ofPublicKey(signerWallet.publicKey)
    const transactionToSign = Transaction.decode(
        Buffer.from(body.raw.slice(2), 'hex'),
        false
    );
    const transactionHash = transactionToSign.getSignatureHash(Address.of(body.origin))
    const signature = Secp256k1.sign(transactionHash.bytes, signerWallet.privateKey)

    return new Response(JSON.stringify({
        signature: Hex.of(signature).toString(),
        address: signerAddress.toString()
    }), {
        status: 200,
        headers: {
            'Content-Type': 'application/json',
            'access-control-allow-origin': '*'
        }
    })
}
```

## Option 2: use [vechain.energy](https://vechain.energy/)

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

{% embed url="https://learn.vechain.energy/vechain.energy/FeeDelegation/Setup/" %}

### [GO TO TUTORIAL](https://learn.vechain.energy/vechain.energy/FeeDelegation/Setup/)

{% hint style="info" %}
Add this address 0xD7B96cAC488fEE053daAf8dF74f306bBc237D3f5 (MAINNET) or 0x7C5114ef27a721Df187b32e4eD983BaB813B81Cb (TESTNET)  in {YourProjectName/FeeDelegation/Configurations/Smart Contract to sponsor  all transactions.
{% endhint %}

{% hint style="info" %}
<pre class="language-solidity"><code class="lang-solidity"><a data-footnote-ref href="#user-content-fn-1">/</a>// Smart contract to allow delegate all requests

/<a data-footnote-ref href="#user-content-fn-1">// SPDX-License-Identifier: MIT</a>
pragma solidity ^0.8.20;

contract FeeDelegation {
    /**
     * @dev Check if a transaction can be sponsored for a user
     */
    function canSponsorTransactionFor(
        address _origin,
        address _to,
        bytes calldata _data
    ) public view returns (bool) {
        return true;
    }
}
</code></pre>
{% endhint %}

{% hint style="info" %}
Enable email notifications to let you know of low VTHO balance.
{% endhint %}



[^1]: 
