# <a name="aspnet-core-response-caching-sample"></a>Ukázka ukládání do mezipaměti ASP.NET Core odpovědi

Tento příklad ukazuje použití ASP.NET Core [Middleware ukládání do mezipaměti odpovědi](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

Aplikace odpoví indexovou stránku, včetně `Cache-Control` záhlaví nakonfigurovat chování ukládání do mezipaměti. Aplikace také nastaví `Vary` záhlaví konfigurace mezipaměti, aby sloužit pouze pokud odpověď `Accept-Encoding` odpovídá záhlaví následné žádosti z původního požadavku.

Při spuštění ukázky, indexovou stránku pochází z mezipaměti při ukládání a uložená v mezipaměti pro až 10 sekund.
