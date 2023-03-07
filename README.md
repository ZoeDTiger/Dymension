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





























