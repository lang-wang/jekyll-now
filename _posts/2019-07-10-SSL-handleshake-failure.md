# Backgroud:
Migrate a api https://localhost/api/ping from ASP.NET to ASP.NET CORE(IIS to Kestrel). 


# Problem:

it works fine when you access via broswer,but it doesn't work when you hit via code
```powershell
Invoke-RestMethod -Method 'Get' -Uri 'https://localhost/api/ping'
```

it will return connect being closed.

# Reason: 

it is because of SSL handshake failure,  when you hit via broswer,the ssl protocol is not as the same as via code.


it works fine if the api is a pure ASP.NET CORE solution targeting to .NET CORE 2.2, but in my case ,it is a ASP.NET CORE Solution targeting to .NET 4.6

# Fix: 
1. upradate to .NET 4.7
2. set SslProtocols = SslProtocols.None,

```csharp
 opt.Listen(IPAddress.Any, sslPort, listenOptions =>
                                            {
                                                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                                                listenOptions.UseHttps(new HttpsConnectionAdapterOptions
                                                {
                                                    SslProtocols = SslProtocols.None,// this is key point
                                                    ServerCertificate = GetCertificateFromStore(serviceContext),
                                                    ClientCertificateMode = ClientCertificateMode.RequireCertificate
                                                });
                                                listenOptions.NoDelay = true;
```

Detail: 
1. https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls
2. https://docs.microsoft.com/en-us/dotnet/api/system.security.authentication.sslprotocols?view=netframework-4.8
