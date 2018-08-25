# faucet-setup



## 安装依赖库

```shell
sudo apt-get install mysql-server libmysqlclient-dev  libreadline-dev
```


## 安装ruby环境

```shell
cd ~
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash

sudo rbenv install 2.2.3
sudo rbenv global 2.2.3
sudo gem install bundler
```



## 下载水龙头代码



```shell
git clone https://github.com/bitshares/faucet
cd faucet
sudo bundle   # ignore warnings
```

## 配置水龙头文件

### 水龙头访问配置faucet.yml

```shell
cp config/faucet-example.yml config/faucet.yml
vim config/faucet.yml
```


```yml
cli_wallet_connection: ws://47.91.22.22:38092  //钱包开放的websocketurl，cli_wallet -r参数对应
registrar_account: nathan                      //提供注册的推荐人用户名，本例子为已创建且升级为会员的用户名nathan
referrer_percent: 50
refcode_prefix: F01

default_url: 127.0.0.1                     //水龙头对外访问的IP   
default_port: 3000                         //水龙头对外访问的端口

exception_notification:
  sender_address: faucet@example.com
  exception_recipients: admin@example.com
smtp:
  address: address
  user_name: user
  password: password
```





