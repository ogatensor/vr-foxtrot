### Out of Scope & Rules

These impacts are out of scope for this bug bounty program.

**All Categories:**

- Impacts requiring attacks that the reporter has already exploited themselves, leading to damage
- Impacts caused by attacks requiring access to leaked keys/credentials
- Impacts caused by attacks requiring access to privileged addresses (governance, strategist) except in such cases where the contracts are intended to have no privileged access to functions that make the attack possible
- Impacts relying on attacks involving the depegging of an external stablecoin where the attacker does not directly cause the depegging due to a bug in code
- Mentions of secrets, access tokens, API keys, private keys, etc. in Github will be considered out of scope without proof that they are in-use in production
- Best practice recommendations
- Feature requests
- Impacts on test files and configuration files unless stated otherwise in the bug bounty program

**Blockchain/DLT & Smart Contract Specific:**

- Incorrect data supplied by third party oracles
    - Not to exclude oracle manipulation/flash loan attacks
- Impacts requiring basic economic and governance attacks (e.g. 51% attack)
- Lack of liquidity impacts
- Impacts from Sybil attacks
- Impacts involving centralization risks

**Websites and Apps**

- Theoretical impacts without any proof or demonstration
- Impacts involving attacks requiring physical access to the victim device
- Impacts involving attacks requiring access to the local network of the victim
- Reflected plain text injection (e.g. url parameters, path, etc.)
    - This does not exclude reflected HTML injection with or without JavaScript
    - This does not exclude persistent plain text injection
- Any impacts involving self-XSS
- Captcha bypass using OCR without impact demonstration
- CSRF with no state modifying security impact (e.g. logout CSRF)
- Impacts related to missing HTTP Security Headers (such as X-FRAME-OPTIONS) or cookie security flags (such as “httponly”) without demonstration of impact
- Server-side non-confidential information disclosure, such as IPs, server names, and most stack traces
- Impacts causing only the enumeration or confirmation of the existence of users or tenants
- Impacts caused by vulnerabilities requiring un-prompted, in-app user actions that are not part of the normal app workflows
- Lack of SSL/TLS best practices
- Impacts that only require DDoS
- UX and UI impacts that do not materially disrupt use of the platform
- Impacts primarily caused by browser/plugin defects
- Leakage of non sensitive API keys (e.g. Etherscan, Infura, Alchemy, etc.)
- Any vulnerability exploit requiring browser bugs for exploitation (e.g. CSP bypass)
- SPF/DMARC misconfigured records)
- Missing HTTP Headers without demonstrated impact
- Automated scanner reports without demonstrated impact
- UI/UX best practice recommendations
- Non-future-proof NFT rendering

**Prohibited Activities:**

- Any testing on mainnet or public testnet deployed code; all testing should be done on local-forks of either public testnet or mainnet
- Any testing with pricing oracles or third-party smart contracts
- Attempting phishing or other social engineering attacks against our employees and/or customers
- Any testing with third-party systems and applications (e.g. browser extensions) as well as websites (e.g. SSO providers, advertising networks)
- Any denial of service attacks that are executed against project assets
- Automated testing of services that generates significant amounts of traffic
- Public disclosure of an unpatched vulnerability in an embargoed bounty

Any bugs that cannot be classified either in the in-scope or out-of-scope lists will be considered for reward at the discretion of Inverse Finance.