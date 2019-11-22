# AzureJWT in Apex #
Heavily based on the work at https://github.com/salesforceidentity/jwt but customized to work with Azure using the `client_credentials` flow.

## Example ##
```
final String azureClientId = '88d888a5-0cf4-473a-b9a0-7c88e6fc888e';
final String azureTenantId = 'b34feb2b-132f-4322-af1d-c888f5d888d0';
final String azureCertThumbprint = '4rElsDFTysrbKhB0zTsrRNSxT6s=';
final String azureScopes = '5384888d-868f-442b-b1b3-8688807de914/.default';

// create JWT with certificate from keys mgmt and set the x5t in the header to the 
// thumbprint of the cert as expected by Azure
AzureJWT jwt = new AzureJWT();
jwt.cert = 'JWT_Callout_Certificate';
jwt.iss = azureClientId;
jwt.sub = azureClientId;
jwt.aud = 'https://login.microsoftonline.com/' + azureTenantId + '/oauth2/v2.0/token';
jwt.x5t = azureCertThumbprint;

// invoke the flow and obtain an access_token
final String access_token = AzureJWTBearerFlow.getAccessToken(azureClientId, azureTenantId, azureScopes, jwt);

// use the access token
HttpRequest req = new HttpRequest();
req.setEndpoint('https://foo-functions-demo.azurewebsites.net/api/MyFunction?name=Salesforce');
req.setMethod('GET');
req.setHeader('Authorization', 'Bearer ' + access_token);
Http http = new Http();
HTTPResponse res = http.send(req);
System.debug(res.getBody());
```
