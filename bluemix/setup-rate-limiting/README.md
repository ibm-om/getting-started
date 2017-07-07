# Set Up Rate Limiting
**Duration**: 15 mins  
**Skill level**: Beginner  


### Objective
This tutorial shows you how to rate limit your APIs.

In **API Connect**, we use *Products* to group APIs together. Products also contain *Plans*, which are used to differentiate between API product offerings. Plans define rules associated with API subscriptions: API rate limits and whether the subscription needs to be approved.

When an application developer wants to use your APIs, they will subscribe to a plan that meets their usage needs.

In this tutorial, you will do the following:
1. Add a new rate-limiting plan to an existing API product.
2. See what happens when an application exceeds the allowed rate limits.


### Prerequisites
You must have already created an API in **API Connect**, secured with at least an API Key. In the instructions below, our starting point is the [Weather Provider API example](https://github.com/ibm-apiconnect/getting-started/blob/master/bluemix/1a/README.md), secured using a [client ID and secret](https://github.com/ibm-apiconnect/getting-started/blob/master/bluemix/2a/README.md).

Complete the following exercises before you start this tutorial:
- Part 1a: Import your API spec, and proxy an existing REST service
- Part 2a: Secure your API with a client ID and secret


---
## Launching API Connect

1. Log in to IBM Bluemix: https://console.ng.bluemix.net/login.
1. In the Bluemix navigation panel, select Services and click Dashboard.
1. Launch the API Connect service.
1. In API Connect, click on Drafts > Products.

## Exploring the Default Plan
1. If you've completed the prerequisite examples, you should see the Weather Provider API product listed. 

   ![](./images/draftproducts.png)      

1. Click the link to open the product details. The Design view opens, listing information about the product.
1. Scroll down to the Plans section of the page. A Default Plan was created when you generated this product. 

   ![](./images/defaultplanlist.png)    
1. Expand the Default plan details. Notice the rate limit (100 calls / 1 Hour) and API list, which can be expanded to show specific operations.

   ![](./images/defaultplandetails.png) 

   
## Creating a new rate-limited plan

Now that we have seen what the default plan looks like, let's create a new plan with more restrictive rate limits, to demonstrate what happens when an API consumer exceeds a plan's limits. 
1. Click the button to add a new plan.
 
    ![](./images/newplanbutton.png) 
    
    A new plan is created for you, and by default, it is set to allow unlimited usage (that is, no rate limits at all). Let's give it a more meaningful name, and set a more restrictive limit. 
2. Click the new plan (`New Plan 1`) to expand the details.
1. Click the Title field and set the plan title to: `Demo`.
1. Click the Name field and set the plan name to `demo-plan`.
1. Click the button to add a new rate-limit.
1. Rename the new rate-limit to `demo-rate-limit`, and set it to `1 / 1 Minute`.
1. Check the `Enforce hard limit` checkbox. (When this setting is enabled, an application will receive an error if it calls an API more than allowed by the subscribed plan limit).
1. Accept all the other default settings and save the product.

   ![](./images/demoplan.png) 


## Staging & publishing an updated product to the Sandbox catalog

In previous examples, you may have published your product using the test tool, which calls your API with a pre-supplied test application's credentials. However, this test application is not subject to rate limits, so we will need to create a new application here for rate limiting purposes. [Reference: API Connect Knowledge Center](https://www.ibm.com/support/knowledgecenter/SSFS6T/com.ibm.apic.toolkit.doc/tapim_create_product.html)

1. Click the button to *stage* the product to the **Sandbox** catalog. This action adds your Draft product changes to the selected catalog. We need to *publish* the product changes next, to make them available to consumers via the Developer Portal.
   ![](./images/stageproduct.png) 
1. Click the >> button to open the navigation menu.
   ![](./images/navigate.png) 
1. Open the Dashboard, then open the **Sandbox** catalog. The Weather Provider API product is listed as **Staged**.
1. Click the ellipsis, and select **Publish** from the menu.
   ![](./images/publish.png) 
1. Accept the default visibility settings and click the **Publish** button.


## Registering a new (consumer) application

1. Launch the Developer Portal. If you don't know the URL, you can find it in the Settings tab of the Sandbox catalog.

   ![](./images/devportalurl.png)
    - To provision the developer portal for the first time, select **IBM Developer Portal** from the drop-down.
    - This may take up to an hour to complete. You will get an email when your Sandbox portal is ready.
1. Log into the Portal using your app developer credentials (**not** your IBM id). ***(Create a new developer account if necessary, using a different address than your IBM id.)***
1. Click the **Apps** link on the toolbar, and click the **Create new App** button.

   ![](./images/createnewapp.png)
1. Give the application a title and click **Submit**.

   ![](./images/mymobileapp.png)
1. Save the client secret and client id displayed. This will be the only time your client secret is available for you to copy!

   ![](./images/clientidandsecret.png)



## Subscribing to an API product

1. Click the **API Products** link on the toolbar. Your Weather Provider API product is listed! 

   ![](./images/apiproducts.png)
1. Click the link to see details and options. You should see two plans available: the original Default Plan, and your new Demo plan. (If you only see one plan, return to API Connect and ensure that your product changes have been saved, staged and published to the Sandbox catalog.) 

   ![](./images/plans.png)
1. Click to **Subscribe** to the Demo plan, and select the application you just registered. Now, your application can call the APIs associated with this plan, at a rate of up to *one* API call every minute. 

We are ready to test this behavior and observe what happens when the application exceeds the specified rate.

## Calling a rate-limited API

1. On the Weather Provider API product page in the Developer Portal, click the API link.

   ![](./images/weatherproviderapi.png)
1. The page will refresh to show you details about the API, its operations, and provide a place to test it. (This is how your API consumers will discover and test out your API as well.) Notice the dark test pane, and scroll down to the first **Try this operation** section.

1. To test the `GET /current` operation, enter your application's client secret and a valid zipcode. Click the **Call operation** button. You should get a `200 OK` response, with data about current weather in that zipcode, 

   ![](./images/trythisop-1.png)

   ![](./images/response-1.png)

1. Now, before a minute is up, click the the **Call operation** button again, with a different zipcode if you like. You should get a `429 Too Many Requests` response this time.

   ![](./images/response-2.png)

1. To validate that the rate-limit resets, wait a minute, try again and confirm that you receive a valid response.


## Conclusion

Congratulations! You have successfully created a rate-limiting plan, associated it with your secure APIs, and verified that your API only responds to requests within the parameters you specified.
