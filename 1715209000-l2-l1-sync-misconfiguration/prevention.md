# Preventative Measures

In the process of troubleshooting the proof generation for cross chain withdrawals, and ultimately experiencing how easy it is to make a mistake during the configuration process, we've documented a new process that should eliminate future mistakes or reduce the risk significantly.

## Deployment Framework

### OFT Deployment

We use the [LayerZero Omnichain Mesh Network](https://docs.layerzero.network/v2/home/protocol/mesh-network) to enable our protocol to function across multiple chains, one of the main components required for that is the [OFT Standard](https://docs.layerzero.network/v2/developers/evm/oft/quickstart) that we deploy with our new and improved process detailed below.

- script the entire process in solidity to be executed via foundry
- [step 1. configure the OFT programmatically](https://github.com/etherfi-protocol/Etherfi-SyncPools/blob/etherfi-upgradeable-deploy-OP-OFT/script/01_OFTConfigure.s.sol)
- [step 2. update OFT peers](https://github.com/etherfi-protocol/Etherfi-SyncPools/blob/etherfi-upgradeable-deploy-OP-OFT/script/02_UpdateOFTPeersTransactions.sol)
- [step 3. validate configuration](https://github.com/etherfi-protocol/weETH-cross-chain/blob/master/test/OFTDeployment.t.sol)
- [step 4. transfer ownership to the correct gnosis](https://github.com/etherfi-protocol/Etherfi-SyncPools/blob/etherfi-upgradeable-deploy-OP-OFT/script/03_OFTOwnershipTransfer.s.sol)
- [step 5. test the OFT send](https://github.com/etherfi-protocol/Etherfi-SyncPools/blob/etherfi-upgradeable-deploy-OP-OFT/script/04_OFTSend.sol)
- [step 6. set all the correct rates limits](https://github.com/etherfi-protocol/Etherfi-SyncPools/blob/etherfi-upgradeable-deploy-OP-OFT/script/05_ProdRateLimit.sol)

### Sync Pools & surrounding contracts

To enable a better user experience via native minting we also need to deploy more custom contracts around the OFT. For that we'll follow the steps

- 1. script contract setup
- 2. do config check and signoff by 3 internal members
- 3. config signoff by 3 external teams (LZ,Paladin,XYZ network we integrate with)
- 4. keep rate limit in place for 1 week, sync 1 ETH back to Ethereum Mainnet
- 5. on receiving ETH within our protocol, up rate limit, and proceed as normal

## Monitoring

### L2 Monitoring bot

We now have a bot that runs on a 12h schedule that reports:
- whether the cross chain config is correctly configured
- how much ETH is in transit from the L2 to ETH mainnet
- what is the expected arrival date
- whether operations are automatically behaving as expected
