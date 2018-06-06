---
title: Middleware v ASP.NET Core přepisování adres URL
author: guardrex
description: Další informace o adrese URL přepisování a přesměrování s Middleware přepisování adresy URL v aplikacích ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/url-rewriting
ms.openlocfilehash: a021c1e133bac6676859f5bf8eb01f3a7a8c63ed
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729249"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>Middleware v ASP.NET Core přepisování adres URL

Podle [Luke Latham](https://github.com/guardrex) a [Mikael Mengistu](https://github.com/mikaelm12)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([stažení](xref:tutorials/index#how-to-download-a-sample))

Přepisování adres URL je v rámci úprava požadavek adresy URL založené na jeden nebo více předdefinovaných pravidel. Přepisování adres URL vytváří abstrakci mezi umístění prostředků a jejich adres, takže umístění a adresy nejsou propojené úzce. Existuje několik situací, kdy je vhodné přepisování adres URL:

* Přesunutí nebo výměna prostředky serveru dočasně nebo trvale při zachování stabilní lokátory pro tyto prostředky.
* Rozdělení požadavek zpracování přes různé aplikace nebo přes oblasti jednu aplikaci.
* Odebrání, přidání nebo změna uspořádání segmenty adres URL na příchozí požadavky.
* Optimalizace veřejné adresy URL pro hledání modul optimalizace (SEO).
* Umožňuje použití popisný veřejné adresy URL pro přehledné předpovědi obsah, který bude najít klepnutím na odkaz.
* Přesměrování nezabezpečené požadavky na zabezpečení koncových bodů.
* Brání hotlinking bitové kopie.

Můžete definovat pravidla pro změny adresy URL několika způsoby, včetně Regex Apache mod_rewrite modulu pravidla a pravidla modul služby IIS a pomocí vlastního pravidla logiku. Toto téma představuje přepisování adres URL s pokyny, jak používat Middleware přepisování adresy URL v aplikacích ASP.NET Core.

> [!NOTE]
> Přepisování adres URL může snížit výkon aplikace. Kde je to vhodné, měli byste omezit počet a složitost pravidel.

## <a name="url-redirect-and-url-rewrite"></a>Adresa URL přesměrování a adresy URL přepisování

Rozdíl ve slov mezi *adresa URL přesměrování* a *přepisování adres URL* se může zdát jemně na první, ale nemá důležité zvážit celkové důsledky pro poskytování prostředků klientům. Middleware přepisování ASP.NET Core adresa URL je schopen splňuje potřeby pro obě.

A *adresa URL přesměrování* je operace na straně klienta, kde je pro přístup k prostředkům na jiné adrese pokyn klientovi. To vyžaduje round-trip k serveru. Adresa URL pro přesměrování vrácen do klienta se zobrazí v panelu Adresa prohlížeče, když klient podá nová žádost o prostředek. 

Pokud `/resource` je *přesměrováno* k `/different-resource`, požadavky klientů `/resource`. Server odpoví, že klient musí získat prostředek na `/different-resource` s stav kód označující, že přesměrování je dočasné nebo trvalé. Klient spustí nová žádost o prostředek na adrese URL pro přesměrování.

![Koncový bod služby WebAPI dočasně změnil z verze 1 (v1) na verze 2 (v2) na serveru. Klient odešle požadavek na službu na /v1/api cesta verze 1. Server odesílá zpět 302 odpověď (Found) se novou, dočasná cesta pro službu na /v2/api verze 2. Klient podá žádost o druhou na službu na adrese URL pro přesměrování. Server odpoví 200 stavový kód (OK).](url-rewriting/_static/url_redirect.png)

Při přesměrování požadavků na jinou adresu URL, které označuje, zda přesměrování trvalé nebo dočasné. 301 (trvale přesunut) kód stavu se používá kde prostředek má novou, trvalé URL a chcete do pokyn klientovi, že všechny budoucí požadavky pro prostředek by měl používat nové adrese URL. *Klient může odpověď do mezipaměti, když je obdržena 301 stavový kód.* 302 stavový kód (Found) se používá, kde přesměrování je dočasný nebo obecně subjektu Pokud chcete změnit, tak, aby klient by neměl uložit a opakovaně používat v budoucnosti adresa URL pro přesměrování. Další informace najdete v tématu [dokumentu RFC 2616: definice stavových kódů](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

A *přepisování adres URL* je operaci na straně serveru zadejte prostředek z adresy jiného prostředku. Přepisování adresu URL nevyžaduje round-trip k serveru. Rewritten adresa URL není vrácen do klienta a nezobrazí se v panelu Adresa prohlížeče. Když `/resource` je *přepsaná* k `/different-resource`, požadavky klientů `/resource`a server *interně* načte prostředek na `/different-resource`. I když může být klient schopný načíst prostředek na adresu URL rewritten, klienta nebude informováni zda prostředek existuje na rewritten adrese URL, když vytvoří požadavku a obdrží odpověď.

![Koncový bod služby WebAPI byl změněn z verze 1 (v1) verze 2 (v2) na serveru. Klient odešle požadavek na službu na /v1/api cesta verze 1. Přístup ke službě v /v2/api cesta verze 2 je přepsaná adrese URL žádosti. Služba odpoví na klienta se 200 stavový kód (OK).](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>Adresa URL třeba přepisovat ukázkové aplikace

Můžete prozkoumat funkce middlewaru přepisování adresy URL se [URL třeba přepisovat ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/). Aplikace používá přepisování a přesměrování pravidla a zobrazuje přepsaná nebo přesměrované adresu URL.

## <a name="when-to-use-url-rewriting-middleware"></a>Kdy použít adresu URL přepisování middlewaru

Použijte adresu URL přepisování Middleware, pokud nelze použít [modul přepisování adres URL](https://www.iis.net/downloads/microsoft/url-rewrite) službou IIS v systému Windows Server, [Apache mod_rewrite modulu](https://httpd.apache.org/docs/2.4/rewrite/) na serveru Apache [přepisování adres URL na Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), nebo aplikace je hostovaná na [HTTP.sys serveru](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener)). Hlavní důvody pro použití třeba přepisovat adresy URL na serveru technologie v IIS, Apache nebo Nginx je, že middleware nepodporuje úplné funkce tyto moduly a výkon middleware pravděpodobně nebude odpovídat moduly. Existují však některé funkce serveru moduly, které nepodporují s projekty ASP.NET Core, jako `IsFile` a `IsDirectory` omezení modul přepisování služby IIS. V těchto scénářích použijte místo toho middleware.

## <a name="package"></a>Balíček

Zahrnout middleware projekt, přidejte odkaz na [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) balíčku. Tato funkce je k dispozici pro aplikace, které cílí ASP.NET Core 1.1 nebo novější.

## <a name="extension-and-options"></a>Rozšíření a možnosti

Vytvoření vaší přepisování adres URL a přesměrování pravidla tak, že vytvoříte instanci `RewriteOptions` se rozšiřující metody pro každou z pravidel. Zřetězené více pravidel v pořadí, které chcete je zpracovat. `RewriteOptions` Jsou předané do Middleware přepisování adresy URL se přidá do kanálu požadavek s `app.UseRewriter(options);`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

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

---

### <a name="url-redirect"></a>Adresa URL přesměrování

Použití `AddRedirect` pro přesměrování požadavků. První parametr obsahuje vaše regulární výraz k porovnání na cestě příchozí adresy URL. Druhý parametr je náhradní řetězec. Třetí parametr, pokud existuje, určuje kód stavu. Pokud nezadáte kód stavu, bude výchozí 302 (nalezeno), která označuje, že je prostředek dočasně přesunout nebo nahradit.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

---

V prohlížeči pomocí nástrojů pro vývojáře, které jsou povolené, vytvořte žádost na ukázkové aplikace s cestou `/redirect-rule/1234/5678`. Regex odpovídá cesta k požadavku na `redirect-rule/(.*)`, a je nahrazený cestu s `/redirected/1234/5678`. Přesměrování URL budou odeslána zpět do klienta se 302 stavový kód (Found). V prohlížeči vytváří nový požadavek na adrese URL přesměrování, která se zobrazí v panelu Adresa prohlížeče. Vzhledem k tomu, že žádná pravidla v ukázkové aplikace odpovídají na adresu URL pro přesměrování, druhý požadavek obdrží odpověď 200 (OK) z aplikace a text odpovědi zobrazí adresa URL pro přesměrování. Když je adresa URL přišla zvyšuje výkon serveru *přesměrováno*.

> [!WARNING]
> Buďte opatrní při vytváření pravidel přesměrování. Přesměrování pravidla se vyhodnocují na každý požadavek pro aplikaci, včetně po přesměrování. Ať už náhodně snadno vytvořit smyčku nekonečné přesměrování.

Původní žádost: `/redirect-rule/1234/5678`

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_redirect.png)

Je volána pro část výrazu uvést v uvozovkách *skupiny zachycení*. Tečky (`.`) výrazu znamená *Porovná libovolný znak*. Hvězdička (`*`) označuje *odpovídat předchozí znak vícekrát nebo*. Proto segmenty posledních dvou cesty adresy URL, `1234/5678`, jsou zachyceny zachycení skupiny `(.*)`. Libovolná hodnota, které zadáte v adrese URL žádosti po `redirect-rule/` zachycenou této skupiny jediné zachycení.

V řetězci nahrazení zaznamenané skupiny jsou vloženy do řetězce s znak dolaru (`$`) následuje pořadové číslo zachytávání. Získá první hodnota skupiny zachycení s `$1`, sekundu s `$2`, a budou pokračovat v pořadí pro zachycení skupiny ve vašem regulární výraz. Je jenom jedna skupina zaznamenané regex pravidlo přesměrování v ukázkové aplikace, takže jenom jedné skupiny vloženého v řetězci nahrazení, který je `$1`. Když se pravidlo se použije, stane se adresa URL `/redirected/1234/5678`.

### <a name="url-redirect-to-a-secure-endpoint"></a>Adresa URL přesměrování na zabezpečený koncový bod

Použití `AddRedirectToHttps` pro přesměrování požadavků HTTP na stejné hostitele a cestu pomocí protokolu HTTPS (`https://`). Pokud je stavový kód není zadaný, middleware výchozí 302 (Found). Pokud není port zadaný, middleware výchozí `null`, což znamená, že protokol změny `https://` a klient přistupuje k prostředku na portu 443. Tento příklad ukazuje, jak nastavit stavový kód na 301 (trvale přesunut) a změnit na 5001.

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

Použití `AddRedirectToHttpsPermanent` přesměrovat nezabezpečené požadavky do stejné hostitele a cestu s zabezpečený protokol HTTPS (`https://` na portu 443). Middleware nastaví stavový kód na 301 (trvale přesunut).

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> Při přesměrování na HTTPS na portu 443 bez nutnosti další přesměrování pravidla, doporučujeme používat protokol HTTPS přesměrování Middleware. Další informace najdete v tématu [vynutit HTTPS](xref:security/enforcing-ssl#require-https) tématu.

Ukázková aplikace je schopen který ukazuje, jak používat `AddRedirectToHttps` nebo `AddRedirectToHttpsPermanent`. Add – metoda rozšíření pro `RewriteOptions`. Proveďte požadavek nezabezpečené aplikace na všechny adresy URL. Zavření prohlížeče zabezpečení upozornění, že není důvěryhodný certifikát podepsaný svým držitelem nebo vytvořte výjimku důvěřovat certifikátu.

Původní žádosti o pomocí `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_redirect_to_https.png)

Původní žádosti o pomocí `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>Přepisování adres URL

Použití `AddRewrite` k vytvoření pravidla pro přepisování adres URL. První parametr obsahuje vaše regulární výraz k porovnání na příchozí cestě adresy URL. Druhý parametr je náhradní řetězec. Třetí parametr `skipRemainingRules: {true|false}`, určuje pro middleware, zda se má vynechat přepisování další pravidla, pokud aktuální pravidlo se použije.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

---

Původní žádost: `/rewrite-rule/1234/5678`

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_rewrite.png)

První věc, kterou jste si všimli regulární výraz je stříšky (`^`) na začátku výrazu. To znamená, že odpovídající začne od začátku cesty URL.

V předchozím příkladu s tímto pravidlem přesměrování `redirect-rule/(.*)`, neexistuje žádný karátová na začátku regex; proto může předcházet znaky `redirect-rule/` v cestě pro úspěšné shodu.

| Cesta                               | Shoda |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | Ano   |
| `/my-cool-redirect-rule/1234/5678` | Ano   |
| `/anotherredirect-rule/1234/5678`  | Ano   |

Přepište pravidlo `^rewrite-rule/(\d+)/(\d+)`, pouze odpovídá cesty, pokud jsou při spuštění systému `rewrite-rule/`. Všimněte si rozdíl mezi níže přepisování pravidla a výše uvedené pravidlo přesměrování s odpovídajícím.

| Cesta                              | Shoda |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | Ano   |
| `/my-cool-rewrite-rule/1234/5678` | Ne    |
| `/anotherrewrite-rule/1234/5678`  | Ne    |

Následující `^rewrite-rule/` část výrazu, existují dvě skupiny zachycení `(\d+)/(\d+)`. `\d` Označuje, že *odpovídat číslice (number)*. Znaménko plus (`+`) znamená *odpovídat nejméně jeden z předchozí znak*. Proto adresa URL musí obsahovat číslo následované lomítko následované jiné číslo. Tyto zachycení skupiny jsou vloženy do rewritten adresu URL jako `$1` a `$2`. Přepište pravidlo náhradní řetězec umístí zachycených skupin do řetězec dotazu. Požadovanou cestu `/rewrite-rule/1234/5678` je přepsaná získat prostředek na `/rewritten?var1=1234&var2=5678`. Pokud řetězec dotazu je na původní žádost, ho se zachová, i když je přepsaná adresu URL.

Neexistuje žádná zpětná transformace k serveru a získat prostředek. Pokud existuje prostředek má načtených a vrátí klientovi s 200 (OK) stavový kód. Protože klient není přesměrován, adresu URL v adresním řádku prohlížeče se nemění. Jak jde klienta, nikdy operaci přepsání adresy URL došlo k chybě.

> [!NOTE]
> Použití `skipRemainingRules: true` kdykoli je to možné, protože odpovídající pravidla je náročný proces a snižuje dobu odezvy aplikace. Odpovědi na nejrychlejší aplikace:
> * Pořadí pravidel přepisování z nejčastěji odpovídající pravidla alespoň často odpovídající pravidlo.
> * Zpracování zbývající pravidla přeskočte, pokud je nalezena shoda a není nutná žádná další pravidla zpracování.

### <a name="apache-modrewrite"></a>Apache mod_rewrite

Použití Apache mod_rewrite pravidel s `AddApacheModRewrite`. Ujistěte se, že je soubor pravidla nasazen s aplikací. Další informace a příklady pravidel mod_rewrite najdete v tématu [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

A `StreamReader` slouží k načtení pravidla ze *ApacheModRewrite.txt* soubor s pravidly.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

První parametr má `IFileProvider`, který je k dispozici prostřednictvím [vkládání závislostí](dependency-injection.md). `IHostingEnvironment` Je vložili zajistit `ContentRootFileProvider`. Druhý parametr je cesta k souboru pravidla, která je *ApacheModRewrite.txt* v ukázkové aplikace.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

---

Ukázková aplikace přesměruje požadavky z `/apache-mod-rules-redirect/(.\*)` k `/redirected?id=$1`. Stavový kód odpovědi je 302 (Found).

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

Původní žádost: `/apache-mod-rules-redirect/1234`

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>Podporované serverových proměnných

Middleware podporuje následující proměnné serveru Apache mod_rewrite:

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

Chcete-li použít pravidla, která se týkají modul přepisování adres URL služby IIS, použijte `AddIISUrlRewrite`. Ujistěte se, že je soubor pravidla nasazen s aplikací. Nemáte přímé middlewaru, který má být použit vaše *web.config* souborů při spuštění na Windows serveru IIS. Se službou IIS, by měly být tato pravidla uložené mimo vaší *web.config* aby nedocházelo ke konfliktům s modulem přepisu služby IIS. Další informace a příklady pravidel modul přepisování adres URL služby IIS najdete v tématu [pomocí přepisování adres Url modulu 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) a [URL přepisování konfigurace odkazu na modul](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

A `StreamReader` slouží k načtení pravidla ze *IISUrlRewrite.xml* soubor s pravidly.

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

První parametr má `IFileProvider`, zatímco druhý parametr je cesta k souboru XML pravidla, která je *IISUrlRewrite.xml* v ukázkové aplikace.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

---

Ukázková aplikace přepíše požadavky od `/iis-rules-rewrite/(.*)` k `/rewritten?id=$1`. Odpověď je odeslána na klienta se 200 stavový kód (OK).

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

Původní žádost: `/iis-rules-rewrite/1234`

![Okno prohlížeče pomocí nástrojů pro vývojáře, sledování požadavků a odpovědí](url-rewriting/_static/add_iis_url_rewrite.png)

Pokud máte aktivní přepisování modulu IIS s nakonfigurovaná pravidla úrovni serveru, které by ovlivnit aplikace nežádoucího způsoby, můžete zakázat modul přepisování služby IIS pro aplikaci. Další informace najdete v tématu [moduly IIS zakázání](xref:host-and-deploy/iis/modules#disabling-iis-modules).

#### <a name="unsupported-features"></a>Nepodporované funkce

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Middleware vydané s ASP.NET Core 2.x nepodporuje následující funkce modul přepisování adres URL služby IIS:

* Odchozí pravidla
* Vlastní serverové proměnné
* Zástupné znaky
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Middleware vydané s ASP.NET Core 1.x nepodporuje následující funkce modul přepisování adres URL služby IIS:

* Globální pravidla
* Odchozí pravidla
* Přepište mapy
* CustomResponse akce
* Vlastní serverové proměnné
* Zástupné znaky
* Akce: CustomResponse
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>Podporované serverových proměnných

Middleware podporuje následující proměnné serveru modul přepisování adres URL služby IIS:

* CONTENT_LENGTH
* TYP_OBSAHU
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
> Můžete také získat `IFileProvider` prostřednictvím `PhysicalFileProvider`. Tento přístup může poskytují větší flexibilitu při umístění vaší přepisování pravidla soubory. Ujistěte se, že vaše soubory přepisování pravidel jsou nasazeny na server v cestu, kterou zadáte.
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>Pravidla na základě – metoda

Použití `Add(Action<RewriteContext> applyRule)` implementace vlastní logiky na metodu. `RewriteContext` Zpřístupní `HttpContext` pro použití ve své metodě. `context.Result` Určuje, jak další kanálu zpracování se zpracovává.

| kontext. Výsledek                       | Akce                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (výchozí) | Pokračovat v použití pravidel                                         |
| `RuleResult.EndResponse`             | Zastavte aplikaci pravidel a odeslání odpovědi                       |
| `RuleResult.SkipRemainingRules`      | Zastavte aplikaci pravidel a kontext poslat další middleware |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

---

Ukázková aplikace ukazuje metodu, který přesměruje požadavky pro cesty, která končit *.xml*. Pokud zadáte požadavek `/file.xml`, je přesměrován na `/xmlfiles/file.xml`. Kód stavu je nastavena na 301 (trvale přesunut). Pro přesměrování je potřeba explicitně nastavit stavový kód odpovědi; jinak se vrátí stavový kód 200 (OK) a přesměrování nedojde v klientovi.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

Původní žádost: `/file.xml`

![Okno prohlížeče s sledování požadavků a odpovědí pro file.xml nástroje pro vývojáře](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>Na základě IRule pravidlo

Použití `Add(IRule)` implementace vlastní logiky na třídu odvozenou z `IRule`. Pomocí `IRule` nabízí větší flexibilitu v porovnání s pomocí přístupu na základě metod pravidlo. Odvozené třídy mohou zahrnovat konstruktor, kde můžete předat parametry pro `ApplyRule` metoda.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

Hodnoty parametrů v ukázková aplikace pro `extension` a `newPath` se kontroluje, že splňují několik podmínek. `extension` Musí obsahovat hodnotu, a hodnota musí být *.png*, *.jpg*, nebo *.gif*. Pokud `newPath` není platný, `ArgumentException` je vyvolána výjimka. Pokud zadáte požadavek *image.png*, je přesměrován na `/png-images/image.png`. Pokud zadáte požadavek *image.jpg*, je přesměrován na `/jpg-images/image.jpg`. Kód stavu je nastavena na 301 (trvale přesunuto) a `context.Result` je nastavena na Zastavit zpracování pravidel a odeslání odpovědi.

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

Původní žádost: `/image.png`

![Okno prohlížeče s sledování požadavků a odpovědí pro image.png nástroje pro vývojáře](url-rewriting/_static/add_redirect_png_requests.png)

Původní žádost: `/image.jpg`

![Okno prohlížeče s sledování požadavků a odpovědí pro image.jpg nástroje pro vývojáře](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Příklady Regex

| Cílem | Řetězec Regex &<br>Příklad shody | Náhradní řetězec &<br>Příklad výstupu |
| ---- | :-----------------------------: | :------------------------------------: |
| Přepište cestu do řetězce dotazu | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| Pruhu koncové lomítko. | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| Vynutit koncové lomítko | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| Vyhněte se přepisování konkrétní požadavky | `^(.*)(?<!\.axd)$` Nebo `^(?!.*\.axd$)(.*)$`<br>Ano: `/resource.htm`<br>Ne: `/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| Změna uspořádání segmenty adres URL | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| Nahraďte segment adresy URL | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>Další zdroje

* [Spuštění aplikace](startup.md)
* [Middleware](xref:fundamentals/middleware/index)
* [Regulární výrazy v rozhraní .NET](/dotnet/articles/standard/base-types/regular-expressions)
* [Jazyk regulárních výrazů – Stručná referenční příručka](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Pomocí modulu přepisování Url 2.0 (pro službu IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [Odkaz na modul konfigurace adresy URL přepsání](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [Fórum modul přepisování adresa URL služby IIS](https://forums.iis.net/1152.aspx)
* [Ponechat jednoduchou strukturu adresy URL](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 přepisování adres URL tipy a triky](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [Lomítko nebo lomítko](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
