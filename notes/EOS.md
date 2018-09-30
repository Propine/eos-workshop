# EOS

## Architecture

* Know the tools you will be using: https://developers.eos.io/eosio-home/docs/how-it-all-fits-together

- `nodeos` (node + eos = nodeos) - the core EOSIO node daemon that can be configured with plugins to run a node. Example uses are block production, dedicated API endpoints, and local development.
- `cleos` (cli + eos = cleos) - command line interface to interact with the blockchain and to manage wallets
- `keosd` (key + eos = keosd) - component that securely stores EOSIO keys in wallets.
- `eosio-cpp` - Part of the 'contract development toolkit', it compiles C++ code to WAST files and generates ABIs

```
             +-----------------+
             |  Local Machine  |
             +------+----------+
                    |
               +----+-----+
     +---------+  cleos   +---------+
     |         +----------+         |
+----+---+                     +----+---+          +----------------+
| keosd  |                     | nodeos +----------+   blockchain   |
+----+---+                     +--------+          +----------------+
     |
     |
+----+----+
|  wallet |
+---------+
```

##### Ref: https://developers.eos.io/eosio-home/docs/setting-up-your-environment
##### temp note for EOS setup, from trying out tutorial
##### up to 1.5
```
➜  eos-contracts docker pull eosio/eos
Using default tag: latest
latest: Pulling from eosio/eos
6b98dfc16071: Already exists
4001a1209541: Already exists
6319fc68c576: Already exists
b24603670dc3: Already exists
97f170c87c6f: Already exists
ca8277dae3e4: Already exists
49bdd58b12e0: Pull complete
babc19f60361: Pull complete
43aee4e7e3fc: Pull complete
bb2379d0cae8: Pull complete
fd68f802c6ca: Pull complete
45da51400e88: Pull complete
Digest: sha256:92d73fb9dcae6cda556036cd7d78531bec61b74bd0ac9c3d3c9897ec466e1fc5
Status: Downloaded newer image for eosio/eos:latest
➜  eos-contracts docker run --name eosio \
  --publish 7777:7777 \
  --publish 127.0.0.1:5555:5555 \
  --volume /home/dolita/stage/repo/eos-contracts:/home/dolita/stage/repo/eos-contracts \
  --detach \
  eosio/eos \
  /bin/bash -c \
  "keosd --http-server-address=0.0.0.0:5555 & exec nodeos -e -p eosio --plugin eosio::producer_plugin --plugin eosio::history_plugin --plugin eosio::chain_api_plugin --plugin eosio::history_plugin --plugin eosio::history_api_plugin --plugin eosio::http_plugin -d /mnt/dev/data --config-dir /mnt/dev/config --http-server-address=0.0.0.0:7777 --access-control-allow-origin=* --contracts-console --http-validate-host=false --filter-on='*'"
cc183eabc00b57a957ad2f789d3ddb1c551eda92b954f2061103097afac28d6d
➜  eos-contracts docker logs --tail 10 eosio
2018-09-30T16:29:51.001 thread-0   producer_plugin.cpp:1419      produce_block        ] Produced block 0000001e69227524... #30 @ 2018-09-30T16:29:51.000 signed by eosio [trxs: 0, lib: 29, confirmed: 0]
2018-09-30T16:29:51.502 thread-0   producer_plugin.cpp:1419      produce_block        ] Produced block 0000001fbfc3fba6... #31 @ 2018-09-30T16:29:51.500 signed by eosio [trxs: 0, lib: 30, confirmed: 0]
2018-09-30T16:29:52.001 thread-0   producer_plugin.cpp:1419      produce_block        ] Produced block 000000206b90d102... #32 @ 2018-09-30T16:29:52.000 signed by eosio [trxs: 0, lib: 31, confirmed: 0]
2018-09-30T16:29:52.502 thread-0   producer_plugin.cpp:1419      produce_block        ] Produced block 00000021aa8cdd0b... #33 @ 2018-09-30T16:29:52.500 signed by eosio [trxs: 0, lib: 32, confirmed: 0]
2018-09-30T16:29:53.001 thread-0   producer_plugin.cpp:1419      produce_block        ] Produced block 00000022a022318d... #34 @ 2018-09-30T16:29:53.000 signed by eosio [trxs: 0, lib: 33, confirmed: 0]
2018-09-30T16:29:53.502 thread-0   producer_plugin.cpp:1419      produce_block        ] Produced block 0000002305152921... #35 @ 2018-09-30T16:29:53.500 signed by eosio [trxs: 0, lib: 34, confirmed: 0]
2018-09-30T16:29:54.002 thread-0   producer_plugin.cpp:1419      produce_block        ] Produced block 00000024e439c573... #36 @ 2018-09-30T16:29:54.000 signed by eosio [trxs: 0, lib: 35, confirmed: 0]
2018-09-30T16:29:54.501 thread-0   producer_plugin.cpp:1419      produce_block        ] Produced block 000000250e83e8de... #37 @ 2018-09-30T16:29:54.500 signed by eosio [trxs: 0, lib: 36, confirmed: 0]
2018-09-30T16:29:55.002 thread-0   producer_plugin.cpp:1419      produce_block        ] Produced block 0000002605451c88... #38 @ 2018-09-30T16:29:55.000 signed by eosio [trxs: 0, lib: 37, confirmed: 0]
2018-09-30T16:29:55.500 thread-0   producer_plugin.cpp:1419      produce_block        ] Produced block 00000027bff7e510... #39 @ 2018-09-30T16:29:55.500 signed by eosio [trxs: 0, lib: 38, confirmed: 0]
➜  eos-contracts


➜  eos-contracts docker exec -it eosio bash
root@cc183eabc00b:/# cleos --wallet-url http://127.0.0.1:5555 wallet list keys
Wallets:
[]
Error 3120006: No available wallet
Ensure that you have created a wallet and have it open
root@cc183eabc00b:/# exit
exit



➜  eos-contracts curl http://localhost:7777/v1/chain/get_info
{"server_version":"0f6695cb","chain_id":"cf057bbfb72640471fd910bcb67639c22df9f92470936cddc1ade0e2f2e7dc4f","head_block_num":347,"last_irreversible_block_num":346,"last_irreversible_block_id":"0000015a7f1cbbe1c092057128988a1c13a201a8923db3d82d07e35b2e2273e3","head_block_id":"0000015b323678970a527202d036e999de60b3f9703d4bff7285b48717f9b7b0","head_block_time":"2018-09-30T16:32:29.500","head_block_producer":"eosio","virtual_block_cpu_limit":282524,"virtual_block_net_limit":1482111,"block_cpu_limit":199900,"block_net_limit":1048576,"server_version_string":"v1.3.0"}%                                                               ➜  eos-contracts alias cleos='docker exec -it eosio /opt/eosio/bin/cleos --url http://127.0.0.1:7777 --wallet-url http://127.0.0.1:5555'

➜  eos-contracts cleos
ERROR: RequiredError: Subcommand required
Command Line Interface to EOSIO Client
Usage: /opt/eosio/bin/cleos [OPTIONS] SUBCOMMAND

Options:
  -h,--help                   Print this help message and exit
  -u,--url TEXT=http://127.0.0.1:8888/
                              the http/https URL where nodeos is running
  --wallet-url TEXT=http://127.0.0.1:8900/
                              the http/https URL where keosd is running
  -r,--header                 pass specific HTTP header; repeat this option to pass multiple headers
  -n,--no-verify              don't verify peer certificate when using HTTPS
  --no-auto-keosd             don't automatically launch a keosd if one is not currently running
  -v,--verbose                output verbose actions on error
  --print-request             print HTTP request to STDERR
  --print-response            print HTTP response to STDERR

Subcommands:
  version                     Retrieve version information
  create                      Create various items, on and off the blockchain
  convert                     Pack and unpack transactions
  get                         Retrieve various items and information from the blockchain
  set                         Set or update blockchain state
  transfer                    Transfer EOS from account to account
  net                         Interact with local p2p network connections
  wallet                      Interact with local wallet
  sign                        Sign a transaction
  push                        Push arbitrary transactions to the blockchain
  multisig                    Multisig contract commands
  sudo                        Sudo contract commands
  system                      Send eosio.system contract action to the blockchain.

➜  eos-contracts cd ..
➜  repo git clone --recursive https://github.com/eosio/eosio.cdt
Cloning into 'eosio.cdt'...
remote: Enumerating objects: 60, done.
remote: Counting objects: 100% (60/60), done.
remote: Compressing objects: 100% (52/52), done.
remote: Total 5929 (delta 20), reused 27 (delta 7), pack-reused 5869
Receiving objects: 100% (5929/5929), 20.23 MiB | 1.72 MiB/s, done.
Resolving deltas: 100% (2629/2629), done.
Checking connectivity... done.
Submodule 'eosio_llvm' (https://github.com/eosio/llvm) registered for path 'eosio_llvm'
Submodule 'libraries/libc++/libcxx' (https://github.com/eosio/libcxx) registered for path 'libraries/libc++/libcxx'
Submodule 'libraries/libc/musl' (https://github.com/eosio/musl) registered for path 'libraries/libc/musl'
Cloning into 'eosio_llvm'...
remote: Enumerating objects: 14, done.
remote: Counting objects: 100% (14/14), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 1540596 (delta 3), reused 1 (delta 1), pack-reused 1540582
Receiving objects: 100% (1540596/1540596), 842.04 MiB | 1.52 MiB/s, done.
Resolving deltas: 100% (1241209/1241209), done.
Checking connectivity... done.
Submodule path 'eosio_llvm': checked out '31e1613c474cab51a80ac898ec31716273df3729'
Submodule 'tools/clang' (https://github.com/eosio/clang.git) registered for path 'tools/clang'
Submodule 'tools/lld' (https://github.com/eosio/lld.git) registered for path 'tools/lld'
Cloning into 'tools/clang'...
remote: Enumerating objects: 11, done.
remote: Counting objects: 100% (11/11), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 691138 (delta 2), reused 3 (delta 1), pack-reused 691127
Receiving objects: 100% (691138/691138), 398.51 MiB | 2.23 MiB/s, done.
Resolving deltas: 100% (575950/575950), done.
Checking connectivity... done.
Submodule path 'eosio_llvm/tools/clang': checked out '0f0efd6a05ee928dfa1b505c55c0d90ae188488e'
Cloning into 'tools/lld'...
remote: Enumerating objects: 88820, done.
remote: Total 88820 (delta 0), reused 0 (delta 0), pack-reused 88820
Receiving objects: 100% (88820/88820), 27.22 MiB | 2.05 MiB/s, done.
Resolving deltas: 100% (71832/71832), done.
Checking connectivity... done.
Submodule path 'eosio_llvm/tools/lld': checked out 'f1b1edd50b6d3f471376034fe6fd96dc1ab0a2f9'
Cloning into 'libraries/libc++/libcxx'...
remote: Enumerating objects: 73633, done.
remote: Total 73633 (delta 0), reused 0 (delta 0), pack-reused 73633
Receiving objects: 100% (73633/73633), 22.83 MiB | 1.87 MiB/s, done.
Resolving deltas: 100% (51710/51710), done.
Checking connectivity... done.
Submodule path 'libraries/libc++/libcxx': checked out '9cd5fcf75de81e6e09ab78fb97bc5c41f4cbc13e'
Cloning into 'libraries/libc/musl'...
remote: Enumerating objects: 27733, done.
remote: Total 27733 (delta 0), reused 0 (delta 0), pack-reused 27733
Receiving objects: 100% (27733/27733), 4.34 MiB | 1.50 MiB/s, done.
Resolving deltas: 100% (19758/19758), done.
Checking connectivity... done.
Submodule path 'libraries/libc/musl': checked out '04df400c7a55ee00ff0d3449167ae2a6fe720e67'
➜  repo cd eosio.cdt

➜  eosio.cdt git:(master) ./build.sh SYS
	=========== Building eosio.cdt ===========


	OS name:
	OS Version: 16.04
	CPU speed: 1696.077Mhz
	CPU cores: 4
	Physical Memory: 3829 Mgb
	Disk install: /dev/sdb2
	Disk space total: 106G
	Disk space available: 84G
	Your system must have 7 or more Gigabytes of physical memory installed.
	Exiting now.
```
