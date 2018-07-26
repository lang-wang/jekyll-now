---
layout: post
title: How to Integrate on-premise ADFS with Amazon Cognito User Pools?
---


Note:   Technically, you can use both user pool and identity pool for to authenticate API Gateway, but in this case we only use user pool.
The Flow has been explained  in  <https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-accessing-resources-api-gateway-and-lambda.html>
 
# How to Config Cognito ? 
1,   Create A new Cognito user pool
<https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html>
2,   federate the user pool with ADFS
    <https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-identity-federation.html>
# How to Config API Gateway?
set the authorization to  authorizer instead of none.
 
# How to Config ADFS ? 
1,  RDP to ADFs Server,  and add relying trust party
 Follow  <https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-federation-with-saml-2-0-idp.html>
 Input  <https://<yourDomainPrefix>.auth.<region>.amazoncognito.com/saml2/idpresponse>,input  urn:amazon:cognito:sp:yourUserPoolID and  
Add claim Rules to map Email
 

# How to Test it? 
open Chrome and enter https://<your_domain>/login?response_type=token&client_id=your_app_client_id&redirect_uri=your_callback_url
Detail can be found: <https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-app-integration.html>
Note:  OAuth 2.0  supports 4 types of authorization grant. (<https://tools.ietf.org/html/rfc6749#section-1.3>)
1.	authorization Code
2.	Implicit Grant
3.	Resource Owner Password Credentials
4.	Client Credentials
In our case, we have too use Implicit Grant mode, that is why we specify response type as token instead of code. Implicit Grant mode is suitable for single page application.
How to cusume the IdToken ?
put the IdToken in the Authorization header. <https://docs.aws.amazon.com/apigateway/api-reference/making-http-requests/>


Useful link: <https://aws.amazon.com/blogs/compute/saml-for-your-serverless-javascript-application-part-i/>
