
First, let's take a look on payment gateway,

 Payment gateways accepts debit or credit cards, cryptocurrency, e-Wallets, and many other payment methods that make them `universal and user-centered`. 
 
 The most popular examples of payment gateways are PayPal, Stripe, Apple Pay, and Amazon Pay.

### Main functions of a payment gateway

- They securely validate the customer's card details and capture the data
- They ensure the funds are available and enable merchants to get paid
- They act as an interface between a merchant's website and its acquirer
- They ensure that information is securely passed from the client to the acquiring bank


#### Attacking


There are different ways for a web app  to integrate the payment functionality, depending on each ways our approach to test the application will be different.


`The most common methods are`:

- Redirecting the user to a third-party payment gateway.

- Loading a third-party payment gateway in an IFRAME on the application.

- Having a HTML form that makes a cross-domain POST request to a third-party payment gateway.

- Accepting the card details directly, and then making a POST from the application backend to the payment gateway’s API.

#### Quantity Tampering

Most e-commerce sites allow adding items to cart. There will be a track kept on items that are added.

The quantity should be a positive integer, try to make the parameter to negative if possible.

#### Obtaining public key

Try to find the public key that is used to encrypt the transaction, this might be found in application's backup or could be exposed from a directory traversal vulnerability.


You can obtain the public key from the server with the following command:

Go to terminal and type:
```bash
echo -e '\0' | openssl s_client -connect example.org:443 2>/dev/null | openssl x509 -pubkey -noout
```

once we got the key try to create an encrypted request based on the payment gateway's documentation.


#### Secure Hashes

Some apps use a secure hash of the transaction details to prevent it from tampering.

Normally it will include the details of the transaction and a secret value.

example:
```
$secure_hash = md5($merchant_id . $transaction_id . $items . $total_value . $secret)
```

The hash may be calculated like this.

This value is then added to the POST request that is send to the payment gateway, and will be verified to check if the transaction has been tampered.

The first step will be to remove the hash from the POST request.

This will succeed unless a specific configuration option has been set not to accept insecure requests.


If you know how the hash is calculated, which will be included in the payment gateway's documentation, then we can attempt to brute force the key.

Also look at the source code and configuration files to obtain the secret key.

Alternatively try to find a backup of the site so you can find a secret key by chance in there.

if once the secret is obtained, tamper with the transaction details and generate a secure hash by our own which will be accepted by the payment gateway.

#### Currency tampering

Try to change the currency if the web-app provides an option for that, for example 10 USD is not equal to 10 GPB changing the currency value might help in reducing the price.

#### Time delayed Requests

If the value of a commodity changes frequently this method would be great.

Example if the user is buying gold:
- View the current price of gold on the site.
- Initiate a buy request for 1gm of gold.
- Intercept and freeze the request.
- Wait one minutes to check the price of gold again:
    - If it increases, allow the transaction to complete, and buy the gold for less than it’s current value.
    - If it decreases, drop the request request.


#### Discount Codes

If the application supports discount codes, then there are various checks that should be carried out:

- Are the codes easily guessable (TEST, TEST10, SORRY, SORRY10, company name, etc)?
    - If a code has a number in, can more codes be found by increasing the number?
- Is there any brute-force protection?
- Can multiple discount codes be applied at once?
- Can discount codes be applied multiple times?
- Can you inject wildcard characters such as `%` or `*`?
- Are discount codes exposed in the HTML source or hidden `<input>` fields anywhere on the application?

In addition to these, the usual vulnerabilities such as SQL injection should be tested for.


#### Breaking Payment Flows

Try to disrupt the flow of payment to cause unintended behaviours,

- Modifying the shipping address after the billing details have been entered to reduce shipping costs.
- Removing items after entering shipping details, to avoid a minimum basket value.
- Modifying the contents of the basket after applying a discount code.
- Modifying the contents of a basket after completing the checkout process.


It may also be possible to skip the entire payment process for the transaction. For example, if the application redirects to a third-party payment gateway, the payment flow may be:

- The user enters details on the application.
- The user is redirected to the third-party payment gateway.
- The user enters their card details.
    - If the payment is successful, they are redirected to `success.php` on the application.
    - If the payment
    -  is unsuccessful, they are redirected to `failure.php` on the application
- The application updates its order database, and processes the order if it was successful.

#### Exploiting transaction processing fee

For each transaction the web app should pay a fixed amount of fee. This will be like $0.01 but if we make a transaction which is less than that it will be a loss to the web-app.

Usually implementations of minimum transaction amount, but if we can find a flaw in there it will be great.

some sites accepts charity where there will be no minimum amount set this will harm the web-app.


#### Test Payment Cards

Most payment gateways have a set of defined test card details, which can be used by developers during testing and debugging.  If the debugging option is turned on then these test card details could be used on live sites.


Examples of these test details for various payment gateways are listed below:

- [ADYEN - Test Card Numbers](https://docs.adyen.com/development-resources/test-cards/test-card-numbers)
- [Global pay - Test Cards](https://developer.globalpay.com/resources/test-card-numbers)
- [Stripe - Basic Test Card Numbers](https://stripe.com/docs/testing#cards)
- [World pay - Test Card Number](http://support.worldpay.com/support/kb/bg/testandgolive/tgl5103.html)


