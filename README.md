# Bitmex REST APIs for Go

## Overview
This bitmexgo package enables golang clients to call [REST APIs](https://www.bitmex.com/app/restAPI) for the Bitmex crypto trading platform.
It fixes critical bugs in the [swagger-generated code](https://github.com/BitMEX/api-connectors/tree/master/auto-generated/go) and enhances its usability.

Notable changes:
- Fixed the authentication logic and API endpoint
- Fixed form data encoding
- Renamed the package from `swagger` to `bitmexgo`
- Removed all external dependencies (`golang.org/x/oauth2` and `github.com/antihax/optional`)
- Added a local `optional` package with mutable states to simplify parameter assignment
- Fixed many type issues and removed the obsolete XAny type

This package also differs from an alternative library at https://github.com/qct/bitmex-go in the following ways:
- `bitmexgo` employs strongly-typed structs for API parameters instead of `map[string]interface{}`.
- `bitmexgo` is forked from a more recent version of the swagger-generated code.
- `bitmexgo` depends on no external packages and is compatible with Google App Engine.

## Installation
```bash
go get github.com/aiviaio/bitmexgo
```

## Usage

```golang
import "github.com/aiviaio/bitmexgo"

// Get your API key/secret pair at https://www.bitmex.com/app/apiKeys
apiKey := "..."
apiSecret := "..."

// Create an authentication context
auth := bitmexgo.NewAPIKeyContext(apiKey, apiSecret)

// Create a shareable API client instance
apiClient := bitmexgo.NewAPIClient(bitmexgo.NewConfiguration())

// Create a testnet API client instance
testnetClient := bitmexgo.NewAPIClient(bitmexgo.NewTestnetConfiguration())

// Call APIs without parameters by passing the auth context.
// e.g. getting exchange-wide turnover and volume statistics:
stats, res, err := apiClient.StatsApi.StatsGet(auth)

// Call APIs with default parameters by passing the auth context and a nil.
// e.g. getting all open positions:
pos, res, err := apiClient.PositionApi.PositionGet(auth, nil)

// Call APIs with additional parameters by constructing a corresponding XXXOpts struct.
// e.g. submitting a limit order to buy 20000 contracts of XBTUSD at $6000.5:
var params bitmexgo.OrderNewOpts
params.OrdType.Set("Limit")
params.Side.Set("Buy")
params.OrderQty.Set(20000)
params.Price.Set(6000.5)
order, res, err := apiClient.OrderApi.OrderNew(auth, "XBTUSD", &params)
```

## Documentation for API Endpoints

All URIs are relative to *https://www.bitmex.com/api/v1*

Class | Method | HTTP request | Description
------------ | ------------- | ------------- | -------------
*APIKeyApi* | [**APIKeyDisable**](docs/APIKeyApi.md#apikeydisable) | **Post** /apiKey/disable | Disable an API Key.
*APIKeyApi* | [**APIKeyEnable**](docs/APIKeyApi.md#apikeyenable) | **Post** /apiKey/enable | Enable an API Key.
*APIKeyApi* | [**APIKeyGet**](docs/APIKeyApi.md#apikeyget) | **Get** /apiKey | Get your API Keys.
*APIKeyApi* | [**APIKeyNew**](docs/APIKeyApi.md#apikeynew) | **Post** /apiKey | Create a new API Key.
*APIKeyApi* | [**APIKeyRemove**](docs/APIKeyApi.md#apikeyremove) | **Delete** /apiKey | Remove an API Key.
*AnnouncementApi* | [**AnnouncementGet**](docs/AnnouncementApi.md#announcementget) | **Get** /announcement | Get site announcements.
*AnnouncementApi* | [**AnnouncementGetUrgent**](docs/AnnouncementApi.md#announcementgeturgent) | **Get** /announcement/urgent | Get urgent (banner) announcements.
*ChatApi* | [**ChatGet**](docs/ChatApi.md#chatget) | **Get** /chat | Get chat messages.
*ChatApi* | [**ChatGetChannels**](docs/ChatApi.md#chatgetchannels) | **Get** /chat/channels | Get available channels.
*ChatApi* | [**ChatGetConnected**](docs/ChatApi.md#chatgetconnected) | **Get** /chat/connected | Get connected users.
*ChatApi* | [**ChatNew**](docs/ChatApi.md#chatnew) | **Post** /chat | Send a chat message.
*ExecutionApi* | [**ExecutionGet**](docs/ExecutionApi.md#executionget) | **Get** /execution | Get all raw executions for your account.
*ExecutionApi* | [**ExecutionGetTradeHistory**](docs/ExecutionApi.md#executiongettradehistory) | **Get** /execution/tradeHistory | Get all balance-affecting executions. This includes each trade, insurance charge, and settlement.
*FundingApi* | [**FundingGet**](docs/FundingApi.md#fundingget) | **Get** /funding | Get funding history.
*InstrumentApi* | [**InstrumentGet**](docs/InstrumentApi.md#instrumentget) | **Get** /instrument | Get instruments.
*InstrumentApi* | [**InstrumentGetActive**](docs/InstrumentApi.md#instrumentgetactive) | **Get** /instrument/active | Get all active instruments and instruments that have expired in &lt;24hrs.
*InstrumentApi* | [**InstrumentGetActiveAndIndices**](docs/InstrumentApi.md#instrumentgetactiveandindices) | **Get** /instrument/activeAndIndices | Helper method. Gets all active instruments and all indices. This is a join of the result of /indices and /active.
*InstrumentApi* | [**InstrumentGetActiveIntervals**](docs/InstrumentApi.md#instrumentgetactiveintervals) | **Get** /instrument/activeIntervals | Return all active contract series and interval pairs.
*InstrumentApi* | [**InstrumentGetCompositeIndex**](docs/InstrumentApi.md#instrumentgetcompositeindex) | **Get** /instrument/compositeIndex | Show constituent parts of an index.
*InstrumentApi* | [**InstrumentGetIndices**](docs/InstrumentApi.md#instrumentgetindices) | **Get** /instrument/indices | Get all price indices.
*InsuranceApi* | [**InsuranceGet**](docs/InsuranceApi.md#insuranceget) | **Get** /insurance | Get insurance fund history.
*LeaderboardApi* | [**LeaderboardGet**](docs/LeaderboardApi.md#leaderboardget) | **Get** /leaderboard | Get current leaderboard.
*LeaderboardApi* | [**LeaderboardGetName**](docs/LeaderboardApi.md#leaderboardgetname) | **Get** /leaderboard/name | Get your alias on the leaderboard.
*LiquidationApi* | [**LiquidationGet**](docs/LiquidationApi.md#liquidationget) | **Get** /liquidation | Get liquidation orders.
*NotificationApi* | [**NotificationGet**](docs/NotificationApi.md#notificationget) | **Get** /notification | Get your current notifications.
*OrderApi* | [**OrderAmend**](docs/OrderApi.md#orderamend) | **Put** /order | Amend the quantity or price of an open order.
*OrderApi* | [**OrderAmendBulk**](docs/OrderApi.md#orderamendbulk) | **Put** /order/bulk | Amend multiple orders for the same symbol.
*OrderApi* | [**OrderCancel**](docs/OrderApi.md#ordercancel) | **Delete** /order | Cancel order(s). Send multiple order IDs to cancel in bulk.
*OrderApi* | [**OrderCancelAll**](docs/OrderApi.md#ordercancelall) | **Delete** /order/all | Cancels all of your orders.
*OrderApi* | [**OrderCancelAllAfter**](docs/OrderApi.md#ordercancelallafter) | **Post** /order/cancelAllAfter | Automatically cancel all your orders after a specified timeout.
*OrderApi* | [**OrderClosePosition**](docs/OrderApi.md#ordercloseposition) | **Post** /order/closePosition | Close a position. [Deprecated, use POST /order with execInst: &#39;Close&#39;]
*OrderApi* | [**OrderGetOrders**](docs/OrderApi.md#ordergetorders) | **Get** /order | Get your orders.
*OrderApi* | [**OrderNew**](docs/OrderApi.md#ordernew) | **Post** /order | Create a new order.
*OrderApi* | [**OrderNewBulk**](docs/OrderApi.md#ordernewbulk) | **Post** /order/bulk | Create multiple new orders for the same symbol.
*OrderBookApi* | [**OrderBookGetL2**](docs/OrderBookApi.md#orderbookgetl2) | **Get** /orderBook/L2 | Get current orderbook in vertical format.
*PositionApi* | [**PositionGet**](docs/PositionApi.md#positionget) | **Get** /position | Get your positions.
*PositionApi* | [**PositionIsolateMargin**](docs/PositionApi.md#positionisolatemargin) | **Post** /position/isolate | Enable isolated margin or cross margin per-position.
*PositionApi* | [**PositionTransferIsolatedMargin**](docs/PositionApi.md#positiontransferisolatedmargin) | **Post** /position/transferMargin | Transfer equity in or out of a position.
*PositionApi* | [**PositionUpdateLeverage**](docs/PositionApi.md#positionupdateleverage) | **Post** /position/leverage | Choose leverage for a position.
*PositionApi* | [**PositionUpdateRiskLimit**](docs/PositionApi.md#positionupdaterisklimit) | **Post** /position/riskLimit | Update your risk limit.
*QuoteApi* | [**QuoteGet**](docs/QuoteApi.md#quoteget) | **Get** /quote | Get Quotes.
*QuoteApi* | [**QuoteGetBucketed**](docs/QuoteApi.md#quotegetbucketed) | **Get** /quote/bucketed | Get previous quotes in time buckets.
*SchemaApi* | [**SchemaGet**](docs/SchemaApi.md#schemaget) | **Get** /schema | Get model schemata for data objects returned by this API.
*SchemaApi* | [**SchemaWebsocketHelp**](docs/SchemaApi.md#schemawebsockethelp) | **Get** /schema/websocketHelp | Returns help text &amp; subject list for websocket usage.
*SettlementApi* | [**SettlementGet**](docs/SettlementApi.md#settlementget) | **Get** /settlement | Get settlement history.
*StatsApi* | [**StatsGet**](docs/StatsApi.md#statsget) | **Get** /stats | Get exchange-wide and per-series turnover and volume statistics.
*StatsApi* | [**StatsHistory**](docs/StatsApi.md#statshistory) | **Get** /stats/history | Get historical exchange-wide and per-series turnover and volume statistics.
*StatsApi* | [**StatsHistoryUSD**](docs/StatsApi.md#statshistoryusd) | **Get** /stats/historyUSD | Get a summary of exchange statistics in USD.
*TradeApi* | [**TradeGet**](docs/TradeApi.md#tradeget) | **Get** /trade | Get Trades.
*TradeApi* | [**TradeGetBucketed**](docs/TradeApi.md#tradegetbucketed) | **Get** /trade/bucketed | Get previous trades in time buckets.
*UserApi* | [**UserCancelWithdrawal**](docs/UserApi.md#usercancelwithdrawal) | **Post** /user/cancelWithdrawal | Cancel a withdrawal.
*UserApi* | [**UserCheckReferralCode**](docs/UserApi.md#usercheckreferralcode) | **Get** /user/checkReferralCode | Check if a referral code is valid.
*UserApi* | [**UserConfirm**](docs/UserApi.md#userconfirm) | **Post** /user/confirmEmail | Confirm your email address with a token.
*UserApi* | [**UserConfirmEnableTFA**](docs/UserApi.md#userconfirmenabletfa) | **Post** /user/confirmEnableTFA | Confirm two-factor auth for this account. If using a Yubikey, simply send a token to this endpoint.
*UserApi* | [**UserConfirmWithdrawal**](docs/UserApi.md#userconfirmwithdrawal) | **Post** /user/confirmWithdrawal | Confirm a withdrawal.
*UserApi* | [**UserDisableTFA**](docs/UserApi.md#userdisabletfa) | **Post** /user/disableTFA | Disable two-factor auth for this account.
*UserApi* | [**UserGet**](docs/UserApi.md#userget) | **Get** /user | Get your user model.
*UserApi* | [**UserGetAffiliateStatus**](docs/UserApi.md#usergetaffiliatestatus) | **Get** /user/affiliateStatus | Get your current affiliate/referral status.
*UserApi* | [**UserGetCommission**](docs/UserApi.md#usergetcommission) | **Get** /user/commission | Get your account&#39;s commission status.
*UserApi* | [**UserGetDepositAddress**](docs/UserApi.md#usergetdepositaddress) | **Get** /user/depositAddress | Get a deposit address.
*UserApi* | [**UserGetMargin**](docs/UserApi.md#usergetmargin) | **Get** /user/margin | Get your account&#39;s margin status. Send a currency of \&quot;all\&quot; to receive an array of all supported currencies.
*UserApi* | [**UserGetWallet**](docs/UserApi.md#usergetwallet) | **Get** /user/wallet | Get your current wallet information.
*UserApi* | [**UserGetWalletHistory**](docs/UserApi.md#usergetwallethistory) | **Get** /user/walletHistory | Get a history of all of your wallet transactions (deposits, withdrawals, PNL).
*UserApi* | [**UserGetWalletSummary**](docs/UserApi.md#usergetwalletsummary) | **Get** /user/walletSummary | Get a summary of all of your wallet transactions (deposits, withdrawals, PNL).
*UserApi* | [**UserLogout**](docs/UserApi.md#userlogout) | **Post** /user/logout | Log out of BitMEX.
*UserApi* | [**UserLogoutAll**](docs/UserApi.md#userlogoutall) | **Post** /user/logoutAll | Log all systems out of BitMEX. This will revoke all of your account&#39;s access tokens, logging you out on all devices.
*UserApi* | [**UserMinWithdrawalFee**](docs/UserApi.md#userminwithdrawalfee) | **Get** /user/minWithdrawalFee | Get the minimum withdrawal fee for a currency.
*UserApi* | [**UserRequestEnableTFA**](docs/UserApi.md#userrequestenabletfa) | **Post** /user/requestEnableTFA | Get secret key for setting up two-factor auth.
*UserApi* | [**UserRequestWithdrawal**](docs/UserApi.md#userrequestwithdrawal) | **Post** /user/requestWithdrawal | Request a withdrawal to an external wallet.
*UserApi* | [**UserSavePreferences**](docs/UserApi.md#usersavepreferences) | **Post** /user/preferences | Save user preferences.
*UserApi* | [**UserUpdate**](docs/UserApi.md#userupdate) | **Put** /user | Update your password, name, and other attributes.


## Documentation For Models

 - [AccessToken](docs/AccessToken.md)
 - [Affiliate](docs/Affiliate.md)
 - [Announcement](docs/Announcement.md)
 - [ApiKey](docs/ApiKey.md)
 - [Chat](docs/Chat.md)
 - [ChatChannel](docs/ChatChannel.md)
 - [ConnectedUsers](docs/ConnectedUsers.md)
 - [ErrorError](docs/ErrorError.md)
 - [Execution](docs/Execution.md)
 - [Funding](docs/Funding.md)
 - [IndexComposite](docs/IndexComposite.md)
 - [InlineResponse200](docs/InlineResponse200.md)
 - [InlineResponse2001](docs/InlineResponse2001.md)
 - [Instrument](docs/Instrument.md)
 - [InstrumentInterval](docs/InstrumentInterval.md)
 - [Insurance](docs/Insurance.md)
 - [Leaderboard](docs/Leaderboard.md)
 - [Liquidation](docs/Liquidation.md)
 - [Margin](docs/Margin.md)
 - [ModelError](docs/ModelError.md)
 - [Notification](docs/Notification.md)
 - [Order](docs/Order.md)
 - [OrderBookL2](docs/OrderBookL2.md)
 - [Position](docs/Position.md)
 - [Quote](docs/Quote.md)
 - [Settlement](docs/Settlement.md)
 - [Stats](docs/Stats.md)
 - [StatsHistory](docs/StatsHistory.md)
 - [StatsUsd](docs/StatsUsd.md)
 - [Trade](docs/Trade.md)
 - [TradeBin](docs/TradeBin.md)
 - [Transaction](docs/Transaction.md)
 - [User](docs/User.md)
 - [UserCommission](docs/UserCommission.md)
 - [UserPreferences](docs/UserPreferences.md)
 - [UserWithdrawalFees](docs/UserWithdrawalFees.md)
 - [Wallet](docs/Wallet.md)
 

## Bitmex customer support

support@bitmex.com
