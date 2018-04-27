# <a name="aspnet-core-web-api-controller-sample"></a>Ukázka kontroler API webové jádro ASP.NET

Tato ukázková aplikace se skládá z následujících projektech:

- **WebApiSample.Api**: ASP.NET Core 2.1 projektu cílení na rozhraní .NET Core 2.1.
- **WebApiSample.Api.Pre21**: cílení na rozhraní .NET 2.0 základní projekt technologii ASP.NET 2.0 jádra.
- **WebApiSample.DataAccess**: knihovny tříd A rozhraní .NET 2.0 standardní slouží jako úroveň přístupu dat pro 2 projekty webového rozhraní API.

Tato ukázka znázorňuje variace vytvoření kontroleru webového rozhraní API:

- [Odvození třídy z ControllerBase](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#derive-class-from-controllerbase)
- [Přidání poznámek ke třídě pomocí ApiControllerAttribute](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#annotate-class-with-apicontrollerattribute)