# CORTI\_MILLI

> BASE_URL https://api.dev.wbdevel.net

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
    <tr><td style="word-break: break-word; white-space: normal;">clientId</td><td>string (UUID)</td><td>No</td><td>Client identifier used to scope the request to a specific client.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fiatAsset</td><td>string</td><td>No</td><td>Optional fiat filter (RUB).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">orderType</td><td>string</td><td>No</td><td>Optional direction filter (SELL).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">destination</td><td>string</td><td>No</td><td>Optional flow destination filter. Recommended value: EXCHANGE</td></tr>
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
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Payment method status: CURRENCY_DISABLED, DIRECTION_DISABLED, ENABLED, UNKNOWN.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">name</td><td>string | null</td><td>Provider display name shown to user in payment method lists.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">isRestricted</td><td>boolean | null</td><td>Shows whether this payment method is restricted.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">isCrypto</td><td>boolean | null</td><td>Indicates whether payment method is crypto type.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">country</td><td>string | null</td><td>Country associated with payment method.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">currency</td><td>string | null</td><td>Primary currency associated with payment method when provided.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">supportedCurrencies</td><td>array of strings | null</td><td>List of supported currencies when provided.</td></tr>
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
    <tr><td style="word-break: break-word; white-space: normal;">clientId</td><td>string (UUID)</td><td>No</td><td>Client identifier used to scope the request to a specific client.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fromAsset / toAsset</td><td>object</td><td>Yes</td><td>Input/output asset pair for quote calculation.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">paymentMethod</td><td>string</td><td>No</td><td>Optional provider type (CORTI_MILLI for this route).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">paymentMethodToken</td><td>string</td><td>No</td><td>Optional provider token/reference for selected payout route.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">destinationCryptoAddress</td><td>string</td><td>No</td><td>Destination wallet address for crypto-out flows.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">comment</td><td>string</td><td>No</td><td>Used only for the TON network as transfer memo. For other networks ignored.</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">quoteId</td><td>string</td><td>Quote identifier used for order creation.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fromAsset / toAsset</td><td>object</td><td>Resolved assets and amounts (code/network/amount).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">rate / plainRate</td><td>number</td><td>Final rate and base reference rate.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fee</td><td>object</td><td>Fee breakdown object.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fee.total</td><td>number</td><td>Total fee amount in fee.asset currency.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fee.service</td><td>number | null</td><td>Service fee component in fee.asset currency.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fee.network</td><td>number | null</td><td>Network/payment component in fee.asset currency.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fee.asset</td><td>string</td><td>Asset code in which fee.total, fee.service, and fee.network are expressed.</td></tr>
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

**Parameters**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">quoteId</td><td>string (UUID)</td><td>Yes</td><td>Quote identifier used to create sell order.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">failureDepositAddress</td><td>string</td><td>No</td><td>Refund wallet address used if the order fails before completion.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">sourceAddress</td><td>string</td><td>No</td><td>Sender wallet address for compliance checks.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">bankIdentifier</td><td>string</td><td>No</td><td>Bank identifier if provider route requires bank selection.</td></tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">id</td><td>string</td><td>Order identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">type</td><td>string</td><td>Order type (SELL).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Current order status: NEW, PROCESSING, COMPLETED, EXPIRED, ERROR.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">depositCryptoAddress</td><td>string</td><td>Address where client should send crypto.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">creationDate</td><td>string</td><td>Order creation timestamp in server date-time format.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">modificationDate</td><td>string</td><td>Last order update timestamp in server date-time format.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">cryptoTransaction</td><td>object | null</td><td>Crypto transaction summary object (hash only when present).</td></tr>
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

**Parameters**

<table width="100%">
  <thead><tr><th width="200" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="100">Required</th><th width="580">Description</th></tr></thead>
  <tbody><tr><td style="word-break: break-word; white-space: normal;">orderId</td><td>string (UUID)</td><td>Yes</td><td>Order identifier returned by sell order creation.</td></tr></tbody>
</table>

**Response**

<table width="100%">
  <thead><tr><th width="240" style="word-break: break-word; white-space: normal;">Name</th><th width="120">Type</th><th width="640">Description</th></tr></thead>
  <tbody>
    <tr><td style="word-break: break-word; white-space: normal;">id</td><td>string</td><td>Order identifier.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">type</td><td>string</td><td>Order type.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Current order lifecycle state: NEW, PROCESSING, COMPLETED, EXPIRED, ERROR.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">number</td><td>number</td><td>Sequential order number displayed for business/payment reference.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation</td><td>object</td><td>Exchange side details (input/output, rates, fees).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation.inputCurrency</td><td>string</td><td>Input currency code.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation.inputAsset</td><td>number</td><td>Input amount.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation.outputCurrency</td><td>string</td><td>Output currency code.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation.outputAsset</td><td>number</td><td>Output amount.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation.exchangeFeeAssetInFiat</td><td>number | null</td><td>Exchange fee amount in fiat.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation.bonusOutputAsset</td><td>number | null</td><td>Bonus output amount when applicable.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation.plainRatio</td><td>number</td><td>Base market ratio before route adjustments.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation.ratio</td><td>number</td><td>Final route ratio applied to order.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeOperation.currencyPair</td><td>object</td><td>Pair of from/to currencies for conversion.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">cryptoTransaction</td><td>object</td><td>Crypto transfer details (addresses, hash, status, fee, type, comment).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fiatTransaction</td><td>object</td><td>Fiat processing details (provider, status, payment metadata).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fiatTransaction.orderIdentity</td><td>string | null</td><td>Provider-side order/payment reference when available.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">client</td><td>object</td><td>Order owner details.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">client.clientId</td><td>string</td><td>Client identifier associated with the order.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">serverDate</td><td>string</td><td>Server timestamp for response generation.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">exchangeType</td><td>string</td><td>Exchange direction type.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">operationType</td><td>string</td><td>Operation type (FIAT_TO_CRYPTO / CRYPTO_TO_FIAT).</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">orderType</td><td>string</td><td>Business order subtype.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">creationDate</td><td>string</td><td>Order creation timestamp in server date-time format.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">modificationDate</td><td>string</td><td>Last order update timestamp in server date-time format.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">completionDate</td><td>string | null</td><td>Completion timestamp for finalized orders.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">resultMessage</td><td>string | null</td><td>Result or error message from order processing.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">submitByResident</td><td>boolean | null</td><td>Resident submission flag when applicable.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">merchantName</td><td>string | null</td><td>Merchant display name.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">merchantBonus</td><td>number | null</td><td>Merchant bonus amount applied to order.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">promoCodeDetails</td><td>string | null</td><td>Promocode details when used.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">fromSource</td><td>string | null</td><td>Source type of input side.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">toSource</td><td>string | null</td><td>Source type of output side.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">expiresAtDate</td><td>string | null</td><td>Order expiration timestamp.</td></tr>
  </tbody>
</table>

For this step, the client must send crypto to the deposit address from the order response:

* **Address field:** `cryptoTransaction.toAddress`
* **Network / asset field:** `cryptoTransaction.currency` (in this example: `TRX`, i.e. Tron network)
* **Amount field:** `exchangeOperation.inputAsset` (in this example: `51.204483`)

The transfer must be made in the correct network and before the order expiration time (`expiresAtDate`).

After the transfer is sent, poll `GET /api/v2/exchange/merchant/order?orderId=**78cc592e-cab0-4199-a70a-d0d4aab3ec6c**` until the order reaches a final status (`COMPLETED` or `FAILED`).
