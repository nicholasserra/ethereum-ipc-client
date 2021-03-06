# Ethereum IPC Client

[![Build Status](https://travis-ci.org/pipermerriam/ethereum-ipc-client.png)](https://travis-ci.org/pipermerriam/ethereum-ipc-client)
[![Documentation Status](https://readthedocs.org/projects/ethereum-ipc-client/badge/?version=latest)](https://readthedocs.org/projects/ethereum-ipc-client/?badge=latest)
[![PyPi version](https://pypip.in/v/ethereum-ipc-client/badge.png)](https://pypi.python.org/pypi/ethereum-ipc-client)
[![PyPi downloads](https://pypip.in/d/ethereum-ipc-client/badge.png)](https://pypi.python.org/pypi/ethereum-ipc-client)
   

Python client for Ethereum over IPC

## Installation

Install with `pip`

```bash
$ pip install ethereum-ipc-client
```

## Basic Usage

Assuming you have a go-ethereum node running with the default settings then you
should be able to simply import and instantiate the client.


```python
>>> from eth_ipc_client import Client
>>> client = Client()
>>> client.get_coinbase()
... '0xd3cda913deb6f67967b99d67acdfa1712c293601'
```

If you are running with a non-default data directory, the client will require
you to specify the path to the ipc socket.

```python
>>> from eth_ipc_client import Client
>>> client = Client("/data/ethereum/geth.ipc")
>>> client.get_coinbase()
... '0xd3cda913deb6f67967b99d67acdfa1712c293601'
```

## API

### `Client.get_coinbase()`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_coinbase

Returns the hex encoded coinbase.

### `Client.get_gas_price()`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gasprice

Returns the gas price in wei as an integer

### `Client.get_balance(address, block="latest")`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getbalance

* **address**: The hex encoded address to lookup the balance for.
* **block**: The block to use for the lookup.

Returns the account balance for the address in wei as an integer.

### `Client.get_code(address, block="latest")`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getcode

* **address**: The hex encoded address to lookup the code for.
* **block**: The block to use for the lookup.

Returns the code at the given address.

### `Client.call(_from=None, to=None, gas=None, gas_price=None, value=0, data=None, block="latest")`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_call

* **_from**: The hex encoded address to use as the source for the call.
* **to**: The hex encoded address of the contract for the call.
* **gas**: Integer gas alotment for the call.
* **gas_price**: Integer gas price in wei.
* **value**: Integer amount in wei to send with the call.
* **data**: The call data.

Returns the call response.

### `Client.send_transaction(_from=None, to=None, gas=None, gas_price=None, value=0, data=None)

https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_sendtransaction

* **_from**: The hex encoded address to use as the source for the transaction.
* **to**: The hex encoded address of the contract for the transaction.
* **gas**: Integer gas alotment for the transaction.
* **gas_price**: Integer gas price in wei.
* **value**: Integer amount in wei to send with the transaction.
* **data**: The transaction data.

### `Client.get_transaction_receipt(txn_hash)`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gettransactionreceipt

* **txn_hash**: The hex encoded transaction hash to lookup.

Returns a dictionary of the transaction receipt or `None` if no receipt is
found.

* **transactionHash**: hex encoded hash of the transaction.
* **transactionIndex**: integer of the transactions index position in the block.
* **blockHash**: hex encoded hash of the block where this transaction was in.
* **blockNumber**: integer block number where this transaction was in.
* **cumulativeGasUsed**: The total amount of gas used when this transaction was executed in the block.
* **gasUsed**: The amount of gas used by this specific transaction alone.
* **contractAddress**: The contract address created, if the transaction was a contract creation, otherwise null.
* **logs**: list of log objects, which this transaction generated


### `Client.get_transaction_by_hash(txn_hash)`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gettransactionbyhash

* **txn_hash**: The hex encoded transaction hash to lookup.

Returns a dictionary of the transaction values or `None` if no transaction is
found.

    * **hash**: DATA, 32 Bytes - hash of the transaction.
    * **nonce**: QUANTITY - the number of transactions made by the sender prior to this one.
    * **blockHash**: DATA, 32 Bytes - hash of the block where this transaction was in. null when its pending.
    * **blockNumber**: QUANTITY - block number where this transaction was in. null when its pending.
    * **transactionIndex**: QUANTITY - integer of the transactions index position in the block. null when its pending.
    * **from**: DATA, 20 Bytes - address of the sender.
    * **to**: DATA, 20 Bytes - address of the receiver. null when its a contract creation transaction.
    * **value**: QUANTITY - value transferred in Wei.
    * **gasPrice**: QUANTITY - gas price provided by the sender in Wei.
    * **gas**: QUANTITY - gas provided by the sender.
    * **input**: DATA - the data send along with the transaction.


### `Client.get_block_number()`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_blocknumber

Returns the number of the most recent block.


### `Client.get_accounts()`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_accounts

Returns a list of the addresses owned by the client.


### `Client.new_filter(from_block=None, to_block=None, address=None, topics=None)`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_newfilter


### `Client.new_block_filter()`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_newblockfilter


### `Client.new_pending_transaction_filter()`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_newpendingtransactionfilter


### `Client.uninstall_filter(filter_id)`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_uninstallFilter


### `Client.get_filter_changes(filter_id)`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getfilterchanges


### `Client.get_filter_logs(filter_id)`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getfilterlogs


### `Client.get_logs(from_block=None, to_block=None, address=None, topics=None)`

> https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_getlogs


## Helpers

### `Client.get_max_gas()`

Returns the gas limit from the latest block


### `Client.wait_for_transaction(txn_hash, max_wait=60)`

Blocks for up to `max_wait` seconds, polling for the transaction receipt for
the provided `txn_hash`.  Returns the transaction hash.


### `Client.wait_for_block(block_number, max_wait=60)`

Blocks for up to `max_wait` seconds, polling the ipc server until the block
specified by `block_number` is seen.  Returns the block.
