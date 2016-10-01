# react-native-paypal

A Cross platform React Native interface for the PayPal Payment UI. Supports both iOS and Android.

![Demo of a Payment using PayPal](/react-native-paypal.gif?raw=true "react-native-paypal")

## Usage
--

Initiating a payment is as simple as creating a single promise

```javascript
let PayPal = require('react-native-paypal');
PayPal.paymentRequest({
  clientId: 'AbyfNDFV53djg6w4yYgiug_JaDfBSUiYI7o6NM9HE1CQ_qk9XxbUX0nwcPXXQHaNAWYtDfphQtWB3q4R',
  environment: 'sandbox',
  price: '42.00',
  currency: 'USD',
  description: 'PayPal Test'
}).then((confirm, payment) => console.log('Paid'); /* At this point you should verify payment independently */)
.catch((error_code) => console.error('Failed to pay through PayPal'));
```

#### Callback parameters:

If all goes OK with the payment than the paymentRequest promise is resolved with
the following arguments as JSON strings:
- A confirm:
``` json
{
  "client": {
    "environment": "mock",
    "paypal_sdk_version": "2.12.4",
    "platform": "Android",
    "product_name": "PayPal-Android-SDK"
  },
  "response": {
    "create_time": "2014-02-12T22:29:49Z",
    "id": "PAY-6RV70583SB702805EKEYSZ6Y",
    "intent": "sale",
    "state": "approved"
  },
  "response_type": "payment"
}
```

- A payment:
```json
{
  "amount": "1.00",
  "currency_code": "USD",
  "short_description": "PayPal Test",
  "intent": "sale"
}
```

Handling callbacks:
```javascript
PayPal.paymentRequest(...).then(function (payment, confirm) {
  sendPaymentToConfirmInServer(payment, confirm);
})
```

If anything fails the promise will be notify an error with a code which will be
one of:
- USER\_CANCELLED
- INVALID\_CONFIG

Handling failures:

``` javascript
PayPal.paymentRequest(...).catch(function (error_code) {
    if (error_code == PayPal.USER_CANCELLED) {
        // User didn't complete the payment
    } else if (error_code == PayPal.INVALID_CONFIG) {
        // Invalid config was sent to PayPal
    }
})
```

## Setup

Android & iOS
-------------

1. Add react-native-paypal to your project

``` bash
npm install --save react-native-paypal
```

2. Link the library with React Native

``` bash
react-native link react-native-paypal
```

The Android portion of this library was originally built by https://github.com/Vizir for https://github.com/Vizir/react-native-paypal. The merging of the API in order to provide cross platform support was done almost entirely by https://github.com/amiuhle.
The iOS portion was built by https://github.com/MattFoley for https://github.com/MattFoley/react-native-paypal