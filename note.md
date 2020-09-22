### Account

- Externally owned account
  - controlled by private keys
  - no code
- Contract account (auto)
  - controlled by code
  - can't initiate new transactions on their own
    - can fire transaction in response to other transaction they received
- message from EA or CA to CA will activate the code of CA.
- consist of:
  - nonce: number of transactions send by account
  - balance: number of wei
  - storageRoot: root node of Merkle tree
  - codeHash
    - empty for EA
    - hash of the code for CA



### Merkel Tree

- Binary tree, father is the Hash of two sons
- Large amount of leaves
- has a key shows the path to value
- full nodes and light nodes
- Merkel proof

### Gas and Payment

- Gas
  - Transaction cause by computation need fee
  - the fee called gas
  - measured in gwei
    - 1 Ether = 1e18 wei
    - 1 gwei = 1e9 wei
- Gas in transaction
  - sender set the gas limit and gas price
    - so the $max~transaction~fee = gas~limit * gas~price$
  - A transaction will use the gas according to the process
  - if the used gas small than max transaction fee, then return the remaining gas to sender
  - else, the transaction failed, state revert and the gas won't return.
  - all the gas will award to "beneficiary" address (typically the miner's address)
- Fees for storage
- Purpose of fees
  - complex uses of smart contracts will put a strain on the network
  - prevent users from overtaxing the network
  - protect the network from deliberate attack such as a loop.
  - encourage the calculate and storage in network

### Transaction and messages