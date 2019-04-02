./bin/cryptogen generate --config=crypto-config.yaml

./bin/configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block

./bin/configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID myfirstchannel

./bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org1MSPAnchors.tx -channelID myfirstchannel -asOrg Org1MSP

./bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org2MSPAnchors.tx -channelID myfirstchannel -asOrg Org2MSP

This gives the list of network which are running. Use the name of the network 
docker network ls

COMPOSE_PROJECT_NAME=fabric-network-demo docker-compose -f docker-compose-cli.yaml up -d

Check if all containers are up and running using

docker ps

If containers already exists then use below commands 
To see the list of existing containers
docker ps -a -q

To stop all existing containers 
docker stop $(docker ps -a -q)

To Remove all existing containers 
docker rm $(docker ps -a -q)

CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp
CORE_PEER_ADDRESS=peer0.org1.peernode.com:7051
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer0.org1.peernode.com/tls/ca.crt


docker exec -it cli bash

export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp
export CORE_PEER_ADDRESS=peer0.org1.peernode.com:7051
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer0.org1.peernode.com/tls/ca.crt

export CHANNEL_NAME=myfirstchannel

peer channel create -o orderer.orderernode.com:7050 -c myfirstchannel -f ./channel-artifacts/channel.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/orderernode.com/orderers/orderer.orderernode.com/msp/tlscacerts/tlsca.orderernode.com-cert.pem

peer channel join -b myfirstchannel.block

CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp 
CORE_PEER_ADDRESS=peer1.org1.peernode.com:7051 
CORE_PEER_LOCALMSPID="Org1MSP" 
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer1.org1.peernode.com/tls/ca.crt 

peer channel join -b myfirstchannel.block


export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/users/Admin@org2.peernode.com/msp
export CORE_PEER_ADDRESS=peer0.org2.peernode.com:7051
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer0.org2.peernode.com/tls/ca.crt 

peer channel join -b myfirstchannel.block

export CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/users/Admin@org2.peernode.com/msp
export CORE_PEER_ADDRESS=peer1.org2.peernode.com:7051
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer1.org2.peernode.com/tls/ca.crt 

peer channel join -b myfirstchannel.block

CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp 
CORE_PEER_ADDRESS=peer0.org1.peernode.com:7051 
CORE_PEER_LOCALMSPID="Org1MSP" 
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer0.org1.peernode.com/tls/ca.crt 

peer channel update -o orderer.orderernode.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/Org1MSPAnchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/orderernode.com/orderers/orderer.orderernode.com/msp/tlscacerts/tlsca.orderernode.com-cert.pem

CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/users/Admin@org2.peernode.com/msp 
CORE_PEER_ADDRESS=peer0.org2.peernode.com:7051 
CORE_PEER_LOCALMSPID="Org2MSP" 
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer0.org2.peernode.com/tls/ca.crt 

peer channel update -o orderer.orderernode.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/Org2MSPAnchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/orderernode.com/orderers/orderer.orderernode.com/msp/tlscacerts/tlsca.orderernode.com-cert.pem

Install chaincode on Peers

CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp
CORE_PEER_ADDRESS=peer0.org1.peernode.com:7051
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer0.org1.peernode.com/tls/ca.crt

peer chaincode install -n myfirstcc -v 1.0 -p github.com/chaincode/go/

CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/users/Admin@org1.peernode.com/msp
CORE_PEER_ADDRESS=peer1.org1.peernode.com:7051
CORE_PEER_LOCALMSPID="Org1MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer1.org1.peernode.com/tls/ca.crt

peer chaincode install -n myfirstcc -v 1.0 -p github.com/chaincode/go/

CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/users/Admin@org2.peernode.com/msp
CORE_PEER_ADDRESS=peer0.org2.peernode.com:7051
CORE_PEER_LOCALMSPID="Org2MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer0.org2.peernode.com/tls/ca.crt

peer chaincode install -n myfirstcc -v 1.0 -p github.com/chaincode/go/

CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/users/Admin@org2.peernode.com/msp
CORE_PEER_ADDRESS=peer1.org2.peernode.com:7051
CORE_PEER_LOCALMSPID="Org2MSP"
CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer1.org2.peernode.com/tls/ca.crt

peer chaincode install -n myfirstcc -v 1.0 -p github.com/chaincode/go/


Instantiate Chaincode on channel

peer chaincode instantiate -o orderer.orderernode.com:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/orderernode.com/orderers/orderer.orderernode.com/msp/tlscacerts/tlsca.orderernode.com-cert.pem -C $CHANNEL_NAME -n myfirstcc -v 1.0 -c '{"Args":["init","a", "100", "b","200"]}' -P "AND ('Org1MSP.peer','Org2MSP.peer')"


Test
Query
peer chaincode query -C $CHANNEL_NAME -n myfirstcc -c '{"Args":["query","a"]}'

peer chaincode invoke -o orderer.orderernode.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/orderernode.com/orderers/orderer.orderernode.com/msp/tlscacerts/tlsca.orderernode.com-cert.pem -C $CHANNEL_NAME -n myfirstcc --peerAddresses peer0.org1.peernode.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.peernode.com/peers/peer0.org1.peernode.com/tls/ca.crt --peerAddresses peer0.org2.peernode.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.peernode.com/peers/peer0.org2.peernode.com/tls/ca.crt -c '{"Args":["invoke","a","b","10"]}'
