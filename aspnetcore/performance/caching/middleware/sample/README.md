# <a name="aspnet-core-response-caching-sample"></a><span data-ttu-id="ae0dd-101">Ukázka ukládání do mezipaměti ASP.NET Core odpovědi</span><span class="sxs-lookup"><span data-stu-id="ae0dd-101">ASP.NET Core Response Caching Sample</span></span>

<span data-ttu-id="ae0dd-102">Tento příklad ukazuje použití ASP.NET Core [Middleware ukládání do mezipaměti odpovědi](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="ae0dd-102">This sample illustrates the usage of ASP.NET Core [Response Caching Middleware](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).</span></span>

<span data-ttu-id="ae0dd-103">Aplikace odpoví indexovou stránku, včetně `Cache-Control` záhlaví nakonfigurovat chování ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ae0dd-103">The app responds with its Index page, including a `Cache-Control` header to configure caching behavior.</span></span> <span data-ttu-id="ae0dd-104">Aplikace také nastaví `Vary` záhlaví konfigurace mezipaměti, aby sloužit pouze pokud odpověď `Accept-Encoding` odpovídá záhlaví následné žádosti z původního požadavku.</span><span class="sxs-lookup"><span data-stu-id="ae0dd-104">The app also sets the `Vary` header to configure the cache to serve the response only if the `Accept-Encoding` header of subsequent requests matches that from the original request.</span></span>

<span data-ttu-id="ae0dd-105">Při spuštění ukázky, indexovou stránku pochází z mezipaměti při ukládání a uložená v mezipaměti pro až 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="ae0dd-105">When running the sample, the Index page is served from cache when stored and cached for up to 10 seconds.</span></span>