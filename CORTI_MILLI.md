# CORTI\_MILLI

CORTI\_MILLI is a payment provider for bank transfer processing in RUB. The payment provider does not require card binding in Whitebird. Payment is processed through the provider banking flow after order creation.

### Available currencies:

* RUB

### Available Bank Card By Region:

* Russia

### Directions & Commission:

* Buy Crypto - not available in current configuration
* Sell Crypto 2 %

## Sell Crypto Flow:

### First step

Get available payment methods for the client

#### POST api/v2/exchange/merchant/payment/method

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
    "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562",
    "fiatAsset": "RUB", // optional (available RUB)
    "orderType": "SELL"  // optional (available SELL)
}
```

**Response**

```jsx
[
    {
        "providerId": "CORTI_MILLI",
        "providerType": "CORTI_MILLI",
        "status": "ENABLED",
        "name": "CORTI_MILLI"
    }
]
```

If `CORTI_MILLI` is not returned (or returned with non-`ENABLED` status), `SELL` via CORTI\_MILLI is not available for the current client/merchant/environment.

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
    <tr><td style="word-break: break-word; white-space: normal;">fiatAsset</td><td>string</td><td>No</td><td>Optional fiat filter (RUB).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">orderType</td><td>string</td><td>No</td><td>Optional direction filter (SELL).</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">providerId</td><td>string</td><td>Provider identifier (CORTI_MILLI).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">providerType</td><td>string</td><td>Provider type.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Availability status (use ENABLED).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">name</td><td>string</td><td>Provider display name.</td></tr>
  </tbody>
</table>

### Second step

Create a quote for the selected CORTI\_MILLI payment method

#### POST api/v2/exchange/merchant/quote

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
  "clientId": "YOUR_CLIENT_ID",
  "fromAsset": {
    "code": "TRX",
    "network": "Tron",
    "amount": "51.204483"
  },
  "toAsset": {
    "code": "RUB",
    "network": null
  },
  "paymentMethod": "CORTI_MILLI",
  "paymentMethodToken": "5058270370320789"
}
```

**Response**

```jsx
{
    "quoteId": "a7e8c187-4e68-4c1f-bb11-0cdfeb43a0a2",
    "fromAsset": {
        "code": "TRX",
        "network": "Tron",
        "amount": "51.204483"
    },
    "toAsset": {
        "code": "RUB",
        "amount": "1284.36"
    },
    "rate": 25.083,
    "plainRate": 25.8587,
    "fee": {
        "total": 39.72,
        "service": null,
        "network": 0,
        "asset": "RUB"
    },
    "expirationDate": "2026-03-31T14:07:05+0000"
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
    <tr><td style="word-break: break-word; white-space: normal;">paymentMethod</td><td>string</td><td>Yes</td><td>Provider type (CORTI_MILLI).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">paymentMethodToken</td><td>string</td><td>Yes</td><td>Payment token for payout route.</td></tr>
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

### Third step

Create a sell order using the created quote.

#### GET api/v2/exchange/merchant/sell?quoteId=420d72d9-715c-41d9-93e8-1d1cb20924e3

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**

```jsx
{
    "id": "78cc592e-cab0-4199-a70a-d0d4aab3ec6c",
    "type": "SELL",
    "status": "PROCESSING",
    "creationDate": "2026-03-31T14:50:42.625369",
    "modificationDate": "2026-03-31T14:50:43.861568",
    "cryptoTransaction": null,
    "expiresAtDate": "2026-03-31T15:20:39.089773",
    "depositCryptoAddress": "TDtMfvu2zafgdwKRhm7JEUqFf3SA9FgRW1"
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
  <tbody><tr><td style="word-break: break-word; white-space: normal;">quoteId</td><td>string (UUID)</td><td>Yes</td><td>Quote identifier used to create sell order.</td></tr></tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">id</td><td>string</td><td>Order identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">type</td><td>string</td><td>Order type (SELL).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Current order status.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">depositCryptoAddress</td><td>string</td><td>Address where client should send crypto.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">expiresAtDate</td><td>string | null</td><td>Order expiration timestamp.</td></tr>
  </tbody>
</table>

### Fourth step

Get order details and provide transfer instructions to the client.

#### GET /api/v2/exchange/merchant/order?orderId=c711b777-5d13-4156-a57d-456cc307626b

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**

```jsx
{
    "id": "78cc592e-cab0-4199-a70a-d0d4aab3ec6c",
    "type": "SELL",
    "status": "PROCESSING",
    "creationDate": "2026-03-31T14:50:42.625369",
    "modificationDate": "2026-03-31T14:50:43.861568",
    "number": 741000003422,
    "exchangeOperation": {
        "inputCurrency": "TRX",
        "inputAsset": 51.204483,
        "outputCurrency": "RUB",
        "outputAsset": 1278.23,
        "exchangeFeeAssetInFiat": 39.53,
        "bonusOutputAsset": null,
        "plainRatio": 25.7352,
        "ratio": 24.9632,
        "currencyPair": {
            "fromCurrency": "TRX",
            "toCurrency": "RUB"
        }
    },
    "cryptoTransaction": {
        "hash": null,
        "externalCryptoAddress": null,
        "internalCryptoAddress": "TDtMfvu2zafgdwKRhm7JEUqFf3SA9FgRW1",
        "fromAddress": null,
        "toAddress": "TDtMfvu2zafgdwKRhm7JEUqFf3SA9FgRW1",
        "status": "NEW",
        "currency": "TRX",
        "fee": null,
        "feePaymentEnabledByClient": false,
        "type": "AUTO",
        "comment": null
    },
    "fiatTransaction": {
        "status": "NEW",
        "paymentToken": "5058270370320789",
        "post": null,
        "brand": null,
        "internalToken": "26226972800000104219",
        "orderIdentity": "47f6f80f-3813-4be1-8402-a5f06d2b5033",
        "link": null,
        "providerType": "CORTI_MILLI",
        "paymentType": null,
        "processingBank": null,
        "resultMessage": null,
        "currency": "RUB"
    },
    "client": {
        "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562"
    },
    "serverDate": "2026-03-31T14:51:52+0000",
    "exchangeType": "BUY",
    "operationType": "CRYPTO_TO_FIAT",
    "orderType": "DEFAULT",
    "completionDate": null,
    "resultMessage": null,
    "submitByResident": null,
    "merchantName": "wb",
    "merchantBonus": null,
    "promoCodeDetails": null,
    "fromSource": "EXT",
    "toSource": "EXT",
    "expiresAtDate": "2026-03-31T15:20:39.089773"
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
  <tbody><tr><td style="word-break: break-word; white-space: normal;">orderId</td><td>string (UUID)</td><td>Yes</td><td>Order identifier returned by sell order creation.</td></tr></tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">id</td><td>string</td><td>Order identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Current order status.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation</td><td>object</td><td>Exchange operation details for this order.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">cryptoTransaction</td><td>object</td><td>Crypto leg details including addresses and status.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fiatTransaction</td><td>object</td><td>Fiat leg details including provider and payout status.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">expiresAtDate</td><td>string | null</td><td>Order expiration timestamp.</td></tr>
  </tbody>
</table>

For this step, the client must send crypto to the deposit address from the order response:

* **Address field:** `cryptoTransaction.toAddress`
* **Network / asset field:** `cryptoTransaction.currency` (in this example: `TRX`, i.e. Tron network)
* **Amount field:** `exchangeOperation.inputAsset` (in this example: `51.204483`)

The transfer must be made in the correct network and before the order expiration time (`expiresAtDate`).

After the transfer is sent, poll `GET /api/v2/exchange/merchant/order?orderId=**78cc592e-cab0-4199-a70a-d0d4aab3ec6c**` until the order reaches a final status (`COMPLETED` or `FAILED`).
