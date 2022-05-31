# Final Chain

Main purpose of Final Chain is to connect PBFT with EVM by PBFT blocks execution and getting results of execution execution. Also State related data is stored there in separate DB, so with methods from this class we can get it. 
It also implements few events that are used for websocket notifications and some internal logic. Uses `StateApi` class which is calling EVM related methods written in Golang. 

Main method is `FinalChain::finalize` which is called from `PbftManager::finalize_` and it executes and stores block related data. 

Link to Doxygen generated docs with methods list. [FinalChain class](https://taraxa-project.github.io/taraxa-node/group___final_chain.html#classtaraxa_1_1final__chain_1_1_final_chain) 