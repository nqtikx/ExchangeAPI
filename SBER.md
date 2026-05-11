# SBER

> BASE_URL https://api.dev.wbdevel.net

SBER is a payment provider for RUB operations. In merchant configuration it can be routed via CA-type payment system.

### Available currencies:

* RUB

### Available Bank Card By Region:

* Russia

### Directions & Commission:

* Buy Crypto 1.5%
* Sell Crypto - not available in current configuration

### Verify that the payment provider is available

#### POST api/v2/exchange/merchant/payment/provider

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
    "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562",
    "fiatAsset": "RUB", // optional (available RUB)
    "orderType": "BUY" // optional (available BUY)
}
```

**Response**

```jsx
{
    "id": "SBER",
    "name": "SBER",
    "addPaymentMethod": false,
    "config": {
        "paymentSystems": [
            {
                "paymentSystem": "MIR",
                "type": "CA",
                "directions": [
                    {
                        "direction": "BUY",
                        "currencies": [
                            {
                                "currency": "RUB",
                                "banks": [
                                    "Sber Bank"
                                ]
                            }
                        ]
                    }
                ]
            }
        ]
    }
}
```

It is sufficient to verify that the payment provider is available via the `id` field. `id = SBER`.

**Headers**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">x-api-key</td><td>string</td><td>Yes</td><td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td></tr></tbody>
</table>

**Request**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">clientId</td><td>string (UUID)</td><td>No</td><td>Client identifier used to scope the request to a specific client.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fiatAsset</td><td>string</td><td>No</td><td>Optional fiat asset filter.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">orderType</td><td>string</td><td>No</td><td>Optional order direction filter. For SBER flow BUY is expected.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">destination</td><td>string</td><td>No</td><td>Optional flow destination filter. Recommended value: EXCHANGE.</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">id</td><td>string</td><td>Provider identifier. For this flow expected value is SBER.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">name</td><td>string</td><td>Provider display name.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">addPaymentMethod</td><td>boolean</td><td>Defines whether provider supports adding payment methods.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">config</td><td>object</td><td>Provider routing configuration.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">config.paymentSystems</td><td>array of objects</td><td>Payment systems list for provider.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">config.paymentSystems[].paymentSystem</td><td>string | null</td><td>Payment system for selected route.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">config.paymentSystems[].type</td><td>string</td><td>Provider channel type.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].direction</td><td>string</td><td>Direction for route (BUY/SELL).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies[].currency</td><td>string</td><td>Supported fiat currency.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies[].banks</td><td>array of strings</td><td>Supported banks list for selected route.</td></tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Provided clientId is invalid or not linked to merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

## Buy Crypto Flow:

### First step

Get available payment methods for the client

#### POST api/v2/exchange/merchant/payment/method

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
    "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562",
    "fiatAsset": "RUB",
    "orderType": "BUY",
    "destination": "EXCHANGE"
}
```

**Response**

```jsx
[
    {
        "providerId": "SBER",
        "providerType": "SBER",
        "status": "ENABLED",
        "name": "SBER"
    }
]
```

If `SBER` is not returned (or returned with non-`ENABLED` status), `BUY` via `SBER` is not available for the current client/merchant/environment.

**Headers**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">x-api-key</td><td>string</td><td>Yes</td><td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td></tr></tbody>
</table>

**Request**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">clientId</td><td>string (UUID)</td><td>No</td><td>Client identifier used to scope the request to a specific client.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fiatAsset</td><td>string</td><td>No</td><td>Optional fiat asset filter.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">orderType</td><td>string</td><td>No</td><td>Optional order direction filter. For SBER flow BUY is expected.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">destination</td><td>string</td><td>No</td><td>Optional flow destination filter. Recommended value: EXCHANGE.</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">id</td><td>string | null</td><td>Payment method token. For provider-level routes may be null.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">number</td><td>string | null</td><td>Masked card/account number when available.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">brand</td><td>string | null</td><td>Card/payment brand when available.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">providerId</td><td>string</td><td>Payment provider identifier used in integrations and filters (for example ASSIST, CA, MTS).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">providerType</td><td>string</td><td>Provider category/type returned by provider integration. Usually matches providerId for standard routes.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Availability status: CURRENCY_DISABLED, DIRECTION_DISABLED, ENABLED, UNKNOWN.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">name</td><td>string | null</td><td>Provider display name shown to user in payment method lists.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">isRestricted</td><td>boolean | null</td><td>Shows whether this payment method is restricted.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">isCrypto</td><td>boolean | null</td><td>Indicates whether the payment method is crypto type.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">country</td><td>string | null</td><td>Country associated with payment method.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">currency</td><td>string | null</td><td>Primary currency associated with payment method when provided.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">supportedCurrencies</td><td>array of strings | null</td><td>List of supported currencies when provided.</td></tr>
  </tbody>
</table>

**Payment method status values**

<table width="100%">
  <thead>
    <tr>
      <th width="220" style="word-break: break-word; white-space: normal;">Status</th>
      <th width="680">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">ENABLED</td>
      <td>Payment method can be used for the selected flow, direction, and currency.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">DIRECTION_DISABLED</td>
      <td>Payment method exists, but is not available for selected orderType.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">CURRENCY_DISABLED</td>
      <td>Payment method exists, but does not support selected fiatAsset.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">UNKNOWN</td>
      <td>Status cannot be resolved because required filters were not provided.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Provided clientId is invalid or not linked to merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation or client scope.</td>
    </tr>
  </tbody>
</table>

### Second step

Create a deposit for the selected SBER payment method

#### POST api/v2/exchange/merchant/balance/fiat/deposit

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
  "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562",
  "accountType": "WALLET",
  "fiatProviderType": "SBER",
  "paymentToken": "de7ea37e-beff-49e4-8064-2b4cd1ae0c60",
  "asset": {
    "code": "RUB",
    "amount": "1299.77"
  }
}
```

**Response**

```jsx
{
    "fiatPaymentLink": null,
    "creationDate": "2026-04-02T12:34:54+0000",
    "expirationMinutes": 30,
    "paymentDetails": {
        "paymentLink": null,
        "notificationPhoneNumber": "375295805525"
    }
}
```

**Headers**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">x-api-key</td><td>string</td><td>Yes</td><td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td></tr></tbody>
</table>

**Request**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">clientId</td><td>string (UUID)</td><td>No</td><td>Client identifier used to scope operation to a specific client.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">accountType</td><td>string</td><td>Yes</td><td>Client account type. Allowed value: WALLET</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fiatProviderType</td><td>string</td><td>Yes</td><td>Fiat provider type. For this flow expected value is SBER.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">paymentToken</td><td>string</td><td>No*</td><td>Payment token/reference for provider route. At least one token field is expected by validation.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">internalToken</td><td>string</td><td>No*</td><td>Internal payment token alternative to paymentToken.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">asset.code</td><td>string</td><td>Yes</td><td>Fiat asset code, for this flow RUB.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">asset.amount</td><td>number</td><td>No</td><td>Requested amount in selected fiat asset.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">tradingAccountId</td><td>string | null</td><td>No</td><td>Trading account id when accountType is TRADING.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">returnUrl</td><td>string | null</td><td>No</td><td>Redirect URL on successful payment flow if applicable.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">failUrl</td><td>string | null</td><td>No</td><td>Redirect URL on failed payment flow if applicable.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">bankIdentifier</td><td>string | null</td><td>No</td><td>Bank identifier when provider route requires bank selection.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">commissionType</td><td>string | null</td><td>No</td><td>Commission mode for balance operation when configured.</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">fiatPaymentLink</td><td>string | null</td><td>Deprecated top-level link field kept for backward compatibility.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">creationDate</td><td>string</td><td>Deposit operation creation timestamp.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">expirationMinutes</td><td>number</td><td>Time to complete payment before expiration.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">paymentDetails.paymentLink</td><td>string | null</td><td>Payment link for provider flow when available.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">paymentDetails.notificationPhoneNumber</td><td>string | null</td><td>Phone number where push notification is sent for payment confirmation.</td></tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="266" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="614">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 INVALID_PAYMENT_TOKEN</td>
      <td>BUSINESS</td>
      <td>Payment token is invalid, unavailable, or unsupported.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation.</td>
    </tr>
  </tbody>
</table>

### Third step

In production flow, after SBER deposit creation the client must complete payment in the **SberBank mobile app**:

![Снимок экрана 2026-04-02 в 16.07.56.png](.gitbook/assets/sber.png)

**“A push notification has been sent to phone `+375295805525`. Open the SberBank mobile app and complete payment.”**

If push is not received, use fallback path in app:

**“Select the account payment card above the wallet and complete payment there.”**

After confirmation in Sber app, keep polling merchant balance operation status (**fourth step**) until final state (`PROCESSED` / `DECLINED`).

### Fourth step

Check current fiat operation status for this client.

#### POST api/v2/exchange/merchant/balance/operation?page=0\&size=10\&sort=creationDate,desc

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
  "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562"
}
```

**Response**

```jsx
{
  "content": [
    {
      "id": "71ef83c6-4fee-413f-ab25-2e0a29a2b9f9",
      "transactionId": "99922b6c-72e8-48a3-9886-a5191e88e96f",
      "number": "4040",
      "type": "DEPOSIT",
      "status": "PROCESSED",
      "post": null,
      "providerType": null,
      "paymentSystem": null,
      "transactionHash": "b2febc6cc62b854dd342657386c9a536cecffac9ef63762eabacaeafbc220487",
      "externalCryptoAddress": "TCT2pKJXo233hrKWQMeCptC8My1KGvtsU4",
      "asset": "TRX",
      "amount": "50",
      "requestedAmount": "50",
      "grossAmount": "50",
      "netAmount": "50",
      "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562",
      "userId": "86c2b12b-a332-49ad-a447-d02c0b621dc4",
      "creationDate": "2026-05-06T07:36:11+0000",
      "completionDate": "2026-05-06T07:37:09+0000"
    }
  ]
}
```

**Headers**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">x-api-key</td><td>string</td><td>Yes</td><td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td></tr></tbody>
</table>

**Parameters**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">page</td><td>number</td><td>No</td><td>Zero-based page index.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">size</td><td>number</td><td>No</td><td>Page size.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">sort</td><td>string</td><td>No</td><td>Sort expression, for example <nobr>creationDate,desc</nobr>.</td></tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">clientId</td><td>string (UUID)</td><td>No</td><td>Client identifier filter for merchant operations.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">userId</td><td>string | null</td><td>No</td><td>User identity filter.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">transactionTypes</td><td>array of strings | null</td><td>No</td><td>Operation type filter. Enum: DEPOSIT, WITHDRAWAL.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">transactionStatuses</td><td>array of strings | null</td><td>No</td><td>Status filter. Supported values for API-level filtering: PROCESSING, DECLINED, PROCESSED.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">currencies</td><td>array of strings | null</td><td>No</td><td>Currency filter list.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">accountTypes</td><td>array of strings | null</td><td>No</td><td>Account type filter. Allowed value: WALLET.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">creationDateFrame</td><td>object | null</td><td>No</td><td>Creation date range filter.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">completionDateFrame</td><td>object | null</td><td>No</td><td>Completion date range filter.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">balanceOperationId</td><td>string | null</td><td>No</td><td>Specific balance operation id filter.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">from / to</td><td>string (date) | null</td><td>No</td><td>Deprecated date filters kept for backward compatibility.</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">content[]</td><td>array of objects</td><td>List of balance operations.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].id</td><td>string</td><td>Balance operation identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].transactionId</td><td>string | null</td><td>Underlying transaction identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].number</td><td>string | null</td><td>Human-readable operation number.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].type</td><td>string</td><td>Operation type. Enum: DEPOSIT, WITHDRAWAL.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].status</td><td>string</td><td>Operation status. Main values used in this flow: PROCESSING, DECLINED, PROCESSED.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].post</td><td>string | null</td><td>Provider post/terminal identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].providerType</td><td>string | null</td><td>Payment provider type enum (PaymentProviderType): ASSIST, CA, MTS</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].paymentSystem</td><td>string | null</td><td>Payment system identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].transactionHash</td><td>string | null</td><td>Blockchain hash when operation has crypto leg.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].externalCryptoAddress</td><td>string | null</td><td>External crypto address linked to operation.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].asset</td><td>string</td><td>Operation asset code.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].amount</td><td>number (deprecated)</td><td>Deprecated amount field.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].requestedAmount</td><td>number</td><td>Requested amount.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].grossAmount</td><td>number</td><td>Gross amount before deductions.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].netAmount</td><td>number</td><td>Net amount after deductions.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].clientId</td><td>string</td><td>Client identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].userId</td><td>string | null</td><td>User identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].creationDate</td><td>string</td><td>Operation creation timestamp.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">content[].completionDate</td><td>string | null</td><td>Operation completion timestamp.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">pageable / totalElements / totalPages / size / number</td><td>object + numbers</td><td>Standard Spring Page metadata.</td></tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Provided clientId is invalid or not linked to merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this operation.</td>
    </tr>
  </tbody>
</table>
