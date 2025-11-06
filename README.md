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
X-Pubkey  |	Your public key derived from your private key (hex format with 0x prefix)
X-Timestamp |	Current Unix timestamp in seconds
X-Signature |	ECDSA signature of the request (DER-encoded hex with 0x prefix)
Content-Type |	Must be application/json