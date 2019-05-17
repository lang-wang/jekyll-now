in asp.net core, you can auth  website with  client. And you can generate your own self signed cert on your local. However, 
# it doesn't work after  deploy to Azure Service Fabric.


```csharp
    return new WebHostBuilder()
              .UseKestrel(opt =>
                  {
                      opt.Listen(IPAddress.Any, 443, listenOptions =>
                      {
                          listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                          listenOptions.UseHttps(new HttpsConnectionAdapterOptions
                          {
                              ServerCertificate = GetCertificateFromStore(),
                              ClientCertificateMode = ClientCertificateMode.RequireCertificate
                          });
                          listenOptions.NoDelay = true;
                      });
                  })
                  
 ```     
 
 if you RDP to the Azure Service fabric Node, and go to mmc, and check the cert, you will find 'The issuer of this certificate could not be found.'
 # How to fix:
 use real cert from trusted CA.






# other problem when hosting on IIS.  self signed cert should not be in trusted.
https://support.microsoft.com/en-us/help/942061/error-message-when-you-visit-a-web-site-that-is-hosted-on-iis-7-0-http
