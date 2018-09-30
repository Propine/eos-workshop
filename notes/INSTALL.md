# Setting Local environment

## EOSIO architecture

* For overview of each components in EOSIO architecture: https://developers.eos.io/eosio-nodeos/docs/overview-1

* Based on the diagram, we will need to install cleos locally

* The easiest way to do so is by installing via Docker

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

## Installing cleos

Ref: https://www.eosdocs.io/dappdevelopment/software/docker/

* Download EOS docker image
```
docker pull eosio/eos-dev
```

* Run the image and gain access to bash shell
```
docker run --rm -it eosio/eos-dev bash
```
Expect to see something similar to this:
```
root@6b18bdc5d569:/#
```

* Running `cleos` should bring up the help for cleos tool:
```
root@6b18bdc5d569:/# cleos
ERROR: RequiredError: Subcommand required
Command Line Interface to EOSIO Client
Usage: cleos [OPTIONS] SUBCOMMAND

Options:
  -h,--help                   Print this help message and exit
  -u,--url TEXT=http://127.0.0.1:8888/
                              the http/https URL where nodeos is running
  --wallet-url TEXT=http://127.0.0.1:8900/
                              the http/https URL where keosd is running
  -r,--header                 pass specific HTTP header; repeat this option to pass multiple headers
  -n,--no-verify              don't verify peer certificate when using HTTPS
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
```

* Type `exit` of press `ctrl+d` to exit the image

* Create docker network to allow containers to communicate with each other
```
docker network create eosnetwork
```

...
