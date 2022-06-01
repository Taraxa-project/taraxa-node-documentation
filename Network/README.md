# Network 

Taraxa peer to peer network is based on the p2p protocol used by Ethereum with some minor customization.
Taraxa uses custom p2p library based on aleth libp2p C++ library.
The library itself implements two main protocols:
 * UDP based discovery protocol: https://github.com/ethereum/devp2p/blob/master/discv5/discv5.md
 * TCP based data exchange protocol: https://github.com/ethereum/devp2p/blob/master/rlpx.md
 
Each node will actively try to discovery and connect up to network_ideal_peer_count and it will accept connections up to network_max_peer_count which are parameters configurable in the taraxa configuration file.

Class Network is the main interface for configuring network on node startup and for other node modules and classes to send data to network.

On new DAG block being verified and added to the DAG, DagManager class invokes Network::onNewBlockVerified function which will gossip the block to all connected peers.
PbftManager will invoke Network class:
 * On new pbft block proposed: Network::onNewPbftBlock
 * On new pbft votes: Network::onNewPbftVotes
 * Bradcasting previous round next votes: Network::broadcastPreviousRoundNextVotesBundle
 
Class Network contains an object of class TaraxaCapability which is implementation of p2p library interface dev::p2p::CapabilityFace. TaraxaCapability is currently the only implemented capability although the underlying rlpx protocol supports multiple capabilities to run in parallel over the same connection.

TaraxaCapability contains packet handlers which contain methods for both processing received packets and to send out data. Chart below is displaying the flow of data from other modules through these networking classes up to the p2p library which actually sends the data packets  to connected peers.


![Network](<network1.jpg>)


Receiving and processing data packets from other nodes is presented in chart below:


![Network](<network2.jpg>)


Once a connection between two nodes is established status messages are sent from one node to the other. Status messages are also sent in regular intervals. This is handled in class StatusPacketHandler. Based on the data received in status messages some actions might be triggered:
 * If there is any mismatch in node version, network id or genesis hash, nodes are not compatible and nodes are disconnected
 * If node is out of sync and it's period is below the period of connected node, trigger pbft syncing
 * If node is not in the same period as connected node, sync next votes

In processing any packet data, it is possible to detect a node sending invalid or malicious data. This could be a malicious/corrupted node and besides diconnecting such node there is a mechanism to mark node as malicious and not accept data from it again. This is done in class PeersState with function PeersState::set_peer_malicious. Peer is marked malicious only temporary up to the timeout defined by configuration parameter network_peer_blacklist_timeout.