---
title: "Moduly služby IIS pomocí ASP.NET Core"
author: guardrex
description: "Odkaz na dokument popisující aktivní i neaktivní moduly služby IIS pro aplikace ASP.NET Core."
keywords: "ASP.NET Core, iis, modulu, reverznímu proxy serveru"
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: fee8e830ab43f731de9c90fad06b577662760f87
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a>Moduly služby IIS pomocí ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Aplikace ASP.NET Core jsou hostované službou IIS v konfiguraci reverznímu proxy serveru. Některé nativní moduly služby IIS a všechny moduly služby IIS spravované nejsou k dispozici pro zpracování požadavků pro aplikace ASP.NET Core. V mnoha případech ASP.NET Core nabízí alternativu k funkcím nativní a spravované moduly služby IIS.

## <a name="native-modules"></a>Nativní moduly
Modul | Aktivní základní rozhraní .NET | ASP.NET jádra
--- | :---: | ---
**Anonymní ověřování**<br>`AnonymousAuthenticationModule` | Ano | 
**Základní ověřování**<br>`BasicAuthenticationModule` | Ano | 
**Ověřování pomocí mapování klientských certifikační**<br>`CertificateMappingAuthenticationModule` | Ano | 
**CGI**<br>`CgiModule` | Ne | 
**Ověření konfigurace**<br>`ConfigurationValidationModule` | Ano | 
**Chyby protokolu HTTP**<br>`CustomErrorModule` | Ne | [Middleware stránky kódu stavu](xref:fundamentals/error-handling#configuring-status-code-pages)
**Vlastní protokolování**<br>`CustomLoggingModule` | Ano | 
**Výchozí dokument**<br>`DefaultDocumentModule` | Ne | [Výchozí soubory middlewaru](xref:fundamentals/static-files#serving-a-default-document)
**Ověřování hodnotou hash**<br>`DigestAuthenticationModule` | Ano | 
**Procházení adresářů**<br>`DirectoryListingModule` | Ne | [Middleware procházení adresáře](xref:fundamentals/static-files#enabling-directory-browsing)
**Komprese dynamického**<br>`DynamicCompressionModule` | Ano | [Middleware komprese odpovědi](xref:performance/response-compression)
**Trasování**<br>`FailedRequestsTracingModule` | Ano | [ASP.NET Core protokolování](xref:fundamentals/logging/index#the-tracesource-provider)
**Ukládání souborů do mezipaměti**<br>`FileCacheModule` | Ne | [Middleware ukládání do mezipaměti odpovědi](xref:performance/caching/middleware)
**Ukládání do mezipaměti HTTP**<br>`HttpCacheModule` | Ne | [Middleware ukládání do mezipaměti odpovědi](xref:performance/caching/middleware)
**Funkce protokolování HTTP**<br>`HttpLoggingModule` | Ano | [ASP.NET Core protokolování](xref:fundamentals/logging/index)<br>Implementace: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
**Přesměrování protokolu HTTP**<br>`HttpRedirectionModule` | Ano | [Adresa URL přepisování middlewaru](xref:fundamentals/url-rewriting)
**Ověřování pomocí mapování certifikátu klienta služby IIS**<br>`IISCertificateMappingAuthenticationModule` | Ano | 
**Omezení domény a IP**<br>`IpRestrictionModule` | Ano | 
**Filtry ISAPI**<br>`IsapiFilterModule` | Ano | [Middleware](xref:fundamentals/middleware)
**ISAPI**<br>`IsapiModule` | Ano | [Middleware](xref:fundamentals/middleware)
**Podpora protokolu**<br>`ProtocolSupportModule` | Ano | 
**Filtrování požadavků**<br>`RequestFilteringModule` | Ano | [Adresa URL přepisování middlewaru`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**Sledování požadavků**<br>`RequestMonitorModule` | Ano | 
**Přepisování adres URL**<br>`RewriteModule` | Yes† | [Adresa URL přepisování middlewaru](xref:fundamentals/url-rewriting)
**Začlenění na straně serveru**<br>`ServerSideIncludeModule` | Ne | 
**Komprese statického**<br>`StaticCompressionModule` | Ne | [Middleware komprese odpovědi](xref:performance/response-compression)
**Statický obsah**<br>`StaticFileModule` | Ne | [Middleware se statickými soubory](xref:fundamentals/static-files)
**Ukládání tokenu do mezipaměti**<br>`TokenCacheModule` | Ano | 
**Ukládání do mezipaměti identifikátorů URI**<br>`UriCacheModule` | Ano | 
**Autorizace adres URL**<br>`UrlAuthorizationModule` | Ano | [Jádro ASP.NET Identity](xref:security/authentication/identity)
**Ověřování systému Windows**<br>`WindowsAuthenticationModule` | Ano | 

†The modul přepisování adres URL na `isFile` a `isDirectory` nefungují s aplikacemi ASP.NET Core z důvodu změn v [adresářovou strukturu](xref:hosting/directory-structure).

## <a name="managed-modules"></a>Spravované moduly
Modul | Aktivní základní rozhraní .NET | ASP.NET jádra
--- | :---: | ---
AnonymousIdentification | Ne | 
DefaultAuthentication | Ne | 
FileAuthorization | Ne | 
Ověřování pomocí formulářů | Ne | [Middleware ověřování souborů cookie](xref:security/authentication/cookie)
OutputCache | Ne | [Middleware ukládání do mezipaměti odpovědi](xref:performance/caching/middleware)
Profil | Ne | 
RoleManager | Ne | 
ScriptModule 4.0 | Ne | 
Relace | Ne | [Middleware relace](xref:fundamentals/app-state)
UrlAuthorization | Ne | 
UrlMappingsModule | Ne | [Adresa URL přepisování middlewaru](xref:fundamentals/url-rewriting)
UrlRoutingModule 4.0 | Ne | [Jádro ASP.NET Identity](xref:security/authentication/identity)
WindowsAuthentication | Ne | 

## <a name="iis-manager-application-changes"></a>Změny aplikace Správce služby IIS
Pokud použijete Správce služby IIS ke konfiguraci nastavení, můžete přímo změnit *web.config* souboru aplikace. Pokud nasazení aplikace a zahrnují *web.config*, veškeré změny provedené s IIS Manager budou přepsány podle nasazené *souboru web.config*. Proto pokud provedete změny na server *web.config* souboru, zkopíruje aktualizovaná *web.config* soubor do místní projektu okamžitě.

## <a name="disabling-iis-modules"></a>Zakázání moduly služby IIS
Pokud je modul služby IIS, která je nakonfigurována na úrovni serveru, který chcete zakázat pro aplikaci, můžete tak učinit s doplňkem k vaší *web.config* souboru. Buď ponechejte modul na místě a deaktivovat pomocí nastavení konfigurace (Pokud je k dispozici) nebo odebrat modul z aplikace.

### <a name="module-deactivation"></a>Deaktivace modulu
Mnoho modulů nabízejí nastavení konfigurace, které vám umožní je zakázat bez odstranění z aplikace. Toto je nejjednodušší a nejrychlejší způsob deaktivovat modul. Například pokud chcete zakázat modul přepisování adres URL služby IIS, použijte `<httpRedirect>` element, jak je uvedeno níže. Další informace o zakázání moduly s nastavením konfigurace, pomocí odkazů v *podřízené elementy* části [IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/).

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>Odebrání modulu
Pokud se můžete rozhodnout pro odebrání modul s nastavením v *web.config*, musíte odemknout modul a odemknutí `<modules>` části *web.config* první. Kroky jsou uvedeny níže:

1. Odemkněte modul na úrovni serveru. Klikněte na server služby IIS ve Správci služby IIS **připojení** bočním panelu. Otevřete **moduly** v **IIS** oblasti. Klikněte na modul v seznamu. V **akce** bočním panelu na pravé straně, klikněte na tlačítko **odemčení**. Odemknout libovolný počet modulů, jak máte v úmyslu odebrat s *web.config* později.

2. Nasazení aplikace bez `<modules>` kapitoly *web.config*. Pokud nasadíte aplikaci pomocí *web.config* obsahující `<modules>` části bez nutnosti odemčený části nejprve v Správce služby IIS, nástroje Configuration Manager vyvolá výjimku při pokusu odemknout oddíl. Proto nasazení aplikace bez `<modules>` části.

3. Odemknutí `<modules>` části *web.config*. V **připojení** bočním panelu, klikněte na web v **lokality**. V **správy** oblast, otevřete **Editor konfigurací**. Vyberte pomocí ovládacích prvků navigace `system.webServer/modules` oddílu. V **akce** bočním panelu na pravé straně, klikněte na **odemčení** části.

4. V tomto okamžiku bude moct přidávat `<modules>` části k vaší *web.config* soubor s `<remove>` elementu, který chcete odebrat modul z aplikace. Můžete přidat více `<remove>` elementy odebrat více modulů. Nezapomeňte, že pokud provedete *web.config* změny na serveru, aby se daly okamžitě v projektu místně. Odebrání modul tímto způsobem nebude mít vliv na použití modulu s jinými aplikacemi na serveru.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Pro instalaci služby IIS s moduly výchozí nainstalován, můžete použít následující `<module>` výchozí moduly, odinstalujte.

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

Rovněž můžete odebrat modul služby IIS s *Appcmd.exe*. Zadejte `MODULE_NAME` a `APPLICATION_NAME` v příkazu vidíte níže:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Tady je postup odebrání `DynamicCompressionModule` z výchozí web:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a>Konfigurace minimální modulu
Pouze moduly potřebnými pro spuštění aplikace ASP.NET Core jsou modul anonymní ověřování a modul ASP.NET Core.

![Správce služby IIS otevřete moduly s minimální konfigurací modulů zobrazí](iis-modules/_static/modules.png)

## <a name="resources"></a>Prostředky
* [Publikování do služby IIS](xref:publishing/iis)
* [Přehled moduly služby IIS](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Přizpůsobení služby IIS 7.0 role a moduly](https://technet.microsoft.com/library/cc627313.aspx)
* [SLUŽBY IIS`<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
