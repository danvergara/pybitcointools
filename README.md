# pybtctools

> Python library for Bitcoin signatures and transactions

[![Tests](https://github.com/danvergara/pybtctools/workflows/Tests/badge.svg)](https://github.com/danvergara/pybtctools/actions?workflow=Tests)

### Features:

* Functions have a simple interface, inputting and outputting in standard formats
* Many functions can be taken out and used individually
* Supports binary, hex and base58
* Transaction deserialization format almost compatible with BitcoinJS
* Electrum and BIP0032 support
* Make and publish a transaction all in a single command line instruction
* Includes non-bitcoin-specific conversion and JSON utilities

### Disadvantages:

* Not a full node, has no idea what blocks are
* Relies on centralized service (blockchain.info) for blockchain operations, although operations do have backups (eligius, blockr.io). (Deprecated, please don't rely on these functions, we're working on replace the current API calls)

### Usage:

```py

  from bitcoin import *

  priv = sha256('some big long brainwallet password')
  print(priv)
  # 57c617d9b4e1f7af6ec97ca2ff57e94a28279a7eedd4d12a99fa11170e94f5a4'

  pub = privtopub(priv)
  print(pub)
  # '0420f34c2786b4bae593e22596631b025f3ff46e200fc1d4b52ef49bbdc2ed00b26c584b7e32523fb01be2294a1f8a5eb0cf71a203cc034ced46ea92a8df16c6e9'

  addr = pubtoaddr(pub)
  print(addr)
  # '1CQLd3bhw4EzaURHbKCwM5YZbUQfA4ReY6'

  h = history(addr)
  print(h)
  # [{'output': u'97f7c7d8ac85e40c255f8a763b6cd9a68f3a94d2e93e8bfa08f977b92e55465e:0', 'value': 50000, 'address': u'1CQLd3bhw4EzaURHbKCwM5YZbUQfA4ReY6'}, {'output': u'4cc806bb04f730c445c60b3e0f4f44b54769a1c196ca37d8d4002135e4abd171:1', 'value': 50000, 'address': u'1CQLd3bhw4EzaURHbKCwM5YZbUQfA4ReY6'}]

  outs = [{'value': 90000, 'address': '16iw1MQ1sy1DtRPYw3ao1bCamoyBJtRB4t'}]
  tx = mktx(h,outs)
  print(tx)
  # '01000000025e46552eb977f908fa8b3ee9d2943a8fa6d96c3b768a5f250ce485acd8c7f7970000000000ffffffff71d1abe4352100d4d837ca96c1a16947b5444f0f3e0bc645c430f704bb06c84c0100000000ffffffff01905f0100000000001976a9143ec6c3ed8dfc3ceabcc1cbdb0c5aef4e2d02873c88ac00000000'

  tx2 = sign(tx,0,priv)
  print(tx2)
  # '01000000025e46552eb977f908fa8b3ee9d2943a8fa6d96c3b768a5f250ce485acd8c7f797000000008b483045022100dd29d89a28451febb990fb1dafa21245b105140083ced315ebcdea187572b3990220713f2e554f384d29d7abfedf39f0eb92afba0ef46f374e49d43a728a0ff6046e01410420f34c2786b4bae593e22596631b025f3ff46e200fc1d4b52ef49bbdc2ed00b26c584b7e32523fb01be2294a1f8a5eb0cf71a203cc034ced46ea92a8df16c6e9ffffffff71d1abe4352100d4d837ca96c1a16947b5444f0f3e0bc645c430f704bb06c84c0100000000ffffffff01905f0100000000001976a9143ec6c3ed8dfc3ceabcc1cbdb0c5aef4e2d02873c88ac00000000'

  tx3 = sign(tx2,1,priv)
  print(tx3)
  # '01000000025e46552eb977f908fa8b3ee9d2943a8fa6d96c3b768a5f250ce485acd8c7f797000000008b483045022100dd29d89a28451febb990fb1dafa21245b105140083ced315ebcdea187572b3990220713f2e554f384d29d7abfedf39f0eb92afba0ef46f374e49d43a728a0ff6046e01410420f34c2786b4bae593e22596631b025f3ff46e200fc1d4b52ef49bbdc2ed00b26c584b7e32523fb01be2294a1f8a5eb0cf71a203cc034ced46ea92a8df16c6e9ffffffff71d1abe4352100d4d837ca96c1a16947b5444f0f3e0bc645c430f704bb06c84c010000008c4930460221008bbaaaf172adfefc3a1315dc7312c88645832ff76d52e0029d127e65bbeeabe1022100fdeb89658d503cf2737cedb4049e5070f689c50a9b6c85997d49e0787938f93901410420f34c2786b4bae593e22596631b025f3ff46e200fc1d4b52ef49bbdc2ed00b26c584b7e32523fb01be2294a1f8a5eb0cf71a203cc034ced46ea92a8df16c6e9ffffffff01905f0100000000001976a9143ec6c3ed8dfc3ceabcc1cbdb0c5aef4e2d02873c88ac00000000'

  pushtx(tx3)
  # 'Transaction Submitted'
```

### CLI
**The legacy CLI is not available at the moment**


## Public interface:

Function             | Description
---------------------|---------------------------------------------------------
privkey_to_pubkey    | (privkey) -> pubkey
privtopub            | (privkey) -> pubkey
pubkey_to_address    | (pubkey) -> address
pubtoaddr            | (pubkey) -> address
privkey_to_address   | (privkey) -> address
privtoaddr           | (privkey) -> address
add                  | (key1, key2) -> key1 + key2 (works on privkeys or pubkeys)
multiply             | (pubkey, privkey) -> returns pubkey * privkey
ecdsa_sign           | (message, privkey) -> sig
ecdsa_verify         | (message, sig, pubkey) -> True/False
ecdsa_recover        | (message, sig) -> pubkey
random_key           | () -> privkey
random_electrum_seed | () -> electrum seed
electrum_stretch     | (seed) -> secret exponent
electrum_privkey     | (seed or secret exponent, i, type) -> privkey
electrum_mpk         | (seed or secret exponent) -> master public key
electrum_pubkey      | (seed or secexp or mpk) -> pubkey
bip32_master_key     | (seed) -> bip32 master key
bip32_ckd            | (private or public bip32 key, i) -> child key
bip32_privtopub      | (private bip32 key) -> public bip32 key
bip32_extract_key    | (private or public bip32_key) -> privkey or pubkey
deserialize          | (hex or bin transaction) -> JSON tx
serialize            | (JSON tx) -> hex or bin tx
mktx                 | (inputs, outputs) -> tx
mksend               | (inputs, outputs, change_addr, fee) -> tx
sign                 | (tx, i, privkey) -> tx with index i signed with privkey
multisign            | (tx, i, script, privkey) -> signature
apply_multisignatures| (tx, i, script, sigs) -> tx with index i signed with sigs
scriptaddr           | (script) -> P2SH address
mk_multisig_script   | (pubkeys, k, n) -> k-of-n multisig script from pubkeys
verify_tx_input      | (tx, i, script, sig, pub) -> True/False
tx_hash              | (hex or bin tx) -> hash
history              | (address1, address2, etc) -> outputs to those addresses
unspent              | (address1, address2, etc) -> unspent outputs to those addresses
fetchtx              | (txash) -> tx if present
pushtx               | (hex or bin tx) -> tries to push to blockchain.info/pushtx
access               | (json list/object, prop) -> desired property of that json object
multiaccess          | (json list, prop) -> like access, but mapped across each list element
slice                | (json list, start, end) -> given slice of the list
count                | (json list) -> number of elements
sum                  | (json list) -> sum of all values
