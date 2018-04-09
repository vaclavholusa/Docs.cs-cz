---
title: Moduly služby IIS s ASP.NET Core
author: guardrex
description: Zjistit aktivní i neaktivní moduly služby IIS pro aplikace ASP.NET Core a jak spravovat moduly služby IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: d9b3de915df333153255f91649f9169f76ba2fe0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="iis-modules-with-aspnet-core"></a>Moduly služby IIS s ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Aplikace ASP.NET Core jsou hostované službou IIS v konfiguraci reverzní proxy server. Některé nativní moduly služby IIS a všechny moduly služby IIS spravované nejsou k dispozici pro zpracování požadavků pro aplikace ASP.NET Core. V mnoha případech ASP.NET Core nabízí alternativu k funkcím nativní a spravované moduly služby IIS.

## <a name="native-modules"></a>Nativní moduly

| Modul | Aktivní základní rozhraní .NET | ASP.NET jádra |
| ------ | :--------------: | ------------------- |
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
| **HTTP Logging**<br>`HttpLoggingModule` | Ano | [ASP.NET Core protokolování](xref:fundamentals/logging/index)<br>Implementace: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **HTTP Redirection**<br>`HttpRedirectionModule` | Ano | [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting) |
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

| Modul                  | Aktivní základní rozhraní .NET | ASP.NET jádra |
| ----------------------- | :--------------: | ------------------- |
| AnonymousIdentification | Ne               | |
| DefaultAuthentication   | Ne               | |
| FileAuthorization       | Ne               | |
| Ověřování pomocí formulářů     | Ne               | [Middleware ověřování souborů cookie](xref:security/authentication/cookie) |
| OutputCache             | Ne               | [Middleware pro ukládání odpovědí do mezipaměti](xref:performance/caching/middleware) |
| Profil                 | Ne               | |
| RoleManager             | Ne               | |
| ScriptModule-4.0        | Ne               | |
| Relace                 | Ne               | [Middleware relace](xref:fundamentals/app-state) |
| UrlAuthorization        | Ne               | |
| UrlMappingsModule       | Ne               | [Middleware pro přepis adres URL](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | Ne               | [Jádro ASP.NET Identity](xref:security/authentication/identity) |
| WindowsAuthentication   | Ne               | |

## <a name="iis-manager-application-changes"></a>Změny aplikace Správce služby IIS

Při použití Správce služby IIS ke konfiguraci nastavení, *web.config* dojde ke změně souboru aplikace. Pokud nasazení aplikace a včetně *web.config*, se přepíší veškeré změny provedené pomocí Správce služby IIS pomocí nasazené *web.config* souboru. Když se změní na server *web.config* souboru, zkopíruje aktualizovaná *web.config* soubor na serveru na místní projekt okamžitě.

## <a name="disabling-iis-modules"></a>Zakázání moduly služby IIS

Pokud modul služby IIS je nakonfigurována na úrovni serveru, který musí být pro aplikace, přidání do aplikace zakázána *web.config* souboru můžete zakázat v modulu. Buď ponechejte modul na místě a deaktivovat pomocí nastavení konfigurace (Pokud je k dispozici) nebo odebrat modul z aplikace.

### <a name="module-deactivation"></a>Deaktivace modulu

Mnoho modulů nabízejí nastavení konfigurace, které umožňuje, aby zakázán bez odebrání modul z aplikace. Toto je nejjednodušší a nejrychlejší způsob deaktivovat modul. Například je třeba zakázat modul přepisování adres URL služby IIS pomocí  **\<httpRedirect >** element v *web.config*:

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

Pro instalaci služby IIS s moduly výchozí nainstalovaný, použijte následující  **\<modulu >** , odinstalujte výchozí moduly.

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
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

## <a name="additional-resources"></a>Další zdroje

* [Hostování ve Windows se službou IIS](xref:host-and-deploy/iis/index)
* [Úvod do architektury služby IIS: modulů ve službě IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Přehled moduly služby IIS](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Přizpůsobení služby IIS 7.0 role a moduly](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
