# Shift-GraphQl-Queries
Document with examples of often used graphql requests

### 1. Question: How to get list of instruments on exchange?

### Answer:

````graphql
query {
  instruments {
    name
    instrument_id
    base_currency_id
    quote_currency_id
    price_decimals
    min_quantity
    max_quantity
    min_quote_quantity
    max_quote_quantity
    is_active
    trading_fees {
      fee_group_id
      maker_progressive
      taker_progressive
      maker_flat
      taker_flat
    }
    price {
      ask
      bid
      price_24h_change
      ts_iso
    }
  }
}
````

### 2. Question: How to get my current permissions?
### Answer:

```graphql
query {
  permissions
}
```
### Response:

```json
{
  "data": {
    "permissions": [
      "accounts",
      "accounts_balances",
      "account_transactions",
      "create_account_transaction",
      "api_keys",
      "create_api_key",
      "update_api_key",
      "delete_api_key",
      "cognito_pools",
      "create_cognito_pool",
      "update_cognito_pool",
      "delete_cognito_pool",
      "conversion_quotes",
      "create_conversion_quote",
      "conversions",
      "create_conversion_order",
      "currencies",
      "create_currency",
      "update_currency",
      "delete_currency",
      "fees_groups",
      "create_fee_group",
      "update_fee_group",
      "delete_fee_group",
      "payments_fees",
      "create_payment_fee",
      "update_payment_fee",
      "delete_payment_fee",
      "trading_fees",
      "create_trading_fee",
      "update_trading_fee",
      "delete_trading_fee",
      "instruments_strategies",
      "create_instrument_strategy",
      "update_instrument_strategy",
      "delete_instrument_strategy",
      "instruments",
      "create_instrument",
      "update_instrument",
      "delete_instrument",
      "limits_groups",
      "create_limit_group",
      "update_limit_group",
      "delete_limit_group",
      "payments_limits",
      "create_payment_limit",
      "update_payment_limit",
      "delete_payment_limit",
      "payments_routes",
      "create_payment_route",
      "update_payment_route",
      "delete_payment_route",
      "create_order",
      "cancel_order",
      "estimate_order",
      "open_orders",
      "closed_orders",
      "hedging_orders",
      "trades",
      "hedging_adapters",
      "create_hedging_adapter",
      "update_hedging_adapter",
      "delete_hedging_adapter",
      "payments",
      "approve_payments",
      "create_withdrawal_crypto",
      "create_withdrawal_fiat",
      "deposit_bank_details_fiat",
      "deposit_addresses_crypto",
      "update_payment_approval_status",
      "system_settings",
      "update_system_settings",
      "upload_user_document",
      "users",
      "update_user",
      "create_user",
      "permissions_share",
      "create_permissions_share",
      "delete_permissions_share",
      "notification_templates",
      "update_notification_template",
      "create_kyc_sum_and_substance_token",
      "create_kyc_prime_trust_token",
      "estimate_network_fee",
      "webhooks",
      "create_webhook",
      "update_webhook",
      "delete_webhook",
      "liquidity_report",
      "daily_balances_report"
    ]
  }
}
```

### 3. Question: How to create admin API key to be used for server to server calls?

### Answer:

```graphql
mutation {
  create_api_key(
    name: "Cirus API key 1"
    expires_at: "2050-01-01 00:00:00"
    is_active: on
    permissions: [
      accounts
      accounts_balances
      account_transactions
      create_account_transaction
      conversion_quotes
      create_conversion_quote
      conversions
      create_conversion_order
      currencies
      fees_groups
      payments_fees
      trading_fees
      instruments
      limits_groups
      payments_limits
      payments_routes
      create_order
      cancel_order
      estimate_order
      open_orders
      closed_orders
      trades
      payments
      create_withdrawal_crypto
      create_withdrawal_fiat
      upload_user_document
      update_user
      create_kyc_sum_and_substance_token
      create_kyc_prime_trust_token
      deposit_addresses_crypto
    ]
  ) {
    api_key_id
    api_key_secret
    expires_at
  }
}
```

### Response:

```json
{
  "data": {
    "create_api_key": {
      "api_key_id": "****",
      "api_key_secret": "****",
      "expires_at": "2050-01-01 00:00:00"
    }
  }
}
```