Generate Cryptographic Material
Below command will create crypto-config folder with all cryptographic material generated

./bin/cryptogen generate --config=crypto-config.yaml

Generate required Tx files
Generate Genesis block

./bin/configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block

Creates channel.tx file

./bin/configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID myfirstchannel

Define Anchor Peers
•	For Org1 on channel
./bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org1MSPAnchors.tx -channelID myfirstchannel -asOrg Org1MSP

•	For Org2 on channel
./bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org2MSPAnchors.tx -channelID myfirstchannel -asOrg Org2MSP

NOTE: This is for debugging
To Check the list of active network
docker network ls

Start the network
COMPOSE_PROJECT_NAME=fabric-network-demo docker-compose -f docker-compose-cli.yaml up -d

Check list of containers running
docker ps

To see the list of existing containers 
docker ps -a -q

To stop all existing containers 
docker stop $(docker ps -a -q)

To Remove all existing containers 
docker rm $(docker ps -a -q)


Enter CLI container
docker exec -it cli bash

Create Channel
peer channel create -o orderer.orderernode.com:7050 -c myfirstchannel -f ./channel-artifacts/channel.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/orderernode.com/orderers/orderer.orderernode.com/msp/tlscacerts/tlsca.orderernode.com-cert.pem

Join Peers
•	For Peer0 and Org1
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp export CORE_PEER_ADDRESS=peer0.org1.peernode.com:7051 export CORE_PEER_LOCALMSPID="Org1MSP" export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer0.org1.peernode.com/tls/ca.crt
export CHANNEL_NAME=myfirstchannel

peer channel join -b myfirstchannel.block

•	For Peer1 and Org1
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp CORE_PEER_ADDRESS=peer1.org1.peernode.com:7051 CORE_PEER_LOCALMSPID="Org1MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer1.org1.peernode.com/tls/ca.crt

peer channel join -b myfirstchannel.block

•	For Peer0 and Org2
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/users/Admin@org2.peernode.com/msp export CORE_PEER_ADDRESS=peer0.org2.peernode.com:7051 export CORE_PEER_LOCALMSPID="Org2MSP" export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer0.org2.peernode.com/tls/ca.crt

peer channel join -b myfirstchannel.block

•	For Pee1 and Org2
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/users/Admin@org2.peernode.com/msp export CORE_PEER_ADDRESS=peer1.org2.peernode.com:7051 export CORE_PEER_LOCALMSPID="Org2MSP" export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer1.org2.peernode.com/tls/ca.crt

peer channel join -b myfirstchannel.block

Define anchor peer for channels
•	For Org1
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp CORE_PEER_ADDRESS=peer0.org1.peernode.com:7051 CORE_PEER_LOCALMSPID="Org1MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer0.org1.peernode.com/tls/ca.crt

peer channel update -o orderer.orderernode.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/Org1MSPAnchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/orderernode.com/orderers/orderer.orderernode.com/msp/tlscacerts/tlsca.orderernode.com-cert.pem



•	For Org2
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/users/Admin@org2.peernode.com/msp CORE_PEER_ADDRESS=peer0.org2.peernode.com:7051 CORE_PEER_LOCALMSPID="Org2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer0.org2.peernode.com/tls/ca.crt

peer channel update -o orderer.orderernode.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/Org2MSPAnchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/orderernode.com/orderers/orderer.orderernode.com/msp/tlscacerts/tlsca.orderernode.com-cert.pem

Install Chaincode on Peers
•	For Pee0 and Org1
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp CORE_PEER_ADDRESS=peer0.org1.peernode.com:7051 CORE_PEER_LOCALMSPID="Org1MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer0.org1.peernode.com/tls/ca.crt

peer chaincode install -n myfirstcc -v 1.0 -p github.com/chaincode/go/

•	For Pee1 and Org1
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp CORE_PEER_ADDRESS=peer1.org1.peernode.com:7051 CORE_PEER_LOCALMSPID="Org1MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer1.org1.peernode.com/tls/ca.crt

peer chaincode install -n myfirstcc -v 1.0 -p github.com/chaincode/go/

•	For Pee0 and Org2
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/users/Admin@org2.peernode.com/msp CORE_PEER_ADDRESS=peer0.org2.peernode.com:7051 CORE_PEER_LOCALMSPID="Org2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer0.org2.peernode.com/tls/ca.crt

peer chaincode install -n myfirstcc -v 1.0 -p github.com/chaincode/go/

•	For Pee1 and Org2
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/users/Admin@org2.peernode.com/msp CORE_PEER_ADDRESS=peer1.org2.peernode.com:7051 CORE_PEER_LOCALMSPID="Org2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer1.org2.peernode.com/tls/ca.crt

peer chaincode install -n myfirstcc -v 1.0 -p github.com/chaincode/go/


Instantiate Chaincode on channel
peer chaincode instantiate -o orderer.orderernode.com:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/orderernode.com/orderers/orderer.orderernode.com/msp/tlscacerts/tlsca.orderernode.com-cert.pem -C $CHANNEL_NAME -n myfirstcc -v 1.0 -c '{"Args":["init","a", "100", "b","200"]}' -P "AND ('Org1MSP.peer','Org2MSP.peer')"

Test Query 
peer chaincode query -C $CHANNEL_NAME -n myfirstcc -c '{"Args":["query","a"]}'
peer chaincode invoke -o orderer.orderernode.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/orderernode.com/orderers/orderer.orderernode.com/msp/tlscacerts/tlsca.orderernode.com-cert.pem -C $CHANNEL_NAME -n myfirstcc --peerAddresses peer0.org1.peernode.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer0.org1.peernode.com/tls/ca.crt --peerAddresses peer0.org2.peernode.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer0.org2.peernode.com/tls/ca.crt -c '{"Args":["invoke","a","b","10"]}'

