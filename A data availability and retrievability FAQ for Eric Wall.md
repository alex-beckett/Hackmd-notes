# A brief data availability and retrievability FAQ
*This is a non-exhaustive list of questions that have come up in Eric Wall's discussions regarding data availability.*

### What is data availability?
Data availability refers to the availability of transactions in a block that is appended to the tip of the chain. During consensus, validators download the block to verify its availability. If the block contains any transactions that are withheld by a validator, the block is unavailable and will be rejected as invalid.

## What is the difference between data availability and what we call data retrievability?
Data availability is only concerned about the availability of a block when it is being proposed by a validator. Once the block has completed the consensus processes, is appended to the tip of the chain, and has propagated throughout the network, then the ability to download transactions from that block is what we call retrievability. This distinction is important because retrievability is a different problem from availability.

## What is the data availability problem?
The data availability problem is concerned with the ability for nodes to verify that a block is available. For validators this occurs during consensus. For non-consensus nodes, this occurs when the block has passed consensus and is propagated throughout the network.

In the context of light clients, the data availability problem is concerned with the ability for them to verify that a block is available without downloading the entire block. In Celestia, this is achieved through data availability sampling.

## What is the data retrievability problem?
The data retrievability problem refers to the ability for historical data to be retrieved - any block that has had another block built on top of it is historical. Fortunately, the ability to retrieve data is the weakest possible assumption, 1-of-N, where only a single copy of the data needs to be retrievable from anywhere on the internet. Additionally, history can be stored on inexpensive hard drives.

## Does a blockchain need to ensure that all historical data is retrievable forever?
No. It is not the job of a blockchain to guarantee that its history is retrievable in perpetuity. This is evident by the fact that the majority of blockchains don’t incentivize storage of historical data, including Bitcoin. However, this does not mean that other parties won’t have motive to store the history partially or fully without incentives from the protocol. 

## Who may store historical data if there is no reward?
Some examples include:
- Indexers that provide API queries for past data, such as The Graph.
- Applications and rollups that require historical data for certain processes.
- End users that want stronger assurances that their history will be retrievable.

## Is the naming of data availability confusing given that it has different meanings in other areas?
Yes.

## Do light nodes need to be aware that other light nodes are sampling?
No. The purpose of sampling is to provide the node a probabilistic guarantee that data is available. This is the same case for full nodes – they don’t need to be aware of other full nodes downloading the block because security guarantees are derived from downloading the block themselves.

## Do light nodes require a connection to other nodes to make samples?
Yes. This is also true for full nodes in all blockchains – they don’t conduct data availability sampling but they require a connection to at least one other node that will provide them with historical data over the p2p network. *Nodes dont receive block data out of thin air.*

## What other assumptions does Celestia require to ensure that data availability sampling is secure?
Celestia assumes that there is a minimum number of light nodes that are sampling blocks such that the blocks can be fully reconstructed from the stored samples. Since Celestia plans to elastically change its block size, it must require that a larger number of light nodes are present in the network to increase the block size. 

The other assumption that is made by light nodes is that they are connected to at least one honest full node. This ensures that they can receive fraud proofs for incorrectly erasure coded blocks. If a light node is not connected to an honest full node, such as during an eclipse attack, it can’t verify that the block is improperly constructed.

## Does the probability of losing some historical data increase as a blockchain’s history grows?
Yes. Even if some history is unable to be retrieved it does not impact the overall security and function of the blockchain. Validators can continue voting and producing new blocks. In PoS, new full nodes can bootstrap needing only the historical data up to the weak subjectivity checkpoint. Any party that requires historical data for certain functions, such as applications or rollups, should store their own data if they require stronger retrievability guarantees.

## If a blockchain wanted to provide stronger assurances of retrievability, what could it do?
A couple things:
- Rewarding nodes based on the amount of data they store and requests for data they serve (this is the case with some data storage blockchains).
- Stick data onto a data storage blockchain that does the above.

___

### If you want to dive further into the topic of what happens if data is removed from the history (history expiry) check out [this AMA with Vitalik]("https://www.reddit.com/r/ethereum/comments/qzvsfq/impromptu_technical_ama_on_history_expiry/")

