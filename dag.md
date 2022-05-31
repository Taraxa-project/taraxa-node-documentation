# DAG

DagBlock class reperesent a DAG block which contains:

* Transactions included in DAG block
* Pivot hash
* Tips hashes
* Dag block level which is a max of tips and pivot level plus one
* VDF sortition&#x20;
* Block author

DagBlock is created in BlockProposer class. Once created it is actually added in DAG in DagManager class. Flow chart below presents the DagBlock proposal algorithm:

![DAG block proposal](<.gitbook/assets/dag\_proposal (1).jpg>)

DAG blocks once proposed are gossiped on the network along with the transactions and they are considered in non-finalized state until they are first proposed in a pbft block and pbft block is finalized. Flow chart below displays DAG block flow within a node depending if the node has proposed the block or if the block was received over the network and needs to be verified. Class DagBlockManager controls the verification and insertions of blocks into DAG which is actually done in the DagManager class:

&#x20;

![DAG block](<.gitbook/assets/dag\_block (1).jpg>)
