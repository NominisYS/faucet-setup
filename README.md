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

### 密码种子文件secrets.yml

***不要在实际部署环境中使用以下密码！ 使用rake secret的密码！***

***不要在实际部署环境中使用以下密码！ 使用rake secret的密码！***

***不要在实际部署环境中使用以下密码！ 使用rake secret的密码！***

```shell
cp config/secrets-example.yml config/secrets.yml
rake secret
vim config/secrets.yml
```

```yml
# 内容如下 （按需要改各个清空的密码种子）
development:                                          
  secret_key_base: abcdefg123456                      
                                                      
test:                                                 
  secret_key_base: abcdefg123456                      
                                                      
# 不要在实际部署环境中使用以下密码！ 使用rake secret的密码！         
production:                                           
  secret_key_base: abcdefg123456
```



### 文件database.yml

```shell
vim config/database.yml
```

```yml
default: &default                    
  adapter: mysql2                    
  encoding: utf8                     
  pool: 5                            
  username: root           #数据库登录用户名                   
  password:                #数据库登录密码，根据安装mysql时的填写
  host: localhost          #数据库url          
                                     
development:                         
  <<: *default                       
  database: bitshares_faucet_dev     
                                     
test:                                
  <<: *default                       
  database: bitshares_faucet_test    
                                     
production:                          
  <<: *default                       
  database: bitshares_faucet
```

### 创建并初始化数据库

```shell
rake db:create; rake db:migrate; rake db:seed
RAILS_ENV=production bundle exec rake db:create db:schema:load
```



### 运行水龙头服务 

```shell
rails s -b 0.0.0.0
```


```shell
# rails s -b 0.0.0.0=> Booting WEBrick
=> Rails 4.2.4 application starting in development on http://0.0.0.0:3000
=> Run `rails server -h` for more startup options
=> Ctrl-C to shutdown server
Starting graphene websocket communication event-loop 'ws://47.91.22.22:38092'
Established connection to 'ws://47.91.22.22:38092'
[2018-08-25 23:46:11] INFO  WEBrick 1.3.1
[2018-08-25 23:46:11] INFO  ruby 2.2.3 (2015-08-18) [x86_64-linux]
[2018-08-25 23:46:11] INFO  WEBrick::HTTPServer#start: pid=11621 port=3000
...
```


##  联合调试


+ 启动witness_node
+ 启动cli_wallet
  - `cli_wallet --wallet-file my-wallet.json --server-rpc-endpoint ws://127.0.0.1:38090 -r  0.0.0.0:38092  -H 0.0.0.0:38099   --chain-id 540a861abb52b7612a66438e1a25285f4543078a2b487461d70f6fc7d7ddcddf`
+ 启动网页钱包服务

### `/bitshares-ui/app/api/apiConfig.js`


```javascript
...

export const settingsAPIs = {    
  DEFAULT_WS_NODE: "ws://47.91.22.22:38090",
  WS_NODE_LIST: [        
    {url: "ws://127.0.0.1:38090", location: "Locally hosted"},        
    {url: "ws://47.91.22.22:38090", location: "Dang"}           
  ],    
  DEFAULT_FAUCET: "http://47.91.22.22:3000", 
  RPC_URL: "http://47.91.22.22:38099"  
};

```

### `node_modules/bitsharesjs-ws/cjs/src/ChainConfig.js`

用当前repo里的对应文件替换原先的`ChainConfig.js`

+ 启动网页钱包`npm start`





