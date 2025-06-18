HVAC Rewards Admin Portal: A Comprehensive Guide
1. Overview
The HVAC Rewards Admin Portal is a web-based interface designed to manage a sophisticated, on-chain customer loyalty and rewards program. It provides a user-friendly layer that abstracts away the complexities of blockchain interactions, allowing an administrator to manage the entire customer rewards lifecycle through simple forms and button clicks.

The system is built around a powerful concept: customers earn non-transferable points (SPN) for services, which can then be converted into a value-bearing token (SRV). This SRV token is then automatically staked to generate yield, which is ultimately paid out to the customer as a real-world reward (e.g., a gift card).

2. System Architecture & Core Components
The entire rewards program is powered by a suite of four interconnected smart contracts, all managed through the Admin Portal.

SPN Token (0x29...1818): The Loyalty Point Token. This is an ERC20 token that represents internal reward points. Its sole purpose is to be an on-chain tracker of rewards earned by customers. It is minted by the admin and has no direct monetary value.

Converter Contract (0x52...FA41): The Redemption Portal. This contract's job is to convert SPN points into the SRV token at a predefined rate. It does this by burning the customer's SPN and issuing them SRV.

Staking Contract (0x69...ddB7): The Staking Vault. This contract accepts SRV tokens for staking. When a user stakes SRV, they receive SREV, which is a "receipt" or "LP" token that represents their share of the staking pool.

Yield Contract (0x71...D9DE): The Yield Engine. This is the heart of the reward generation system. It reads users' SREV balances, calculates the yield they've earned based on a configurable APR, and distributes a separate RewardToken (e.g., USDC) as the actual payout.

3. How to Use the Admin Portal
The portal is designed to be intuitive, following a logical workflow from top to bottom.

Step 1: Initial Setup
Connect Wallet: Click the "Connect Wallet" button in the header. You must connect with the wallet that has administrative privileges (i.e., the "DAO Operator") over the smart contracts.

Configure Addresses: The primary contract addresses are pre-filled. After connecting, the portal will attempt to automatically fetch the dependent token addresses (SRV, SREV, and RewardToken). If this fails, they must be entered manually.

Step 2: Customer Management
Add a Customer: In the "Customer Actions" card, enter a unique identifier in the "Customer Number" field (e.g., CUST-1234).

Click "Add New Customer". This will generate a new, unique Ethereum wallet for that customer and add them to the dashboard below.

Issue Points: To grant rewards, select the customer from the dashboard (or re-enter their number), specify the amount of points in the "SPN Amount to Issue" field, and click "Issue SPN Points".

Step 3: The Automated Rewards Flow
Monitor Balances: The "Customer Dashboard" provides a real-time view of every customer's balance across all relevant tokens. You can use the "Refresh All Balances" button to fetch the latest on-chain data.

Convert & Stake: Once a customer has a sufficient SPN balance, click the "Convert & Stake" button in their row. This triggers the core automated sequence:

The portal, acting on behalf of the customer, approves the Converter contract to use their SPN.

It then calls burnAndConvert, swapping SPN for SRV.

Finally, it approves the Staking contract and stakes the newly acquired SRV, which starts the yield generation process.

Step 4: System & Payout Management
Fund the Yield Pool: The Yield Contract needs to be funded with the RewardToken that you intend to pay out. In the "System & Payout" card, enter an amount and click "Fund Yield Contract".

Set the APR: You can adjust the Annual Percentage Rate for staking rewards at any time by entering a new value (in Basis Points, where 100 BPS = 1%) and clicking "Update APR".

Process Payouts: The "Pending Yield" column shows how much each customer has earned. The payout process is a hybrid on/off-chain flow:

Off-Chain: Send the customer their real-world reward (e.g., a gift card for the amount shown in "Pending Yield").

On-Chain: Click the "Claim Yield" button for that customer. This reconciles the books on the blockchain by executing the claim() function, which transfers the equivalent amount of RewardToken from the Yield Contract (likely to your treasury wallet) and resets the customer's pending rewards to zero.

4. CRITICAL SECURITY NOTE
This Admin Portal is a powerful demonstration and management tool. However, for security reasons, its current implementation of customer wallet management is for simulation purposes only and is NOT production-ready.

In the code, when a new customer is added, a wallet is generated and its private key is stored in the browser's memory. This is done to allow the portal to automatically sign transactions on the customer's behalf for the "Convert & Stake" flow.

In a real-world, production environment, you must NEVER store or handle user private keys directly. The architecture should be transitioned to a secure backend system:

Wallet Management: A secure server should be responsible for creating and storing customer wallets, using a proper secrets management solution like AWS KMS, Google Cloud KMS, or HashiCorp Vault.

Transaction Signing: All transactions (approvals, conversions, staking) should be constructed and signed securely on the backend server before being sent to the blockchain.

The frontend portal should only be responsible for displaying data and sending requests to your secure backend API, never for handling sensitive key material.
