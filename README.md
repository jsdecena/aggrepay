# Aggreg82r Pay

PHP SDK for calling the checkout method of [Aggreg82r.com](https://aggreg82r.com)

## Requirements

- cURL
- PHP >7.3

## Installation

```
composer require aggreg82r/pay
```

## Usage

1. Create your form

```html
<form action="https://aggreg82r.com/customer/checkout" method="post">
    <input type="hidden" name="payment_gateway" value="stripe">
    <input type="hidden" name="recurring" value="false">
    <input type="hidden" name="reference" value="your-reference-string">
    <input type="hidden" name="name" value="Your total cart amount from example.com">
    <input type="hidden" value="amount" name="500.45">
    <input type="hidden" name="currency" value="php">
    <button type="submit" class="btn btn-sm">Pay with Stripe</button>
</form>
```

### Required fields

- payment_gateway (string) - Intended payment gateway
- recurring (boolean) - If this is a recurring payment of not
- reference (string) - Your reference code for this transaction, this is important to map out in your system the customer who made for this transaction
- name (string) - The description in the checkout form of the chosen payment gateway
- amount (string) - The total amount
- currency (string) - Intended currency to bill your customer

2. Grab the form post data and do **CHECKOUT**

```php

// Get the form POST data
$postData = $request->all(); // Important: Sanitize your data

// Initialize your checkout
$checkout = new \Aggreg82r\Pay\Checkout(
    'https://aggreg82r.com',
    '30aab829-ee2f-49c0-be87-a2149b382c7e', // API KEY
    '4c3e523d-1f78-4a97-8535-49e36763d0f5' // API SECRET
);

// get your api key and secret in https://aggreg82r.com 

$checkout->now($postData);
// This will redirect to the intended payment gateway checkout page
```

3. Once your customer has filled their credit card details, it will go back to your `success_url` you defined in your credential dashboard at https://aggreg82r.com

___

# Not PHP? No Problem. Try curl

1. Your backend should request for the token first and save it to be used in Step 2.

```curl
curl --request POST \
  --url https://aggreg82r.com/customer/auth \
  --header 'Content-Type: application/json' \
  --data '{
        "api_key": "<YOUR-API-KEY>",
        "api_secret": "<YOUR-API-SECRET>",
        "grant_type": "client_credentials"
    }'
```

2. Your html form above should post in your backend. Then, once you collected the data from the frontend post it to aggreg82r.com

```curl
curl --request POST \
  --url https://aggreg82r.com/customer/checkout \
  --header 'Authorization: Bearer <TOKEN-FROM-ABOVE-REQUEST>' \
  --header 'Content-Type: application/json' \
  --data '{
        "payment_gateway": "stripe",
        "recurring": "false",
        "reference": "your-reference-string",
        "name": "Your total cart amount from example.com",
        "amount": "500.45",
        "currency": "USD"
    }'
```

3. Done.

> This will redirect to the intended payment gateway for your customer to input their credit card number. Once they have finished, they will be redirected back to the `success_url` you set in your credential dashboard at https://aggreg82r.com
