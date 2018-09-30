# Testnet INSTALLATION

## Install EOSIO

* use screen so that installation will keep going on even if we lose connection to server

* build EOSIO
  We have built testnet for this workshop. However if you are keen to try on your own, here are the steps.
    1. `git clone https://github.com/EOSIO/eos.git`
    2. cd to the eos directory
    3. `git submodule update --init --recursive`
    4. run `./eosio_build.sh`
    5. navigate to eos/build
    6. `sudo make install`

## Start Testnet

* cd to eos directory
* run this to start testnet
```
cd build/programs/nodeos
./nodeos -e -p eosio --plugin eosio::chain_api_plugin --plugin eosio::history_api_plugin
```
Expect to see a log messages like so,
```
2018-08-20T16:53:30.651 thread-0   chain_plugin.cpp:271          plugin_initialize    ] initializing chain plugin
2018-08-20T16:53:30.654 thread-0   block_log.cpp:124             open                 ] Log is nonempty
2018-08-20T16:53:30.654 thread-0   block_log.cpp:142             open                 ] Index is nonempty
2018-08-20T16:53:30.662 thread-0   http_plugin.cpp:344           plugin_initialize    ] configured http to listen on 127.0.0.1:8888
2018-08-20T16:53:30.663 thread-0   net_plugin.cpp:2905           plugin_initialize    ] Initialize net plugin
2018-08-20T16:53:30.663 thread-0   net_plugin.cpp:2930           plugin_initialize    ] host: 0.0.0.0 port: 9876
2018-08-20T16:53:30.663 thread-0   net_plugin.cpp:3000           plugin_initialize    ] my node_id is 5bd1dd2469e571e9a508f8d54375f376d3929fbf14b886dab61c71c110a243c3
2018-08-20T16:53:30.663 thread-0   main.cpp:105                  main                 ] nodeos version ad4ba283
2018-08-20T16:53:30.663 thread-0   main.cpp:106                  main                 ] eosio root is /home/ubuntu/.local/share
2018-08-20T16:53:30.663 thread-0   chain_plugin.cpp:596          plugin_startup       ] starting chain in read/write mode
2018-08-20T16:53:30.663 thread-0   chain_plugin.cpp:600          plugin_startup       ] Blockchain started; head block is #17, genesis timestamp is 2018-06-01T12:00:00.000
2018-08-20T16:53:30.663 thread-0   http_plugin.cpp:401           plugin_startup       ] start listening for http requests
2018-08-20T16:53:30.663 thread-0   chain_api_plugin.cpp:75       plugin_startup       ] starting chain_api_plugin
2018-08-20T16:53:30.663 thread-0   http_plugin.cpp:447           add_handler          ] add api url: /v1/chain/abi_bin_to_json
2018-08-20T16:53:30.663 thread-0   http_plugin.cpp:447           add_handler          ] add api url: /v1/chain/abi_json_to_bin
2018-08-20T16:53:30.663 thread-0   http_plugin.cpp:447           add_handler          ] add api url: /v1/chain/get_abi
...
```
At this point, `nodeos` is running with a single producer, `eosio`. You can press `ctrl + c` to stop producing blocks

## Issue

When I run above command at first, I got this error:
```
2018-08-07T15:37:03.853 thread-0   chain_plugin.cpp:271          plugin_initialize    ] initializing chain plugin
2018-08-07T15:37:03.853 thread-0   chain_plugin.cpp:580          plugin_initialize    ] 3190000 block_log_exception: Block log exception
Block log was not setup properly with genesis information.
...
```

I think that is because at first I used outdated command from https://medium.com/@blockgenic/eosio-single-node-testnet-setup-on-windows-ae7a59900e69 :
```
cd build/programs/nodeos
./nodeos -e -p eosio --plugin eosio::wallet_api_plugin --plugin eosio::chain_api_plugin --plugin eosio::account_history_api_plugin
```
which gave me this:
```

ubuntu@ip-172-31-17-253:~/eos/build/programs/nodeos$ ./nodeos -e -p eosio --plugin eosio::wallet_api_plugin --plugin eosio::chain_api_plugin --plugin eosio::account_history_api_plugin
2018-08-07T15:06:48.965 thread-0   wallet_plugin.cpp:42          plugin_initialize    ] initializing wallet plugin
2018-08-07T15:06:48.966 thread-0   http_plugin.cpp:344           plugin_initialize    ] configured http to listen on 127.0.0.1:8888
2018-08-07T15:06:48.966 thread-0   chain_plugin.cpp:271          plugin_initialize    ] initializing chain plugin
2018-08-07T15:06:48.968 thread-0   chain_plugin.cpp:508          plugin_initialize    ] Starting up fresh blockchain with default genesis state.
CHAINBASE:   Failed to pin chainbase shared memory (of size 1024 MB) in RAM. Performance degradation is possible.
CHAINBASE:   Failed to pin chainbase shared memory (of size 340 MB) in RAM. Performance degradation is possible.
2018-08-07T15:06:48.998 thread-0   main.cpp:131                  main                 ] /home/ubuntu/eos/libraries/appbase/application.cpp(304): Throw in function appbase::abstract_plugin &appbase::application::get_plugin(const string &) const
Dynamic exception type: boost::exception_detail::clone_impl<boost::exception_detail::error_info_injector<std::runtime_error> >
std::exception::what: unable to find plugin: eosio::account_history_api_plugin
```

Solution:
https://github.com/EOSIO/eos/issues/3545
```
clear this folder as in remove the data folder altogether.

rm -rf ~/.local/share/eosio/nodeos/data
```
