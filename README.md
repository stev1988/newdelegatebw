# newdelegatebw
只授权非转账抵押功能

1. 修改合约代码,把etbexchange1改成要授权的账户		
require_auth( N(etbexchange1) );

2. 编译合约		
eosiocpp -o /home/root1/work/eos/contracts/newdelegatebw/newdelegatebw.wast /home/root1/work/eos/contracts/newdelegatebw/newdelegatebw.cpp

eosiocpp -g /home/root1/work/eos/contracts/newdelegatebw/newdelegatebw.abi /home/root1/work/eos/contracts/newdelegatebw/newdelegatebw.cpp

3. 部署合约,YOUR_ACCOUNT是合约账户		
cleos --wallet-url http://127.0.0.1:6667 --url http://47.52.170.62:8001 set contract YOUR_ACCOUNT /home/root1/work/eos/contracts/newdelegatebw -p YOUR_ACCOUNT

4. 授权给合约,YOUR_PUBLICKEY是合约账户的公钥		
cleos --wallet-url http://127.0.0.1:6667 --url http://47.52.170.62:8001  set account permission YOUR_ACCOUNT active '{"threshold": 1,"keys": [{"key": "YOUR_PUBLICKEY","weight": 1}],"accounts": [{"permission":{"actor":"YOUR_ACCOUNT","permission":"eosio.code"},"weight":1}]}' owner -p YOUR_ACCOUNT

5. 测试,RECEIVE_ACCOUNT是接收抵押的用户,etbexchange1是合约中指定的执行者,		
cleos --wallet-url http://127.0.0.1:6667 --url http://47.52.170.62:8001 push action YOUR_ACCOUNT delegatebw '["RECEIVE_ACCOUNT", "0.0000 EOS","1.0000 EOS"]' -p etbexchange1
