---
title: "Práce s statické soubory v ASP.NET Core"
author: rick-anderson
description: "Naučte se pracovat s statické soubory v ASP.NET Core."
keywords: "ASP.NET Core, statické soubory, statické prostředky, HTML, CSS, JavaScript"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0751576a1391f26f045c3f8c42ea39c0ff6e5d9
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a>Práce s statické soubory v ASP.NET Core

<a name="fundamentals-static-files"></a>

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Statické soubory, jako je například HTML, CSS, image a JavaScript, jsou prostředky, které může aplikace ASP.NET Core poskytují přímo klientům.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="serving-static-files"></a>Zpracování statických souborů.

Statické soubory jsou obvykle umístěny v `web root` (*\<obsah kořenové > / wwwroot*) složky. V tématu [obsahu kořenové](xref:fundamentals/index#content-root) a [kořenový Web](xref:fundamentals/index#web-root) Další informace. Obecně nastavíte kořenu obsahu na aktuálním adresáři tak, aby váš projekt `web root` budou nacházet v vývoj.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

Statické soubory mohou být uloženy v libovolné složky pod `web root` a přístupu k nim s relativní cestou tohoto kořenového adresáře. Například při vytváření projektu výchozí webové aplikace pomocí sady Visual Studio existuje několik složek v rámci vytvořili *wwwroot* složky - *šablon stylů css*, *bitové kopie*, a *js*. Identifikátor URI pro přístup k obrazu v *bitové kopie* podsložky:

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

Aby mohl obsluhovat statické soubory, musíte nakonfigurovat [Middleware](middleware.md) přidat statické soubory do kanálu. Middleware se statickými soubory lze nastavit tak, že přidáte závislost na *Microsoft.AspNetCore.StaticFiles* balíčku do projektu a pak volání `UseStaticFiles` metoda rozšíření z `Startup.Configure`:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

`app.UseStaticFiles();`vytvoří soubory v `web root` (*wwwroot* ve výchozím nastavení) servable. Později zobrazí jak provádět další obsah adresáře servable s `UseStaticFiles`.

Musí zahrnovat balíček NuGet "Microsoft.AspNetCore.StaticFiles".

> [!NOTE]
> `web root`použije se výchozí hodnota *wwwroot* adresáře, ale můžete nastavit `web root` adresáři s `UseWebRoot`.

Předpokládejme, že máte projekt hierarchii, kde jsou statické soubory, které chcete poskytovat mimo `web root`. Příklad:

* Wwwroot
  * šablon stylů CSS
  * obrázky
  * ...
* MyStaticFiles
  * test.PNG

Pro žádost o přístup k *test.png*, nakonfigurujte middleware statické soubory takto:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

Požadavek na `http://<app>/StaticFiles/test.png` bude sloužit *test.png* souboru.

`StaticFileOptions()`můžete nastavit hlavičky odpovědi. Například následující kód nastaví z obsluhu statických souborů *wwwroot* složku a nastaví `Cache-Control` záhlaví, aby byly veřejně lze uložit do mezipaměti po dobu 10 minut (600 sekund):

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) metoda má k dispozici [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) balíčku. Přidat `using Microsoft.AspNetCore.Http;` k vaší *csharp* soubor, pokud je metoda není k dispozici.

![Byla přidána hlavičky odpovědi zobrazující hlavička Cache-Control](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Statický soubor autorizace

Modul statických souborů poskytuje **žádné** kontroly autorizace. Všechny soubory obsluhovat, včetně těch na *wwwroot* jsou veřejně dostupné. Do mají soubory podle autorizace:

* Uchováváte mimo *wwwroot* a libovolného adresáře, které jsou přístupné pro middleware se statickými soubory **a**

* Je poskytovat prostřednictvím akce kontroleru, vrácení `FileResult` v případě použití autorizace

## <a name="enabling-directory-browsing"></a>Povolit procházení adresářů

Procházení adresářů umožňuje uživatelům vaší webové aplikace najdete v seznamu adresářů a souborů v rámci zadaného adresáře. Procházení adresářů je ve výchozím nastavení z bezpečnostních důvodů zakázán (viz [aspekty](#considerations)). Chcete-li povolit procházení adresářů, zavolejte `UseDirectoryBrowser` metoda rozšíření z `Startup.Configure`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

A přidat požadované služby voláním `AddDirectoryBrowser` metoda rozšíření z `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

Výše uvedený kód umožňuje z procházení adresářů *wwwroot nebo obrázky* složku pomocí adresu URL http://\<aplikace > / MyImages s odkazy na všechny soubory a složky:

![procházení adresářů](static-files/_static/dir-browse.png)

V tématu [aspekty](#considerations) na bezpečnostní rizika při povolování procházení.

Poznamenejte si dva `app.UseStaticFiles` volání. První z nich je potřeba poskytovat šablon stylů CSS, bitové kopie a JavaScript v *wwwroot* složku a druhé volání pro procházení adresářů z *wwwroot nebo obrázky* složku pomocí adresu URL http://\<aplikace > / MyImages:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a>Obsluhující výchozí dokument

Nastavení výchozí domovskou stránku poskytuje místo, kde můžete spustit při návštěvě webu návštěvníky. V pořadí pro webovou aplikaci, která bude sloužit výchozí stránku aniž uživatel k plnému určení identifikátor URI, zavolejte `UseDefaultFiles` metoda rozšíření z `Startup.Configure` následujícím způsobem.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> `UseDefaultFiles`musí být volána před provedením `UseStaticFiles` k obsluze výchozí soubor. `UseDefaultFiles`je adresa URL opakované zapisovač, který není ve skutečnosti tento soubor zpracovat. Je nutné povolit middleware se statickými soubory (`UseStaticFiles`) pro tento soubor zpracovat.

S `UseDefaultFiles`, bude hledat požadavky do složky:

* default.htm
* default.HTML
* index.htm
* index.HTML

První soubor najde v seznamu se zpracuje, jako kdyby požadavek nebyl plně kvalifikovaného identifikátoru URI (i když adresu URL prohlížeče budou nadále zobrazit požadovaný identifikátor URI).

Následující kód ukazuje, jak změnit výchozí název souboru do *mydefault.html*.

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a>UseFileServer

`UseFileServer`kombinuje funkce `UseStaticFiles`, `UseDefaultFiles`, a `UseDirectoryBrowser`.

Následující kód umožňuje statické soubory a výchozí soubor ke zpracování, ale není povoleno procházení adresářů:

```csharp
app.UseFileServer();
   ```

Následující kód umožňuje statické soubory, výchozí soubory a procházení adresářů:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

V tématu [aspekty](#considerations) na bezpečnostní rizika při povolování procházení. Stejně jako u `UseStaticFiles`, `UseDefaultFiles`, a `UseDirectoryBrowser`, pokud chcete poskytovat souborů, které existují mimo `web root`, můžete vytvořit a nakonfigurovat `FileServerOptions` objekt, který můžete předat jako parametr pro `UseFileServer`. Například uděleno u následující hierarchie directory ve vaší webové aplikaci:

* Wwwroot

  * šablon stylů CSS

  * obrázky

  * ...

* MyStaticFiles

  * test.PNG

  * default.HTML

Pomocí výše uvedený příklad hierarchie, můžete chtít povolit statické soubory, výchozí soubory a procházení `MyStaticFiles` adresáře. V následující fragment kódu, který se provádí na základě jednoho volání do `FileServerOptions`.

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

Pokud `enableDirectoryBrowsing` je nastaven na `true` je nutné volat `AddDirectoryBrowser` metoda rozšíření z `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

Pomocí souboru hierarchie a výše uvedený kód:

| Identifikátor URI            |                             Odpověď  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      MyStaticFiles/test.png |
| `http://<app>/StaticFiles`              |     MyStaticFiles/default.html |

Pokud žádná výchozí hodnota s názvem soubory *MyStaticFiles* adresář, http://\<aplikace > / StaticFiles vrátí seznam s odkazy, můžete kliknout adresářů:

![Seznam statické soubory](static-files/_static/db2.PNG)

> [!NOTE]
> `UseDefaultFiles`a `UseDirectoryBrowser` bude trvat adresu url http://\<aplikace > / StaticFiles bez koncové lomítko a příčina straně klienta přesměrovat na http://\<aplikace > /StaticFiles/ (přidání do adresy koncové lomítko). Bez koncové lomítko relativní adresy URL v rámci dokumenty by být nesprávné.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

`FileExtensionContentTypeProvider` Třída obsahuje kolekci, která mapuje přípony souborů obsahu typy MIME. V následující ukázce několik přípony souborů, které jsou registrovány k známé typy MIME, budou nahrazeny ".rtf" a ".mp4" se odebere.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

V tématu [obsahu typy MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Nestandardní typy obsahu

Middleware se statickými soubory technologii ASP.NET rozumí téměř 400 typy obsahu známému souboru. Pokud uživatel požaduje soubor neznámý typ souborů, middleware se statickými soubory vrátí odpověď HTTP 404 (není nalezena). Pokud je povoleno procházení adresářů, zobrazí se odkaz na soubor, ale identifikátor URI vrátí chybu HTTP 404.

Následující kód umožňuje obsluhující neznámé typy a vykreslí Neznámý soubor jako obrázek.

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

Kód výše bude vrácen žádost o soubor s Neznámý typ obsahu jako obrázek.

>[!WARNING]
> Povolení `ServeUnknownFileTypes` představuje bezpečnostní riziko a jeho použití se nedoporučuje.  `FileExtensionContentTypeProvider`(vysvětlené dřív) poskytuje bezpečnější alternativu k obsluhovat soubory s nestandardní rozšíření.

### <a name="considerations"></a>Důležité informace

>[!WARNING]
> `UseDirectoryBrowser`a `UseStaticFiles` může se nevrací tajných klíčů. Doporučujeme vám **není** v produkčním prostředí pro procházení adresářů povolte. Dávejte pozor, o které adresáře povolíte s `UseStaticFiles` nebo `UseDirectoryBrowser` jako celý adresář a všechny podadresářích budou přístupné. Doporučujeme zachovat veřejnému obsahu v její vlastní adresář, jako  *\<obsahu kořenové > / wwwroot*, mimo zobrazení aplikací, konfigurační soubory, atd.

* Adresy URL pro obsah vystaveny s `UseDirectoryBrowser` a `UseStaticFiles` podléhají malá a velká písmena a znak omezení jejich podkladový systém souborů. Například systému Windows je malá a velká písmena, ale nejsou Mac a Linux.

* Aplikace ASP.NET Core hostované ve službě IIS použít modul ASP.NET Core předávat všechny požadavky na aplikaci, včetně žádostí pro statické soubory. Obslužné rutiny statických souborů služby IIS není použít, protože ho nepodporuje získat možnost zpracování požadavků, než jsou zpracovávány modul ASP.NET Core.

* Postup odebrání obslužné rutiny statických souborů služby IIS (na úrovni serveru nebo webu):

     * Přejděte na **moduly** funkce

     * Vyberte **iisver** v seznamu

     * Klepněte na **odebrat** v **akce** bočním panelu

>[!WARNING]
> Pokud je povoleno obslužné rutiny statických souborů služby IIS **a** ASP.NET Core modulu (ANCM) není správně nakonfigurována (například pokud *web.config* nebyla nasazena), se zpracuje statické soubory.

* Soubory kódu (včetně c# a Razor) musí být umístěny mimo projekt aplikace `web root` (*wwwroot* ve výchozím nastavení). Tím se vytvoří čisté oddělení mezi vaší aplikace obsah straně klienta a serveru straně zdrojový kód, který brání úniku kódu na straně serveru.

## <a name="additional-resources"></a>Další prostředky

* [Middleware](middleware.md)

* [Úvod do ASP.NET Core](../index.md)
