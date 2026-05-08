# CARUSELL

CARUSELL is a payment provider for СБП (Faster Payments System). The payment provider does not require card to be bound, payment is made in the bank's app through which the payment will be processed. Whitebird's connection with the client's bank is via a phone number. A phone number is required to make a payment

### Available currencies:

* RUB

### Available Bank Card By Region:

* Russia

### Directions & Commission:

* Buy Crypto 2,5 %
* Sell Sell 2 %

## Buy Crypto Flow:

### First step

Get available payment methods for the client

#### POST api/v2/exchange/merchant/payment/method

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
    "clientId": "93a8b39b-d883-4de3-9527-d92b1eabe38c",
    "fiatAsset": "RUB", -- optional (available BYN, RUB, USD, EUR)
    "orderType": "BUY"  -- optional (available BUY and SELL)
}
```

**Response**

```jsx
[
    {
        "providerId": "CARUSELL",
        "providerType": "CARUSELL",
        "status": "ENABLED",
        "name": "CARUSELL"
    }
]
```

**Headers**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">x-api-key</td><td>string</td><td>Yes</td><td>Authenticates the merchant server-to-server request.</td></tr></tbody>
</table>

**Request**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">clientId</td><td>string (UUID)</td><td>Yes</td><td>Client identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fiatAsset</td><td>string</td><td>No</td><td>Optional fiat filter (RUB for CARUSELL flows).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">orderType</td><td>string</td><td>No</td><td>Order direction filter (BUY/SELL).</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">providerId</td><td>string</td><td>Provider identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">providerType</td><td>string</td><td>Provider type.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Availability status (use ENABLED).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">name</td><td>string</td><td>Provider display name.</td></tr>
  </tbody>
</table>

### Second step

To create a quota, specify a random uuid in the paymentMethodToken field

#### POST api/v2/exchange/merchant/quote

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
   "clientId": "76b758b0-5603-4419-9c35-c01e5f775269",
   "fromAsset": {
        "code": "RUB",
        "network": null,
        "amount": "2000"
   },
   "toAsset": {
      "code": "TRX",
      "network": "Tron"
   },
   "paymentMethod": "CARUSELL",
   "paymentMethodToken": "generateUUID()"
}
```

**Headers**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">x-api-key</td><td>string</td><td>Yes</td><td>Authenticates the merchant server-to-server request.</td></tr></tbody>
</table>

**Request**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">clientId</td><td>string (UUID)</td><td>Yes</td><td>Client identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fromAsset / toAsset</td><td>object</td><td>Yes</td><td>Input/output asset pair for quote calculation.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">paymentMethod</td><td>string</td><td>Yes</td><td>Provider type (CARUSELL).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">paymentMethodToken</td><td>string</td><td>Yes</td><td>Temporary/random token for CARUSELL quote route.</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">quoteId</td><td>string</td><td>Quote identifier used for order creation.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fromAsset / toAsset</td><td>object</td><td>Resolved assets and amounts.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">rate / plainRate</td><td>number</td><td>Final rate and base reference rate.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fee</td><td>object</td><td>Fee breakdown object.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">expirationDate</td><td>string</td><td>Quote expiration timestamp.</td></tr>
  </tbody>
</table>

**Response**

```jsx
{
    "quoteId": "accd548b-8bbd-4f8a-9285-eebf031fb943",
    "fromAsset": {
        "code": "RUB",
        "amount": "2000"
    },
    "toAsset": {
        "code": "TRX",
        "network": "Tron",
        "amount": "87.312113"
    },
    "rate": 22.9063,
    "plainRate": 22.2666,
    "fee": {
        "total": 50,
        "service": null,
        "network": 0.263,
        "asset": "RUB"
    },
    "expirationDate": "2026-03-04T16:19:05+0000"
}
```

**Headers**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">x-api-key</td><td>string</td><td>Yes</td><td>Authenticates the merchant server-to-server request.</td></tr></tbody>
</table>

**Request**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">quoteId</td><td>string (UUID)</td><td>Yes</td><td>Quote identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">returnUrl</td><td>string</td><td>No</td><td>Redirect URL on success.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">failUrl</td><td>string</td><td>No</td><td>Redirect URL on failure.</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">id</td><td>string</td><td>Order identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">type</td><td>string</td><td>Order type (BUY).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Current order status.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fiatPaymentLink</td><td>string</td><td>Provider payment link for client checkout.</td></tr>
  </tbody>
</table>

### Third step

Create an order specifying returnUrl and failUrl to ensure the client is redirected back to the website after payment

#### GET api/v2/exchange/merchant/buy?quoteId=27690c04-e344-4d31-81aec58296ba6ddc\&returnUrl=https://www.google.com/\&failUrl=https://www.google.com/

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**

```jsx
{
    "id": "53ab15af-31c9-4c8d-8ac8-55f9e353c09e",
    "type": "BUY",
    "status": "PROCESSING",
    "fiatPaymentLink": "https://sandbox.carusell.world/processing/api/v1/test/checkout/payment?payment-id=284949544680468480&status=CAPTURED&settlement-account-id=SA241575994854899712&signature=8f5a61dcdab280b21eb552e510f5c77a5b42f1da0e1c2d0896834328a5e32ad9",
    "createDate": "2026-02-26T11:45:38+0000",
    "modificationDate": "2026-02-26T11:45:38+0000"
}
```

### Fourth step

Open the link where the client selects the bank through which the payment will be made and complete the payment.

## Sell Crypto Flow:

### First step

Get available payment methods for the client

#### POST api/v2/exchange/merchant/payment/method

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
    "clientId": "93a8b39b-d883-4de3-9527-d92b1eabe38c",
    "fiatAsset": "RUB", -- optional (available BYN, RUB, USD, EUR)
    "orderType": "SELL"  -- optional (available BUY and SELL)
}
```

**Response**

```jsx
[
    {
        "providerId": "CARUSELL",
        "providerType": "CARUSELL",
        "status": "ENABLED",
        "name": "CARUSELL"
    }
]
```

### Second step

To create a quota, specify a random uuid in the paymentMethodToken field

#### POST api/v2/exchange/merchant/quote

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
   "clientId": "76b758b0-5603-4419-9c35-c01e5f775269",
   "fromAsset": {
        "code": "TRX",
        "network": "Tron"
   },
   "toAsset": {
        "code": "RUB",
        "network": null,
        "amount": "2000"
   },
   "paymentMethod": "CARUSELL",
   "paymentMethodToken": "generateUUID()"
}
```

**Response**

```jsx
{
    "quoteId": "a4c95ffd-e38d-419f-b1be-833a9293bd23",
    "fromAsset": {
        "code": "TRX",
        "network": "Tron",
        "amount": "91.717713"
    },
    "toAsset": {
        "code": "RUB",
        "amount": "2000"
    },
    "rate": 21.806,
    "plainRate": 22.2511,
    "fee": {
        "total": 40.82,
        "service": null,
        "network": 0,
        "asset": "RUB"
    },
    "expirationDate": "2026-03-04T16:40:53+0000"
}
```

### Third step

Get the list of available banks for transfers via **CARUSELL**

#### GET https://carusell-service.dev.wbdevel.net/api/v1/bank

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**

```jsx
{
    "id": "100000000118",
    "description": "AGROPROMKREDIT",
    "active": true
},
{
    "id": "200000000006",
    "description": "AK BARS BANK",
    "active": true
}
...
```

**Headers**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">x-api-key</td><td>string</td><td>Yes</td><td>Header shown in example; endpoint is external CARUSELL service.</td></tr></tbody>
</table>

**Request**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">body</td><td>none</td><td>-</td><td>GET endpoint without request body.</td></tr></tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">id</td><td>string</td><td>Bank identifier used as bankIdentifier in sell request.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">description</td><td>string</td><td>Bank display name.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">active</td><td>boolean</td><td>Availability flag.</td></tr>
  </tbody>
</table>

The client chooses the bank to which the payment will be made

### Fourth step

Create an order with the bankIdentifier selected by the client

#### GET /api/v2/exchange/merchant/sell?quoteId=27690c04-e344-4d31-81ae-c58296ba6ddc\&bankIdentifier=100000000118

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**

```jsx
{
    "id": "53ab15af-31c9-4c8d-8ac8-55f9e353c09e",
    "type": "SELL",
    "status": "PROCESSING",
    "depositCryptoAddress": "TYPAe4vUYtUAKgrRt7iUc2fLuVhRHfCiJG",
    "createDate": "2026-02-26T11:45:38+0000",
    "modificationDate": "2026-02-26T11:45:38+0000"
}
```

**Headers**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">x-api-key</td><td>string</td><td>Yes</td><td>Authenticates the merchant server-to-server request.</td></tr></tbody>
</table>

**Request**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">quoteId</td><td>string (UUID)</td><td>Yes</td><td>Quote identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">bankIdentifier</td><td>string</td><td>Yes</td><td>Bank id selected from CARUSELL bank list.</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">id</td><td>string</td><td>Order identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">type</td><td>string</td><td>Order type (SELL).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Current order status.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">depositCryptoAddress</td><td>string</td><td>Address where client should send crypto.</td></tr>
  </tbody>
</table>

For the test environment, it does not matter which bank is selected (bankIdentifier) — the payment will be processed successfully in any case.

### **CARUSELL** also has an anti-fraud check. It is performed automatically:

* At the moment of order creation for the crypto-to-fiat flow
* At the moment of order execution for the fiat-to-crypto flow

If an order fails to be created or returns an error at any stage, there is a possibility that the anti-fraud system has declined the transaction.
