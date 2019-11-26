### Cloud Foundry

The primary purpose of this endpoint is to enable integration with the Pivotal Apps Manager. This endpoint is similar to Hypermedia Actuator but is preconfigured for Apps Manager integration. When used, the Steeltoe Cloud Foundry management endpoint enables the following additional functionality on Cloud Foundry:

* Provides an alternate, secured route to the endpoints expected by Apps Manager and configured in your application
* Exposes an endpoint that can be queried to return the IDs of and links to the enabled management endpoints in the application.
* Adds Cloud Foundry security middleware to the request pipeline, to secure access to the management endpoints by using security tokens acquired from the UAA.
* Adds extension methods that simplify adding the Steeltoe management endpoints necessary for Apps Manager integration with HTTP access to the application.

When adding this management endpoint to your application, the [Cloud Foundry security middleware](https://github.com/SteeltoeOSS/Management/blob/master/src/Steeltoe.Management.EndpointCore/CloudFoundry/CloudFoundrySecurityMiddleware.cs) is added to the request processing pipeline of your application to enforce that when a request is made of any of the management endpoints, a valid UAA access token is provided as part of that request. Additionally, the security middleware uses the token to determine whether the authenticated user has permissions to access the management endpoint.

>NOTE: The Cloud Foundry security middleware is automatically disabled when your application is not running on Cloud Foundry (for example, running locally on your desktop).

#### Configure Settings

Typically, you need not do any additional configuration. However, the following table describes the additional settings that you could apply to the Cloud Foundry endpoint:

|Key|Description|Default|
|---|---|---|
|id|The ID of the Cloud Foundry endpoint|""|
|enabled|Whether to enable Cloud Foundry management endpoint|true|
|validateCertificates|Whether to validate server certificates|true|
|applicationId|The ID of the application used in permissions check|VCAP settings|
|cloudFoundryApi|The URL of the Cloud Foundry API|VCAP settings|

**Note**: **Each setting above must be prefixed with `management:endpoints:cloudfoundry`**.

#### Enable HTTP Access

The default path to the Cloud Foundry endpoint is computed by combining the global `path` prefix setting together with the `id` setting from above. The default path is `/cloudfoundryapplication`.

The coding steps you take to enable HTTP access to the endpoint differs depending on the type of .NET application your are developing.  The sections which follow describe the steps needed for each of the supported application types.

##### ASP.NET Core App

Refer to the [HTTP Access ASP.NET Core](#http-access-asp-net-core) section below to see the overall steps required to enable HTTP access to endpoints in an ASP.NET Core application.

To add the Cloud Foundry actuator to the service container, you can use the `AddCloudFoundryActuator()` extension method from [EndpointServiceCollectionExtensions](https://github.com/SteeltoeOSS/Management/blob/master/src/Steeltoe.Management.EndpointCore/CloudFoundry/EndpointServiceCollectionExtensions.cs).

To add the Cloud Foundry actuator and security middleware to the ASP.NET Core pipeline, use the `UseCloudFoundryActuator()` and `UseCloudFoundrySecurity()` extension methods from [EndpointApplicationBuilderExtensions](https://github.com/SteeltoeOSS/Management/blob/master/src/Steeltoe.Management.EndpointCore/CloudFoundry/EndpointApplicationBuilderExtensions.cs).

##### ASP.NET 4.x App

Refer to the [HTTP Access ASP.NET 4.x](#http-access-asp-net-4-x) section below to see the overall steps required to enable HTTP access to endpoints in a 4.x application.

To add the Cloud Foundry actuator endpoint, use the `UseCloudFoundrySecurity()` and `UseCloudFoundryActuator()` methods from [ActuatorConfigurator](https://github.com/SteeltoeOSS/Management/blob/master/src/Steeltoe.Management.EndpointWeb/ActuatorConfigurator.cs).

##### ASP.NET OWIN App

Refer to the [HTTP Access ASP.NET OWIN](#http-access-asp-net-owin) section below to see the overall steps required to enable HTTP access to endpoints in an ASP.NET 4.x OWIN application.

To add the Cloud Foundry actuator and security middleware to the ASP.NET OWIN pipeline, use the `UseCloudFoundryActuator()` from [CloudFoundryEndpointAppBuilderExtensions](https://github.com/SteeltoeOSS/Management/blob/master/src/Steeltoe.Management.EndpointOwin/CloudFoundry/CloudFoundryEndpointAppBuilderExtensions.cs).
and `UseCloudFoundrySecurityMiddleware()` from [CloudFoundrySecurityAppBuilderExtensions](https://github.com/SteeltoeOSS/Management/blob/master/src/Steeltoe.Management.EndpointOwin/CloudFoundry/CloudFoundrySecurityAppBuilderExtensions.cs).