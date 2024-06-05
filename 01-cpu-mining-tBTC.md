# 01 - CPU mining tBTC

The SRI community provides a hosted SV2 pool aimed at mining `tBTC` on `testnet4`.

Let's send some hashrate from our CPU towards it:
```
# nix-shell -p cpuminer
$ minerd -a sha256d -o stratum+tcp://75.119.150.111:34255 -q -D -P
```

You are going to see some SV1 logs. That is because `minerd` is a legacy implementation ossified on SV1.

Since a big part of the mining industry still revolves around ASICs controlled by SV1 firwmare, SRI provides an implementation of a [Translator Proxy](https://github.com/stratum-mining/stratum/tree/main/roles/translator) (or tProxy for short) as a strategy to bridge the SV1/SV2 gap.

The TCP socket `75.119.150.111:34255` is actually running a tProxy that receives SV1 shares and relays them to the Pool as SV2 shares.

The SV2 Pool implementation provided by SRI is just a dummy hash aggregator. It does not have any logic for share accounting, nor reward distribution. Whenever the pool finds a block, the entire coinbase goes to a specific address, locked under one of the following script flavors:
- P2PK
- P2PKH
- P2SH
- P2WSH
- P2WPKH
- P2TR

More specifically, the SRI Pool receiving the shares sent towards `stratum+tcp://75.119.150.111:34255` will pay the entire coinbase to `tb1qau7l338knthgjs035edxv5l9r9xs4tpcs8uudx` via a P2PK script on the coinbase. 

---

## Questions

- What is missing from SRI pool implementation in order to make it suitable for production? Hint: the SV2 protocol spec is agnostic to pool internal economics.
- As a hasher, what kind of block template are you working on? Who chose the transactions, and under what criteria? Are you comfortable allocating your hashpower like this? Hint: SV2 introduces something called Job Declaration protocol, which was not used during this lesson.
