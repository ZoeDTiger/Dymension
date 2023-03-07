## Dymension 非激励公共测试网35-C验证节点部署
### 项目基本情况
    Dymension是模块化区块链网络，DYM为其原生代币。Dymension Hub是模块化区块链的桥接中心，也称为RollApps。35-C做为Dymension的第一个非激励公共测试网以开始测试模块化链论文。
    
    在Discord上担任RollApp Fam角色的社区成员将能够在今年第二季度的激励测试网上部署自己的RollApp，有关RollAppFam角色和Q2激励测试网的更多信息，可通过https://discord.gg/dymension获得。
    
    第一个RollApp将称为RollApp X，RollApp X是它自己的应用程序链，具有专用的序列器全节点，该节点利用Celestia的Mocha测试网进行数据发布，并将Dymension Hub作为事实来源。用户将能够将IBC代币从Dymension Hub转移到RollApp X。这是第一个支持IBC的模块化区块链。在RollApp X上，用户将能够质押RollApp X的原生资产并获得铸币奖励。请注意，这些奖励不计入激励测试网参与。
    
    其它两个RollApps：
    EVM RollApp：用户将能够通过熟悉的Solidity智能合约体验超低潜在区块链
    CosmWasm RollApp：用户将能够部署强大的CosmWasm智能合约

### 部署节点的基本条件
#### 支持的操作系统
    darwin/arm64
    darwin/x86_64
    linux/arm64
    linux/x86_64
#### 硬件条件
    CPU：双核
    磁盘：500GB及以上SSD
    内存：16GB及以上
    网络：100mbps及以上
#### 软件条件
    安装 Go 是运行 Dymension Hub 全节点的先决条件，安装步骤如下：
    cd $HOME
    wget "https://go.dev/dl/go1.19.4.linux-amd64.tar.gz"
    sudo rm -rf /usr/local/go
    sudo tar -C /usr/local -zxvf go1.19.4.linux-amd64.tar.gz
    
    echo "export GOROOT=/usr/local/go" |  sudo tee -a /etc/profile
    echo "export GOPATH=$HOME/go" |  sudo tee -a /etc/profile
    echo "export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin" |  sudo tee -a /etc/profile
    echo "export GO111MODULE=on" |  sudo tee -a /etc/profile
    echo "export GOPROXY=https://goproxy.cn" |  sudo tee -a /etc/profile
    
    source /etc/profile
    
### 完整节点部署

#### 获得Dymension Hub
    git clone https://github.com/dymensionxyz/dymension.git --branch v0.2.0-beta
    cd dymension
    
#### 构建，在GOPATH环境变量中生成可执行文件dymd
    make install
    sudo cp $HOME/go/bin/dymd /usr/local/bin
    
#### 验证 Dymension Hub 完整节点是否已正确安装
    dymd version --long
<img width="519" alt="微信图片_20230307154446" src="https://user-images.githubusercontent.com/100336530/223357077-71a3e0a9-ba36-40bf-9d86-d1ecd435e050.png">

#### 节点配置

##### 初始化节点
    dymd init <MONIKER> --chain-id 35-C

##### 获得创世文件
    wget -O genesis.json https://snapshots.polkachu.com/testnet-genesis/dymension/genesis.json --inet4-only
    mv $HOME/genesis.json $HOME/.dymension/config/

##### 修改gas
    vim ~/.dymension/config/app.toml
    设置为：minimum-gas-prices = "0.15udym"

##### 建立external_address
    sed -i -e 's/external_address = \"\"/external_address = \"'$(curl httpbin.org/ip | jq -r .origin)':26656\"/g' ~/.dymension/config/config.toml

##### 建立seed_mode
    在种子模式下，您的节点会不断爬网网络并查找对等方，要在种子模式下运行完整节点，请设置为：seed_mode = true
    vim ~/.dymension/config/config.toml

##### 加种子节点seeds
###### Dymension的35-C测试网的Dymension核心团队种子节点：c6cdcc7f8e1a33f864956a8201c304741411f219@3.214.163.125:26656
    
    sed -i 's/seeds = ""/seeds = "f97a75fb69d3a5fe893dca7c8d238ccc0bd66a8f@dymension-testnet.seed.brocha.in:30584,ebc272824924ea1a27ea3183dd0b9ba713494f83@dymension-testnet-seed.autostake.net:27086,b78dd0e25e28ec0b43412205f7c6780be8775b43@dym.seed.takeshi.team:10356,babc3f3f7804933265ec9c40ad94f4da8e9e0017@testnet-seed.rhinostake.com:20556,c6cdcc7f8e1a33f864956a8201c304741411f219@3.214.163.125:26656"/' ~/.dymension/config/config.toml

##### 加持久对等节点persistent_peers
###### 您指定的节点是受信任的持久对等节点，可以帮助在 p2p 网络中锚定您的节点。
    
    sed -i 's/persistent_peers = ""/persistent_peers = "ebc272824924ea1a27ea3183dd0b9ba713494f83@dymension-testnet-peer.autostake.net:27086,9111fd409e5521470b9b33a46009f5e53c646a0d@178.62.81.245:45656,f8a0d7c7db90c53a989e2341746b88433f47f980@209.182.238.30:30657,1bffcd1690806b5796415ff72f02157ce048bcdd@144.76.67.53:2580,c17a4bcba59a0cbb10b91cd2cee0940c610d26ee@95.217.144.107:20556,e6ea3444ac85302c336000ac036f4d86b97b3d3e@38.146.3.199:20556,b473a649e58b49bc62b557e94d35a2c8c0ee9375@95.214.53.46:36656,db0264c412618949ce3a63cb07328d027e433372@146.19.24.101:26646,281190aa44ca82fb47afe60ba1a8902bae469b2a@dymension.peers.stavr.tech:17806,290ec1fc5cc5667d4e072cf336758d744d291ef1@65.109.104.118:60556,d8b1bcfc123e63b24d0ebf86ab674a0fc5cb3b06@51.159.97.212:26656,55f233c7c4bea21a47d266921ca5fce657f3adf7@168.119.240.200:26656,139340424dddf85e54e0a54179d06875013e1e39@65.109.87.88:24656"/' ~/.dymension/config/config.toml

##### 修改链版本
    sed -i 's/chain-id = ""/chain-id = "35-C"/' ~/.dymension/config/client.toml

#### 设置本地节点
##### 生成帐户
    dymd keys add <KEY_NAME_HERE>
<img width="598" alt="微信图片_20230307160428" src="https://user-images.githubusercontent.com/100336530/223361230-92b810e7-7a4d-4aea-affc-d0d1cfec8a52.png">

##### 添加账户并设置初始余额
    dymd add-genesis-account <ADDRESS_HERE> 600000000000udym
<img width="615" alt="微信图片_20230307160515" src="https://user-images.githubusercontent.com/100336530/223361400-d1009b94-2ddc-4924-9237-c81686912fa5.png">

##### 通过创世文件中包含的称为 gentx 的特殊事务声明您的验证者和自我委托
    dymd gentx <KEY_NAME> 500000000000udym --chain-id 35-C
<img width="884" alt="微信图片_20230307160701" src="https://user-images.githubusercontent.com/100336530/223361745-e35a468d-4a5c-483f-9d68-5b67072fd0df.png">

##### 将 gentx 添加到创世文件中
    dymd collect-gentxs

##### 运行完整节点
    screen -S <YOUR_NAME>
    dymd start

##### 查看状态
###### 可以通过latest_block_height与catching_up来确认是否完成同步
    dymd status
<img width="886" alt="微信图片_20230307160941" src="https://user-images.githubusercontent.com/100336530/223362341-22eb9b50-b74d-4a24-aa2e-285a6ad4d558.png">

#### 创建验证器
###### 完整节点必须同步到当前最新高度后才可操作
    dymd tx staking create-validator \
    --amount=500000000000udym \
    --pubkey=$(dymd tendermint show-validator) \
    --moniker="<your-moniker>" \
    --chain-id=35-C \
    --from=<key-name> \
    --commission-rate="0.10" \
    --commission-max-rate="0.20" \
    --commission-max-change-rate="0.01" \
    --min-self-delegation="1"

##### 确认验证器处于活动状态
###### 如果运行以下命令返回某些内容，则您的验证器处于活动状态
    dymd query tendermint-validator-set | grep "$(dymd tendermint show-address)"







