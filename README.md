## C# JWT Validator
This project was created by starting with the new project wizard for Web APIs in Visual Studio 2022. The goal of this sample project is to create an Web API in c# that will validate the ID Token supplied by your Ionic Application that uses the Microsoft Intune plugin. 

In `Startup.cs` the following line was added:
`services.AddMicrosoftIdentityWebApiAuthentication(Configuration, "AzureAd");`
and this default line was removed:
`services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)...`

In `Controllers/WeatherForecastController.cs` we made sure to add `[Authorize]` to our API

In `appsettings.json` the `TenantId` and `ClientId` were set to the same ones used in our Ionic Application. I also added `Audience` as a property and used the same value as `ClientId`.

Finally, for this sample application I wasn't interested in Scope or Role claims so I set the property `AllowWebApiToBeAuthorizedByACL` to `true`.

Note: The default version of `Microsoft.Identity.Web` doesn't know about `AllowWebApiToBeAuthorizedByACL` so I needed to upgrade to `1.15.2`.

Now when testing the API it will validate that the token is authentic and was issued to our Ionic Application only.

## Testing the API
I use Postman to test the api. I created a new request:
`GET https://localhost:44348/WeatherForecast`
I added a header called `Authorization` with:
`bearer [id-token]`

`[id-token]` is the token you obtain from Ionic Application when this code is called:
`const authInfo = await IntuneMAM.acquireToken({ scopes: ['https://graph.microsoft.com/.default'] });`

and you use `authInfo.idToken`

## Useful Reading
This sample application was based on this Microsoft documentation.
- [Validating an ID Token](https://docs.microsoft.com/en-us/azure/active-directory/develop/id-tokens#validating-an-id-token)
- [Validating an Access Token](https://docs.microsoft.com/en-us/azure/active-directory/develop/access-tokens#validating-tokens)
- [Protected Web API](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-protected-web-api-app-configuration#starting-from-an-existing-aspnet-core-31-application)

