# Import your API spec, and proxy an existing REST service
Duration: 5 mins  
Skill level: Beginner  

<a href="https://github.com/ibm-apiconnect/getting-started/tree/master/bluemix/0-prereq" target="blank">Prerequisites</a>

### Objective
This tutorial is to help you get started quickly with **API Connect** by illustrating how you can bring your existing API under management control. We'll start by importing an OpenAPI spec, and then create a passthrough API proxy for an existing REST service.

---


### Explore the sample app, and test the target endpoints
A sample _weather provider_ app has been created for this tutorial; its corresponding API specification (Swagger 2.0) can be found ![here](https://raw.githubusercontent.com/ibm-apiconnect/getting-started/master/bluemix/1a/weather-provider-api_1.0.0.yaml).
- To explore the app, click: http://gettingstartedweatherapp.mybluemix.net/
- Enter a valid 5-digit US zipcode to get the _**current weather**_ and _**today's forecast**_  
![](images/explore-weatherapp-1.png)

  - Endpoint to get the **current** weather data is:     _**https:// myweatherprovider<span></span>.mybluemix.net/current?zipcode={zipcode}**_
  - Test it out by clicking: https://myweatherprovider.mybluemix.net/current?zipcode=90210  
  ![](images/explore-weatherapp-2.png)

  - Endpoint to get **today's** forecast data is:  
   _**https:// myweatherprovider<span></span>.mybluemix.net/today?zipcode={zipcode}**_
  - Test it out by clicking: https://myweatherprovider.mybluemix.net/today?zipcode=90210  
  ![](images/explore-weatherapp-3.png)


---

### Import the sample app's OpenAPI spec to create a REST API proxy
- Log in to IBM Bluemix: https://new-console.ng.bluemix.net/login
  - In the Bluemix navigation panel (left hand), select **Services** and click **Dashboard**
  - Launch the API Connect service  
   ![](images/login-1.png)   ![](images/login-2.png)  
  - In API Connect, click on **Drafts > APIs**
  - In the **APIs** panel, click on **Add > Import API from a file or URL**  
    ![](images/import-1.png) 
 
  - We will now import the OpenAPI weather definition.  In the "Import OpenAPI (Swagger) dialog box that pops up, enter this URL:
https://raw.githubusercontent.com/ibm-apiconnect/getting-started/master/bluemix/1a/weather-provider-api_1.0.0.yaml
  - Leave the other options as default and click **IMPORT**  

    ![](images/import-2.png)  

- In the API's **Design** view, scroll down to the **Host** panel.   
_You'll notice that the Host value is set to_ ```$(catalog.host)``` _. This is the base URL for your API proxy._
- Save your API  




### Test your API proxy
###### Test with the _API Manager test tool_
- In the **Assemble** tab, select **More Actions > Generate a default product**.  
  ![](/bluemix/1a/images/generate-default-product-1.png)   
  
- Accept the default options in the **New Product** dialog pop-up, and select **Create Product**. The **Weather Provider API product** is created and published to the Sandbox catalog. A message indicating successful product generation is displayed.  
  ![](/bluemix/1a/images/generate-default-product-2.png)  
  
  ![](/bluemix/1a/images/generate-default-product-3.png) 

  _In API Connect, **Products** provide a mechanism to  group APIs that intended for a particular use. Products are published to a **Catalog**.  [Reference: API Connect glossary]_

- On the Assemble tab, click ► to test your API proxy's target invocation
  - Choose the **get /current** operation.  
  - Zipcode is a required parameter for this operation, so enter a valid US zip (e.g. 90210).  
  - Click **invoke**, and verify the response.  
  _If you run into a CORS error, follow the instructions in the error message. Click the link in the error to add the exception to your browser, and then hit the "invoke" button again_
  - The expected response is:  
    - 200 OK response
    - Current weather data for 90210  

 -   
    ![](/bluemix/1b/images/test-invoke-1.png)




###### Test with the _Explore tool_
_The Explore Tool allows users to test the correct operation of the API by enforcing any parameter requirements set in the OpenAPI definition. This enforcement is not done in the API Test Tool found in the Assemble tab so as to allow the user to verify the API behavior when the parameter is missing._
- To test your API proxy endpoints
  - Click the _Explore_ button, and select **Sandbox**
    ![](images/test-explore-1.png)
  - Click on the **GET /current** operation from the palette (on the left side)
  - Click "Try it" in the right-hand panel  
  - Enter a valid US zipcode (e.g. 90210) in the test box
  - Click **Call operation** to see the response
  ![](images/test-explore-2.png)

    ![](images/test-explore-3.png)


### Conclusion
In this tutorial, you saw how an existing REST service can be invoked through an API passthrough proxy. We started by quickly checking the availability of the sample service through the web browser. Then we created an API proxy in API Connect, and linked the proxy to the sample service to be invoked. We packaged our API into a product, published the product to catalog, and tested the proxy.

