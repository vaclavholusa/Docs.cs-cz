---
title: Statické soubory v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak a zabezpečení statické soubory a nakonfigurovat statický soubor, který je hostitelem chování middlewaru ve webové aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: f0d34b5b64235d136f7df1b3ffdbb9fb10eca316
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="static-files-in-aspnet-core"></a>Statické soubory v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Scott Addie](https://twitter.com/Scott_Addie)

Statické soubory, jako je například HTML, CSS, Image a JavaScript, jsou prostředky, které aplikace ASP.NET Core slouží přímo na klienty. Některé konfigurace je potřeba povolit ke zpracování těchto souborů.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Poskytovat statické soubory

Statické soubory jsou uloženy v kořenovém adresáři projektu na web. Výchozí adresář je  *\<content_root > / wwwroot*, ale může být změněna prostřednictvím [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) metoda. V tématu [obsahu kořenové](xref:fundamentals/index#content-root) a [kořenový Web](xref:fundamentals/index#web-root) Další informace.

Hostitel webové aplikace musí být upozorněni obsahu kořenového adresáře.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x/)

`WebHost.CreateDefaultBuilder` Metoda nastaví kořenu obsahu k aktuálnímu adresáři:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Nastavit kořenu obsahu k aktuálnímu adresáři vyvoláním [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) uvnitř `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

Statické soubory jsou přístupné prostřednictvím cesta relativní vůči kořenovému adresáři web. Například **webové aplikace** šablona projektu obsahuje několik složek v rámci *wwwroot* složky:

* **wwwroot**
  * **šablon stylů CSS**
  * **Bitové kopie**
  * **js**

Pro přístup k souboru ve formátu URI *bitové kopie* je podsložka *http://\<server_address > /images/\<image_file_name >*. Například *http://localhost:9189/images/banner3.svg*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Pokud cílení na rozhraní .NET Framework, přidejte [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) balíčku do projektu. Pokud cílení na .NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) zahrnuje tento balíček.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Přidat [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) balíčku do projektu.

---

Konfigurace [middleware](xref:fundamentals/middleware/index) který povolí zpracování statických souborů.

### <a name="serve-files-inside-of-web-root"></a>Soubory uvnitř kořenový web poskytovat

Vyvolání [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metoda v rámci `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

Bezparametrový `UseStaticFiles` přetížení metody označí soubory v kořenové webové jako servable. Následující odkazy značek *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Soubory mimo kořenový web poskytovat

Vezměte v úvahu hierarchie adresářů, ve kterém obsluhovat statické soubory nacházejí mimo kořenový web:

* **wwwroot**
  * **šablon stylů CSS**
  * **Bitové kopie**
  * **js**
* **MyStaticFiles**
  * **Bitové kopie**
      * *banner1.svg*

Žádost o přístup *banner1.svg* souboru nakonfigurováním middleware se statickými soubory:

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

V předchozí kód *MyStaticFiles* hierarchie adresářů je vystaven veřejně prostřednictvím *StaticFiles* segment identifikátoru URI. Požadavek na *http://\<server_address > /StaticFiles/images/banner1.svg* slouží *banner1.svg* souboru.

Následující odkazy značek *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>Nastavit hlavičky HTTP odpovědi

A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) objekt můžete použít k nastavení hlavičky HTTP odpovědi. Kromě konfigurace statických souborů slouží z kořenový web, následující kód nastaví `Cache-Control` hlavičky:

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) metoda nachází [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) balíčku.

Soubory byly provedeny veřejně lze uložit do mezipaměti po dobu 10 minut (600 sekund):

![Byla přidána hlavičky odpovědi zobrazující hlavička Cache-Control](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Statický soubor autorizace

Middleware se statickými soubory neposkytuje kontroly autorizace. Všechny soubory obsluhovat, včetně těch na *wwwroot*, jsou veřejně přístupné. Do mají soubory podle autorizace:

* Uchováváte mimo *wwwroot* a libovolného adresáře, které jsou přístupné pro middleware se statickými soubory **a**
* Sloužit je prostřednictvím metody akce, ke které se použije autorizace. Vrátí [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) objektu:

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Povolit procházení adresářů

Procházení adresářů umožňuje uživatelům vaší webové aplikace najdete v seznamu adresářů a souborů v rámci zadaného adresáře. Procházení adresářů je ve výchozím nastavení z bezpečnostních důvodů zakázán (viz [aspekty](#considerations)). Povolit procházení vyvoláním adresářů [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) metoda v `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Přidat požadované služby vyvoláním [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) metoda z `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

Předchozí kód umožňuje z procházení adresářů *wwwroot nebo obrázky* složky pomocí adresy URL *http://\<server_address > / MyImages*, s odkazy na všechny soubory a složky:

![procházení adresářů](static-files/_static/dir-browse.png)

V tématu [aspekty](#considerations) na bezpečnostní rizika při povolování procházení.

Poznamenejte si dva `UseStaticFiles` volá v následujícím příkladu. Povolí zpracování statických souborů v první volání *wwwroot* složky. Druhé volání umožňuje z procházení adresářů *wwwroot nebo obrázky* složky pomocí adresy URL *http://\<server_address > / MyImages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Výchozí dokument

Nastavení výchozí domovskou stránku poskytuje návštěvníky logické výchozí bod při návštěvě webu. Chcete-li poskytovat výchozí stránku bez plně kvalifikovaného identifikátoru URI uživatel, volejte [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metoda z `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles` musí být volána před provedením `UseStaticFiles` k obsluze výchozí soubor. `UseDefaultFiles` je RW adresu URL, která ve skutečnosti není tento soubor zpracovat. Povolit middleware se statickými soubory přes `UseStaticFiles` pro tento soubor zpracovat.

S `UseDefaultFiles`, požadavky na složku Hledat:

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

První soubor najde v seznamu je zpracovat, jako by byl plně kvalifikovaného identifikátoru URI požadavku. Adresu URL prohlížeče pokračuje tak, aby odrážela požadovaný identifikátor URI.

Následující kód změní výchozí název souboru do *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) kombinuje funkce `UseStaticFiles`, `UseDefaultFiles`, a `UseDirectoryBrowser`.

Následující kód povolí zpracování statických souborů a výchozí soubor. Procházení adresářů není povoleno.

```csharp
app.UseFileServer();
```

Následující kód staví na bez parametrů přetížení povolením procházení adresářů:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Vezměte v úvahu následující hierarchii:

* **wwwroot**
  * **šablon stylů CSS**
  * **Bitové kopie**
  * **js**
* **MyStaticFiles**
  * **Bitové kopie**
      * *banner1.svg*
  * *default.html*

Následující kód umožňuje statické soubory, výchozí soubory a procházení adresářů z `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser` musí být voláno, když `EnableDirectoryBrowsing` hodnota vlastnosti je `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

Použití souboru hierarchie a předchozím kódem, adresy URL přeložit takto:

| Identifikátor URI            |                             Odpověď  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

Pokud neexistuje žádný soubor s názvem výchozí v *MyStaticFiles* directory *http://\<server_address > / StaticFiles* vrátí seznam s odkazy, můžete kliknout adresářů:

![Seznam statické soubory](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles` a `UseDirectoryBrowser` použijte adresu URL *http://\<server_address > / StaticFiles* bez do adresy koncové lomítko k aktivaci straně klienta přesměrovat na *http://\<server_address > / StaticFiles /*. Všimněte si, přidání do adresy koncové lomítko. Relativní adresy URL v rámci dokumenty jsou považovány za neplatné bez koncové lomítko.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) třída obsahuje `Mappings` vlastnost slouží jako mapování přípony souborů obsahu typy MIME. V následující ukázce je několik přípony souborů zaregistrovaný známé typy MIME. *.Rtf* rozšíření je nahrazena, a *.mp4* se odebere.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

V tématu [obsahu typy MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Nestandardní typy obsahu

Middleware se statickými soubory rozumí téměř 400 typy obsahu známému souboru. Pokud uživatel požaduje soubor neznámý typ souborů, middleware se statickými soubory vrátí odpověď HTTP 404 (není nalezena). Pokud je povoleno procházení adresářů, zobrazí se odkaz na soubor. Identifikátor URI vrátí chybu HTTP 404.

Následující kód umožňuje obsluhující neznámé typy a vykreslí Neznámý soubor jako obrázek:

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Předchozí kód je žádost o soubor s Neznámý typ obsahu vrácena jako obrázek.

> [!WARNING]
> Povolení [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) představuje bezpečnostní riziko. Ve výchozím nastavení vypnutá, a jeho použití se nedoporučuje. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) poskytuje bezpečnější alternativu k obsluhovat soubory s nestandardní rozšíření.

### <a name="considerations"></a>Důležité informace

> [!WARNING]
> `UseDirectoryBrowser` a `UseStaticFiles` může se nevrací tajných klíčů. Důrazně doporučujeme zakázání v produkčním prostředí pro procházení adresářů. Pečlivě zkontrolujte adresáře, které jsou povolené prostřednictvím `UseStaticFiles` nebo `UseDirectoryBrowser`. Celý adresář a jeho dílčí adresáře budou veřejně přístupná. Úložiště soubory vhodný pro slouží veřejnosti vyhrazené adresáře, například  *\<content_root > / wwwroot*. Oddělte tyto soubory z zobrazení MVC, stránky Razor (pouze 2.x), konfiguračních souborů, atd.

* Adresy URL pro obsah vystaveny s `UseDirectoryBrowser` a `UseStaticFiles` podléhají malá a velká písmena a znak omezení podkladový systém souborů. Například je malá a velká písmena Windows&mdash;systému macOS a Linux nejsou.

* Aplikace ASP.NET Core hostované služby IIS používá [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) předávat všechny požadavky na aplikace, včetně žádostí statických souborů. Obslužné rutiny statických souborů služby IIS se nepoužije. Nemá žádné šanci na zpracování požadavků, než se zpracovávají modulem.

* Pomocí následujících kroků ve Správci služby IIS k odebrání obslužné rutiny statických souborů služby IIS na úrovni serveru nebo webu:
    1. Přejděte na **moduly** funkce.
    1. Vyberte **iisver** v seznamu.
    1. Klikněte na tlačítko **odebrat** v **akce** bočním panelu.

> [!WARNING]
> Pokud je povoleno obslužné rutiny statických souborů služby IIS **a** modul základní technologie ASP.NET není správně nakonfigurovaná, jsou obsluhovat statické soubory. K tomu dojde, například pokud *web.config* soubor není nasazená.

* Umístěte soubory kódu (včetně *.cs* a *.cshtml*) mimo projekt aplikace kořenový web. Proto vytvoří logického oddělení se mezi obsah klientské a serverové kód aplikace. To brání úniku kódu na straně serveru.

## <a name="additional-resources"></a>Další zdroje

* [Middleware](xref:fundamentals/middleware/index)
* [Úvod do ASP.NET Core](xref:index)
