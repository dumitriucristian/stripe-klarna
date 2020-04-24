stripe-demo

A small demo stripe klarna app that use Symfony with phpfmp server.

How it works:
 We need two distinct components. The klarna widget and the stripe source object.

 The klarna widget:
  The klarna widget gives us access to the klarna object that has only two purposes:
  To Generates the form throgh the load method and Authorize the payment. 
  The klarna widget is just a small peace of javascript code that will generate the form, that will guide the user through the payment     flow. When the user interact with the form, klarna is sending the data to stripe. Stripe will update the source object, that will       hold all the info about the payment status.

  The stripe source:
  The stripe source is a stripe endpoint that can generate and update a stripe object that contain all the payment information.
  Through the stripe source we can manage the payment flow. 

  
  
Requirements
  - docker  
  - composer
 
Install
  1. download or clone 
  2. in the root folder run docker-compose build
  3. use composer install 
  4. access http://localhost:8080

Files:  
  For testing in postman import json file:  
  Klarna-SLICE.postman_collection.json

Stripe Documentation available here: https://stripe.com/docs/sources/klarna

There are two different implementations:
 Branch master has the most simple implementation and a second a little bit more complex called service-test. 
Switch branch to test both of them.


Stripe documentation for source here:
https://stripe.com/docs/sources/klarna

Steps:
Create a stripe source 
Stripe documentation here
Stripe documentation for create source api here:
https://stripe.com/docs/api/sources/create
   Data required:
      Payment type as type = “klarna”
      amount,
      currency,
      order deatils as source_order[items][item_detail(type quantity amount currency description)]
   Data returned:
     Source object:
       source id as id, 
       klarna token as klarna_token,
       klarna payment methods available as payment_method_categorie,
      the order details as source_order

 Create a klarna widget to load and authorize the payment.
 Klarna documentation here: https://developers.klarna.com/documentation/klarna-payments/single-call-descriptions/load-klarna-payments/


Load the widget on the page and 
Authorize the payment
Create a stripe charge 

Create a source object making a request to: https://api.stripe.com/v1/sources. This requires a source_order object. If the source_order is missing than payin4 payment will not be available. 
The request-response will provide a source object that will have the klarna token required in the second step. The authorization of the payment. 

Create a Charge to charge the payment if the authorize token is received from the klarna widget.
Charge details here:
https://stripe.com/docs/api/charges/create
Charge request need the source id and the source order

If the charge is successful it will generate a charge id that can be used for a refund if required.
A full working demo  can be found on github here:
https://github.com/dumitriucristian/stripe-demo/tree/service-test 

A simpler working demo can be found on github here:
https://github.com/dumitriucristian/stripe-demo 
 

This is WIP and new branch with different implementation will be added.
