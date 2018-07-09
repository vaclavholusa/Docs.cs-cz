# <a name="aspnet-core-web-api-controller-sample"></a>Ukázka Kontroleru rozhraní API ASP.NET Core Web

Tato ukázková aplikace se skládá z následující projekty:

- **WebApiSample.Api**: cílení na .NET Core 2.1 projektu ASP.NET Core 2.1.
- **WebApiSample.Api.Pre21**: ASP.NET Core 2.0 projekt cílí na .NET Core 2.0.
- **WebApiSample.DataAccess**: Knihovna tříd A .NET Standard 2.0 slouží jako vrstvě přístupu k datům pro 2 projekty webového rozhraní API.

Tato ukázka ilustruje variace vytvoření kontroleru webového rozhraní API:

- [Odvodit třídu z ControllerBase](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [Třída s atributem ApiControllerAttribute opatřit poznámkami](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
