# VTB

> BASE_URL https://api.dev.wbdevel.net

VTB is a payment provider for bank transfer processing in RUB. The payment provider does not require card binding in Whitebird. Payment is processed through the provider banking flow after order creation.

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
        "providerId": "VTB",
        "providerType": "VTB",
        "status": "ENABLED",
        "name": "VTB"
    }
]
```

If `VTB` is not returned (or returned with non-`ENABLED` status), `SELL` via `VTB` is not available for the current client/merchant/environment.

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
    <tr><td style="word-break: break-word; white-space: normal;">status</td><td>string</td><td>Payment method status: CURRENCY_DISABLED, DIRECTION_DISABLED, ENABLED, UNKNOWN.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">name</td><td>string | null</td><td>Provider display name shown to user in payment method lists.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">isRestricted</td><td>boolean | null</td><td>Shows whether this payment method is restricted.</td></tr>
    <tr><td style="word-break: break-word; white-space: normal;">isCrypto</td><td>boolean | null</td><td>Indicates whether payment method is crypto type.</td></tr>
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
      <td>Merchant has no permission for this operation.</td>
    </tr>
  </tbody>
</table>

### Second step

Create a quote for the selected VTB payment method

#### POST api/v2/exchange/merchant/quote

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
  "clientId": "3e1469fa-8d35-441c-87b1-a007aeba2562",
  "fromAsset": {
    "code": "TRX",
    "network": "Tron",
    "amount": "50.629578"
  },
  "toAsset": {
    "code": "RUB",
    "network": null
  },
  "paymentMethod": "VTB"
}
```

**Response**

```jsx
{
    "quoteId": "a66199ab-da47-469b-ae75-721f040d5f17",
    "fromAsset": {
        "code": "TRX",
        "network": "Tron",
        "amount": "50.629578"
    },
    "toAsset": {
        "code": "RUB",
        "amount": "1299.77"
    },
    "rate": 25.6721,
    "plainRate": 26.1961,
    "fee": {
        "total": 26.53,
        "service": null,
        "network": 0,
        "asset": "RUB"
    },
    "expirationDate": "2026-03-31T10:15:07+0000"
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
    <tr><td style="word-break: break-word; white-space: normal;">paymentMethod</td><td>string</td><td>No</td><td>Optional provider type (VTB for this route).</td></tr>
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
      <td style="word-break: break-word; white-space: normal;">400 INVALID_QUOTE</td>
      <td>BUSINESS</td>
      <td>Quote cannot be calculated for provided values.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CURRENCY_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Invalid asset or network.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 CLIENT_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Client id is invalid or not linked to the merchant.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 Bad Request</td>
      <td>HTTP</td>
      <td>Request validation failed for one or more fields.</td>
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

### Third step

Create a sell order using the created quote.

#### GET api/v2/exchange/merchant/sell?quoteId=e50dc4e9-d61c-4617-9ccc-1eea152e7d8a

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**

```jsx
{
    "id": "f6f5524d-c933-42be-8e67-128379dfd43e",
    "type": "SELL",
    "status": "PROCESSING",
    "creationDate": "2026-03-31T11:50:47.618594",
    "modificationDate": "2026-03-31T11:50:48.370130",
    "cryptoTransaction": null,
    "expiresAtDate": "2026-03-31T12:20:45.931382",
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
      <td style="word-break: break-word; white-space: normal;">400 QUOTE_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Blockchain address that must be shown to the client as the destination for crypto deposit.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 INVALID_QUOTE</td>
      <td>BUSINESS</td>
      <td>Quote cannot be calculated or cannot be used for order creation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 AML_FRAUD_VALIDATION_ERROR</td>
      <td>BUSINESS</td>
      <td>AML/fraud checks blocked order creation.</td>
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

### Fourth step

Get order details and provide transfer instructions to the client.

#### GET /api/v2/exchange/merchant/order?orderId=c711b777-5d13-4156-a57d-456cc307626b

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**

```jsx
{
    "id": "c711b777-5d13-4156-a57d-456cc307626b",
    "type": "SELL",
    "status": "PROCESSING",
    "creationDate": "2026-03-31T10:42:22.598426",
    "modificationDate": "2026-03-31T10:42:22.939084",
    "number": 211000003411,
    "exchangeOperation": {
        "inputCurrency": "TRX",
        "inputAsset": 50.629578,
        "outputCurrency": "RUB",
        "outputAsset": 1299.36,
        "exchangeFeeAssetInFiat": 26.52,
        "bonusOutputAsset": null,
        "plainRatio": 26.1879,
        "ratio": 25.664,
        "currencyPair": {
            "fromCurrency": "TRX",
            "toCurrency": "RUB"
        }
    },
    "cryptoTransaction": {
        "hash": null,
        "externalCryptoAddress": null,
        "internalCryptoAddress": "TY3GBBkVMcYra6c8gPqXwdi4msvd7HU7Mx",
        "fromAddress": null,
        "toAddress": "TY3GBBkVMcYra6c8gPqXwdi4msvd7HU7Mx",
        "status": "NEW",
        "currency": "TRX",
        "fee": null,
        "feePaymentEnabledByClient": false,
        "type": "AUTO",
        "comment": null
    },
    "fiatTransaction": {
        "status": "NEW",
        "paymentToken": "008eda7d-1b99-4f5b-90fb-291e3fbb128d",
        "post": null,
        "brand": null,
        "internalToken": "26226972800000104219",
        "orderIdentity": "d1a3fd86-5764-48dd-91c2-275a562019ea",
        "link": null,
        "providerType": "VTB",
        "paymentType": null,
        "processingBank": null,
        "resultMessage": null,
        "currency": "RUB"
    },
    "client": {
        "clientId": "d028460b-95cd-4562-ac5e-767bf87df40b"
    },
    "serverDate": "2026-03-31T10:56:46+0000",
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
    "expiresAtDate": "2026-03-31T11:12:21.361309"
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
      <td style="word-break: break-word; white-space: normal;">404 ORDER_NOT_FOUND</td>
      <td>BUSINESS</td>
      <td>Order not found or not accessible in merchant scope.</td>
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

For this step, the client must send crypto to the deposit address from the order response:

* **Address field:** `cryptoTransaction.toAddress`
* **Network / asset field:** `cryptoTransaction.currency` (in this example: `TRX`, i.e. Tron network)
* **Amount field:** `exchangeOperation.inputAsset` (in this example: `50.629578`)

The transfer must be made in the correct network and before the order expiration time (`expiresAtDate`).

After the transfer is sent, poll `GET /api/v2/exchange/merchant/order?orderId=**c711b777-5d13-4156-a57d-456cc307626b**` until the order reaches a final status (`COMPLETED` or `FAILED`).
