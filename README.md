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

- payment_gateway - Intended payment gateway
- reference - Your reference code for this transaction, this is important to map out in your system the customer who made for this transaction
- currency - Intended currency to bill your customer
- name - The description in the checkout form of the chosen payment gateway
- amount - The total amount

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
