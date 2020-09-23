## Account

#### Externally owned account

- controlled by private keys
- no code

#### Contract account (auto)

- controlled by code
- can't initiate new transactions on their own
  - can fire transaction in response to other transaction they received

#### message from EA or CA to CA will activate the code of CA.

#### consist of:

- nonce: number of transactions send by account
- balance: number of wei
- storageRoot: root node of Merkle tree
- codeHash
  - empty for EA
  - hash of the code for CA



## Merkle Tree

- Binary tree, father is the Hash of two sons
- Large amount of leaves
- has a key shows the path to value
- full nodes and light nodes
- Merkle proof



## Gas and Payment

#### Gas

- Transaction cause by computation need fee
- the fee called gas
- measured in gwei
  - 1 Ether = 1^18^ wei
  - 1 gwei = 1^9^ wei

#### Gas in transaction

- sender set the gas limit and gas price
  - so the $max~transaction~fee = gas~limit * gas~price$
- A transaction will use the gas according to the process
- if the used gas small than max transaction fee, then return the remaining gas to sender
- else, the transaction failed, state revert and the gas won't return.
- all the gas will award to "beneficiary" address (typically the miner's address)

#### Fees for storage

- storage cost resource in Ethereum

#### Purpose of fees

- complex uses of smart contracts will put a strain on the network
- prevent users from overtaxing the network
- protect the network from deliberate attack such as a loop.
- encourage the calculate and storage in network



## Transaction and messages

#### Transaction

- is a cryptographically signed piece of **instruction**

- two types: message calls and contract creations

  - contract creation is to create a CA

- consist:

  - nonce: number of transaction sent by the sender

  - gasPrice: price of unit gas

  - gasLimit: limit of gas

    > Why the fees be split into gasPrice and gasLimit?

  - to: address of the recipient

    > the contract account address does not yet exist, and so an empty value is used.

  - value: amount of wei to be transferred

  - v, r, s: variables to generate the signature

  - init(only exists for contract-creating transactions): Initialize the new contract account

  - data(optional and only exists for message calls): store data

#### Message and internal transactions:

- internal transactions will activate the code in CA
- from CA to CA

#### process:

![image-20200923111243358](note.assets/image-20200923111243358.png)
- internal transactions or messages don't contain a **gasLimit**.

  >  If, in the chain of transactions and messages, a particular message execution runs out of gas, then that **message’s execution will revert**, along with any subsequent messages triggered by the execution. However, the parent execution **does not** need to revert.

  

## Blocks

#### consist of: 

- the **block header**
- a **set of transactions**
- a **set of other block headers for the current block’s ommers.**

#### Ommers:

- Orphaned Blocks: The competing blocks do not make it into the main chain

- Ommer Blocks: within the sixth generation or smaller of the present block.

- Ommers is for award the miners who mined the orphaned blocks

  > Because of the way Ethereum is built, **block times are much lower (~15 seconds) than those of other blockchains**, like Bitcoin (~10 minutes). This enables faster transaction processing. However, one of the downsides of shorter block times is that **more competing block solutions are found by miners.**

#### Block header:

- parentHash: a hash of the parent header

- ommersHash: hash of list of ommers

- beneficiary: the account will receives the award

- stateRoot: root of the trie of the state

- transactionsRoot: root of the trie of the transactions

- receiptsRoot: root of the trie of the receipts of the transactions

  > these three root are the root of Merkle tree

- logsBloom: a **Bloom filter**(data structure) consists of log information

- difficulty: difficulty level

- number: length to the genesis (depth)

- gasLimit: current gas limit

- gasUsed: total used gas in the block

- timestamp: time stamp of the block's inception (be created)

- extraData: data about the block

- mixHash and nonce: to proves the enough computation (prove of work)

![image-20200923121529942](note.assets/image-20200923121529942.png)

#### Logs

- account address and event data
- stored in bloom filter



#### Transaction receipt

- Logs stored in the header come from the log information contained in the transaction receipt.

- include:
  - the block number
  - block hash 
  - transaction hash 
  - gas used by the current transaction
  - ...

#### Block difficulty

- increase the difficulty when a block validated quickly than the previous block.

- difficulty affect by **nonce**

- relationship between difficulty and nonce
  $$
  n \leq \frac{2^{256}}{H_d}
  $$
  where the H~d~  is the difficulty.

- adjust the time of validate a block by adjust the block difficulty



## Transaction Execution

#### Basic requirement of a transaction

- properly formatted **RLP**(Recursive Length Prefix)

- valid transaction signature

- valid transaction nonce (equal to sender's nonce)

- gas limit not less than the **intrinsic gas**:

  - a predefined cost (21,000 gas for executing the transaction)

    > why not in wei?

  - a gas fee for data sent(4 gas/byte for zero bytes, 68 gas/byte for non-zero bytes)

  - contract creation (32,000 gas)

    ![image-20200923161236959](C:\Users\89137\AppData\Roaming\Typora\typora-user-images\image-20200923161236959.png)

- **Upfront cost**

  ![image-20200923161223435](C:\Users\89137\AppData\Roaming\Typora\typora-user-images\image-20200923161223435.png)

#### Main process

1. calculate the remaining gas = gas limit - intrinsic gas

   ![image-20200923163101557](C:\Users\89137\AppData\Roaming\Typora\typora-user-images\image-20200923163101557.png)

2. transaction starts executing

   1. Ethereum keeps tracking the "substate".
   2. record information
      - self-destruct set: set of account will be self-destruct after transaction completes
      - log series: log information generated by VM
      - Refund balance: refund to sender, delete the storage will increase the refund

3. sender get the refund, mean while:
   - miner get the ether of the gas
   - gas used add to the block  gas counter
   - accounts in self-destruct set be deleted

#### Contract creation

- purpose: create a contract account

- declare the address using a special formula

- Initialize the new account:

  - setting the nonce to zero

  - setting the account balance 

    > If the sender sent some amount of Ether as value with the transaction, setting the account balance to that value

  - setting the storage as empty

  - setting the **codeHash** as the hash of a empty string

- Use init code send with transaction (account created)

- activate code will use gas

  - when gas is not enough, the out-of-gas (OOG) exception

- pay the contract-creation cost

- if all above done without exception, the state is allowed to persist.

#### Message calls

- similar to contract creation, it doesn't contain code but it can contain input data.

- there are no way to stop or revert the execution before, but

  > **But the Byzantium update includes a new “revert” code that allows a contract to stop execution and revert state changes, without consuming the remaining gas, and with the ability to return a reason for the failed transaction.** 



## Execution model

#### EVM

> skipped



#### Finalize a block

1. validate ommers: each ommer is valid and in 6^th^ generations
2. validate transactions: check the gas used
3. apply rewards (only mining): award miner
4. verify state and nonce: check the state trie



## Mining proof of work

#### Ethash

- algorithm
  $$
  (m, n) = PoW(H_{n'}, H_n, d)
  $$

  - m: mixHash
  - n: nonce
  - H~n'~ : new block's header (excluding the nonce and mixHash components)
  - H~n~ : nonce of the block header
  - d: DAG, a large data set



#### Mining as a security mechanism

> This is exactly what the PoW algorithm does: it ensures that a particular blockchain will remain canonical into the future, making it incredibly difficult for an attacker to create new blocks that overwrite a certain part of history (e.g. by erasing transactions or creating fake transactions) or maintain a fork.

- ensure there are only one truth that everyone believe in.
- keeping away from 51% attack by increase the difficulty.



#### Mining as a wealth distribution mechanism

> In order to ensure that the use of the PoW consensus mechanism for security and wealth distribution is sustainable in the long run, Ethereum strives to instill these two properties: 
>
> - Make it accessible to as many people as possible. In other words, **people shouldn’t need specialized or uncommon hardware to run the algorithm.** The purpose of this is to make the wealth distribution model as open as possible so that anyone can provide any amount of compute power in return for Ether. 
> - **Reduce the possibility for any single node (or small set) to make a disproportionate amount of profit**. Any node that can make a disproportionate amount of profit means that the node has a large influence on determining the canonical blockchain. This is troublesome because it reduces network security

- from PoW to PoS (Prove of Stake)















