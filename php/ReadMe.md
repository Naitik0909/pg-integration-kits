# Getting Started

Download the php folder from the above repository.You can refer [here](https://stackoverflow.com/questions/7106012/download-a-single-folder-or-directory-from-a-github-repo) to download a single folder form the repository.

Extract the downloaded zip file and Place it in the root directory of your web server.Let's assume that the root directory is /var/www/html.The directory should look like /var/www/html/php/. 


## Prerequisites

you need have the below requirements running on your system to start working with the php 
integration kit.

```
1)Apache web server
2)Php 7.0

```

## Testing & Integrating 

Lets start by testing our payment gateway, 
You can access the test credentials from merchant dashboard (API access > credentials) [here](https://test.gocashfree.com/merchant/pg#api-key).

**Step 1**

  - Open the file *request.php*, and update the value of the variable *$mode* to "TEST"(for testing) or "PROD"(for production) depending on your environment.

  - Update the variable *$secretKey* with the correct value for the mode you have selected in *request.php* and *response.php* files.

**Step 2**

  - Visit *localhost/php/start.php* in the browser, fill in the details as required, give the returnUrl as *localhost/php/response.php* and click submit.

  - Once the payment page opens enter the below card details for testing purpose
  
  ```
  Card Number : 4111 1111 1111 1111
  CVV : 123
  ```
  You can enter any name,month and year.
  For more details, see [Test Data](https://docs.cashfree.com/docs/resources/#test-data).

**Step 3**

  - Simulate a failed/success transaction and you will be redirected to the *returnUrl*(given in step 2) with the transaction details.

[NOTE :](#) 

-In the file request.php, please make sure that you are using the correct integration mode. 
-Give a valid returnUrl, since all the transaction details will be sent to it.

## More Details

Since you have completed testing ,you now know how our payment gateway works.
Start integrating by changing the *$mode* in *request.php* to "PROD" and get the production credentials from (API access > credentials) [here](https://merchant.cashfree.com/merchant/pg#api-key). Also update the variable *$secretkey* in *request.php* and *response.php* files.

***Start.php***

This file collects the required details for processing a payment request.you can easily modify and integrate it into your site.Note that the form action should be as "request.php".

```html

      <form id="redirectForm" method="post" action="request.php">

```
***Request.php***

Lets see what's happening in the background. The following code generates a signature from the details given.

```php
$secretKey = "<YOUR_SECRET_KEY_HERE>";
  $postData = array( 
  "appId" => $appId, 
  "orderId" => $orderId, 
  "orderAmount" => $orderAmount, 
  "orderCurrency" => $orderCurrency, 
  "orderNote" => $orderNote, 
  "customerName" => $customerName, 
  "customerPhone" => $customerPhone, 
  "customerEmail" => $customerEmail,
  "returnUrl" => $returnUrl, 
  "notifyUrl" => $notifyUrl,
);
ksort($postData);
$signatureData = "";
foreach ($postData as $key => $value){
    $signatureData .= $key.$value;
}
$signature = hash_hmac('sha256', $signatureData, $secretKey,true);
$signature = base64_encode($signature);

```
It uses a hidden form to submit the details.If the signature matches with signature generated by us you will be allowed to proceed to the gateway.we will send the transaction details to the returnUrl given.

***Response.php***

Place/Upload this file at the correct location as per the returnUrl.It collects the transaction details and displays them.

## Support

For further queries reach us at [techsupport@gocashfree.com](techsupport@gocashfree.com). 

*****************************************************************************************