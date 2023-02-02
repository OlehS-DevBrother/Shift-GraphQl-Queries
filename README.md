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

### 2. Question: How to create a new instrument?

### Answer:

````graphql
mutation {
  create_instrument(
    instrument_id: "new_instrument_id"
    name: "test instrument name"
    base_currency_id: "TEST1"
    quote_currency_id: "TEST2"
    price_decimals: 5
    min_quantity: 10
    max_quantity: 1000
    min_quote_quantity: 20
    max_quote_quantity: 2000
    is_active: on
  ) {
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
    price_bars {
      ts
      close
      price_24h_change
      open
      low
      volume_to
      volume_from
    }
    recent_price_bar(periodicity: hour) {
      instrument_id
      ts_iso
      ts
    	close
      price_24h_change
      open
      low
      volume_to
      volume_from
    }
    trading_fees {
      instrument_id
      fee_group_id
      maker_progressive
      taker_progressive
    }
    price {
      ts
      ask
      price_24h_change
    }
  }
}
````
### Response:

```json
{
  "data": {
    "create_instrument": {
      "name": "test instrument name",
      "instrument_id": "new_instrument_id",
      "base_currency_id": "TEST1",
      "quote_currency_id": "TEST2",
      "price_decimals": 5,
      "min_quantity": 10,
      "max_quantity": 1000,
      "min_quote_quantity": 20,
      "max_quote_quantity": 2000,
      "is_active": "on",
      "price_bars": [],
      "recent_price_bar": null,
      "trading_fees": {
        "instrument_id": "new_instrument_2",
        "fee_group_id": "default",
        "maker_progressive": 0,
        "taker_progressive": 0
      },
      "price": null
    }
  }
}
```

### 3. Question: How to get my current permissions?
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

### 4. Question: How to create admin API key to be used for server to server calls?

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

### 5. Question: How to create transaction for user account?

### Answer:

```graphql
mutation {
  create_account_transaction(
    items: [
      {
        user_id: "xxxxx-user_id"
        currency_id: "BTC"
        type: credit
        amount: 0.0001
        transaction_class: manual
        comment: "Credit balance from exchange XXX"
      }
    ]
  ) {
    parent_transaction_id
  }
}
```

#### Note: 
Every transaction request receives an array of items. Each one of them is the certain operation on user account.
All the items are being executed inside of the same transaction, so the failure of one operation provokes the rollback of the whole transaction.

#### Example: 

```graphql
mutation {
  create_account_transaction(
    items: [
      {
        user_id: "xxxxx-sender_id"
        currency_id: "BTC"
        type: credit
        amount: 0.0010
        transaction_class: manual
        comment: "Credit balance from sender (payment body)"
      },
      {
        user_id: "xxxxx-sender_id"
        currency_id: "BTC"
        type: credit
        amount: 0.0001
        transaction_class: fee
        comment: "Credit balance from sender (payment fee)"
      },
      {
        user_id: "fees-beneficiary-user-id"
        currency_id: "BTC"
        type: debit
        amount: 0.0001
        transaction_class: fee
        comment: "Debit balance of commission receiver (payment fee)"
      },
      {
        user_id: "xxxxx-receiver_id"
        currency_id: "BTC"
        type: debit
        amount: 0.0010
        transaction_class: manual
        comment: "Debit balance of payment receiver (payment body)"
      }
    ]
  ) {
    parent_transaction_id
  }
}
```

#### Note: 
Amount of accounts allowed to receive commission payments is limited, those are called beneficiary accounts;
To find the user id you need to send the commission fee can be found by executing request for required fee group.

To complete this you need to get the value of your account's `fee_group_id` . It's `default` for ordinary case;
Than just complete the similar request:

```graphql
query {
  fees_groups(fee_group_id: "default") {
    fee_group_id
    beneficiary_user_id
    trading_fees {
      fee_group_id
      instrument_id
    }
    description
    payment_fees {
      fee_group_id
      deposit_flat_fee
    }
  }
}
```

`beneficiary_user_id` - is the required field for commission receiver id.

### Response:

```json
{
  "data": {
    "create_account_transaction": {
      "parent_transaction_id": "94d43616-be73-4638-8b86-1d8abb4b947f"
    }
  }
}
```

### 5. Question: How to get all open orders for user account?

### Answer:

```graphql
query {
  open_orders(user_id: "xxxxx-user_id") {
    serial_id
    order_id
    client_order_id
    time_in_force
    type
    side
    status
    message
    version
    expires_at
    expires_at_iso
    updated_at
    updated_at_iso
    instrument_id
    instrument_strategy_id
  }, 
}
```

### Response:

```json
{
  "data": {
    "open_orders": []
  }
}
```

### 6. Question: How to verify two-factor authentication token?

### Answer:

```graphql
mutation {
  verify_user_mfa_token(
      token: "xxxxx_token_xxxxx"
    )
}
```

### Response:

```json
{
  "data": {
    "verify_user_mfa_token": true
  }
}
```

### 7. Question: How to count estimate order price?

### Answer:

```graphql
query {
  estimate_order(
    source_currency_id: "USD"
    target_currency_id: "BTC"
    price: null
    target_currency_amount: 0.01711615
  ) {
    type,
    instrument {
      name
      is_active
      instrument_id
      min_quantity
      max_quantity
    },
    time_in_force,
    side
    price,
    quantity_mode,
    quantity,
    fees {
      amount
    }
  }
}
```

### Response:

```json
{
  "data":
  {
    "estimate_order":
      {
        "type": "market",
        "price": 23369.54,
        "quantity": 0.01711615,
        "side": "buy",
        "quantity_mode": "base",
        "instrument":
          {
            "instrument_id": "BTCUSD"
          },
        "fees":
          [
            {
              "currency_id": "BTC",
              "amount": 0.00017116
            }
          ]
       }  
    }
  }
```

### 8. Question: How to calculate and return fee for current user, payment type and currency?

### Answer:

```graphql
mutation {
  estimate_network_fee(currency_id: "BTC", network: "default") {
    low {
      fee_per_byte
      gas_price
      network_fee
      base_fee
      priority_fee
    }
    medium {
      fee_per_byte
      gas_price
      network_fee
      base_fee
      priority_fee
    }
    high {
      fee_per_byte
      gas_price
      network_fee
      base_fee
      priority_fee
    }
  }
}
```

### Response:

```json
{
  "data": {
    "estimate_network_fee": {
      "low": {
        "fee_per_byte": 7,
        "gas_price": 43,
        "network_fee": 7,
        "base_fee": 81,
        "priority_fee": 62
      },
      "medium": {
        "fee_per_byte": 89,
        "gas_price": 33,
        "network_fee": 84,
        "base_fee": 35,
        "priority_fee": 72
      },
      "high": {
        "fee_per_byte": 99,
        "gas_price": 66,
        "network_fee": 17,
        "base_fee": 17,
        "priority_fee": 97
      }
    }
  }
}
```

### 9. Question: How to get information about all available fee groups?

### Answer:

```graphql
query {
  fees_groups {
    fee_group_id
    name
    description
    beneficiary_user_id
  }
}
```

##### Note: For admins only

### Response:

```json
{
  "data": {
    "fees_groups": [
      {
        "fee_group_id": "9676ff1c-931e-4747-a608-55a0fb514ea1",
        "name": "QA Fee Update",
        "description": "Test Update",
        "beneficiary_user_id": "fees-beneficiary-user-id"
      },
      {
        "fee_group_id": "kyc-3-fee-group",
        "name": "KYC-3 Fee Group",
        "description": "KYC-3 fee group",
        "beneficiary_user_id": "fees-beneficiary-user-id"
      },
      {
        "fee_group_id": "kyc-2-fee-group",
        "name": "KYC-2 Fee Group",
        "description": "KYC-2 fee group",
        "beneficiary_user_id": "fees-beneficiary-user-id"
      },
      {
        "fee_group_id": "kyc-1-fee-group",
        "name": "KYC-1 Fee Group",
        "description": "KYC-1 fee group",
        "beneficiary_user_id": "fees-beneficiary-user-id"
      },
      {
        "fee_group_id": "default",
        "name": "Default Fee Group",
        "description": "Default fee group for all new users",
        "beneficiary_user_id": "fees-beneficiary-user-id"
      }
    ]
  }
}
```

### 10. Question: How to create new fee group?

### Answer:

```graphql
mutation {
  create_fee_group(name: "test-fee-group", description: "Test fee group") {
    fee_group_id
    name
    description
    beneficiary_user_id
  }
}
```

##### Note: For admins only

### Response:

```json
{
  "data": {
    "create_fee_group": {
      "fee_group_id": "44bcc50e-9101-41b5-8e28-4cd941ef858e",
      "name": "test-fee-group",
      "description": "Test fee group",
      "beneficiary_user_id": null
    }
  }
}
```

### 11. Question: How to update fee group and assign beneficiary user?

### Answer:

```graphql
mutation {
  update_fee_group(
    fee_group_id: "44bcc50e-9101-41b5-8e28-4cd941ef858e"
    name: "test-fee-group", 
    description: "Test fee group",
    beneficiary_user_id: "6c19360a-c0a1-4213-8f6a-28900a8507e6"
  )
}
```

##### Note: For admins only

### Response:

```json
{
  "data": {
    "update_fee_group": true
  }
}
```

### 12. Question: How to delete fee group?

### Answer:

```graphql
mutation {
  delete_fee_group(fee_group_id: "44bcc50e-9101-41b5-8e28-4cd941ef858e")
}
```

##### Note: For admins only

### Response:

```json
{
  "data": {
    "delete_fee_group": true
  }
}
```

### 13. Question: How to get the profile data for certain user?

### Answer:

#### For trader role:

```graphql
query {
  user {
    email
    username
    user_id
    language
    parent_user_id
    integer_tracking_id
    username
    email
    mobile_nr
    language
    timezone
    primary_market_currency
    is_active
    first_name
    last_name
    address_country
    address_state
    address_city
    address_line_1
    address_line_2
    address_zip
    date_of_birth
    fee_group_id
    limit_group_id
    kyc_level
    kyc_status
    kyc_message
    created_at 
    mfa_for_withdraw
    updated_at
    version
    fee_group {
      name
      description
    }
    limit_group {
      name
      description
      limit_group_id
    }
    favorite_instruments
    notifications_settings
    favorite_addresses_crypto {
      address
      name
    }
    favorite_fiat_destinations {
      name
      bank_address
    }
    profile_pic_url
    passport_url
    national_identity_url
    driver_license_url
    birth_certificate_url
    bank_statement_url
    mfa_status
    utility_bill_url
    parent_user {
      user_id
    }
    created_at_iso
    updated_at_iso
    crypto_pay
  }
}
```

### Response:

```json
{
  "data": {
    "user": {
      "email": null,
      "username": "oleg-test-trader",
      "user_id": "7766ad4a-57eb-4934-83ff-5757ab3ed276",
      "language": "english",
      "parent_user_id": null,
      "integer_tracking_id": 9969132823,
      "mobile_nr": null,
      "timezone": null,
      "primary_market_currency": "USD",
      "is_active": "on",
      "first_name": null,
      "last_name": null,
      "address_country": null,
      "address_state": null,
      "address_city": null,
      "address_line_1": null,
      "address_line_2": null,
      "address_zip": null,
      "date_of_birth": null,
      "fee_group_id": "default",
      "limit_group_id": "default",
      "kyc_level": null,
      "kyc_status": null,
      "kyc_message": null,
      "created_at": "2023-01-31 09:51:18",
      "mfa_for_withdraw": "on",
      "updated_at": "2023-01-31 10:32:41",
      "version": 0,
      "fee_group": {
        "name": "Default Fee Group",
        "description": "Default fee group for all new users"
      },
      "limit_group": {
        "name": "Default Limit Group",
        "description": "Default limit group for all new users",
        "limit_group_id": "default"
      },
      "favorite_instruments": [],
      "notifications_settings": [],
      "favorite_addresses_crypto": [],
      "favorite_fiat_destinations": [],
      "profile_pic_url": null,
      "passport_url": null,
      "national_identity_url": null,
      "driver_license_url": null,
      "birth_certificate_url": null,
      "bank_statement_url": null,
      "mfa_status": "off",
      "utility_bill_url": null,
      "parent_user": null,
      "created_at_iso": "2023-01-31T09:51:18+00:00",
      "updated_at_iso": "2023-01-31T10:32:41+00:00",
      "crypto_pay": "on"
    }
  }
}
```
### 14. Question: How to get deposit address details for crypto deposits?

### Answer:

```graphql
query {
  deposit_address_crypto(
    currency_id: "BTC"
    network: "default"
  ) {
        deposit_address_crypto_id
        user_id
        currency_id
        address
        address_tag_type
        address_tag_value
        network
        psp_service_id
        reference
        created_at
        updated_at
    }
}
```

### Response:

```json
{
  "data": {
    "deposit_address_crypto": {
      "deposit_address_crypto_id": "95f3d75a-120d-4754-b8fe-ef1f7682858f",
      "user_id": "6c19360a-c0a1-4213-8f6a-28900a8507e6",
      "currency_id": "BTC",
      "address": "dGiHWBPPjYg1uCinJjoGXu9kKt2CjemGTU",
      "address_tag_type": null,
      "address_tag_value": "",
      "network": "default",
      "psp_service_id": "SANDBOX",
      "reference": null,
      "created_at": "2023-02-02 09:56:56",
      "updated_at": "2023-02-02 09:56:56"
    }
  }
}
```