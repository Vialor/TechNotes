# Basic Concepts
Blockchain: Smart Contract Platform
Bitcoin ⇒ Ethereum, transaction ⇒ transaction & agreement
Oracle: a device that provide data from external world to the smart contracts
Decentralized Oracle Network (Chainlink)
Hybrid Smart Contract: on-chain + off-chain agreements
  
web1: servers send static information to clients
web2: servers maintain platforms that allow clients to produce and consume information
web3: decentralized permissionless web with dynamic content without the servers
  
DeFi: decentrailized finance
DAO: decentrailized autonomous organization
  
address: derived from public key
public key: derived from private key
private key/secret key (access to one account)
mnemonic (access to all accounts within an account)
  
gas: a unit of computational measurement
gas usage < gas limit
transaction fee = (base fee + max priority fee) * gas usage
gas price = base fee + max priority fee
money to miners = max priority fee * gas usage
burnt amount = base fee * gas usage (This fee is gone forever)
# Blockchain
**Hash256**
**Blockchain:**
[https://andersbrownworth.com/blockchain/tokens](https://andersbrownworth.com/blockchain/tokens)
Block: find nonce such that (Block number, Nonce, Prev, Tx) is hashed to a special hash (starting with 4 zeros for instance, the specialty of this hash is well-maintained, so that the mining time (**block time**) of each block is relatively stable.)
Block number, Nonce, Tx(transactions), Prev(previous block’s hash), Hash
Blockchain: a chain of blocks (chained by Prev)
Tx transaction: amount, from, to, signature
  
## **Related Questions:**
### **Miner**
Miners get rewards for finding that special number in a new block, which is essentially minting or mining. (Block reward)
Miners get fewer and fewer rewards as the blockchain grows to avoid inflation, but miner still can get optional transction fees from user donation.
  
### Consensus Protocal
mechanism to agree on the state of blockchain, roughly contains two parts: **sybil resistance, chain selection**
  
**Sybil resistance mechanism:**
A **Sybil Attack** is launched by a single node by creating numerous fake identities in the network to gain disproportionate advantage over other users. **Sybil resistance mechanism** use certain methods to verify identities and avoid Sybil Attack.
**POW proof of work**
miners: get paid by part of transaction fees and block rewards, every one is competing to work more
huge energy cost due to the competition
**POS proof of stake**
validators: also get paid for appending blockchain, but validators are randomly selected based on the amount of their blockchain tokens. To cheat the system, one has to acquire a large amount of blockchain tokens.
  
**Chain selection algorithm:**
The algorithm to determine the correct blockchain
**Nakamoto concensus: POW + Longest chain rule**
**Longest chain rule:** When conflicts happen, believe the **longest blockchain** or the ledger that has most computational work put into it. (believe in proof of work or the majority) You have to posess half of all the computational capability of the world in order to cheat blockchain, and your pseudo accounts are useless without proof of honest work.
**longest chain:** the blockchain that has taken the most energy to build (not the the blockchain with most blocks)
**block confirmations:** number of blocks being added after the block of current transaction, greater the number, more secure the transaction.
  
### **Crypto signature**
Sign(message, sk secret key): Signiture
Verify(message, Signiture, pk public key): Boolean
  
### Scalability
**Sharding and Rollups**
**Layer 1**: base layer blockchain implementation
**Layer 2**: application built on top of layer 1