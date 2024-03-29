<h3><p style="font-size:14px" align="center"> Presented to you By :
<a href="https://twitter.com/DanteCraze" target="_blank">Dante Craze</a></p></h3>
<h3><p style="font-size:14px" align="center">Hetzner :
<a href="https://hetzner.cloud/?ref=IHufP2lNzBje" target="_blank">You can Get 20€ Bonus for deploying VPS!</a></h3>
<hr>

<p align="center">
  <img height="100" height="auto" src="https://user-images.githubusercontent.com/34649601/207593974-32d7cb69-eca9-4096-bc96-246fe7038c88.png">
</p>

# Nolus Testnet | Chain ID : nolus-rila

### Official documentation:
>- [Validator setup instructions](https://docs-nolus-protocol.notion.site/Nolus-Protocol-Docs-a0ddfe091cc5456183417a68502705f8)

### Explorer via Node Guru:
>-  https://nolus.explorers.guru/

### Automatic Installer
You can setup your Nolus fullnode in few minutes by using automated script below.
```
wget -O nolus.sh https://raw.githubusercontent.com/dantecrz123/NoderHolic/main/nolus/nolus.sh && chmod +x nolus.sh && ./nolus.sh
```
### Public Endpoint

>- API : https://api.nolus.crazeblock.xyz
>- RPC : https://rpc.nolus.crazeblock.xyz
>- gRPC : https://grpc.nolus.crazeblock.xyz
>- gRPC Web : SOON

### Snapshot from kj(Update every 5 hours)
```
sudo systemctl stop nolusd
cp $HOME/.nolus/data/priv_validator_state.json $HOME/.nolus/data/priv_validator_state.json.backup
rm -rf $HOME/.nolus/data

curl -L https://snapshots.kjnodes.com/nolus-testnet/snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.nolus

mv $HOME/.nolus/data/priv_validator_state.json.backup $HOME/.nolus/data/priv_validator_state.json

sudo systemctl start nolusd && sudo journalctl -fu nolusd -o cat
```

### State Sync
```
nolusd tendermint unsafe-reset-all --home $HOME/.nolus --keep-addr-book

SNAP_RPC="https://rpc.nolus.crazeblock.xyz:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.nolus/config/config.toml

sudo systemctl start nolusd && sudo journalctl -fu nolusd -o cat
```

### Seed Peers
```
sed -i -e "s|^seeds *=.*|seeds = \"3f472746f46493309650e5a033076689996c8881@nolus-testnet.rpc.kjnodes.com:43659\"|" $HOME/.nolus/config/config.toml
```
### Addrbook (Update every hour)
```
curl -Ls https://snapshots.kjnodes.com/nolus-testnet/addrbook.json > $HOME/.nolus/config/addrbook.json
```
### Genesis
```
curl -Ls hhttps://snapshots.kjnodes.com/nolus-testnet/genesis.json > $HOME/.nolus/config/genesis.json
```
