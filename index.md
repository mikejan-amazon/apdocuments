---
title: "Step 3: Add a button widget"
permalink: index.html
sidebar: one_time
type: homepage
class: first
custom_breadcrumb: Getting Started

---

The Amazon Pay and Login with Amazon product consists of the Amazon Pay and the Login with Amazon services, which are technically closely related and run on the same platform.

* Amazon Pay is a checkout option that lets the buyer choose a shipping address and payment method that is stored in their Amazon user account.
* Login with Amazon encourages your buyer to sign in using their Amazon login credentials when they arrive on your home page.

When the buyer successfully signs in to Amazon, an access token is returned. The access token is specific to your client and buyer and the type of profile data you are requesting. You will need the access token to retrieve customer profile data.

* TOC
{:toc}

## Procedure

Note: You must encode your data before processing. We recommend that you encode any data that you receive from us before outputting it in any form, like HTML, JavaScript, or in a URL. Output encoding ensures that malicious scripts or any other injected executable cannot be executed on your website. For additional details, see "Encode your data before processing" in the JavaScript section.

Follow the steps below to add a button widget for buyer authentication. Do the steps in the order presented. In particular, you must not add the widgets.js script (step 2) before completing step 1. A complete code sample is offered at the end of this topic.

Add callback for onAmazonLoginReady and onAmazonPaymentsReady to the <head> section of any page where you want the button to appear. The onAmazonPaymentsReady callback will render the button.

``` 
<head>
  <script type='text/javascript'>
    window.onAmazonLoginReady = function() {
      amazon.Login.setClientId('CLIENT-ID');
    };
    window.onAmazonPaymentsReady = function() {
                showButton();
    };
  </script>
</head>
```

For more information, see "Adding callback and widget.js code to your web pages" in the JavaScript section.
Add the widget.js file below the callback functions.

```
<head>
  <script type='text/javascript'>
    window.onAmazonLoginReady = function() {
      amazon.Login.setClientId('CLIENT-ID');
    };
    window.onAmazonPaymentsReady = function() {
                showButton();
    };
  </script>

  <script async="async" src='https://static-na.payments-amazon.com/OffAmazonPayments/us/sandbox/js/Widgets.js'>
  </script>
</head>
```

The script tag in the sample above contains an async attribute that allows the web page to load more quickly while the JavaScript code required by Amazon Pay is loaded in the background. This is the only asynchronous method supported by Amazon Pay.
Add the button code to the body of your web page by adding JavaScript code in the <body> of every page where you want the button to appear.

```
 <div id="AmazonPayButton">
 </div>
  ...
 <script type="text/javascript">
    function showButton(){
      var authRequest; 
      OffAmazonPayments.Button("AmazonPayButton", "SELLER-ID", { 
        type:  "TYPE", 
        color: "COLOR", 
        size:  "SIZE", 

        authorization: function() { 
        loginOptions = {scope: "SCOPES", 
          popup: "POPUP-PARAMETER"}; 
        authRequest = amazon.Login.authorize (loginOptions, 
          "REDIRECT-URL"); 
        } 
  }); 
</script>
```

The code sample above shows parameters that you can replace with your own values. For more information, see Button widget parameters.
Add error handling to the button code.

```
. . .
onError: function(error) { 
  // your error handling code.
  // alert("The following error occurred: " 
  //        + error.getErrorCode() 
  //        + ' - ' + error.getErrorMessage());
} 
. . .
```

For more information about the onError handler, see Handling errors from Amazon Pay widgets.
Add a logout option.

```
<script type="text/javascript">
  document.getElementById('Logout').onclick = function() {
    amazon.Login.logout();
  };
</script>
```

The logout option, often set up as a link, should delete any cached tokens and remove the user's profile information, like their name, from your website. Then your website can present the login button again. Call the amazon.Login.logout() method to delete any cached tokens and clear the session created by Amazon. Subsequent calls to amazon.Login.authorize will present the login screen by default.



## Complete code sample

```

<head>
  <script type='text/javascript'>
    window.onAmazonLoginReady = function() {
      amazon.Login.setClientId('CLIENT-ID');
    };
    window.onAmazonPaymentsReady = function() {
                showButton();
    };
  </script>
    <script async="async" src='https://static-na.payments-amazon.com/OffAmazonPayments/us/sandbox/js/Widgets.js'>
  </script>
</head>

<body>
. . .
 <div id="AmazonPayButton">
 </div>
  ...
 <script type="text/javascript">
    function showButton(){
      var authRequest; 
      OffAmazonPayments.Button("AmazonPayButton", "SELLER-ID", { 
        type:  "TYPE", 
        color: "COLOR", 
        size:  "SIZE", 

        authorization: function() { 
        loginOptions = {scope: "SCOPES", 
          popup: "POPUP-PARAMETER"}; 
        authRequest = amazon.Login.authorize (loginOptions, 
          "REDIRECT-URL"); 
        }, 
 
        onError: function(error) { 
          // your error handling code.
          // alert("The following error occurred: " 
          //        + error.getErrorCode() 
          //        + ' - ' + error.getErrorMessage());
        } 
     });
    }; 
   </script>
   . . .
   <script type="text/javascript">
     document.getElementById('Logout').onclick = function() {
       amazon.Login.logout();
     };
   </script>

</body>
```



