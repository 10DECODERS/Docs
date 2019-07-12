## Getting Started

#### *SSO Integration for centralized Application using Auth0 as a Provider*



![](https://i1.wp.com/engaged-md.com/wp-content/uploads/2018/05/SSO-Icon.png?ssl=1)





> _Note : Slack requires Business plan upgrade for SSO Integration_

------------


**Steps**

------------


**Step - 1**.  Login to **https://manage.auth0.com**


 **Step - 2**.  Navigate to **Dashboard > SSO Integrations and click + Create New SSO Integration**

------------



![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/1.jpeg)


 **Step - 3**. Select the **Slack** option


![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/2.jpeg)


**Step - 4**.  Set the name for your SSO Integration Basically the **team** name.

**Step - 5**. Click Create

![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/3.jpeg)


**Step - 6**. Once created in the instruction page make sure to keep note of **SAML 2.0 HTTP Endpoint**

**Step - 7**.  Also the **Public Certificate** in the same page


![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/4.jpeg)


**Step - 8**. Optionally use the idp provided in the same page.

**Step - 9**. All these three will be used while setting up in slack.

**Step - 10**.  Once we have all these three in sight.

**Step - 11**.  Optional : We can configure our own database in the Connections tab in dashboard.

**Step - 12**.  Go to **Dashboard - > Universal Login** .

**Step - 13**.  Customize the login page as needed based on our company requirements


![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/5.jpeg)

**Step - 14**. Click on preview to view template changes after modification, this will be the screen where user will login with your application

![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/6.jpeg)


**Step - 15**.  Go to Dashboard once done, and click on **Connections → Database** to configure custom database if needed


![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/7.jpeg)




**Step - 16**. Configre to use own database and modify the login, create etc scrips like in below screenshot 



![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/8.jpeg)


------------


#### Javascript
````javascript
function login(email, password, callback) {
    const request = require('request');
    request.post({
        url: 'https://alien-dev.appspot.com/v1/user/login',
        body: JSON.stringify({
            email: email,
            password: password
        }),
        headers: {
            'Content-Type': 'application/json'
        },
    }, function(err, response, body) {
        if (err) return callback(err);
        if (response.statusCode === 200) {
            const user = JSON.parse(body);
            if (user.token != null) {
                callback(null, {
                    email: email,
                    user_id: email
                });
            } else {
                callback("Login Error")
            }
        } else {
            callback("Login Error");
        }
    });
}

````

------------



**Step - 17**. This way user will be prompted to the page and enter the credentials, then the script in login will be called and callbacks the response. It can use any database like mongo, mysql, postgres, or it can even be a rest Endpoint. 


![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/9.jpeg)




**Step - 18**. Once everything is done, in **auth0** side, now lets move to Slack and finish the configuration wire up 


**Step - 19**. Login to your team workspace in slack 

**Step - 20**. From your browser, click your workspace name in the top left.

**Step - 21**. Select **Administration**, then Workspace settings from the menu.



![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/10.jpeg)




**Step - 22**.  Click the **Authentication** tab.

**Step - 23**.  Click Configure next to **SAML authentication**.


![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/11.jpeg)



 
**Step - 24**.  In the top right, toggle Test mode on.

**Step - 25**.  Next to SAML SSO URL, enter your **SAML 2.0 Endpoint URL(HTTP)**.

**Step - 26**. Which we got from auth0 earlier, those three htp endpoint,**idp and public certificate**


![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/12.jpeg)




**Step - 27**. Enter your **IDP Entity ID** next to Identity Provider Issuer. 

**Step - 28**. Copy the entire **x.509 Certificate** from your identity provider and paste it into the Public Certificate field.

**Step - 29**. Click Expand next to **Advanced Options**. Choose how the SAML response from your IDP is signed. If you need an end-to-end encryption key, check the box next to Sign AuthnRequest to show the certificate.

**Step - 30**. Under Settings, decide if members can edit their profile information (like their email or display name) after **SSO** is enabled. You can also choose whether SSO is required, partially required or optional.*

**Step - 31**. Under Customize, enter a **Sign In Button Label.**

**Step - 32**. Select **Save** Configuration to finish.


![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/13.jpeg)




**Step - 33**. Once the configuration is saved, we can open our team url or share to others to login with our application like **https://alien.slack.com**
- The end users will see a page like below when they put the url on browser 


![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/14.jpeg)




**Step - 34**. When clicked n sign in button it will land up in below page
which is our cusomized login page from auth0.

![](https://raw.githubusercontent.com/10DECODERS/Docs/master/SSOIntegration/15.jpeg)


**Step - 35**. The credentials provided in this page, will pass through the login script in connection tab in auth0, which wil return a redirect url with a token for redirecitng the users to the success callback url after login. 


------------

&copy; Copyright 2019