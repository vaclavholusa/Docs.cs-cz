# <a name="aspnet-core-response-caching-sample-aspnet-core-2x"></a>Ukládání do mezipaměti ukázková odpověď ASP.NET Core (ASP.NET základní 2.x)

Tento příklad ukazuje použití ASP.NET Core [Middleware ukládání do mezipaměti odpovědi](xref:performance/caching/middleware) s ASP.NET Core 2.x. Ukázka 1.x ASP.NET Core, najdete v části [ASP.NET Core odpovědi ukládání do mezipaměti ukázkové (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/1.x).

Aplikace odešle `Hello World!` zprávy a spolu s aktuálním časem `Cache-Control` záhlaví nakonfigurovat chování ukládání do mezipaměti. Aplikace také odesílá `Vary` záhlaví konfigurace mezipaměti, aby sloužit pouze pokud odpověď `Accept-Encoding` odpovídá záhlaví následné žádosti z původního požadavku.

Při spuštění ukázky, odpověď pochází z mezipaměti při možné a uložené pro až 10 sekund.
