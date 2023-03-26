
# Notional V3 contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Resources

- [Notional V3 Blog Post](https://blog.notional.finance/introducing-notional-v3/)
- [Notional V3 Specification](https://docs.google.com/document/d/1d2chGQ3TMxxhAweZ7OkBQtTWDp0iP2GOeEAPpJrey2E/edit?usp=sharing)
- [Notional V3 Full Pull Request](https://github.com/notional-finance/contracts-v2/pull/107)
- [Notional V3 Smart Contract Changes Only Diff](https://github.com/notional-finance/contracts-v2/pull/108)
- [Notional V2 Docs](https://docs.notional.finance/notional-v2/)
- [Notional V2 Technical Overview](https://www.youtube.com/watch?v=-8a5kY0QeYY&list=PLnKdM8f8QEJ2lJ59ZjhVCcJvrT056X0Ga)
- [Notional V2 Technical Blog Posts](https://blog.notional.finance/tag/technical/)


This is an upgrade to a fairly complex protocol that is running in production. A strong understanding of Notional V2 will definitely aid in understanding the changes. This [pull request](https://github.com/notional-finance/contracts-v2/pull/107) defines the full scope of changes to the protocol. 

It's recommended that auditors take a look at the individual commits in order to understand the nature of the changes. The commits have been rebased into thematic chunks that broadly correlate with the documentation in the [Notional V3 Specification](https://docs.google.com/document/d/1d2chGQ3TMxxhAweZ7OkBQtTWDp0iP2GOeEAPpJrey2E/edit?usp=sharing).


# On-chain context
 
```
DEPLOYMENT: Currently Mainnet, considering Arbitrum and Optimisim in the near future.
ERC20:  Any Non-Rebasing token. ex. USDC, DAI, USDT (future), wstETH, WETH, WBTC, FRAX, CRV, etc.
ERC721: None
ERC777: None
FEE-ON-TRANSFER: None planned, some support for fee on transfer
REBASING TOKENS: None, strictly prohibited due to balance tracking mechanism. Must use wrapped token versions.
ADMIN: Trusted
EXTERNAL-ADMINS: Trusted
```

Please answer the following questions to provide more context: 
### Q: Are there any additional protocol roles? If yes, please explain in detail:
1) The roles
2) The actions those roles can take 
3) Outcomes that are expected from those roles 
4) Specific actions/outcomes NOT intended to be possible for those roles

A: 

1) [Notional Owner](https://etherscan.io/address/0x22341fB5D92D3d801144aA5A925F401A91418A05) is permitted to upgrade the contract and set governance parameters. It is a Gnosis multisig. It is expected to act in accordance with Snapshot votes from NOTE token holders.

2) [Pause Guardian](https://etherscan.io/address/0xD9D5a9dc6a952b7aD6B05a983b399537B7c0Ee88) is permitted to downgrade the contract to the Pause Router in emergency scenarios. Only Notional Owner can upgrade the contract.

3) [Notional Manager](https://etherscan.io/address/0x02479BFC7Dce53A02e26fE7baea45a0852CB0909) is permitted to set total fCash debt outstanding figures in the Migrate Prime Cash contract during the Notional V2 to Notional V3 migration. It is trusted that this manaager will set the proper values for total fCash debt, which can only be inspected off chain at the moment.

3) [Treasury Manager](https://etherscan.io/address/0x53144559C0d4a3304e2DD9dAfBD685247429216d) is a smart contract that is permitted to claim COMP incentives, withdraw excess fee reserves to the protocol, and rebalance invested money market tokens. It is only permitted to do a handful of actions allowed by the Treasury Action contract.


___
### Q: Is the code/contract expected to comply with any EIPs? Are there specific assumptions around adhering to those EIPs that Watsons should be aware of?
A: Yes. The Notional V3 Proxy should adhere to ERC1155 for fCash and vault tokens. Note that fCash is transferrable, while Vault tokens are not. Events for both fCash and Vault tokens should be properly emitted from Notional V3. Balances for both fCash and vault tokens should be query-able via Notional V3.

The Notional V3 proxy will also deploy ERC20/ERC4626 compatible proxies for Prime Cash, nTokens and Prime Debt. All three should emit proper Transfer events for mints, burns and transfers. Existing Notional V2 nToken proxies do not emit proper Transfer events and cannot be upgraded. Full ERC4626 compatibility is not in this version (deposit, mint, withdraw, redeem are not fully functional) but view methods are implemented.

___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding.
A: 

Common audit issues that have already been considered:

- ETH transfers do not use .call() and use .transfer() instead. This mitigates re-entrancy and read-only re-entrancy issues.
- Chainlink oracle prices are not validated to be stale on the main Notional V3 proxy. We have made the design choice to ensure that liquidations can proceed in the face of a stale Chainlink price over reverting due to a stale price.

____
### Q: Please provide links to previous audits (if any).
A:

https://github.com/notional-finance/contracts-v2/tree/master/audits
___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, input validation expectations, etc)? 
A: 

1. Treasury Manager is expected to regularly run the rebalancing mechanism based on the rebalancing cool down time, currently expected to be once or twice a week.

2. Liquidation bots are expected to be online, this is a reasonable expectation. Notional V2 has a robust liquidator community.

_____

### Q: In case of external protocol integrations, are the risks of an external protocol pausing or executing an emergency withdrawal acceptable? If not, Watsons will submit issues related to these situations that can harm your protocol's functionality. 

A: Pausing is acceptable, emergency withdraws from the system would be strictly prohibited. This depends on the external money market that is integrated via IPrimeCashHoldingsOracles.


# Audit scope


[contracts-v2 @ b20a45c912785fab5f2b62992e5260f44dbae197](https://github.com/notional-finance/contracts-v2/tree/b20a45c912785fab5f2b62992e5260f44dbae197)
- [contracts-v2/contracts/external/CalculationViews.sol](contracts-v2/contracts/external/CalculationViews.sol)
- [contracts-v2/contracts/external/Router.sol](contracts-v2/contracts/external/Router.sol)
- [contracts-v2/contracts/external/SettleAssetsExternal.sol](contracts-v2/contracts/external/SettleAssetsExternal.sol)
- [contracts-v2/contracts/external/Views.sol](contracts-v2/contracts/external/Views.sol)
- [contracts-v2/contracts/external/actions/AccountAction.sol](contracts-v2/contracts/external/actions/AccountAction.sol)
- [contracts-v2/contracts/external/actions/ActionGuards.sol](contracts-v2/contracts/external/actions/ActionGuards.sol)
- [contracts-v2/contracts/external/actions/BatchAction.sol](contracts-v2/contracts/external/actions/BatchAction.sol)
- [contracts-v2/contracts/external/actions/ERC1155Action.sol](contracts-v2/contracts/external/actions/ERC1155Action.sol)
- [contracts-v2/contracts/external/actions/GovernanceAction.sol](contracts-v2/contracts/external/actions/GovernanceAction.sol)
- [contracts-v2/contracts/external/actions/InitializeMarketsAction.sol](contracts-v2/contracts/external/actions/InitializeMarketsAction.sol)
- [contracts-v2/contracts/external/actions/LiquidateCurrencyAction.sol](contracts-v2/contracts/external/actions/LiquidateCurrencyAction.sol)
- [contracts-v2/contracts/external/actions/LiquidatefCashAction.sol](contracts-v2/contracts/external/actions/LiquidatefCashAction.sol)
- [contracts-v2/contracts/external/actions/TradingAction.sol](contracts-v2/contracts/external/actions/TradingAction.sol)
- [contracts-v2/contracts/external/actions/TreasuryAction.sol](contracts-v2/contracts/external/actions/TreasuryAction.sol)
- [contracts-v2/contracts/external/actions/VaultAccountAction.sol](contracts-v2/contracts/external/actions/VaultAccountAction.sol)
- [contracts-v2/contracts/external/actions/VaultAction.sol](contracts-v2/contracts/external/actions/VaultAction.sol)
- [contracts-v2/contracts/external/actions/nTokenAction.sol](contracts-v2/contracts/external/actions/nTokenAction.sol)
- [contracts-v2/contracts/external/actions/nTokenMintAction.sol](contracts-v2/contracts/external/actions/nTokenMintAction.sol)
- [contracts-v2/contracts/external/actions/nTokenRedeemAction.sol](contracts-v2/contracts/external/actions/nTokenRedeemAction.sol)
- [contracts-v2/contracts/external/patchfix/BasePatchFixRouter.sol](contracts-v2/contracts/external/patchfix/BasePatchFixRouter.sol)
- [contracts-v2/contracts/global/Constants.sol](contracts-v2/contracts/global/Constants.sol)
- [contracts-v2/contracts/global/Deployments.sol](contracts-v2/contracts/global/Deployments.sol)
- [contracts-v2/contracts/global/LibStorage.sol](contracts-v2/contracts/global/LibStorage.sol)
- [contracts-v2/contracts/global/Types.sol](contracts-v2/contracts/global/Types.sol)
- [contracts-v2/contracts/internal/AccountContextHandler.sol](contracts-v2/contracts/internal/AccountContextHandler.sol)
- [contracts-v2/contracts/internal/balances/BalanceHandler.sol](contracts-v2/contracts/internal/balances/BalanceHandler.sol)
- [contracts-v2/contracts/internal/balances/Incentives.sol](contracts-v2/contracts/internal/balances/Incentives.sol)
- [contracts-v2/contracts/internal/balances/TokenHandler.sol](contracts-v2/contracts/internal/balances/TokenHandler.sol)
- [contracts-v2/contracts/internal/balances/protocols/CompoundHandler.sol](contracts-v2/contracts/internal/balances/protocols/CompoundHandler.sol)
- [contracts-v2/contracts/internal/balances/protocols/GenericToken.sol](contracts-v2/contracts/internal/balances/protocols/GenericToken.sol)
- [contracts-v2/contracts/internal/liquidation/LiquidateCurrency.sol](contracts-v2/contracts/internal/liquidation/LiquidateCurrency.sol)
- [contracts-v2/contracts/internal/liquidation/LiquidatefCash.sol](contracts-v2/contracts/internal/liquidation/LiquidatefCash.sol)
- [contracts-v2/contracts/internal/liquidation/LiquidationHelpers.sol](contracts-v2/contracts/internal/liquidation/LiquidationHelpers.sol)
- [contracts-v2/contracts/internal/markets/CashGroup.sol](contracts-v2/contracts/internal/markets/CashGroup.sol)
- [contracts-v2/contracts/internal/markets/Market.sol](contracts-v2/contracts/internal/markets/Market.sol)
- [contracts-v2/contracts/internal/nToken/nTokenHandler.sol](contracts-v2/contracts/internal/nToken/nTokenHandler.sol)
- [contracts-v2/contracts/internal/portfolio/BitmapAssetsHandler.sol](contracts-v2/contracts/internal/portfolio/BitmapAssetsHandler.sol)
- [contracts-v2/contracts/internal/portfolio/PortfolioHandler.sol](contracts-v2/contracts/internal/portfolio/PortfolioHandler.sol)
- [contracts-v2/contracts/internal/portfolio/TransferAssets.sol](contracts-v2/contracts/internal/portfolio/TransferAssets.sol)
- [contracts-v2/contracts/internal/settlement/SettleBitmapAssets.sol](contracts-v2/contracts/internal/settlement/SettleBitmapAssets.sol)
- [contracts-v2/contracts/internal/settlement/SettlePortfolioAssets.sol](contracts-v2/contracts/internal/settlement/SettlePortfolioAssets.sol)
- [contracts-v2/contracts/internal/valuation/AssetHandler.sol](contracts-v2/contracts/internal/valuation/AssetHandler.sol)
- [contracts-v2/contracts/internal/valuation/ExchangeRate.sol](contracts-v2/contracts/internal/valuation/ExchangeRate.sol)
- [contracts-v2/contracts/internal/valuation/FreeCollateral.sol](contracts-v2/contracts/internal/valuation/FreeCollateral.sol)
- [contracts-v2/contracts/internal/vaults/VaultAccount.sol](contracts-v2/contracts/internal/vaults/VaultAccount.sol)
- [contracts-v2/contracts/internal/vaults/VaultConfiguration.sol](contracts-v2/contracts/internal/vaults/VaultConfiguration.sol)
- [contracts-v2/contracts/external/PauseRouter.sol](contracts-v2/contracts/external/PauseRouter.sol)
- [contracts-v2/contracts/external/actions/VaultAccountHealth.sol](contracts-v2/contracts/external/actions/VaultAccountHealth.sol)
- [contracts-v2/contracts/external/actions/VaultLiquidationAction.sol](contracts-v2/contracts/external/actions/VaultLiquidationAction.sol)
- [contracts-v2/contracts/external/patchfix/MigratePrimeCash.sol](contracts-v2/contracts/external/patchfix/MigratePrimeCash.sol)
- [contracts-v2/contracts/external/proxies/BaseERC4626Proxy.sol](contracts-v2/contracts/external/proxies/BaseERC4626Proxy.sol)
- [contracts-v2/contracts/external/proxies/PrimeCashProxy.sol](contracts-v2/contracts/external/proxies/PrimeCashProxy.sol)
- [contracts-v2/contracts/external/proxies/PrimeDebtProxy.sol](contracts-v2/contracts/external/proxies/PrimeDebtProxy.sol)
- [contracts-v2/contracts/external/proxies/nTokenERC20Proxy.sol](contracts-v2/contracts/external/proxies/nTokenERC20Proxy.sol)
- [contracts-v2/contracts/internal/Emitter.sol](contracts-v2/contracts/internal/Emitter.sol)
- [contracts-v2/contracts/internal/markets/InterestRateCurve.sol](contracts-v2/contracts/internal/markets/InterestRateCurve.sol)
- [contracts-v2/contracts/internal/markets/DeprecatedAssetRate.sol](contracts-v2/contracts/internal/markets/DeprecatedAssetRate.sol)
- [contracts-v2/contracts/internal/pCash/PrimeCashExchangeRate.sol](contracts-v2/contracts/internal/pCash/PrimeCashExchangeRate.sol)
- [contracts-v2/contracts/internal/pCash/PrimeRateLib.sol](contracts-v2/contracts/internal/pCash/PrimeRateLib.sol)
- [contracts-v2/contracts/internal/vaults/VaultSecondaryBorrow.sol](contracts-v2/contracts/internal/vaults/VaultSecondaryBorrow.sol)
- [contracts-v2/contracts/internal/vaults/VaultState.sol](contracts-v2/contracts/internal/vaults/VaultState.sol)
- [contracts-v2/contracts/internal/vaults/VaultValuation.sol](contracts-v2/contracts/internal/vaults/VaultValuation.sol)
- [contracts-v2/contracts/math/FloatingPoint.sol](contracts-v2/contracts/math/FloatingPoint.sol)
- [contracts-v2/contracts/math/SafeInt256.sol](contracts-v2/contracts/math/SafeInt256.sol)
- [contracts-v2/contracts/external/pCash/CompoundV2HoldingsOracle.sol](contracts-v2/contracts/external/pCash/CompoundV2HoldingsOracle.sol)
- [contracts-v2/contracts/external/pCash/ProportionalRebalancingStrategy.sol](contracts-v2/contracts/external/pCash/ProportionalRebalancingStrategy.sol)
- [contracts-v2/contracts/external/pCash/UnderlyingHoldingsOracle.sol](contracts-v2/contracts/external/pCash/UnderlyingHoldingsOracle.sol)
- [contracts-v2/contracts/external/pCash/adapters/CompoundV2AssetAdapter.sol](contracts-v2/contracts/external/pCash/adapters/CompoundV2AssetAdapter.sol)



# About Notional Finance

[Notional Finance](https://notional.finance) is a lending protocol on Ethereum Mainnet. Notional V2 has been in production for over a year and has processed over $750 million in lending and borrowing volume via its fixed rate marketse. Notional V3 is an upgrade to that system which enables variable rate lending and borrowing markets.
