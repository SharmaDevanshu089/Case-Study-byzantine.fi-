# Case Study
#### This is the Case Study for https://www.byzantine.fi/ and https://www.turnkey.com/ how they work and how are they integrated.

### My Perspective about Byzantine : 
I have been Reading about *byzantine.fi* for few moments , according to their website they are a ***An institutional-grade digital credit product providing stable daily returns - without digital asset swings.***

 In my Opinion and in Simple words they are a Digital Credit lender which works by lending money to various sources(according to their website), their website doesnt show any more detail how they work. 
 

 #### What they Show they offer :
 - Stable returns: Target yield of Base (T-Bills) + 400bps (so around 8% per year)
 - Overcollateralised lending in USD-backed digital assets.
 - 24/7 liquidity: Assets are always accessible
 - Safety and compliance: Strategy curated by Keyrock, a regulated EU & US Digital Asset Service  Provider, under full AML, KYC, MiCA, and SOC 2 Type II frameworks.
 - Institutional grade custody and insurance on 100% of the capital deposited
 - Active risk monitoring: Continuous rebalancing and diversification to preserve capital.
 - Transparent reporting: Real-time visibility on portfolio and performance.
 - Byzantine Prime is built to deliver MMF-like safety, S&P-level returns, and modern liquidity - without forcing treasurers to compromise between performance and trust.

Their Website Specify how they are Using money to Show the returns. Firstly major Corpos can flood their excess **liquidity** to them and their dev docs specify they expose ***unified, composable, and simple to use*** . These money gets converted to digital ***StableCoins***, which are know for their remarkable stablity as they are backed by govts. This money which recently has been converted to Stable Coin emits same value of the currency they repesent . Which is then lent to various institutions. 

### Developers Perspective :
The byzantine.fi exposes various apis with both ***REST*** and ***https***. This makes me think they are built with automation as well as third party interface integrations. 
#### Why would a Investment application need automations ? 
In my Opinion this was probably done to help the bots directly fetch and flood the application without any human intervention  and the web apis problably so that others can integrate it with their application. 

## How to Interact with the APIS? 

The Api Guide book clearly specify before any Requests the application needs authentication headers, the 4 main headers specified in the Documentation are: 

Header |	Description
----------|--------------
`X-Pubkey`  |	Your public key derived from your private key (hex format with 0x prefix)
`X-Timestamp` |	Current Unix timestamp in seconds
`X-Signature` |	ECDSA signature of the request (DER-encoded hex with 0x prefix)
`Content-Type` |	Must be application/json

### Here are some more Requirement by their apis:

 - `timestamp`: Unix timestamp in seconds.
 - `HTTP method in uppercase`: Like `GET` or `POST`.
 - Hashing the message using SHA-256.
 - Encoding the signature in DER format and converting to hex.
 - Signing the hash with your private key using ECDSA (secp256k1 curve).

 ## Basic Nodejs Example : 

 ```
 import elliptic from "elliptic";
import { createHash } from "crypto";

const EC = elliptic.ec;
const ec = new EC("p256");

const INTEGRATOR_PRIVATE_KEY = process.env.INTEGRATOR_PRIVATE_KEY;

export function generateHeaders(method, pathAndQuery, body = "") {
  // Get the key pair
  const cleanPrivateKey = INTEGRATOR_PRIVATE_KEY.replace(/^0x/, "");
  const keyPair = ec.keyFromPrivate(cleanPrivateKey, "hex");
  const publicKey = "0x" + keyPair.getPublic(true, "hex");

  // Get the timestamp in seconds since the Unix epoch (UTC)
  const timestamp = Math.floor(Date.now() / 1000).toString();

  // Build the message to sign
  // Format: {timestamp}{method}{path_and_query}{body}
  const bodyStr = typeof body === "object" ? JSON.stringify(body) : body;
  const message = `${timestamp}${method.toUpperCase()}${pathAndQuery}${bodyStr}`;

  // Hash the message and sign it
  const messageHash = createHash("sha256").update(message).digest();
  const signature = keyPair.sign(messageHash);

  // Encode the signature in DER then in hex
  const signatureHex = "0x" + signature.toDER("hex");

  return {
    "X-Pubkey": publicKey,
    "X-Timestamp": timestamp,
    "X-Signature": signatureHex,
    "Content-Type": "application/json",
  };
}
 ```


---

 # Turnkey: 

 ## What is Turnkey?
Turnkey provides secure, scalable and programmable crypto infrastructure for embedded wallets and onchain transaction automation. This is like a Vault Manager in a Bank. When using turnkey you can move your crypto keys securly without ever compromising in security or storing localy.

## Why is Turnkey?
 Coin Key is a one of the most valuble things in a Crypto Circlution. And holding the is more difficult. Storing the key in a server and / or in a client envirment is prone to malvare attack or even ram leak. ***Turnkey*** solves this problem by storing the key in a secure enclave with various level of encription. It is like bank teller it opens its api to sign or transver the keys but never let end user or even server acess the key.

 ## Core Conecpts:

 ***Organizations (parent orgs)***: The initial parent organization typically represents an entire Turnkey-powered application.

***Sub-organizations (sub-orgs)***: Fully segregated organizations, typically representing an end user, nested under the parent organization. Parent orgs cannot modify the contents of a sub-org.

***Resources***: All identifiers within parent orgs such as users, policies, and wallets are collectively referred to as resources.

***Users: ***Resources within organizations or sub-organizations that can submit activities to Turnkey via a valid credential.

***Authenticators:*** Each parent org, sub-org and user contain their own sets of authenticators that you can configure, including their own wallets, API keys, and private keys.

***Activities:*** All actions Organizations can take such as signing transactions or creating users are known as activities.

***Policies:*** Policies govern all activities and permissions within Organizations.
***Root users:*** Users with root permissions, meaning they can bypass the policy engine to take any action within that specific organization.

***Root quorum:*** A pre-determined consensus threshold that consists of a set of Root Users. This consensus threshold must be reached in order for any root permissions to take place.

***Wallets:*** A collection of cryptographic private/public key pairs that share a common seed. HD seed phrases can generate multiple wallet accounts (addresses) for signing operations.