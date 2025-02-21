
# Monnify-ts

A simple Typescript wrapper for integrating with the Monnify payment gateway. This library allows you to easily configure and initialize payments using the Monnify Web SDK.


## Installation

First, install the package:

```bash

npm  install  monnify-ts

```

  

## Usage

### Import and Initialize

To use the Monnify wrapper, you need to create an instance with your Monnify API key and contract code:

```javascript

import  Monnify  from  'monnify-ts';

const  monnify = new  Monnify('YOUR_API_KEY', 'YOUR_CONTRACT_CODE');

```

  

### Initializing a Payment

  

To initialize a payment, call the `initializePayment` method with a `PaymentRequest` object:

  

```javascript

monnify.initializePayment({
    amount:  100,
    currency:  "NGN",
    reference:  new  String((new  Date()).getTime()),
    customerFullName:  "Damilare Ogunnaike",
    customerEmail:  "ogunnaike.damilare@gmail.com",
    apiKey:  "MK_PROD_FLX4P92EDF",
    contractCode:  "626609763141",
    paymentDescription:  "Lahray World",
    metadata:  {
        "name":  "Damilare",
        "age":  45
    },
    incomeSplitConfig:  [
    {
        "subAccountCode":  "MFY_SUB_342113621921",
        "feePercentage":  50,
        "splitAmount":  1900,
        "feeBearer":  true
    },  
    {
        "subAccountCode":  "MFY_SUB_342113621922",
        "feePercentage":  50,
        "splitAmount":  2100,
        "feeBearer":  true
    }],
    onLoadStart:  ()  =>  {
        console.log("loading has started");
    },
    onLoadComplete:  ()  =>  {
        console.log("SDK is UP");
    },
    onComplete:  function(response)  {
        //Implement what happens when the transaction is completed.
        console.log(response);
    },
    onClose:  function(data)  {
        //Implement what should happen when the modal is closed here
        console.log(data);
    }
  });
```

  

### `PaymentRequest` Object

| Field | Type | Required | Description |
| -- | -- | -- | -- |
| `amount` | `number` | Yes | Amount to be paid in decimal (e.g 100.50). |
| `currency` | `string` | Yes | Currency code (e.g., 'NGN'). |
| `reference` | `string` | Yes | Unique reference for the payment transaction. |
| `customerFullName` | `string` | Yes | Full name of the customer making the payment. |
| `customerEmail` | `string` | Yes | Email address of the customer. |
| `paymentDescription` | `string` | Yes | Description of the payment. |
| `redirectUrl` | `string` | No | URL to redirect to after payment completion. |
| `paymentMethods` | `Array<string>` | No | List of payment methods to display (e.g., 'CARD', 'ACCOUNT_TRANSFER'). Defaults to all methods i.e. [ "CARD", "ACCOUNT_TRANSFER" , "USSD", "PHONE_NUMBER" ]. |
| `metadata` | `Object` | No | Additional metadata for the transaction. |
| `incomeSplitConfig` | `Array<Object>` | No | Configuration for splitting payments between sub-accounts. |
| `onLoadStart` | `function` | No | Callback function executed when Monnify SDK starts loading. |
| `onLoadComplete` | `function` | No | Callback function executed when Monnify SDK is loaded and ready. |
| `onComplete` | `function` | No | Callback function executed on successful completion of the transaction. |
| `onClose` | `function` | No | Callback function executed when the payment modal is closed or canceled. |

  

### Example Payment Response

The `onComplete` function provides a `PaymentResponse` object with details about the transaction:

```javascript
{
    "amount": 100,
    "amountPaid": 100,
    "completed": true,
    "completedOn": "2022-03-31T10:53:50.000+0000",
    "createdOn": "2022-03-31T10:52:09.000+0000",
    "currencyCode": "NGN",
    "customerEmail": "ogunnaike.damilare@gmail.com",
    "customerName": "Damilare Ogunnaike",
    "fee": 10.75,
    "metaData": {
        "deviceType":  "mobile",
        "ipAddress":  "127.0.0.1"
    },
    "payableAmount": 100,
    "paymentMethod": "CARD",
    "paymentReference": "MNFY|PAYREF|GENERATED|1648723909057299503",
    "paymentStatus": "PAID",
    "transactionReference": "MNFY|67|20220331115209|000063"
}
```

  

### Example User Cancelled Response

The `onClose` function provides a `UserCancelledResponse` object if the user cancels the transaction:

```javascript
{
    "authorizedAmount": 30,
    "paymentStatus": "USER_CANCELLED",
    "redirectUrl": null,
    "responseCode": "USER_CANCELLED",
    "responseMessage": "User cancelled Transaction"
}
```