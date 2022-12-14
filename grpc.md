# gRPC

Configure Kestrel and the gRPC client to use HTTP/2 without TLS

Kestrel must configure an HTTP/2 endpoint without TLS in Program.cs:

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.WebHost.ConfigureKestrel(options =>
{
    // Setup a HTTP/2 endpoint without TLS.
    options.ListenLocalhost(<5287>, o => o.Protocols =
        HttpProtocols.Http2);
});
```

Learn more [here](https://learn.microsoft.com/en-us/aspnet/core/grpc/troubleshoot?view=aspnetcore-7.0#unable-to-start-aspnet-core-grpc-app-on-macos)

