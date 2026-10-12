# clone-lin-lido-shares

Experimental i64-scale LIN clone of [lidofinance/lido-dao](https://github.com/lidofinance/lido-dao) tag `v4.1.0` stETH share-rate (`contracts/0.4.24/StETH.sol` + `contracts/common/lib/Math256.sol`).

- **LIN clone:** [`src/lin_lido_shares.lin`](src/lin_lido_shares.lin)
- **Proofs (results only) live in lin-open:** https://github.com/kbelludoo/lin-open
- **Event receipt:** `docs/events/EVENT_LIDO_SHARES_CLONE_LIN.rulel` in lin-open

This repository is the standalone LIN copy. It is **not** Lido, **not** a staking router, and does **not** claim Solidity uint256 / mainnet stETH / wstETH wrap parity.

Upstream StETH.sol is GPL-3.0. Math256.ceilDiv is MIT (copied by Lido from OpenZeppelin Math.sol). This LIN kernel is an original uint64-scale implementation of the published share-rate, not a verbatim Solidity dump.

Canonical goldens (uint64-scale, 128-bit product):

- `getSharesByPooledEth(1000, 0, 0)` = **0** (empty numerator is on-chain div0; LIN fail-closes)
- named contrast OZ ERC-4626 +1/+1: `1000` shares for 1000 ETH empty
- named contrast Morpho VIRTUAL_SHARES=1e6: `1000000000` shares for 1000 ETH empty
- after 1-wei bootstrap + 1e6-ETH donation, victim of 1e6 ETH: Lido **0** vs OZ **1** vs Morpho **1999996**
- `getSharesByPooledEth(1000, 5000, 2000)` = **400**; `getPooledEthByShares(400, 5000, 2000)` = **1000**
- `getPooledEthByShares(1, 3, 100)` = **0**; `getPooledEthBySharesRoundUp` = **1** (remainder+1, not `(a+b-1)/b`)
- `1e18 * 1e18 / 1` fail-closes to **0**
