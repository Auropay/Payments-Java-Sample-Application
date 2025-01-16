# Auropay SDK

The Auropay SDK provides a streamlined integration for merchants to interact with Auropay's payment APIs. This SDK abstracts complex API interactions, enabling merchants to focus on building great user experiences. It includes support for creating payment links, refunds, querying payment status, and generating payment QR codes.

---

## Features

- **Create Payment Link**: Simplify generating payment links with extensive customization options.
- **Create Refund**: Enable seamless refund processing for orders.
- **Get Payment Status by Transaction ID or Reference ID**: Retrieve transaction details easily.
- **Generate Payment QR Code**: Support for dynamic QR code generation for payments.
- **Secure and User-friendly**: Built with security and ease of use in mind.
- **Comprehensive Documentation**: Includes detailed examples and configuration guides for easy onboarding.

---

## Getting Started

### Requirements

- **Programming Language**: Compatible with [supported language versions] (e.g., PHP, .NET, Node.js, Java, etc.).
- **API Keys**: Access your API key and secret from the Auropay merchant dashboard.

---

### Installation

Add the Auropay SDK to your project by including it as a dependency in your maven dependency.

```xml

<dependency>
  <groupId>net.auropay</groupId>
  <artifactId>payments-java</artifactId>
  <version>0.0.1</version>
</dependency>
```

---

### Prerequisites for Configuration

```properties
API_KEY 	: "FF************************E3A8"
API_SECRET 	: "wqr0i2O******************************ANtO1TA="
```

---

## Usage Examples

### 1. Initialize the AuropayConnect Object

```java
AuropayConnect auropay = new AuropayConnect(API_KEY, API_SECRET, ENVIRONMENT.UAT, AuropayConnect.ENVIRONMENT.UAT);
```

---

### 2. Create Payment Link

**Method:** `createPaymentLink`

This method utilizes the settings provided in the `PaymentConfigBuilder` to generate a payment link. The returned JSONObject contains details such as the payment link URL, expiration time, and other relevant metadata. Following methods can be utilized to create `PaymentConfigBuilder` object and pass on to `createPaymentLink` method.

| **Methods**                    | **Data Types** | **Details**                                                                                                                                                                                                                               |
| ------------------------------------ | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `setExpireOn`                      | String               | Time when the generated link will expire (e.g., "31-01-2025 23:59:59").                                                                                                                                                                         |
| `setAmount`                        | double               | Transaction amount for the payment link.                                                                                                                                                                                                        |
| `setCustomers`                     | Array<Customer> | Requires Array of Customer details.<br /><br /> `ArrayList<Customer> customers = new ArrayList<>();`<br />`Customer customer = new Customer("first_name", "last_name","your@email.com", "9999999999");` <br /> `customers.add(customer);` |
| `setInvoiceNumber` (optional)      | String               | Custom invoice number to show on payment form details.                                                                                                                                                                                          |
| `setCallbackParameters`            | CallbackParameters   | Requires callback object<br /><br /> `CallbackParameters params = new CallbackParameters("ref_no", "Order", "http://your_domin.com/callback");`                                                                                               |
| `setShortDescription` (optional)  | String               | Brief description of the payment.                                                                                                                                                                                                               |
| `setTitle`                         | String               | Title for the payment link.                                                                                                                                                                                                                     |
| `Settings`                         | Settings               | Create a `Settings` object and pass if displaySummary is required or not as boolean.  <br /><br /> `Settings settings = new Settings(false);` <br /> `builder.setSettings(settings);` |
| `setResponseType`                  | int                  | Desired response format (e.g., JSON).|
| `setPaymentDescription` (optional) | String               | Detailed description of the payment.                                                                                                                                                                                                            |

**Code Snippet:**

```java
try
{
	PaymentConfigBuilder builder = new PaymentConfigBuilder();
	builder.setAmount(10);
	builder.setTitle("your_unique_id");
	builder.setShortDescription("your_short_description");
	builder.setPaymentDescription("your_detailed_description");
	builder.setInvoiceNumber("your_invoice");
	builder.setExpireOn("23-10-2026 18:00:00");
	builder.setResponseType(1);

	ArrayList<Customer> customers = new ArrayList<>();
	Customer customer = new Customer("FirstName", "LastName","your@email.com", "your_number");
	customers.add(customer);builder.setCustomers(customers);

	CallbackParameters params = new CallbackParameters("your_unique_id", "Order", "http://www.your_callback.com");
	builder.setCallbackParameters(params);

	Settings settings = new Settings(false);
	builder.setSettings(settings);


	try {
		JSONObject result =  connect.createPaymentLink(builder);
		System.out.println(result);
	} 
	catch (IOException e) {
		throw new RuntimeException(e);
	}
}
catch (AuropayConnectError error)
{
	System.out.println(error);
}
```

---

### 3. Create Refund

**Method:** `createRefund`

| **Input Parameters** | **Data Types** | **Details**                               |
| -------------------------- | -------------------- | ----------------------------------------------- |
| `transactionId`                | String               | Unique identifier for the order to be refunded. |
| `amountToRefund`                 | decimal              | Amount to refund. Should be more then 1 and less than `originalAmount`                      |
| `originalAmount`                 | decimal              | Amount of original transaction. |
| `remark` (optional)     | String               | Reason for the refund.                          |

**Code Snippet:**

```java
JSONObject result = connect.createRefund("c1416654-XXXX-XXXX-XXXX-XXXXXXX1333", 100.5, "Reason for refund");
System.out.println(result);
```

---

### 4. Get Payment Status by Transaction ID

**Method:** `getTransactionById`

| **Input Parameters** | **Data Types** | **Details**                      |
| -------------------------- | -------------------- | -------------------------------------- |
| `transactionId`          | String               | Unique identifier for the transaction. |

**Code Snippet:**

```java
JSONObject result = auropay.getTransactionById("c1416654-XXXX-XXXX-XXXX-XXXXXXX1333");
System.out.println(result);
```

---

### 5. Create Payment QR Code

**Method:** `createQRCode`

This method utilizes the settings provided in the `PaymentConfigBuilder` to generate a payment link. The returned JSONObject contains details such as the payment link URL, expiration time, and other relevant metadata. Following methods can be utilized to create `PaymentConfigBuilder` object and pass on to `createQRCode` method.

| **Methods**                    | **Data Types** | **Details**                                                                                                                                                                                                                               |
| ------------------------------------ | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `setExpireOn`                      | String               | Time when the generated link will expire (e.g., "31-01-2025 23:59:59").                                                                                                                                                                         |
| `setAmount`                        | double               | Transaction amount for the payment link.                                                                                                                                                                                                        |
| `setCustomers`                     | Array<Customer> | Requires Array of Customer details.<br /><br /> `ArrayList<Customer> customers = new ArrayList<>();`<br />`Customer customer = new Customer("first_name", "last_name","your@email.com", "9999999999");` <br /> `customers.add(customer);` |
| `setInvoiceNumber` (optional)      | String               | Custom invoice number to show on payment form details.                                                                                                                                                                                          |
| `setCallbackParameters`            | CallbackParameters   | Requires callback object<br /><br /> `CallbackParameters params = new CallbackParameters("ref_no", "Order", "http://your_domin.com/callback");`                                                                                               |
| `setShortDescription` (optional)  | String               | Brief description of the payment.                                                                                                                                                                                                               |
| `setTitle`                         | String               | Title for the payment link.                                                                                                                                                                                                                     |
| `Settings`                         | Settings               | Create a `Settings` object and pass if displaySummary is required or not as boolean.  <br /><br /> `Settings settings = new Settings(false);` <br /> `builder.setSettings(settings);` |
| `setResponseType`                  | int                  | Desired response format (e.g., JSON).|
| `setPaymentDescription` (optional) | String               | Detailed description of the payment.                                                                                                                                                                                                            |

**Code Snippet:**

```java
try
{
	PaymentConfigBuilder builder = new PaymentConfigBuilder();
	builder.setAmount(10);
	builder.setTitle("your_unique_id");
	builder.setShortDescription("your_short_description");
	builder.setPaymentDescription("your_detailed_description");
	builder.setInvoiceNumber("your_invoice");
	builder.setExpireOn("23-10-2026 18:00:00");
	builder.setResponseType(1);

	ArrayList<Customer> customers = new ArrayList<>();
	Customer customer = new Customer("FirstName", "LastName","your@email.com", "your_number");
	customers.add(customer);builder.setCustomers(customers);

	CallbackParameters params = new CallbackParameters("your_unique_id", "Order", "http://www.your_callback.com");
	builder.setCallbackParameters(params);

	Settings settings = new Settings(false);
	builder.setSettings(settings);


	try {
		JSONObject result =  connect.createPaymentLink(builder);
		System.out.println(result);
	} 
	catch (IOException e) {
		throw new RuntimeException(e);
	}
}
catch (AuropayConnectError error)
{
	System.out.println(error);
}
```
---
## License

Distributed under the Unlicense License. See `LICENSE.txt` for more information.


<br />
<br />