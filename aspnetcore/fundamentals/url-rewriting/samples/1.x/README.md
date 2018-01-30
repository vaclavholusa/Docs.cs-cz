# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a>Adresa URL ASP.NET Core přepisování ukázkové (ASP.NET základní 1.x)

Tento příklad ukazuje využití ASP.NET Core 1.x Middleware přepisování adresy URL. Aplikace se dozvíte adresy URL přesměrování a možnosti přepisování adres URL. Ukázka 2.x ASP.NET Core, najdete v části [ASP.NET základní adresa URL přepisování ukázkové (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).

Při spuštění ukázky, odpověď se zpracuje zobrazující adresu URL přepsaná nebo přesměrované, když jedním z pravidel se použije k adrese URL žádosti.

## <a name="examples-in-this-sample"></a>Příklady v této ukázce

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Úspěch stavový kód: 302 (nalezeno)
  - Příklad (přesměrování): **/redirect-rule / {capture_group}** k **/redirected/ {capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Úspěch stavový kód: 200 (OK)
  - Příklad (revize): **/rewrite-rule / {capture_group_1} / {capture_group_2}** k **/ přepsaná? var1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Úspěch stavový kód: 302 (nalezeno)
  - Příklad (přesměrování): **/apache-mod-rules-redirect / {capture_group}** k **/ přesměrováno? id = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Úspěch stavový kód: 200 (OK)
  - Příklad (revize): **/iis-rules-rewrite / {capture_group}** k **/ přepsaná? id = {capture_group}**
* `Add(RedirectXMLRequests)`
  - Úspěch stavový kód: 301 (trvale přesunut)
  - Příklad (přesměrování): **/file.xml** k **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Úspěch stavový kód: 301 (trvale přesunut)
  - Příklad (přesměrování): **/image.png** k **/png-images/image.png**
  - Příklad (přesměrování): **/image.jpg** k **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Použití`PhysicalFileProvider`
Můžete také získat `IFileProvider` tak, že vytvoříte `PhysicalFileProvider` k předání do `AddApacheModRewrite()` a `AddIISUrlRewrite()` metody:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Zabezpečené rozšíření přesměrování
Tato ukázka obsahuje `WebHostBuilder` konfigurace pro aplikaci, aby používala adresy URL (**https://localhost:5001**, **https://localhost**) a testovací certifikát (**testCert.pfx**) pomoct v těchto zkoumat přesměrování metody. Přidat kterýkoliv z nich k `RewriteOptions()` v **Startup.cs** studovat jejich chování.

Metoda | Stavový kód | port
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | Null (465)
`.AddRedirectToHttps()` | 302 | Null (465)
`.AddRedirectToHttps(301)` | 301 | Null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
