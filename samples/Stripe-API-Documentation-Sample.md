# Introduction

The Stripe API allows merchants to build a platform, marketplace, or any other type of business with a solid foundation that allows for secure transactions and money movement.

The Stripe API allows a developer to:

-   Check a Stripe account's balance.
-   Retrieve an account's balance history, which includes a list of transactions (charges, payouts, etc.) that contributed to its balance.
-   List, create, update, and search for charges.
-   Create, update, or list refunds.

# Connection Prerequisites

To connect to the Stripe API, you need the base URL: <https://api.stripe.com/>.

**NOTE**: See [Using Postman to Connect to the Stripe API](https://github.com/pvega62/Portfolio/wiki/Using-Postman-to-Connect-to-the-Stripe-API) for detailed instructions on generating the token and using it to authenticate your connection.

# Authentication

Stripe API requires a secret API token for authentication. All requests must be authorized and made over HTTPS. Unauthorized requests or requests made over HTTP will fail.

These tokens, also called secret keys, are found on your Stripe dashboard. See [Using Postman to Connect to the Stripe API](https://github.com/pvega62/Portfolio/wiki/Using-Postman-to-Connect-to-the-Stripe-API) for detailed instructions on generating the token and using it to authenticate your connection.

**NOTE**: For security purposes, the secret key must not be shared with other users.

# Functions

## Balance

Obtain the balance of a Stripe account.

Endpoint: <https://api.stripe.com/v1/balance>.

Operation: `GET`

### Examples
**HTTPS Example**
```http
GET /v1/balance HTTP/1.1
Host: api.stripe.com
Authorization: Bearer <YOUR_SECRET_KEY>
```

**cUrl Example:**
```bash
curl https://api.stripe.com/v1/balance \
    -u <YOUR_SECRET_KEY>:
```
### Responses

Responses will return standard codes.

`200: OK`
```json
{
  "object": "balance",
  "available": [
    {
      "amount": 0,
      "currency": "usd",
      "source_types": {
        "card": 0
      }
    }
  ],
  "livemode": false,
  "pending": [
    {
      "amount": 0,
      "currency": "usd",
      "source_types": {
        "card": 0
      }
    }
  ],
  "refund_and_dispute_prefunding": {
    "available": [
      {
        "amount": 0,
        "currency": "usd"
      }
    ],
    "pending": [
      {
        "amount": 0,
        "currency": "usd"
      }
    ]
  }
}
```
### Error Responses

`400: Bad Request`
```json
{
  "error": {
    "message": "If passing an array with explicit keys (e.g. foo[0]=a&foo[1]=b) instead of as an array (e.g. foo[]=a&foo[]=b), the keys must be numeric and sequential starting from 0. You passed the keys `1`, we expected to have a key with the value `0`.",
    "param": "expand",
    "request_log_url": "https://dashboard.stripe.com/test/logs/req_kCeAL5FAly5bkh?t=1734734555",
    "type": "invalid_request_error"
  }
}
```
`401: Unauthorized`
```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid API Key provided: sk_test_********************1234"
  }
}
```
### Parameters

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `expand[]` | array of strings | Specifies which fields in the response should be expanded. |

## Retrieve a Balance Transaction

Returns the transactions that make up the Stripe account's balance (e.g., charges, transfers, payouts, refunds, etc.). The transactions are returned in sorted order, with the most recent transactions appearing first.

Endpoint: <https://api.stripe.com/v1/balance_transactions/:id>.

Operation: `GET`

### Examples
**HTTPS Example**
```http
GET /v1/balance_transactions/tr_1234567890 HTTP/1.1
Host: api.stripe.com
Authorization: Bearer <YOUR_SECRET_KEY>
```
**cUrl Example:**
```bash
curl https://api.stripe.com/v1/balance_transactions/tr_1234567890 \
    -u <YOUR_SECRET_KEY>:
```

### Responses
Responses will return standard codes.

`200: OK`
```json
{
  "object": "balance_transaction",
  "id": "tr_1234567890",
  "amount": 1000,
  "currency": "usd",
  "description": "Payment for invoice",
  "fee": 59,
  "fee_details": [
    {
      "amount": 59,
      "currency": "usd",
      "description": "Stripe processing fees",
      "type": "stripe_fee"
    }
  ],
  "net": 941,
  "status": "available",
  "type": "charge"
}
```
`400: Bad Request`
```json
{
  "error": {
    "message": "If passing an array with explicit keys (e.g. foo[0]=a&foo[1]=b) instead of as an array (e.g. foo[]=a&foo[]=b), the keys must be numeric and sequential starting from 0. You passed the keys `1`, we expected to have a key with the value `0`.",
    "param": "expand",
    "request_log_url": "https://dashboard.stripe.com/test/logs/req_kCeAL5FAly5bkh?t=1734734555",
    "type": "invalid_request_error"
  }
}
```
`401: Unauthorized`
```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid API Key provided: sk_test_********************1234"
  }
}
```
### Parameters

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `expand[]` | array of strings | Specifies which fields in the response should be expanded. |
#### Path Variables
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `id` | string | Required | Path variable identifier |

## List all Payment Intents

Returns a list of Payment Intents.

Endpoint: <https://api.stripe.com/v1/payment_intents>.

Operation: `GET`

### Examples
**HTTPS Example**
```http
GET /v1/payment_intents HTTP/1.1
Host: api.stripe.com
Authorization: Bearer <YOUR_SECRET_KEY>
Content-Type: application/x-www-form-urlencoded
```
**cUrl Example:**
```bash
curl https://api.stripe.com/v1/payment_intents \
    -u <YOUR_SECRET_KEY>:
```

### Responses

Responses will return standard codes.

`200: OK`
```json
{
  "object": "list",
  "data": [
    {
      "id": "pi_1234567890",
      "object": "payment_intent",
      "amount": 1000,
      "currency": "usd",
      "status": "requires_confirmation"
    }
  ],
  "has_more": false,
  "url": "/v1/payment_intents"
}
```

`400: Bad Request`
```json
{
  "error": {
    "message": "If passing an array with explicit keys (e.g. foo[0]=a&foo[1]=b) instead of as an array (e.g. foo[]=a&foo[]=b), the keys must be numeric and sequential starting from 0. You passed the keys `1`, we expected to have a key with the value `0`.",
    "param": "expand",
    "request_log_url": "https://dashboard.stripe.com/test/logs/req_kCeAL5FAly5bkh?t=1734734555",
    "type": "invalid_request_error"
  }
}
```
`401: Unauthorized`
```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid API Key provided: sk_test_********************1234"
  }
}
```

### Parameters

#### Query Parameters

| Parameter | Type | Description |
|------------------|------------------|--------------------------------------------------------------------------------------------------------------------|
| `limit` | integer | A limit on the number of objects to be returned. Limit can range between 1 and 100, and the default is 10. |
| `starting_after` | string | A cursor for use in pagination. `starting_after` is an object ID that defines your place in the list. |
| `ending_before` | string | A cursor for use in pagination. `ending_before` is an object ID that defines your place in the list. |
| `expand[]` | array of strings | Specifies which fields in the response should be expanded. |

## Create a Payment Intent

Once a Payment Intent is created, attach a payment method and confirm to continue payment.

Using `confirm=true` during creation is equivalent to creating and confirming the Payment Intent in the same call. Applying `confirm=true` enables the use of any parameters in the **confirm** API.

Endpoint: <https://api.stripe.com/v1/payment_intents>.

Operation: `POST`

**NOTE**: The **confirm** request must be made over **HTTPS**.
### Examples
**HTTPS Example**  
```http
POST /v1/payment_intents HTTP/1.1
Host: api.stripe.com
Authorization: Bearer <YOUR_SECRET_KEY>
Content-Type: application/x-www-form-urlencoded
```
**cUrl Example:**
```bash⁠
curl https://api.stripe.com/v1/payment_intents \
  -u <YOUR_SECRET_KEY>: \
  -d amount=212 \
  -d currency=usd \
  -d payment_method=pm_card_visa \
  -d confirm=true
```  
### Responses

Responses will return standard codes.

`200: OK`
```json
{
    "id": "pi_3Rsx4dDtWMrPDtgt0mUrePJ9",
    "object": "payment_intent",
    "amount": 212,
    "amount_capturable": 0,
    "amount_details": {
        "tip": {}
    },
    "amount_received": 0,
    "application": null,
    "application_fee_amount": null,
    "automatic_payment_methods": {
        "allow_redirects": "always",
        "enabled": true
    },
    "canceled_at": null,
    "cancellation_reason": null,
    "capture_method": "automatic_async",
    "client_secret": "pi_3Rsx4dDtWMrPDtgt0mUrePJ9_secret_7sqfufxuIYiBhbw52Vi51Z47o",
    "confirmation_method": "automatic",
    "created": 1754447247,
    "currency": "usd",
    "customer": null,
    "description": null,
    "last_payment_error": null,
    "latest_charge": null,
    "livemode": false,
    "metadata": {},
    "next_action": null,
    "on_behalf_of": null,
    "payment_method": null,
    "payment_method_configuration_details": {
        "id": "pmc_1RsGk1DtWMrPDtgtcaQ0d3Sg",
        "parent": null
    },
    "payment_method_options": {
        "amazon_pay": {
            "express_checkout_element_session_id": null
        },
        "card": {
            "installments": null,
            "mandate_options": null,
            "network": null,
            "request_three_d_secure": "automatic"
        },
        "cashapp": {},
        "klarna": {
            "preferred_locale": null
        },
        "link": {
            "persistent_token": null
        }
    },
    "payment_method_types": [
        "card",
        "klarna",
        "link",
        "cashapp",
        "amazon_pay"
    ],
    "processing": null,
    "receipt_email": null,
    "review": null,
    "setup_future_usage": null,
    "shipping": null,
    "source": null,
    "statement_descriptor": null,
    "statement_descriptor_suffix": null,
    "status": "requires_payment_method",
    "transfer_data": null,
    "transfer_group": null
}
```

`400: Bad Request`
```json
{
  "error": {
    "code": "parameter_invalid_integer",
    "doc_url": "https://stripe.com/docs/error-codes/parameter-invalid-integer",
    "message": "Invalid integer: <integer>",
    "param": "amount",
    "request_log_url": "https://dashboard.stripe.com/test/logs/req_UQf6XBwBl9yk66?t=1734829676",
    "type": "invalid_request_error"
  }
}
```
`401: Unauthorized`
```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid API Key provided: sk_test_********************1234"
  }
}
```
### Parameters
#### Body Parameters

| Parameter | Type | Description |
|--------------------------|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `amount` | integer | **(Required)** Amount intended to be collected. A positive integer representing how much to charge in the smallest currency unit (e.g., 100 cents to charge $1.00). |
| `currency` | string | **(Required)** Three-letter ISO currency code, in lowercase. Must be a supported currency. |
| `confirm` | boolean | Set to true to attempt to confirm this PaymentIntent immediately. |
| `customer` | string | ID of the Customer this PaymentIntent belongs to, if one exists. |
| `description` | string | An arbitrary string attached to the object. |
| `payment_method` | string | ID of the payment method to attach to this PaymentIntent. |
| `payment_method_types[]` | array of strings | The list of payment method types that this PaymentIntent is allowed to use. |
| `shipping` | object | Shipping information for the PaymentIntent. |
| `statement_descriptor` | string | An arbitrary string to be displayed on your customer's credit card statement. |
| `statement_descriptor_suffix` | string | Provides additional details to be displayed on your customer's credit card statement. |
| `expand[]` | array of strings | Specifies which fields in the response should be expanded. |
## Search Payment Intents

Using Stripe’s Search Query Language, you can search for previously created Payment Intents.

Do not use the search request in read-after-write flows where strict consistency is needed. Under normal operation, data is searchable for less than a minute. Occasionally, the propagation of new or updated data can be up to an hour behind during outages.

**NOTE**: The search function is not available to merchants in India.

Endpoint: <https://api.stripe.com/v1/payment_intents/search>.

Operation: `GET`
### Examples
**HTTPS Example**
```http
GET /v1/payment_intents/search?query=amount:1000 HTTP/1.1
Host: api.stripe.com
Authorization: Bearer <YOUR_SECRET_KEY>
```
**cUrl Example:**
```bash
curl https://api.stripe.com/v1/payment_intents/search?query=amount:1000 \
    -u <YOUR_SECRET_KEY>:
```
### Responses

Responses will return standard codes.

`200: OK`
```json
{
  "object": "search_result",
  "data": [],
  "has_more": false,
  "next_page": null,
  "url": "/v1/payment_intents/search"
}
```
`400: Bad Request`
```json
{
  "error": {
    "code": "parameter_invalid_string",
    "doc_url": "https://stripe.com/docs/error-codes/parameter-invalid-string",
    "message": "Provide a query parameter with a valid search query string.",
    "param": "query",
    "type": "invalid_request_error"
  }
}
```
`401: Unauthorized`
```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid API Key provided: sk_test_********************1234"
  }
}
```

### Parameters

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `expand[]` | array of strings | Specifies which fields in the response should be expanded. |
| `limit` | integer | A limit on the number of objects to be returned. Limit can range between 1 and 100, and the default is 10. |
| `page` | string | A cursor for pagination across multiple pages of results. Don't include this parameter on the first call. Use the `next_page` value returned in a previous response to request subsequent results. |
| `query` | string | **(Required)** The search query string. See search query language and the list of supported query fields for payment intents. |

## Create a Refund
Creates a refund for a charge already created but not yet refunded. The amount to be refunded can be specified, or the entire charge amount can be refunded by not specifying an amount. The refund will be created in the same currency as the charge and to the same payment method as the charge.

**A charge cannot be refunded more than once.** If you attempt to refund a charge that has already been refunded, an error will be returned. This will also happen if you attempt to refund more than the original charge amount.

Endpoint: <https://api.stripe.com/v1/refunds>.

Operation: `POST`
### Examples
**HTTPS Example**
```http
POST /v1/refunds HTTP/1.1
Host: api.stripe.com
Authorization: Bearer <YOUR_SECRET_KEY>
```
**cUrl Example:**
```bash
curl https://api.stripe.com/v1/refunds \
  -u <YOUR_SECRET_KEY>: \
  -d charge=ch_1NirD82eZvKYlo2CIvbtLWuY
```
### Responses

Responses will return standard codes.

`200: OK`
```json
{
  "id": "re_1Nispe2eZvKYlo2Cd31jOCgZ",
  "object": "refund",
  "amount": 212,
  "balance_transaction": "txn_1Nispe2eZvKYlo2CYezqFhEx",
  "charge": "ch_1NirD82eZvKYlo2CIvbtLWuY",
  "created": 1692942318,
  "currency": "usd",
  "destination_details": {
    "card": {
      "reference": "123456789012",
      "reference_status": "available",
      "reference_type": "acquirer_reference_number",
      "type": "refund"
    },
    "type": "card"
  },
  "metadata": {},
  "payment_intent": "pi_1GszsK2eZvKYlo2CfhZyoZLp",
  "reason": null,
  "receipt_number": null,
  "source_transfer_reversal": null,
  "status": "succeeded",
  "transfer_reversal": null
}
```
`400: Bad Request`
```json
{
  "error": {
    "code": "parameter_invalid_integer",
    "doc_url": "https://stripe.com/docs/error-codes/parameter-invalid-integer",
    "message": "Invalid integer: <integer>",
    "param": "amount",
    "request_log_url": "https://dashboard.stripe.com/test/logs/req_UQf6XBwBl9yk66?t=1734829676",
    "type": "invalid_request_error"
  }
}
```
`401: Unauthorized`
```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid API Key provided: sk_test_********************1234"
  }
}
```
### Parameters
#### Body Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| `amount` | integer | Amount to refund, in cents. If not specified, the entire charge amount will be refunded. |
| `charge` | string | **(Required)** ID of the charge to refund. |
| `currency` | string | Three-letter ISO currency code, in lowercase. Must be a supported currency. If not specified, the currency of the charge will be used. |
| `expand[]` | array of strings | Specifies which fields in the response should be expanded. |
| `customer` | string | ID of the customer to whom the charge belongs. |
| `reason` | string | Reason for the refund. If set, possible values are `duplicate`, `fraudulent`, or `requested_by_customer`. If set as `fraudulent`, the card and email associated with the charge will be added to your block lists, and will help improve Stripe's fraud detection. |
| `payment_intent` | string | ID of the PaymentIntent to refund. |
| `refund_application_fee` | boolean | Indicates whether the application fee should be refunded. If a the full amount is refunded, the application fee will be refunded as well. Otherwise, the application fee will be refunded proportionally to the refund. An application fee can only be the application that created the charge.| 
| `origin`| string | The origin of the refund. |
| `reverse_transfer` | boolean | Indicates whether the transfer should be reversed. Transfer is reversed proportionally to the refund amount (full or partial refund). |
| `instructions_email` | string | Uses customer's email address for refund instructions when payment method without refund support is used. |

## Update a Refund
Updates the specified refund by setting the values of the parameters passed. Any parameters not provided will be left unchanged.

**NOTE**: Request only accepts `metadata` as an argument.

Endpoint: <https://api.stripe.com/v1/refunds/:id>.

Operation: `POST`
### Examples
**HTTPS Example**
```http
POST /v1/refunds/:refunds HTTP/1.1
Host: api.stripe.com
Authorization: <YOUR_SECRET_KEY>
```
**cUrl Example:**
```bash
curl https://api.stripe.com/v1/refunds/:refunds \
  -u <YOUR_SECRET_KEY>: \
  -d metadata[order_id]=6735
```
### Responses

Responses will return standard codes.

`200: OK`
```json
{
  "id": "re_1Nispe2eZvKYlo2Cd31jOCgZ",
  "object": "refund",
  "amount": 1000,
  "balance_transaction": "txn_1Nispe2eZvKYlo2CYezqFhEx",
  "charge": "ch_1NirD82eZvKYlo2CIvbtLWuY",
  "created": 1692942318,
  "currency": "usd",
  "destination_details": {
    "card": {
      "reference": "123456789012",
      "reference_status": "available",
      "reference_type": "acquirer_reference_number",
      "type": "refund"
    },
    "type": "card"
  },
  "metadata": {},
  "payment_intent": "pi_1GszsK2eZvKYlo2CfhZyoZLp",
  "reason": null,
  "receipt_number": null,
  "source_transfer_reversal": null,
  "status": "succeeded",
  "transfer_reversal": null
}
```
`400: Bad Request`
```json
{
  "error": {
    "code": "parameter_invalid_integer",
    "doc_url": "https://stripe.com/docs/error-codes/parameter-invalid-integer",
    "message": "Invalid integer: <integer>",
    "param": "amount",
    "request_log_url": "https://dashboard.stripe.com/test/logs/req_UQf6XBwBl9yk66?t=1734829676",
    "type": "invalid_request_error"
  }
}
```
`401: Unauthorized`
```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid API Key provided: sk_test_********************1234"
  }
}
```
### Parameters 
#### Body Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| `expand[]` | array of strings | Specifies which fields in the response should be expanded. |
#### Path Variables
| Name | Type | Description |
|------|------|-------------|
| `refund` | string |**(Required)** The ID of the refund to update. |
## Retrieve a Refund
Retrieves the details of a specific refund.

Endpoint: <https://api.stripe.com/v1/refunds/:id>.

Operation: `GET`
### Examples
**HTTPS Example**
```http
GET /v1/refunds/:refund HTTP/1.1
Host: api.stripe.com
Authorization: <YOUR_SECRET_KEY>
```
**cUrl Example:**
```bash
curl https://api.stripe.com/v1/refunds/re_1Nispe2eZvKYlo2Cd31jOCgZ \
  -u <YOUR_SECRET_KEY>: \
```
### Responses

Responses will return standard codes.

`200: OK`
```json
{
  "id": "re_1Nispe2eZvKYlo2Cd31jOCgZ",
  "object": "refund",
  "amount": 212,
  "balance_transaction": "txn_1Nispe2eZvKYlo2CYezqFhEx",
  "charge": "ch_1NirD82eZvKYlo2CIvbtLWuY",
  "created": 1692942318,
  "currency": "usd",
  "destination_details": {
    "card": {
      "reference": "123456789012",
      "reference_status": "available",
      "reference_type": "acquirer_reference_number",
      "type": "refund"
    },
    "type": "card"
  },
  "metadata": {},
  "payment_intent": "pi_1GszsK2eZvKYlo2CfhZyoZLp",
  "reason": null,
  "receipt_number": null,
  "source_transfer_reversal": null,
  "status": "succeeded",
  "transfer_reversal": null
}
```
`400: Bad Request`
```json
{
  "error": {
    "code": "parameter_invalid_integer",
    "doc_url": "https://stripe.com/docs/error-codes/parameter-invalid-integer",
    "message": "Invalid integer: <integer>",
    "param": "amount",
    "request_log_url": "https://dashboard.stripe.com/test/logs/req_UQf6XBwBl9yk66?t=1734829676",
    "type": "invalid_request_error"
  }
}
```
`401: Unauthorized`
```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid API Key provided: sk_test_********************1234"
  }
}
```
### Parameters
None.
## List all Refunds
Returns a list of existing refunds. Refunds return in sorted order, with the most recent refunds appearing first. The 10 most recent refunds are returned by default.

Endpoint: <https://api.stripe.com/v1/refunds>.

Operation: `GET`
### Examples
**HTTPS Example**
```http
GET /v1/refunds HTTP/1.1
Host: api.stripe.com
Authorization: <YOUR_SECRET_KEY>
```
**cUrl Example:**
```bash
curl https://api.stripe.com/v1/refunds \
  -u <YOUR_SECRET_KEY>: \
```
### Responses
Responses will return standard codes. 

`200: OK`
```json
{
  "object": "list",
  "url": "/v1/refunds",
  "has_more": false,
  "data": [
    {
      "id": "re_1Nispe2eZvKYlo2Cd31jOCgZ",
      "object": "refund",
      "amount": 212,
      "balance_transaction": "txn_1Nispe2eZvKYlo2CYezqFhEx",
      "charge": "ch_1NirD82eZvKYlo2CIvbtLWuY",
      "created": 1692942318,
      "currency": "usd",
      "destination_details": {
        "card": {
          "reference": "123456789012",
          "reference_status": "available",
          "reference_type": "acquirer_reference_number",
          "type": "refund"
        },
        "type": "card"
      },
      "metadata": {},
      "payment_intent": "pi_1GszsK2eZvKYlo2CfhZyoZLp",
      "reason": null,
      "receipt_number": null,
      "source_transfer_reversal": null,
      "status": "succeeded",
      "transfer_reversal": null
    }
  ]
}
```
`400: Bad Request`
```json
{
    "error": {
        "code": "resource_missing",
        "doc_url": "https://stripe.com/docs/error-codes/resource-missing",
        "message": "No such charge: 'ch_1NirD82eZvKYlo2CIvbtLWuY'",
        "param": "charge",
        "request_log_url": "https://dashboard.stripe.com/test/logs/req_HIH2jWkJGecwCk?t=1754504399",
        "type": "invalid_request_error"
    }
}
```
`401: Unauthorized`
```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid API Key provided: sk_test_********************1234"
  }
}
```
### Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| `charge` | string | ID of the charge to retrieve refunds for. If not specified, all refunds will be returned. |
| `created [gt]` | integer | Returns refunds only created during the given date intervals. |
| `created [gte]` | integer | Returns refunds only created during the given date intervals. |
| `created [lt]` | integer | Returns refunds only created during the given date intervals. |
| `created [lte]` | integer | Returns refunds only created during the given date intervals. |
| `ending_before` | string | A cursor for use in pagination. Use the object ID `ending_before` to define your place in the list. If you make a list request and recieve 100 results, starting with `obj_bar`, your subsequent call can include `ending_before=obj_bar` to fetch the previous page of the list. |
| `limit` | integer | A limit on the number of objects to be returned. Limit can range between 1 and 100. The default is 10. |
| `payment_intent` | string | ID of the PaymentIntent to retrieve refunds for. If not specified, all refunds will be returned. |
| `starting_after` | string | A cursor for use in pagination. Use the object ID `starting_after` to define your place in the list. If you make a list request and receive 100 results, ending with `obj_foo`, your subsequent call can include `starting_after=obj_foo` to fetch the next page of the list. |
| `expand[]` | array of strings | Specifies which fields in the response should be expanded. |

## Retrieve a Charge
Retrieves the details of a charge that has previously been created. Supply the unique charge ID that was created when the charge was created. Basically, this is a search request for a specific charge.

Endpoint: <https://api.stripe.com/v1/charges/:id>.

Operation: `GET`
### Examples
**HTTPS Example**
```http
GET /v1/charges/ch_1NirD82eZvKYlo2CIvbtLWuY HTTP/1.1
Host: api.stripe.com
Authorization: <YOUR_SECRET_KEY>
```
**cUrl Example:**
```bash
curl https://api.stripe.com/v1/charges/ch_1NirD82eZvKYlo2CIvbtLWuY \
  -u <YOUR_SECRET_KEY>:
```
### Responses
Responses will return standard codes.

`200: OK`
```json
{
  "id": "re_1Nispe2eZvKYlo2Cd31jOCgZ",
  "object": "refund",
  "amount": 212,
  "balance_transaction": "txn_1Nispe2eZvKYlo2CYezqFhEx",
  "charge": "ch_1NirD82eZvKYlo2CIvbtLWuY",
  "created": 1692942318,
  "currency": "usd",
  "destination_details": {
    "card": {
      "reference": "123456789012",
      "reference_status": "available",
      "reference_type": "acquirer_reference_number",
      "type": "refund"
    },
    "type": "card"
  },
  "metadata": {},
  "payment_intent": "pi_1GszsK2eZvKYlo2CfhZyoZLp",
  "reason": null,
  "receipt_number": null,
  "source_transfer_reversal": null,
  "status": "succeeded",
  "transfer_reversal": null
}
```
`404: Not Found`
```json
{
  "error": {
    "code": "resource_missing",
    "doc_url": "https://stripe.com/docs/error-codes/resource-missing",
    "message": "No such refund: 'ch_1NirD82eZvKYlo2CIvbtLWuY'",
    "param": "charge",
    "request_log_url": "https://dashboard.stripe.com/test/logs/req_HIH2jWkJGecwCk?t=1754504399",
    "type": "invalid_request_error"
  }
}
```
`401: Unauthorized`
```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "Invalid API Key provided: sk_test_********************1234"
  }
}
```
### Parameters
#### Query Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| `expand[]` | array of strings | Specifies which fields in the response should be expanded. |
#### Path Variables
| Name | Type | Description |
|------|------|-------------|
| `charge` | string |**(Required)** The ID of the refund to retrieve. |

# Conclusion
This document is meant to prove a sampling of the Stripe API documentation as I would write it as their technical writer. It includes commonly used API endpoints, examples on how to make calles to the API, the parameters to attach to them and the expected responses.

The API documentation is not exhaustive and I would not recommend using it to build a functional application. (Unless you can, in which case, let me know so I can take all the credit.)

This document was created using the Stripe API collection from April 2024 in the Postman application. Please visit the [Official Stripe API documentation](https://stripe.com/docs/api) for the most up-to-date branches and pertaining documentation.