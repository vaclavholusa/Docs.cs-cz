# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="f521f-101">Ukázka Kontroleru rozhraní API ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="f521f-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="f521f-102">Tato ukázková aplikace se skládá z následující projekty:</span><span class="sxs-lookup"><span data-stu-id="f521f-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="f521f-103">**WebApiSample.Api**: cílení na .NET Core 2.1 projektu ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="f521f-103">**WebApiSample.Api**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="f521f-104">**WebApiSample.Api.Pre21**: ASP.NET Core 2.0 projekt cílí na .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="f521f-104">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="f521f-105">**WebApiSample.DataAccess**: Knihovna tříd A .NET Standard 2.0 slouží jako vrstvě přístupu k datům pro 2 projekty webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f521f-105">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="f521f-106">Tato ukázka ilustruje variace vytvoření kontroleru webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="f521f-106">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="f521f-107">Odvodit třídu z ControllerBase</span><span class="sxs-lookup"><span data-stu-id="f521f-107">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="f521f-108">Třída s atributem ApiControllerAttribute opatřit poznámkami</span><span class="sxs-lookup"><span data-stu-id="f521f-108">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
