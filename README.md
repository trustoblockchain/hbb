# Trusto Hybrid Biometric Blockcain (HBB) Protocol

The core library for running the Trusto transactions blockchain.

Trusto technology is a 2 layers block chain:

*   a transactions blockchain implemented by the HBB Protocol
*   a document blockchain implemented by the BDB Protocol

The structure of dual blockchains enables highspeed transactions (we aim at least
1500 trasactions per second) combined with high blockchain storage for documents. The
2 blockchains will be intimately linked with eachother, one will provice high speed
transactions and the other high database storage in the blockchain.

The HBB Protocol library implements the basic low level functionality of the Trusto transactions
block chain, like creating a block, creating a transaction, broadcasting a transaction
into the network, connecting with other peers. It is independent from the document
block chain (BDB) protocol. Also, provides functions for consensus protocol in the transactions
blockchain.

The transactions validation is not a function of the HBB Protocol. The process of transactions
validation is a function of the Hybrid Biomentrics Smart Contracts (HBS) library, because
transactions are valid or not in terms of a smart contract execution. HBS library links together
the two blockchains, the transactions and document blockchains.

## Installation

The only supported installation method, right now, is from source files,
[stack](http://www.haskellstack.org/).

## Building from source

### Prerequisites

You will need to [download and install stack](https://docs.haskellstack.org/en/stable/README/#how-to-install).

You also need [git](https://git-scm.com/) to get the latest source code.

### Get the source

    git clone https://github.com/trusto/hbb.git
    
### Building

To build the project, you need first to run `stack setup`. This command
will make sure you have the correct haskell compiler, and, if you don't
have it, it will download and install one in a separate location in such
a way to not interract with your existing haskell environment (if you have one):

    #> cd /the/location/of/trusto/hbb
    #> stack setup
    
After the setup (you only need to run setup once) you may build, test or install
the software. To build, simply issue:

    #> stack build
    
To run the tests:

    #> stack test
    
To install it in the stack's install directory, type:

    #> stack install

## Architecture

Trusto hybrid bolckchain is a high speed blockchain aiming to provide more than
1500 transactions per second. To do this wee need to address 2 kind of problems:

*   the consensus validation speed
*   the block size

We address the consensus validation speed by using a mixed PoW-PoS consensus protocol.
The miners will mine blocks using a classic proof of work algorithm. For each mined block,
the network will generate and sign 100 PoS blocks.

So, there will be 2 kind of blocks:

*   mined blocks obtained by a classic PoW system
*   generated blocks, generated and signed by a PoS system

Mined blocks will not contain transactions. They will contain and, thus, sign the previously
generated blocks. The generated blocks will contain transactions. Generated blocks, to be
valid, must be included in a mined block. The generated blocks are broadcast to the network.
Other nodes sign the generated block. They also can generate a new block with linking to the
signed block and broadcast both into the network. This will form a generated chain. The longest
generated chain is the winner chain and will be used for the next mined block.

When a new block is mined, the generated chain will start from zero. Since the root of generated
chains is always a mined block, only the chains having as root the newest mined block are valid
for generation, the others should be dropped by the network.

The size of blocks is an important issue, because we need the blocks to move fast between the
nodes in order to achieve highe speed transactions. For this reason, transactions will only
contain hashes for the actual transaction data. The transaction data will be stored in the
docuement blockchain, and will be able to contain complex validation rules, discussed in the
trusto hybrid biometric smart contract (HBS) library.

