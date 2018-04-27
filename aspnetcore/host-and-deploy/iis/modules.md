---
title: Moduly služby IIS s ASP.NET Core
author: guardrex
description: Zjistit aktivní i neaktivní moduly služby IIS pro aplikace ASP.NET Core a jak spravovat moduly služby IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: e88526d997618658f58488adb37ae1e519ea3f59
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="iis-modules-with-aspnet-core"></a>Moduly služby IIS s ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Aplikace ASP.NET Core jsou hostované službou IIS v konfiguraci reverzní proxy server. Některé nativní moduly služby IIS a všechny moduly služby IIS spravované nejsou k dispozici pro zpracování požadavků pro aplikace ASP.NET Core. V mnoha případech ASP.NET Core nabízí alternativu k funkcím nativní a spravované moduly služby IIS.

## <a name="native-modules"></a>Nativní moduly

Tabulka udává nativní moduly služby IIS, které jsou funkční na reverzní proxy server požadavky na aplikace ASP.NET Core.

| Modul | Funkční s aplikacemi ASP.NET Core | ASP.NET jádra |
| ------ | :-------------------------------: | ------------------- |
| **Anonymní ověřování**<br>`AnonymousAuthenticationModule` | Ano | |
| **Základní ověřování**<br>`BasicAuthenticationModule` | Ano | |
| **Ověřování pomocí mapování klientských certifikační**<br>`CertificateMappingAuthenticationModule` | Ano | |
| **CGI**<br>`CgiModule` | Ne | |
| **Ověření konfigurace**<br>`ConfigurationValidationModule` | Ano | |
| **Chyby protokolu HTTP**<br>`CustomErrorModule` | Ne | [Middleware stránky kódu stavu](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **Vlastní protokolování**<br>`CustomLoggingModule` | Ano | |
| **Výchozí dokument**<br>`DefaultDocumentModule` | Ne | [Výchozí soubory middlewaru](xref:fundamentals/static-files#serve-a-default-document) |
| **Ověřování hodnotou hash**<br>`DigestAuthenticationModule` | Ano | |
| **Procházení adresářů**<br>`DirectoryListingModule` | Ne | [Middleware procházení adresáře](xref:fundamentals/static-files#enable-directory-browsing) |
| **Komprese dynamického**<br>`DynamicCompressionModule` | Ano | [Middleware pro kompresi odpovědí](xref:performance/response-compression) |
| **Trasování**<br>`FailedRequestsTracingModule` | Ano | [ASP.NET Core protokolování](xref:fundamentals/logging/index#the-tracesource-provider) |
| **Ukládání souborů do mezipaměti**<br>`FileCacheModule` | Ne | [Middleware pro ukládání odpovědí do mezipaměti](xref:performance/caching/middleware) |
| **Ukládání do mezipaměti HTTP**<br>`HttpCacheModule` | Ne | [Middleware pro ukládání odpovědí do mezipaměti](xref:performance/caching/middleware) |
| **Funkce protokolování HTTP**<br>`HttpLoggingModule` | Ano | [ASP.NET Core protokolování](xref:fundamentals/logging/index)<br>Implementace: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **Přesměrování protokolu HTTP**<br>`HttpRedirectionModule` | Ano | [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting) |
| **Ověřování pomocí mapování certifikátu klienta služby IIS**<br>`IISCertificateMappingAuthenticationModule` | Ano | |
| **Omezení domény a IP**<br>`IpRestrictionModule` | Ano | |
| **Filtry ISAPI**<br>`IsapiFilterModule` | Ano | [Middleware](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Ano | [Middleware](xref:fundamentals/middleware/index) |
| **Podpora protokolu**<br>`ProtocolSupportModule` | Ano | |
| **Filtrování požadavků**<br>`RequestFilteringModule` | Ano | [Adresa URL přepisování middlewaru `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Sledování požadavků**<br>`RequestMonitorModule` | Ano | |
| **Přepisování adres URL**<br>`RewriteModule` | Yes&#8224; | [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting) |
| **Začlenění na straně serveru**<br>`ServerSideIncludeModule` | Ne | |
| **Komprese statického**<br>`StaticCompressionModule` | Ne | [Middleware pro kompresi odpovědí](xref:performance/response-compression) |
| **Statický obsah**<br>`StaticFileModule` | Ne | [Middleware se statickými soubory](xref:fundamentals/static-files) |
| **Ukládání tokenu do mezipaměti**<br>`TokenCacheModule` | Ano | |
| **Ukládání do mezipaměti identifikátorů URI**<br>`UriCacheModule` | Ano | |
| **Autorizace adres URL**<br>`UrlAuthorizationModule` | Ano | [Jádro ASP.NET Identity](xref:security/authentication/identity) |
| **Ověřování systému Windows**<br>`WindowsAuthenticationModule` | Ano | |

&#8224;Adresa URL přepisování modulu `isFile` a `isDirectory` typům nefungují s aplikacemi ASP.NET Core z důvodu změn v [adresářovou strukturu](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Spravované moduly

Spravované moduly jsou *není* funkční hostované aplikace ASP.NET Core verze modulu .NET CLR fond aplikací je nastavena na **bez spravovaného kódu**. ASP.NET Core nabízí alternativy middlewaru v několika případech.

| Modul                  | ASP.NET jádra |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| Ověřování pomocí formulářů     | [Middleware ověřování souborů cookie](xref:security/authentication/cookie) |
| OutputCache             | [Middleware pro ukládání odpovědí do mezipaměti](xref:performance/caching/middleware) |
| Profil                 | |
| RoleManager             | |
| ScriptModule 4.0        | |
| Relace                 | [Middleware relace](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting) |
| UrlRoutingModule 4.0    | [Jádro ASP.NET Identity](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>Změny aplikace Správce služby IIS

Při použití Správce služby IIS ke konfiguraci nastavení, *web.config* dojde ke změně souboru aplikace. Pokud nasazení aplikace a včetně *web.config*, se přepíší veškeré změny provedené pomocí Správce služby IIS pomocí nasazené *web.config* souboru. Když se změní na server *web.config* souboru, zkopíruje aktualizovaná *web.config* soubor na serveru na místní projekt okamžitě.

## <a name="disabling-iis-modules"></a>Zakázání moduly služby IIS

Pokud modul služby IIS je nakonfigurována na úrovni serveru, který musí být pro aplikace, přidání do aplikace zakázána *web.config* souboru můžete zakázat v modulu. Buď ponechejte modul na místě a deaktivovat pomocí nastavení konfigurace (Pokud je k dispozici) nebo odebrat modul z aplikace.

### <a name="module-deactivation"></a>Deaktivace modulu

Mnoho modulů nabízejí nastavení konfigurace, které umožňuje, aby zakázán bez odebrání modul z aplikace. Toto je nejjednodušší a nejrychlejší způsob deaktivovat modul. Například modul Přesměrování protokolu HTTP lze zakázat pomocí  **\<httpRedirect >** element v *web.config*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Další informace o zakázání moduly s nastavením konfigurace, pomocí odkazů v *podřízené elementy* části [IIS \<system.webServer >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Odebrání modulu

Pokud vyjádření výslovného odebrat modul s nastavením v *web.config*, odemknutí modul a odemknutí  **\<moduly >** části *web.config* první:

1. Odemkněte modul na úrovni serveru. Ve Správci služby IIS vyberte server služby IIS **připojení** bočním panelu. Otevřete **moduly** v **IIS** oblasti. Vyberte modul, v seznamu. V **akce** bočním panelu na pravé straně, vyberte **odemčení**. Odemknout libovolný počet modulů, jak máte v úmyslu odebrat z *web.config* později.

2. Nasazení aplikace bez  **\<moduly >** kapitoly *web.config*. Pokud je aplikace nasazena pomocí *web.config* obsahující  **\<moduly >** vyvolá výjimku, část bez nutnosti odemčený části nejprve v Správce služby IIS, nástroje Configuration Manager Při pokusu odemknout oddíl. Proto nasazení aplikace bez  **\<moduly >** části.

3. Odemknutí  **\<moduly >** části *web.config*. V **připojení** bočním panelu, vyberte web, v **lokality**. V **správy** oblast, otevřete **Editor konfigurací**. Vyberte pomocí ovládacích prvků navigace `system.webServer/modules` oddílu. V **akce** bočním panelu na pravé straně, vyberte možnost **odemčení** části.

4. V tomto okamžiku  **\<moduly >** části lze přidat do *web.config* soubor s  **\<odebrat >** elementu, který chcete odebrat modul z aplikace. Více  **\<odebrat >** elementy lze přidat k odebrání více modulů. Pokud *web.config* změn na serveru, okamžitě provést stejné změny do projektu *web.config* soubor místně. Odebrání modul tímto způsobem nebude mít vliv na používání modulu s jinými aplikacemi na serveru.

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

Modul služby IIS může být odebrán také s *Appcmd.exe*. Zadejte `MODULE_NAME` a `APPLICATION_NAME` v příkazu:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Například odebrat `DynamicCompressionModule` z výchozí web:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Minimální konfigurací modulů

Pouze moduly potřebnými pro spuštění aplikace ASP.NET Core jsou modul anonymní ověřování a modul ASP.NET Core.

![Správce služby IIS otevřete moduly s minimální konfigurací modulů zobrazí](modules/_static/modules.png)

Modul ukládání do mezipaměti identifikátorů URI (`UriCacheModule`) umožňuje konfiguraci webu mezipaměti na úrovni adresy URL služby IIS. Bez tohoto modulu musí IIS číst a analyzovat konfiguraci u každého požadavku i v případě, že opakovaně požadavku na stejnou adresu URL. Analýza konfigurace snížení výkonu významné výsledkem každou žádost. *I když modul ukládání do mezipaměti identifikátorů URI není nezbytně nutné pro hostované aplikace ASP.NET Core ke spuštění, doporučujeme, aby byl povolen modul ukládání do mezipaměti identifikátorů URI pro všechna nasazení základní technologie ASP.NET.*

Ukládání do mezipaměti modulu protokolu HTTP (`HttpCacheModule`) implementuje výstupní mezipaměť služby IIS a také logiku pro ukládání do mezipaměti položky v mezipaměti ovladače HTTP.sys. Bez tohoto modulu obsah je už uložený v mezipaměti v režimu jádra a mezipaměti profily jsou ignorovány. Obvykle odstranění modulu HTTP ukládání do mezipaměti nemá negativní vliv na výkon a využití prostředků. *I když modul ukládání do mezipaměti HTTP není nezbytně nutné pro hostované aplikace ASP.NET Core ke spuštění, doporučujeme povolení ukládání do mezipaměti modulu protokolu HTTP pro všechna nasazení základní technologie ASP.NET.*

## <a name="additional-resources"></a>Další zdroje

* [Hostování ve Windows se službou IIS](xref:host-and-deploy/iis/index)
* [Úvod do architektury služby IIS: modulů ve službě IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Přehled moduly služby IIS](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Přizpůsobení služby IIS 7.0 role a moduly](https://technet.microsoft.com/library/cc627313.aspx)
* [SLUŽBY IIS `<system.webServer>`](/iis/configuration/system.webServer/)
