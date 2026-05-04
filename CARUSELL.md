CARUSELL is a payment provider for СБП (Faster Payments System). The payment provider does not require card to be bound, payment is made in the bank's app through which the payment will be processed. Whitebird's connection with the client's bank is via a phone number. A phone number is required to make a payment

## Available currencies:

- RUB

## Available Bank Card By Region:

- Russia

## Directions & Commission:

- Buy Crypto 2,5 %
- Sell Sell 2 %

# Buy Crypto Flow:

## First step

Get available payment methods for the client 

### POST api/v2/exchange/merchant/payment/method

### Request Header:

x-api-key 

### Request Body:

```jsx
{
    "clientId": "93a8b39b-d883-4de3-9527-d92b1eabe38c",
    "fiatAsset": "RUB", -- optional (available BYN, RUB, USD, EUR)
    "orderType": "BUY"  -- optional (available BUY and SELL)
}
```

### Response:

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

## Second step

To create a quota, specify a random uuid in the paymentMethodToken field

### POST api/v2/exchange/merchant/quote

### Request Header:

x-api-key 

### Request Body:

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

### Response:

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

## Third step

Create an order specifying returnUrl and failUrl to ensure the client is redirected back to the website after payment

### POST api/v2/exchange/merchant/buy?quoteId=27690c04-e344-4d31-81aec58296ba6ddc&returnUrl=https://www.google.com/&failUrl=https://www.google.com/

### Request Header:

x-api-key 

### Response:

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

## Fourth step

Open the link where the client selects the bank through which the payment will be made and complete the payment. 

# Sell Crypto Flow:

## First step

Get available payment methods for the client 

### POST api/v2/exchange/merchant/payment/method

### Request Header:

x-api-key 

### Request Body:

```jsx
{
    "clientId": "93a8b39b-d883-4de3-9527-d92b1eabe38c",
    "fiatAsset": "RUB", -- optional (available BYN, RUB, USD, EUR)
    "orderType": "SELL"  -- optional (available BUY and SELL)
}
```

### Response:

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

## Second step

To create a quota, specify a random uuid in the paymentMethodToken field

### POST api/v2/exchange/merchant/quote

### Request Header:

x-api-key 

### Request Body:

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

### Response:

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

## Third step

Get the list of available banks for transfers via **CARUSELL**

### GET https://carusell-service.dev.wbdevel.net/api/v1/bank

### Request Header:

x-api-key 

### Response:

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

The client chooses the bank to which the payment will be made 

## Fourth step

 Create an order with the bankIdentifier selected by the client

### POST /api/v2/exchange/merchant/sell?quoteId=27690c04-e344-4d31-81ae-c58296ba6ddc&bankIdentifier=100000000118

### Request Header:

x-api-key 

### Response:

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

For the test environment, it does not matter which bank is selected (bankIdentifier) — the payment will be processed successfully in any case.

## **CARUSELL** also has an anti-fraud check. It is performed automatically:

- At the moment of order creation for the crypto-to-fiat flow
- At the moment of order execution for the fiat-to-crypto flow

If an order fails to be created or returns an error at any stage, there is a possibility that the anti-fraud system has declined the transaction.
