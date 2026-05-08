# ALFA

ALFA is a payment provider of Belarus AlfaBank. To use it, client need to bind their card

### Available currencies:

* BYN
* RUB
* USD
* EUR

### Available Bank Card By Region:

* Belarus
* Azerbaijan
* Armenia
* Kazakhstan
* Kyrgyzstan
* Moldova
* Tajikistan
* Turkmenistan
* Uzbekistan
* Georgia

### Directions & Commission:

* Buy Crypto 2,5 %
* Sell Sell 1,5 %

### Flow for binding card:

![image.png](<.gitbook/assets/image (1).png>)

## First step

Verify that the payment provider is available

#### POST api/v2/exchange/merchant/payment/provider

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
    "clientId": "93a8b39b-d883-4de3-9527-d92b1eabe38c",
    "fiatAsset": "BYN", -- optional (available BYN, RUB, USD, EUR)
    "orderType": "BUY"  -- optional (available BUY and SELL)
}
```

**Response**

```jsx
{
    "id": "ALFA",
    "name": "ALFA",
    "addPaymentMethod": true,
    "config": {
        "paymentSystems": [
            {
                "paymentSystem": "VISA",
                "type": "PSP",
                "directions": [
                    {
                        "direction": "SELL",
                        "currencies": [
                            {
                                "currency": "BYN",
                                "banks": [
                                    "Alfa Bank (Belarus)",
                                    "Belinvestbank",
                                    "Bank Dabrabyt",
                                    "Bank BelVEB",
                                    "Sber Bank (Belarus)"
                                ]
                            },
                            {
                                "currency": "USD",
                                "banks": [
                                    "Alfa Bank (Belarus)",
                                    "Belinvestbank",
                                    "Bank Dabrabyt",
                                    "Bank BelVEB",
                                    "Sber Bank (Belarus)"
                                ]
                            },
                            {
                                "currency": "EUR",
                                "banks": [
                                    "Alfa Bank (Belarus)",
                                    "Belinvestbank",
                                    "Bank Dabrabyt",
                                    "Bank BelVEB",
                                    "Sber Bank (Belarus)"
                                ]
                            },
                            {
                                "currency": "RUB",
                                "banks": [
                                    "Alfa Bank (Belarus)",
                                    "Belinvestbank",
                                    "Bank Dabrabyt",
                                    "Bank BelVEB",
                                    "Sber Bank (Belarus)"
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

It is sufficient to verify that the payment provider is available via the id field. id = ALFA

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string (UUID)</td>
      <td>No</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAsset</td>
      <td>string</td>
      <td>No</td>
      <td>Optional fiat filter. Allowed values: BYN, RUB, USD, EUR.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderType</td>
      <td>string</td>
      <td>No</td>
      <td>Optional order direction filter. Allowed values: BUY, SELL.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destination</td>
      <td>string</td>
      <td>No</td>
      <td>Optional flow destination filter. Recommended value: EXCHANGE or SDK_EXCHANGE.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Provider identifier. For Alfa flow expected value is ALFA.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">name</td>
      <td>string</td>
      <td>Provider display name.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">addPaymentMethod</td>
      <td>boolean</td>
      <td>Defines whether provider supports adding payment methods.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config</td>
      <td>object</td>
      <td>Provider routing configuration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems</td>
      <td>array of objects</td>
      <td>Payment systems list for provider.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].paymentSystem</td>
      <td>string</td>
      <td>Payment system name (for example VISA).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].type</td>
      <td>string</td>
      <td>Provider channel type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions</td>
      <td>array of objects</td>
      <td>Supported operation directions for this payment system.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].direction</td>
      <td>string</td>
      <td>Direction for payment system route (BUY/SELL).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies</td>
      <td>array of objects</td>
      <td>Supported currencies for selected direction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies[].currency</td>
      <td>string</td>
      <td>Fiat currency for this route.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">config.paymentSystems[].directions[].currencies[].banks</td>
      <td>array of strings</td>
      <td>Supported bank list for selected currency and direction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions</td>
      <td>array of objects</td>
      <td>Provider commission settings returned for merchant route.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions[].bank</td>
      <td>string | null</td>
      <td>Bank group key for commission row.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions[].buyCommission</td>
      <td>string | null</td>
      <td>Commission value/range for buy direction.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">commissions[].sellCommission</td>
      <td>string | null</td>
      <td>Commission value/range for sell direction.</td>
    </tr>
  </tbody>
</table>

## Second step

Generate link to bind client card

#### POST api/v2/exchange/merchant/payment/card/bind

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

`returnUrl` - Link to the resource where the client should be redirected after binding the card

```jsx
{
    "clientId": "1a0e2c64-8a90-4144-ac05-5e66bde1ca84",
    "providerType": "ALFA",
    "returnUrl": "https://www.google.com"
}
```

**Response**

```jsx
{
    "url": "https://abby.rbsuat.com/payment/merchants/whitebird/payment.html?mdOrder=13e70051-a19b-73d3-a7e9-309600dfc911&language=ru"
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>No</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string (UUID)</td>
      <td>Yes</td>
      <td>Client identifier used to bind payment method to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providerType</td>
      <td>string</td>
      <td>No</td>
      <td>Payment provider type. For this flow expected value is ALFA.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">returnUrl</td>
      <td>string</td>
      <td>No</td>
      <td>Optional URL where client is redirected after card binding flow is finished.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">url</td>
      <td>string</td>
      <td>Provider URL that must be opened by client to complete card binding.</td>
    </tr>
  </tbody>
</table>

## Third step

Open link for the client to bind their card

Test card data: Number: 4585220000858497 Expiration time: 05/30 CVV: 078

```
                                                         Enter your card details 
```

![Screenshot 2026-03-04 at 18.40.56.png](.gitbook/assets/alfa1.png)

```
                             Confirm 3ds verification by click on the “Success” button
```

![Screenshot 2026-03-04 at 18.41.09.png](.gitbook/assets/alfa2.png)

```
          After completing the card binding, you will be redirected to the returnUrl you specified
```

How can I find out the result of a card binding?\
Use webhooks from Whitebird or use the request from step four

#### There are three webhooks from Wthitebird:

#### client.payment.method.binding

Сlient initiated card binding

```
{
    "id": "webhook-id",
    "clientId": "ed8ff528-3017-45bc-9d4d-f90e58f91bf9",
    "bindId": "856c460d-7081-433b-904d-c46e313b1225",
    "providerType": "ALFA",
    "createdAt": "2025-04-21T09:00:17+0000",
    "type": "client.payment.method.binding"
}
```

#### client.payment.method.bound

Сlient's card was successfully bound

```
{
    "id": "webhook-id",
    "clientId": "ed8ff528-3017-45bc-9d4d-f90e58f91bf9",
    "paymentToken": "7ee5900d-7a02-4bcf-a757-7a7b2fce462d",
    "providerType": "ALFA",
    "createdAt": "2025-04-21T13:51:26+0000",
    "type": "client.payment.method.bound"
}
```

#### client.payment.method.failed

Сlient did not bind the card or the binding was declined by the bank

```
{
    "id": "webhook-id",
    "clientId": "5646c1b7-d934-44ce-8490-938feb810910",
    "bindId": "97d009fe-80e6-426d-8ea2-9784f676e08e",
    "cardMask": "0380",
    "brand": "MASTERCARD",
    "providerType": "ALFA",
    "createdAt": "2025-04-22T07:47:31+0000",
    "type": "client.payment.method.failed"
} 
```

If the client has successfully bound their card, you can get the card id in Whitebird as the paymentToken value

## Fourth step

Get available payment methods for the client

#### POST api/v2/exchange/merchant/payment/method

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**

```jsx
{
    "clientId": "93a8b39b-d883-4de3-9527-d92b1eabe38c",
    "fiatAsset": "BYN", -- optional (available BYN, RUB, USD, EUR)
    "orderType": "BUY"  -- optional (available BUY and SELL)
}
```

**Response**

```jsx
[
    {
        "id": "ee6693e1-c340-47cb-8b9e-29304b6d9fd8",
        "number": "**** **** **** 8497",
        "brand": "VISA",
        "providerId": "ALFA",
        "providerType": "ALFA",
        "status": "ENABLED",
        "isRestricted": false,
        "isCrypto": false,
        "country": "Russia"
    }
]
```

If the card status is ENABLED, the id field value can be used for the exchange operation as the paymentToken field

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates the merchant server-to-server request. Use the API key issued for the merchant and target environment.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string (UUID)</td>
      <td>No</td>
      <td>Client identifier used to scope the request to a specific client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">fiatAsset</td>
      <td>string</td>
      <td>No</td>
      <td>Optional fiat filter. Allowed values: BYN, RUB, USD, EUR.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">orderType</td>
      <td>string</td>
      <td>No</td>
      <td>Optional order direction filter. Allowed values: BUY, SELL.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">destination</td>
      <td>string</td>
      <td>No</td>
      <td>Optional flow destination filter. Recommended value: EXCHANGE or SDK_EXCHANGE.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>Payment method token. Use this value as paymentToken/paymentMethodToken in exchange operations.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">number</td>
      <td>string</td>
      <td>Masked card number shown to client.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">brand</td>
      <td>string</td>
      <td>Card brand returned by provider (for example VISA, MASTERCARD).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providerId</td>
      <td>string</td>
      <td>Payment provider identifier used in integrations and filters.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">providerType</td>
      <td>string</td>
      <td>Provider category/type returned by provider integration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Payment method status. Use ENABLED methods for operation creation.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isRestricted</td>
      <td>boolean</td>
      <td>Shows whether this payment method is restricted.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isCrypto</td>
      <td>boolean</td>
      <td>Indicates whether the payment method is crypto type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">country</td>
      <td>string</td>
      <td>Country associated with payment method.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">currency</td>
      <td>string | null</td>
      <td>Primary currency associated with payment method when provided.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">supportedCurrencies</td>
      <td>array of strings | null</td>
      <td>List of currencies supported by this payment method when provided.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">name</td>
      <td>string | null</td>
      <td>Legacy provider display field (deprecated in API model, may be absent).</td>
    </tr>
  </tbody>
</table>
