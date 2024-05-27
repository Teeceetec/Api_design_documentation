## SKILLHUB LMS API Documentation

**Introduction**

**Overview**

The SKILLHUB LMS API provides programmatic access to the XYZ Learning Management System, enabling developers to mint NFTs, manage ownership, and handle transactions on the blockchain. This documentation details the endpoints, request formats, response structures, and error codes necessary for integrating with the SKILLHUB LMS API.

## Audience

This documentation is intended for blockchain developers, software engineers, and technical integrators who need to interact with the SKILLHUB LMS smart contract. Familiarity with Ethereum smart contracts, Solidity, and ERC-721 tokens is assumed.

## Getting Started

**Authentication**

All interactions with the LMS smart contract require the user to have a valid Ethereum wallet. The API does not require traditional API keys but uses Ethereum addresses for authentication.

## Base URL

The base URL for all API requests is the Ethereum blockchain network where the smart contract is deployed. The contract address is needed for interactions.

## Endpoints

Overview
The SKILLHUB LMS API consists of several functions that manage NFTs, ownership, and transactions. Below is the detailed documentation for each function.

## Detailed Endpoint Documentation

1. MintNFT
Description:
Mints a new NFT using the pre-defined URI.

**Function Signature**:

```solidity
function MintNFT() public
```
Request:

Method: Transaction
Gas: Required gas for minting
Response:

Emits CreatedNft event
Emits TransferNftToContract event
Error Messages:

EXCEEDED_NUMBER_NFT_TOKENS_AVAILABLE: Max number of tokens exceeded.

**Code Sample**:

```solidity
LMS lms = LMS(contractAddress);
lms.MintNFT();
```
2. tokenURI
   
**Description**:
Returns the metadata URI of a given token.

**Function Signature**:

```solidity
function tokenURI(uint256 _tokenId) public view override returns (string memory)
```
Request:

Method: Call
Parameters: _tokenId (uint256)
Response:

Returns: string (URI of the token)
Error Messages:

BasicNft_TokenUriNotFound: Token URI not found.

**Code Sample**:

```solidity
string memory uri = lms.tokenURI(tokenId);
```
3. depositToken
Description:
Deposits tokens into the contract.

**Function Signature**:

```solidity
function depositToken(string memory _name, uint256 _amount) external payable nonReentrant
```

Request:

Method: Transaction
Parameters: _name (string), _amount (uint256)
Value: Ether value to deposit
Response:

Emits DepositLog event
Error Messages:

NOT_ENOUGH_ETHER: Not enough Ether sent.

**Code Sample**:

```solidity
lms.depositToken{value: amount}("John Doe", amount);
```

4. claimReward
Description:
Transfers an NFT to the user as a reward.

**Function Signature**:

```solidity
function claimReward() public``


Request:

Method: Transaction
Response:

Emits TransferNftToAnotherContract event

**Code Sample**:

```solidity
lms.claimReward();
```

5. updateURI
Description:
Updates the base URI for NFTs (owner only).

**Function Signature**:

```solidity
function updateURI(string memory uri) public _onlyOwner
```
Request:

Method: Transaction
Parameters: uri (string)
Response:

None
Error Messages:

NOT_AN_OWNER: Caller is not the owner.
Code Sample:

```solidity
lms.updateURI("ipfs://newUri");
```

6. transferOwnership
Description:
Transfers contract ownership to a new address.

**Function Signature**:

```solidity
function transferOwnership(address newOwner) public _onlyOwner
```
Request:

Method: Transaction
Parameters: newOwner (address)
Response:

Emits OwnerShipTransfer event
Error Messages:

INVALID_NEW_ADDRESS: New owner address is invalid.

**Code Sample**:

```solidity
lms.transferOwnership(newOwnerAddress);
```
7. withdraw
Description:
Withdraws all Ether from the contract to the owner's address.

Function Signature:

```solidity
function withdraw() external _onlyOwner nonReentrant
```
Request:

Method: Transaction
Response:

Emits Withdrawal event
Error Messages:

BALANCE_TOO_LOW_FOR_WITHDRAWAL: Contract balance is too low.
WITHDRAWAL_FAILED: Withdrawal failed.

**Code Sample**:

```solidity
lms.withdraw();
```
## User Journey Map

**Scenario 1**: Basic NFT Minting and Transfer

**Objective**: Mint an NFT and transfer it to a user.

**Steps**:
Deploy the LMS contract.
Call MintNFT to mint a new NFT.
Call claimReward to transfer the NFT to a user.
Expected Outcome: NFT is minted and transferred to the user.
Scenario 2: Handling Deposits and Withdrawals
Objective: Deposit tokens and withdraw them.

**Steps**:
Call depositToken to deposit Ether.
Call withdraw to transfer the balance to the owner's address.
Expected Outcome: Tokens are deposited and withdrawn successfully.
Guidelines for Common Scenarios
Data Retrieval
To retrieve data such as token metadata or contract owner, use the provided view functions.

**Example**:

```solidity
string memory uri = lms.tokenURI(tokenId);
address owner = lms.getOwner();
```
Data Submission
To submit data such as depositing tokens or updating URIs, use the corresponding transaction functions.

**Example**:

```solidity
lms.depositToken{value: amount}("John Doe", amount);
lms.updateURI("ipfs://newUri");
```
Error Handling
Handle errors by catching the revert messages and providing meaningful feedback.

**Example**:

```solidity

try lms.withdraw() {
    // Success
} catch Error(string memory reason) {
    // Handle error
}
```
Appendix
Glossary
- **ERC-721**: A standard for non-fungible tokens on the Ethereum blockchain.
- **ReentrancyGuard**: A modifier to prevent reentrant calls.
- **NonReentrant**: Ensures a function cannot be re-entered.
