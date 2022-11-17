
# Stride Mainnet stride-1 Validator Node Setup

[![Stride Mainnet stride-1 Validator Node Setup](https://www.synergynodes.com/youtube/Synergy-Nodes-Stride-Validator-Node-Youtube.jpg)](https://youtu.be/JbfkHVWr5BQ)

## Install Ubuntu 20.04 on a new server and login as root

## Install ``ufw`` firewall and configure the firewall

```
apt-get update
apt-get install ufw
ufw default deny incoming
ufw default allow outgoing
ufw allow 22
ufw allow 26656
ufw enable
```

## Create a new User

```
# add user
adduser node

# add user to sudoers
usermod -aG sudo node

# login as user
su - node
```

## Install Prerequisites

```
sudo apt update
sudo apt install pkg-config build-essential libssl-dev curl jq git libleveldb-dev -y
sudo apt-get install manpages-dev -y

# install go
curl https://dl.google.com/go/go1.18.5.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf -

```

```
# Update environment variables to include go
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF

```

```
source $HOME/.profile

# check go version
go version

```

## Install Stride Node

```
git clone https://github.com/Stride-Labs/stride
cd stride
git checkout v2.0.3
make install
cd
strided version --long
```

You should get the following output:

```
name: strided
server_name: stride
version: v2.0.3
commit: 650c9903b5adb7a4c4420f6bfcd3153c39308c77
```

## Initialise your Validator node
```
#Choose a name for your validator and use it in place of “<moniker-name>” in the following command:
strided init <moniker-name> --chain-id stride-1

#Example
strided init Synergy_Nodes --chain-id stride-1
```

## Download genesis.json file.
```
cd ~/.stride/config
rm genesis.json

wget https://raw.githubusercontent.com/Stride-Labs/stride/main/genesis/genesis.json

cd


```

## Update configuration with seeds, peers list

```
cd

seeds="ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@seeds.polkachu.com:12256"

peers="c427e6b2b9ae3c58f3578c5bad398a469ed3c903@65.108.142.47:36656,b1c9d2f2627bc2d2a0a857e408cdcc44f56c5d5b@65.108.206.56:16656,5b193f60f2b8378c42d7d30bd70d45de2b70730e@65.108.202.143:16656,cbbc8c1c9da23b71ccd2138fbf41bb710afe44b1@65.21.170.3:28656,0e67ce079f4e26ad5d22d7b1ba61e7df214d626c@65.108.101.50:60556,9ee75491e354965d8bfd8434aa093f8613bc1dce@65.108.238.103:12256,74b693b1b0745d250becfbdb550d36504e03bf92@93.115.25.15:26656,67e90a4956250adf62a2e257f569d80907d11d86@141.95.103.115:26656,f02b8862ab5a9add71148340cc28d765fba8069a@138.201.220.51:26666,ea30fb0d90563efd98131d7778e7a53d0e9ad633@116.202.236.115:21016,84ff28824a911409e2c24f2f5ede87ae1b859b5f@5.9.147.22:26156,d056dcd5ac8dddb23e2962a5ade6ee51f9bfd785@162.19.89.8:10456,97e90b4d658dc7b10c05e387b1384f6670f64943@83.136.249.85:26656,dc9241e56b67b2d9b39a79f4aa9dc432d78c1dbc@195.3.223.204:10156,6a1087004245692128a6ad11b812bb3640955b86@162.55.235.69:25656,8cddcfa2ebcd25df7116f18bdbb0b22ae41392a6@65.108.193.133:16656,a757fc9ea95a7f643d392ec9fdaa31cbf06e76d9@195.3.221.21:12256,04ea9eceee16db90872fee3fbef9ac50a87702c5@185.248.24.29:26656,ec4f27b82691f44d38b4801889b3b92643bdc5c2@185.144.99.234:26656,dfc62810eeaab86587b2975c79f3c12d4830652d@15.235.114.54:26656,b549e0f88cbebe6cfd3f772937a70640b950fd98@66.172.36.133:28656,f400b2b2665c62d3ba64a940a4c08a7db874ff6a@34.94.25.244:26656,cfd27429d382ecf366ddad02c88f15a8753092c8@66.172.36.135:28656,cb0b38aa612e8ac05f704d9b2feb7526607afb77@159.203.191.62:26656,e0b4c670b35cc0cf16b570ea3397a5f785c9d1b6@65.109.48.57:26656,a83cd29f4f9a4711346184966f9fb6c80bb658d2@65.108.103.184:21656,9063fce4a1fc50185b2cafd56bc8176a45072c09@57.128.133.23:26656,e1e23e16a1c75c558fcff844699f976eed6dec5f@194.163.132.150:16656,9731222819ddacf2b0238e51527aa95156a04b06@57.128.133.22:26656,64920ef07c20c22f26a2164ceae2aac60ec2840d@95.216.73.250:26656,a63421772de24812d62a842ffb40cd93946ab1df@194.163.136.90:26656,061dcf3318978ac0448e848507c0b51bfa706b6d@35.193.224.120:26656,d3cee85a10d72b6f4eb40f544323acd104b29c23@51.68.44.219:23856,f93ce5616f45d6c20d061302519a5c2420e3475d@135.125.5.31:54356,64be41ff925b32a81cfb13a81fd4847aef2524aa@34.72.49.125:26656,f5732d5a406bdbbf08acad017c0993c0aa8ebe70@35.247.108.210:26656,ad6700400ff6a76b442e96e772e1f1d641bd3560@34.172.46.68:26656,27e3200f2b3f83c403ad9dfa09bf83ae73b179b3@149.102.143.220:10173,99237ee23683d1bf4cb426a7042793464d8f5401@128.199.15.159:31437,7ef5ff00fe94933b8ba4b7ae4a8632ece5db11df@104.198.4.170:26656,1f78bdc1c2e2268185dac25fa076f743d8bbd154@95.217.109.143:56656,71082b73b93979f772b6e53bd700ca13cb69b847@162.251.235.253:26656,91e2689222d249c2e5edc5e84612acb37de955d7@65.144.145.234:26656,4b177aeb56846ef7b56b0078c09e6b326276880e@34.135.162.111:26656,9c69539f7d6db4f5637c64a1552677fcd0c83c7d@34.170.162.50:26656,96f8656af3a669bd0981823bf2cd38be801977b2@65.108.9.25:16656,20f56a68a04eedc764b7e1b87b7032a50b9d4fe9@51.81.155.97:10456,1d6ea027efc5025146c84f52294cd0b44ab25cdc@167.235.138.94:26656,d88e3dd9d94dcb32d908a5d5fa19682dffcd52a5@146.59.47.210:26656,189d892ad5d7991dd38a1dfcf830726fbb699cf9@88.99.94.122:26666,1ec2a654e00e22279ee50f13f074f2bce7218681@15.235.114.194:10156,edb5e5a80654041e2a6e83852b75f8325e88bfd2@34.71.129.146:26656,402a88e10c202d93e4723aa2c1f78a443d942a0d@65.21.95.180:36656,607a6fd540bd1983cb83e4a8b5d4adc854366875@212.23.222.82:26656,59a13b0e8ce91c6d507b06c09b0ed44a1574cad3@54.177.215.240:26656,609251c15cd2330f13436a23cb185d7f50aac707@34.171.123.54:26656,2dc710a07b9884ddc0ef4c1105bef3bfa45f87ec@116.203.60.232:16656,7e0a230dfbecb877f05fe9f1dde6c91d3b633c43@95.216.142.94:26656,1387946c04bceb472113f657f55f670f71709230@65.108.4.188:12256,0b551e21f06a55db7644386167f29641a29f1ed7@185.252.232.110:36656,6436bc36411be81644188673937f1fbee398448c@65.108.141.226:26656,05c410efdebfc0638c868dc0d6f9154168d57604@146.19.24.101:26646,2254e6968e5c7ebc98ef5b79b388502fa44e10e1@5.161.134.44:26656,ea6a7b2f366bc343f0670f1673fd86001dd08eb0@65.108.122.246:26636,fbebe11a12def69c115c25b4bf871bc5976dfe50@65.109.59.118:26656,ede57d0b0373d95666626f259aa4f030d6660a57@65.109.49.163:36656,b5f9fa874781f975687018ae559f0d952d3a2e24@52.52.208.179:26656,076e97f47762a477f2ae3dd3e798a7970b6bb20d@52.52.110.228:26656,741fb30c627ad944b2553512171fbbb4138110c5@65.108.123.244:25656,ef62c7e1bb793ef03481f71697be5ff28e191405@65.108.43.116:56136,abbbbc191bfcddb0462fb9ceeee90185af8b8cd7@141.95.171.41:46656,689bbb368fc3a7222a60091700af15d5e9af3388@77.52.182.194:26656,722884e3add85791c34a0563253dc47901320878@65.108.238.61:36656,c484f998e1a9e464a68af04d8d15d6fb0aeceb1e@65.21.129.95:26656,bf9168fbcc7250c7c5b9d8080cd4eeee6e399913@139.162.242.71:26886,ebc272824924ea1a27ea3183dd0b9ba713494f83@185.16.39.158:26886,5676fee42425893f90d0724994661d172230587a@188.165.252.51:26656,f0aa82d5f0b62b8f251a6e474340cd78809d2944@65.109.55.186:16656,d248bb8274ee50e38b4c558e1190b6d35a6edc87@136.243.111.31:26656,b42011f01bd3987a0eb38092cbcfb44a8e4dc7f1@185.248.24.16:16656,8d7d0f32d53467c4d5e8871faf4ec58ea970fed2@157.90.179.182:26456,03ea9949005f23540a66cc54b0b4ac4e7832e8a6@65.109.53.244:16656,55e62732b4a122567441c05b73c147e8a2afee35@216.202.234.76:26656,4d44c8d6cd2dc2fae0b607c380b040b3651e81d6@95.214.53.27:12256,e821acdaf0c7a3c60ea3cd4eb4a98a62dad06f58@43.201.12.41:26656,2f02a4012f90f5d1a9a85748dd9aa14155ed4a71@66.172.36.134:28656,df7ea6950a4a58199ba03ba9454ded0d51f38003@93.147.113.43:26656,c772d6c7ef54aa91ef604e2222270c5622cae2c4@49.12.203.138:16656,5c86d4e37dd7bf8f5d4de300f64ce2ef7c9fcfab@135.181.176.109:14656,23180f90318d0003a4e8140a1e67407bf874d69d@78.107.234.44:25656,0108a0ea6e5a23019bd351901ffc1b0de76cc885@65.109.28.224:14656,63722a9aed0225d7a5f6a49d1c53b5c979137b13@74.96.207.62:26656,8df658cd50f0488d2967340dff07732a30ac9301@178.211.139.224:26776,98004cf40a82aa7015d5f4fba75564201e62de26@141.95.124.151:21016"


sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|; s|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.stride/config/config.toml
```

## Download snapshot and decompress the files.

Download the snapshot and follow the steps present in the following link:

https://www.synergynodes.com/download-info.html?network=stride


## Start the node and let it Sync
```
strided start

# Press Ctrl+c to exit

```

## Running the validator as a systemd unit
```
cd /etc/systemd/system
sudo nano strided.service
```
Copy the following content into ``strided.service`` and save it.

```
[Unit]
Description=Strided Daemon
#After=network.target
StartLimitInterval=350
StartLimitBurst=10

[Service]
Type=simple
User=node
ExecStart=/home/node/go/bin/strided start
Restart=on-abort
RestartSec=30

[Install]
WantedBy=multi-user.target

[Service]
LimitNOFILE=1048576
```

```
sudo systemctl daemon-reload
sudo systemctl enable strided

# Start the service
sudo systemctl start strided

# Stop the service
sudo systemctl stop strided

# Restart the service
sudo systemctl restart strided


# For Entire log
journalctl -t strided

# For Entire log reversed
journalctl -t strided -r

# Latest and continuous
journalctl -fu strided -o cat
```

## Execute the folloiwng command to get the node id

```
strided tendermint show-node-id
```

## Create a Wallet for your Validator Node

Make sure to copy the 24 words Mnemonics Phrase, save it in a file and store it on a safe location.

```
strided keys add <wallet-name>

#Example
strided keys add my_wallet

```

## Purchase some STRD tokens from Osmosis and transfer it to the above created wallet present at Stride network.

https://app.osmosis.zone/?from=USDC&to=STRD


## Create and Register Your Validator Node
```
strided tx staking create-validator -y \
  --chain-id stride-1 \
  --moniker <moniker-name> \
  --pubkey "$(strided tendermint show-validator)" \
  --amount 1000000ustrd \
  --identity "<Keybase.io ID>" \
  --website "<website-address>" \
  --details "Some description" \
  --from <wallet-name> \
  --commission-rate=0.05 \
  --commission-max-rate=0.15 \
  --commission-max-change-rate=0.01 \
  --min-self-delegation 1
  
#Example

strided tx staking create-validator -y \
  --chain-id stride-1 \
  --moniker "Synergy Nodes" \
  --pubkey "$(strided tendermint show-validator)" \
  --amount 1000000ustrd \
  --identity "D74433D32938F013" \
  --website "http://www.mywebsite.com" \
  --details "Some description" \
  --from my_wallet \
  --commission-rate=0.0 \
  --commission-max-rate=0.05 \
  --commission-max-change-rate=0.01 \
  --min-self-delegation 1
  
```

## Get Validator Operator Address (Valoper Address)

Make sure to change ``<wallet-name>`` to correct values.

```
strided keys show <wallet-name> --bech val --output json | jq -r .address

#Example
strided keys show my_wallet --bech val --output json | jq -r .address
```

## Delegate ``STRD`` to Your Node
```
strided tx staking delegate <validator-address> 1000000ustrd --from <wallet-name> --chain-id stride-1 -y

#Example
strided tx staking delegate stridevaloper1lcgzkqstk4jjtphfdfjpw9dd9yfczyzmw8n8mf 1000000ustrd --from my_wallet --chain-id stride-1 -y
```
## Backup Validator node file

Take a backup of the following files after you have created and registered your validator node successfully.

```
/home/node/.stride/config/node_key.json
/home/node/.stride/config/priv_validator_key.json
/home/node/.stride/data/priv_validator_state.json
```

## Withdraw Rewards

Make sure to change ``<validator-operator-address>``, ``<wallet-name>`` to correct values.

```
strided tx distribution withdraw-rewards <validator-address> --from <wallet-name> --chain-id stride-1 -y

#Example
strided tx distribution withdraw-rewards stridevaloper1lcgzkqstk4jjtphfdfjpw9dd9yfczyzmw8n8mf --from synergy --chain-id stride-1 -y
```

## Check Balance of an Address

```
strided query bank balances <wallet-address>

#Example:
strided query bank balances stride1xzern65ppact6sgh4htllnr8pg9dpnc8quexl4
```
