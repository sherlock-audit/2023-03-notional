
# [project name] contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Resources

- [[resource1]](url)
- [[resource2]](url)

# On-chain context

The README is a **very important** document for the audit. Please fill it out thoroughly and include any other specific info that security experts will need in order to effectively review the codebase.

**Some pointers for filling out the section below:**  
ERC20/ERC721/ERC777/FEE-ON-TRANSFER/REBASING TOKENS:  
*Which tokens do you expect will interact with the smart contracts? Please note that these answers have a significant impact on the issues that will be submitted by Watsons. Please list specific tokens (ETH, USDC, DAI) where possible, otherwise "Any"/"None" type answers are acceptable as well.*

ADMIN:
*Admin/owner of the protocol/contracts.
Label as TRUSTED, If you **don't** want to receive issues about the admin of the contract being able to steal funds. 
If you want to receive issues about the Admin of the contract being able to steal funds, label as RESTRICTED & list specific acceptable/unacceptable actions for the admins.*

EXTERNAL ADMIN:
*These are admins of the protocols your contracts integrate with (if any). 
If you **don't** want to receive issues about this Admin being able to steal funds or result in loss of funds, label as TRUSTED
If you want to receive issues about this admin being able to steal or result in loss of funds, label as RESTRICTED.*
 
```
DEPLOYMENT: [e.g. mainnet, Arbitrum, Optimism, ..]
ERC20: [e.g. any, none, USDC, USDC and USDT]
ERC721: [e.g. any, none, UNI-V3]
ERC777: [e.g. any, none, {token name}]
FEE-ON-TRANSFER: [e.g. any, none, {token name}]
REBASING TOKENS: [e.g. any, none, {token name}]
ADMIN: [trusted, restricted, n/a]
EXTERNAL-ADMINS: [trusted, restricted, n/a]
```


Please answer the following questions to provide more context: 
### Q: Are there any additional protocol roles? If yes, please explain in detail:
1) The roles
2) The actions those roles can take 
3) Outcomes that are expected from those roles 
4) Specific actions/outcomes NOT intended to be possible for those roles

A: 

___
### Q: Is the code/contract expected to comply with any EIPs? Are there specific assumptions around adhering to those EIPs that Watsons should be aware of?
A:

___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding.
A: 

____
### Q: Please provide links to previous audits (if any).
A:

___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, input validation expectations, etc)? 
A: 
_____

### Q: In case of external protocol integrations, are the risks of an external protocol pausing or executing an emergency withdrawal acceptable? If not, Watsons will submit issues related to these situations that can harm your protocol's functionality. 
A: [ACCEPTABLE/NOT ACCEPTABLE] 


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



# About [project name]
