---
title: Middleware v ASP.NET Core přepisování adres URL
author: guardrex
description: Další informace o adrese URL přepsání a přesměrování s Middleware pro přepis adres URL v aplikacích ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d9f33f34f75fe7bf534146c5a426335e74635018
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326066"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Middleware v ASP.NET Core přepisování adres URL

Podle [Luke Latham](https://github.com/guardrex) a [Mikael Mengistu](https://github.com/mikaelm12)

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Přepisování adres URL je úprava žádosti o adresy URL v závislosti na jeden nebo více předdefinovaných pravidel. Přepisování adres URL vytvoří abstrakci mezi umístění prostředků a jejich adresy tak, aby umístění a adresy nejsou připojeny úzce. Existuje několik scénářů, kdy je vhodné přepisování adres URL:

* Přesunutí nebo nahrazení prostředky serveru dočasně nebo trvale při zachování stabilní lokátory pro tyto prostředky.
* Zpracování v různých aplikací nebo víc oblastech jednu aplikaci rozdělení požadavků.
* Odebrání, přidání nebo změna uspořádání segmenty adres URL na příchozí požadavky.
* Optimalizace veřejné adresy URL pro hledání modulu optimalizace (SEO).
* Umožňující použití popisný veřejné adresy URL, který pomůže uživatelům předpovědět, obsah, který se najdou pomocí následujícího odkazu.
* Přesměrování nezabezpečené požadavky na zabezpečení koncových bodů.
* Brání hotlinking bitové kopie.

Můžete definovat pravidla pro změny adresy URL několika způsoby, včetně regulárního výrazu, Apache mod_rewrite modul pravidel, pravidel modul služby IIS a pomocí logiky vlastní pravidlo. Toto téma představuje přepisování adres URL s pokyny, jak používat Middleware pro přepis adres URL v aplikacích ASP.NET Core.

> [!NOTE]
> Přepisování adres URL, může snížit výkon aplikace. Tam, kde je to proveditelné, měli omezit počet a složitost pravidel.

## <a name="url-redirect-and-url-rewrite"></a>Adresa URL pro přesměrování a URL revize

Rozdíl mezi formulace *adresy URL přesměrování* a *přepsání adresy URL* se může zdát drobným na první, ale má důležité důsledky pro poskytování prostředků pro klienty. Middleware pro přepis adres URL ASP.NET Core je schopen splnit potřebu obojí.

A *adresy URL přesměrování* je operace na straně klienta, kde klientovi je odeslán pokyn k přístupu k prostředku na jinou adresu. To vyžaduje zpátečního převodu na server. Adresa URL přesměrování, který je vrácen do klienta se zobrazí v adresním řádku prohlížeče, pokud klient odešle novou žádost o prostředku. 

Pokud `/resource` je *přesměrováno* k `/different-resource`, požadavky klientů `/resource`. Tento server odpoví, že klient by měl získat prostředek na `/different-resource` s kód stavu označující, že přesměrování je dočasný nebo trvalý. Klient spustí nového požadavku na prostředek adresy URL pro přesměrování.

![Koncový bod služby WebAPI dočasně změnila z verze 1 (v1) verze 2 (v2) na serveru. Klient odešle požadavek na službu na cestu /v1/api verze 1. Server odesílá zpět 302 odpověď (Found) se nové, dočasné cesty pro službu v /v2/api verze 2. Klient provádí druhou žádost ve službě na adrese URL přesměrování. Server jako odpověď vrátí stavový kód 200 (OK).](url-rewriting/_static/url_redirect.png)

Při přesměrování požadavků na jinou adresu URL, určujete, jestli je přesměrování trvalé nebo dočasné. 301 (trvale přesunuto) stavový kód se používá ve kterém prostředek má novou, trvalé adresy URL a chcete, aby dáte pokyn, aby klient, všechny budoucí žádosti o prostředku by měl použít novou adresu URL. *Klient může odpověď do mezipaměti, když je obdržena 301 stavový kód.* 302 stavový kód (Found) se používá, kde přesměrování je dočasná nebo obecně subjektu změnit tak, aby klient by neměly ukládat a opakovaně používat adresy URL pro přesměrování v budoucnu. Další informace najdete v tématu [dokumentu RFC 2616: definice stavových kódů](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

A *přepsání adresy URL* je operace na straně serveru k poskytování prostředků z adresy jiný prostředek. Přepisování adres URL nevyžaduje zpátečního převodu na server. Přepsaný adresa URL není vrácen do klienta a nezobrazí se v adresním řádku prohlížeče. Když `/resource` je *přepsán* k `/different-resource`, požadavky klientů `/resource`a server *interně* načte prostředek na `/different-resource`. I když se klient může být schopný načíst prostředek na přepsaný adresu URL, nebude klient informován, že prostředky existují na přepsaný adrese URL, když vytvoří jeho žádost a obdrží odpověď.

![Koncový bod služby WebAPI se změnil z verze 1 (v1) verze 2 (v2) na serveru. Klient odešle požadavek na službu na cestu /v1/api verze 1. Přístup k této službě na cestu /v2/api verze 2 je přepsán adresu URL požadavku. Služby jsou reaguje na klienta se stavovým kódem 200 (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Ukázková aplikace přepis adres URL

Můžete prozkoumat funkce Middleware přepisování adres URL se [ukázkovou aplikaci přepis adres URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/). Aplikace použije revize a přesměrování pravidla a zobrazuje přepsaný nebo přesměrované adresu URL.

## <a name="when-to-use-url-rewriting-middleware"></a>Kdy použít Middleware pro přepis adres URL

Použijte Middleware pro přepis adres URL, pokud nemůžete použít [modul přepisování adres URL](https://www.iis.net/downloads/microsoft/url-rewrite) službou IIS v systému Windows Server, [Apache mod_rewrite modulu](https://httpd.apache.org/docs/2.4/rewrite/) na serveru Apache [přepisování adres URL na serveru Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), nebo je vaše aplikace hostovaná na [serveru HTTP.sys](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)). Hlavní důvody k používání přepis adres URL serverovými technologiemi v IIS, Apache nebo Nginx se, že middleware nepodporují úplné funkce aplikace tyto moduly výkonu middleware pravděpodobně nebude odpovídat moduly. Existují však některé funkce serveru moduly, která nepracují s projekty ASP.NET Core, jako `IsFile` a `IsDirectory` omezení modulu IIS Rewrite. V těchto scénářích platí místo toho použijte middleware.

## <a name="package"></a>Balíček

Aby byly middleware ve vašem projektu, přidejte odkaz na [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) balíčku. Tato funkce je k dispozici pro aplikace, které cílí ASP.NET Core 1.1 nebo novější.

## <a name="extension-and-options"></a>Rozšíření a možnosti

Vytvoření vašeho přepsání adresy URL a přesměrovat pravidla tak, že vytvoříte instanci [RewriteOptions](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptions) třída s atributem rozšiřující metody pro každý z vašich pravidel. Zřetězit více pravidel v pořadí, které chcete je zpracována. `RewriteOptions` Jsou předány do Middleware pro přepis adres URL, protože se přidá do kanálu požadavku s `app.UseRewriter(options);`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="redirect-non-www-to-www"></a>Přesměrovat na www webové služby.

Povolení aplikace pro přesměrování jinou hodnotu než tři možnosti`www` vyžádá, aby `www`:

* [AddRedirectToWwwPermanent(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowwwpermanent) &ndash; požadavek na trvalé přesměrování `www` subdomény, pokud je požadavek jinou hodnotu než`www`. Přesměruje se [Status308PermanentRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status308permanentredirect) stavový kód.
* [AddRedirectToWww(RewriteOptions)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; přesměrovat žádosti `www` subdomény, pokud je příchozí požadavek jinou hodnotu než`www`. Přesměruje se [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) stavový kód.
* [AddRedirectToWww (RewriteOptions, Int32)](/dotnet/api/microsoft.aspnetcore.rewrite.rewriteoptionsextensions.addredirecttowww) &ndash; přesměrovat žádosti `www` subdomény, pokud je příchozí požadavek jinou hodnotu než`www`. Umožňuje poskytovat pro odpověď se stavovým kódem. Použijte pole [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) třídu pro přiřazení k `AddRedirectToWww`.

::: moniker-end

### <a name="url-redirect"></a>Adresa URL pro přesměrování

Použití `AddRedirect` pro přesměrování požadavků. První parametr obsahuje vaše regulárního výrazu pro porovnávání v cestě adresy URL příchozích. Druhý parametr je řetězci pro nahrazení. Třetí parametr, pokud jsou k dispozici, určuje kód stavu. Pokud nezadáte kód stavu, použije se výchozí 302 (Found), což znamená, že prostředek je dočasně nepřesunul ani nahradit.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

::: moniker-end

V prohlížeči pomocí nástrojů pro vývojáře, vytvořte žádost na ukázkovou aplikaci s cestou `/redirect-rule/1234/5678`. Odpovídá regulárnímu cestu požadavku na `redirect-rule/(.*)`, a cesta se nahradí `/redirected/1234/5678`. Přesměrování URL budou odeslána zpět do klienta se 302 stavovým kódem (Found). Prohlížeč odešle nový požadavek na adrese URL přesměrování, které se zobrazí v adresním řádku prohlížeče. Vzhledem k tomu, že žádná pravidla v ukázkové aplikaci odpovídat na adresu URL pro přesměrování, druhou žádost přijme odpověď 200 (OK) z aplikace a tělo odpovědi zobrazí adresu URL přesměrování. Zpětná je provedeno na server, když je adresa URL *přesměrováno*.

> [!WARNING]
> Buďte opatrní při vytváření pravidla pro přesměrování. Pro každý požadavek na aplikaci, včetně po přesměrování se vyhodnocují pravidla pro přesměrování. Je snadné neúmyslně vytvořit smyčku nekonečné přesměrování.

Původní žádosti: `/redirect-rule/1234/5678`

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_redirect.png)

Část výraz v závorkách se nazývá *skupina zachycení*. Tečka (`.`) výrazu znamená, že *odpovídá jakémukoli znaku*. Hvězdička (`*`) označuje *odpovídá předchozímu znaku nula nebo vícekrát*. Proto segmenty poslední dvě cesty adresy URL, `1234/5678`, jsou zachyceny na základě skupina zachycení `(.*)`. Libovolná hodnota je zadat v adrese URL požadavku po `redirect-rule/` zachycena této skupiny jediné zachycení.

V řetězci pro nahrazení zachycené skupiny jsou vloženy do řetězce bez znak dolaru (`$`) následované pořadové číslo pro zachytávání. Hodnota první skupiny zachycení se získá pomocí `$1`, druhý s `$2`, a budou pokračovat v práci v pořadí pro zachycení skupiny ve vaší regulární výraz. Je pouze jeden zachycené skupině regex pravidlo přesměrování v ukázkovou aplikaci, takže pouze jednu skupinu vložený v řetězci pro nahrazení, který je `$1`. Když se pravidlo použije, adresa URL stane `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Adresa URL přesměrování do zabezpečeného koncového bodu

Použití `AddRedirectToHttps` pro přesměrování požadavků protokolu HTTP na stejného hostitele a cestu pomocí protokolu HTTPS (`https://`). Pokud stavový kód není zadán, výchozí hodnota middleware 302 (Found). Pokud není port zadaný, middleware použije se výchozí `null`, což znamená, že protokol změn `https://` a klient přistupuje k prostředku na portu 443. Tento příklad ukazuje, jak nastavit stavový kód 301 (trvale přesunuto) a změňte port 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Použití `AddRedirectToHttpsPermanent` přesměrovat nezabezpečené požadavky na stejného hostitele a cestu s zabezpečený protokol HTTPS (`https://` na portu 443). Middleware nastaví stavový kód 301 (trvale přesunuto).

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Při přesměrování na HTTPS bez nutnosti další přesměrování pravidla, doporučujeme používat Middleware přesměrování protokolu HTTPS. Další informace najdete v tématu [vynucení HTTPS](xref:security/enforcing-ssl#require-https) tématu.

Ukázková aplikace je schopná představením toho, jak používat `AddRedirectToHttps` nebo `AddRedirectToHttpsPermanent`. Přidat metodu rozšíření k `RewriteOptions`. Ujistěte se, nezabezpečený požadavek na aplikaci na libovolnou adresu URL. Zavřít upozornění, že nedůvěryhodný certifikát podepsaný svým držitelem zabezpečení v prohlížeči nebo vytvořte výjimku důvěřovat certifikátům.

Původní žádost o prostřednictvím `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_redirect_to_https.png)

Původní žádost o prostřednictvím `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Přepsání adresy URL

Použití `AddRewrite` k vytvoření pravidla pro přepis adres URL. První parametr obsahuje vaše regulárního výrazu pro porovnávání na příchozí cestě adresy URL. Druhý parametr je řetězci pro nahrazení. Třetí parametr `skipRemainingRules: {true|false}`, pozná middleware, jestli se mají přeskočit další přepsání pravidla, pokud aktuální pravidlo použije.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

::: moniker-end

Původní žádosti: `/rewrite-rule/1234/5678`

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_rewrite.png)

První věc, kterou zjistíte v regulární výraz je stříšky (`^`) na začátku výrazu. To znamená, že odpovídající začne na začátku cesty URL.

V předchozím příkladu se pravidlo přesměrování `redirect-rule/(.*)`, neexistuje žádný ikonu kosočtverce na začátku regulárnímu; proto mohou předcházet žádné znaky `redirect-rule/` v cestě k porovnání.

| Cesta                               | Shoda |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Ano   |
| `/my-cool-redirect-rule/1234/5678` | Ano   |
| `/anotherredirect-rule/1234/5678`  | Ano   |

Pravidlo pro přepis adres `^rewrite-rule/(\d+)/(\d+)`, pouze odpovídá cesty, pokud jsou při spuštění systému `rewrite-rule/`. Všimněte si rozdílu v odpovídající pravidlo pro přepis adres od výše uvedené pravidlo přesměrování.

| Cesta                              | Shoda |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Ano   |
| `/my-cool-rewrite-rule/1234/5678` | Ne    |
| `/anotherrewrite-rule/1234/5678`  | Ne    |

Následující `^rewrite-rule/` část výrazu, jsou dvě skupiny zachycení `(\d+)/(\d+)`. `\d` Označuje, že *odpovídá číslici (číslo)*. Znaménko plus (`+`) znamená, že *odpovídat nejméně jeden z předchozí znak*. Proto se adresa URL musí obsahovat číslo za nímž následuje lomítko následované jiné číslo. Tyto zachytávání skupiny jsou vloženy do přepsaný adresy URL jako `$1` a `$2`. Přepište řetězci pro nahrazení pravidlo umístí zachycené skupiny do řetězec dotazu. Požadovaná cesta `/rewrite-rule/1234/5678` je přepsán získat prostředek na `/rewritten?var1=1234&var2=5678`. Pokud dotazu je k dispozici na původní požadavek, to se zachová, i když adresa URL je přepsán.

Neexistuje žádná zpětná transformace na server se získat prostředek. Pokud existuje prostředek má načíst a vrácen do klienta s kódem stavový kód 200 (OK). Protože klient není přesměrován, adresy URL v adresním řádku prohlížeče se nezmění. Jak jde klienta, nikdy operace přepisování adres URL došlo k chybě.

> [!NOTE]
> Použití `skipRemainingRules: true` kdykoli je to možné, protože pravidel je nákladný proces a zkracuje dobu odezvy aplikace. Nejrychlejší odezvu aplikace:
> * Pořadí přepsání pravidel z nejčastěji odpovídající pravidla nejméně často odpovídající pravidla.
> * Zpracování zbývající pravidla přeskočte, pokud je nalezena shoda, a je potřeba žádná další pravidla zpracování.

### <a name="apache-modrewrite"></a>Apache mod_rewrite

Použití Apache mod_rewrite pravidel s `AddApacheModRewrite`. Ujistěte se, že soubor pravidel je nasazen s aplikací. Další informace a příklady pravidel mod_rewrite najdete v tématu [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

::: moniker range=">= aspnetcore-2.0"

A `StreamReader` slouží k načtení pravidla z *ApacheModRewrite.txt* soubor pravidel.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

První parametr přebírá `IFileProvider`, která se poskytuje prostřednictvím [injektáž závislostí](dependency-injection.md). `IHostingEnvironment` Se vloží poskytnout `ContentRootFileProvider`. Druhý parametr je cesta k souboru pravidel, která je *ApacheModRewrite.txt* v ukázkové aplikaci.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

::: moniker-end

Ukázková aplikace přesměruje požadavky z `/apache-mod-rules-redirect/(.\*)` k `/redirected?id=$1`. Je stavový kód odpovědi 302 (Found).

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

Původní žádosti: `/apache-mod-rules-redirect/1234`

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_apache_mod_redirect.png)

Middleware podporuje následující proměnné na serveru Apache mod_rewrite:

* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
* HTTP_USER_AGENT
* HTTPS
* IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* SERVER_PORT
* SERVER_PROTOCOL
* ČAS
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* MODULU KPI

### <a name="iis-url-rewrite-module-rules"></a>Modul přepisování adres URL služby IIS pravidla

Pokud chcete použít pravidla, která platí pro modul přepisování adres URL služby IIS, použijte `AddIISUrlRewrite`. Ujistěte se, že soubor pravidel je nasazen s aplikací. Není přímá middlewaru, který má být použit vaše *web.config* souboru při spuštění ve Windows serveru služby IIS. Se službou IIS, tato pravidla by měly být uloženy mimo vaši *web.config* aby nedocházelo ke konfliktům s modulem IIS Rewrite. Další informace a příklady pravidel modul přepisování adres URL služby IIS najdete v tématu [pomocí přepisování adres Url modul 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) a [URL přepsání konfigurace odkazu na modul](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

::: moniker range=">= aspnetcore-2.0"

A `StreamReader` slouží k načtení pravidla z *IISUrlRewrite.xml* soubor pravidel.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

První parametr přebírá `IFileProvider`, zatímco druhý parametr je cesta k souboru XML pravidla, která je *IISUrlRewrite.xml* v ukázkové aplikaci.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

::: moniker-end

Ukázková aplikace přepíše žádosti od `/iis-rules-rewrite/(.*)` k `/rewritten?id=$1`. Odpověď je odeslána na klienta se stavovým kódem 200 (OK).

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

Původní žádosti: `/iis-rules-rewrite/1234`

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí](url-rewriting/_static/add_iis_url_rewrite.png)

Pokud máte aktivní revize modul IIS s nakonfigurovaná pravidla úrovni serveru, které by ovlivnily aplikace nežádoucí způsoby, můžete zakázat modul IIS revize pro danou aplikaci. Další informace najdete v tématu [moduly IIS zakázání](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Nepodporované funkce

::: moniker range=">= aspnetcore-2.0"

Middleware vydané s ASP.NET Core 2.x nepodporuje následující funkce modul přepisování adres URL služby IIS:

* Odchozí pravidla
* Vlastní serverové proměnné
* Zástupné znaky
* LogRewrittenUrl

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Middleware vydané s ASP.NET Core 1.x nepodporuje následující funkce modul přepisování adres URL služby IIS:

* Globální pravidla
* Odchozí pravidla
* Přepište mapy
* CustomResponse akce
* Vlastní serverové proměnné
* Zástupné znaky
* Akce: CustomResponse
* LogRewrittenUrl

::: moniker-end

#### <a name="supported-server-variables"></a>Podporované serverové proměnné

Middleware podporuje následující proměnné serveru modul přepisování adres URL služby IIS:

* CONTENT_LENGTH
* CONTENT_TYPE
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> Můžete také získat `IFileProvider` prostřednictvím `PhysicalFileProvider`. Tento přístup může poskytnout větší flexibilitu pro umístění vaší revize souborů pravidel. Ujistěte se, že soubory přepsání pravidel jsou nasazeny na server v cestě, které zadáte.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Pravidlo na základě – metoda

Použití `Add(Action<RewriteContext> applyRule)` implementovat vlastní logiku pravidla v metodě. `RewriteContext` Zpřístupňuje `HttpContext` pro použití v metodě. `context.Result` Určuje, jak další kanálu probíhá zpracování.

| kontext. Výsledek                       | Akce                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (výchozí) | Pokračovat v použití pravidel                                         |
| `RuleResult.EndResponse`             | Zastavit použití pravidel a odeslání odpovědi                       |
| `RuleResult.SkipRemainingRules`      | Zastavit použití pravidel a odeslat kontextu do dalšího middlewaru |

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

::: moniker-end

Ukázková aplikace předvádí metodu, který přesměruje požadavky pro cesty, která končit *.xml*. Pokud vytvoříte žádost o `/file.xml`, přesměruje se na `/xmlfiles/file.xml`. Stavový kód je nastaven na 301 (trvale přesunuto). Pro přesměrování je potřeba explicitně nastavit stavový kód odpovědi; v opačném případě se vrátí stavový kód 200 (OK) a přesměrování nedojde na straně klienta.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

Původní žádosti: `/file.xml`

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí pro file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Pravidlo na základě IRule

Použití `Add(IRule)` k implementaci vlastní logiky pravidla ve třídě, která je odvozena z `IRule`. Pomocí `IRule` nabízí větší flexibilitu v porovnání s využitím pravidel založených na volání metody přístupu. Odvozené třídy mohou být konstruktor, kde můžete předat parametry `ApplyRule` metody.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

::: moniker-end

Hodnoty parametrů v ukázkové aplikaci pro `extension` a `newPath` jsou kontrolovány pro splnění určitých podmínek. `extension` Musí obsahovat hodnotu, a hodnota musí být *.png*, *.jpg*, nebo *.gif*. Pokud `newPath` není platný, `ArgumentException` je vyvolána výjimka. Pokud vytvoříte žádost o *image.png*, přesměruje se na `/png-images/image.png`. Pokud vytvoříte žádost o *image.jpg*, přesměruje se na `/jpg-images/image.jpg`. Kód stavu je nastaven na 301 (trvale přesunuto) a `context.Result` je nastavena na zastavení zpracování pravidel a odeslání odpovědi.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

Původní žádosti: `/image.png`

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí pro image.png](url-rewriting/_static/add_redirect_png_requests.png)

Původní žádosti: `/image.jpg`

![Okno prohlížeče s vývojářskými nástroji pro sledování požadavků a odpovědí pro image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Příklady regulární výraz

| Cíl | Řetězec regulárního výrazu &<br>Příklad shody | Náhradní řetězec &<br>Příklad výstupu |
| ---- | :-----------------------------: | :------------------------------------: |
| Přepsat cestu do řetězce dotazu | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Odstranit koncové lomítko | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Vynucení adresy koncové lomítko | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Vyhněte se přepsání určité žádosti | `^(.*)(?<!\.axd)$` Nebo `^(?!.*\.axd$)(.*)$`<br>Ano: `/resource.htm`<br>Ne: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Změna uspořádání segmenty adres URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Nahraďte segment adresy URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Další zdroje

* [Spuštění aplikace](startup.md)
* [Middleware](xref:fundamentals/middleware/index)
* [Regulární výrazy v rozhraní .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Jazyk regulárních výrazů – Stručná referenční příručka](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Pomocí modulu přepisování adres Url 2.0 (pro IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Odkaz na modul konfigurace adresy URL revize](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Fórum modulu přepsání adresy URL služby IIS](https://forums.iis.net/1152.aspx)
* [Zachovat jednoduchá struktura adresy URL](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 přepisování adres URL tipy a triky](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Lomítko nebo lomítko](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
