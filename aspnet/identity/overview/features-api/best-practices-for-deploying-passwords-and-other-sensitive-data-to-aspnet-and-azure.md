---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Osvědčené postupy pro nasazení hesel a dalších citlivých dat do ASP.NET a službě Azure App Service | Dokumentace Microsoftu
author: Rick-Anderson
description: Tento kurz ukazuje, jak může váš kód bezpečně ukládat a přistupovat k zabezpečeným informacím. Nejdůležitější bod je, že nikdy byste měli uložit hesla nebo dalších eslat...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: eda2277a4baad8f2a63aa2fdf6ab84f57f1eb0e0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756710"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Osvědčené postupy nasazení hesel a dalších citlivých dat do ASP.NET a službě Azure App Service
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> Tento kurz ukazuje, jak může váš kód bezpečně ukládat a přistupovat k zabezpečeným informacím. Nejdůležitější bod je ve zdrojovém kódu by nikdy ukládání hesel nebo jiných citlivých dat. a tajných kódů v produkčním prostředí byste neměli používat v režimu pro vývoj a testování.
> 
> Ukázkový kód je jednoduchý WebJob konzolové aplikace a aplikace ASP.NET MVC, která potřebuje přístup k databáze připojovací řetězec heslo, Twilio, Google a SendGrid zabezpečovací klíče.
> 
> Místní nastavení a PHP zmíní se taky.


- [Práce s hesly ve vývojovém prostředí](#pwd)
- [Práce s připojovací řetězce ve vývojovém prostředí](#con)
- [WebJobs konzolové aplikace](#wj)
- [Tajné kódy nasazení do Azure](#da)
- [Poznámky pro místní a PHP](#not)
- [Další prostředky](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Práce s hesly ve vývojovém prostředí

Kurzy vám ukážou často citlivá data ve zdrojovém kódu, snad s výstrahou, že byste nikdy ukládat citlivá data ve zdrojovém kódu. Například Moje [aplikace ASP.NET MVC 5 s SMS a e-mailu 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) kurzu se dozvíte v následující *web.config* souboru:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config* souboru je zdrojový kód, takže by nikdy neměly být uloženy těchto tajných kódů v tomto souboru. Naštěstí `<appSettings>` element má `file` atribut, který vám umožní zadat externí soubor, který obsahuje nastavení konfigurace citlivé aplikace. Všechny tajné klíče můžete přesunout na externí soubor, tak dlouho, dokud externí soubor se změnami do stromu zdrojového kódu. Například v následující kód souboru *AppSettingsSecrets.config* obsahuje všechny tajné kódy aplikace:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Kód v externí soubor (*AppSettingsSecrets.config* v této ukázce), stejné značky najdete v *web.config* souboru:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Modul runtime ASP.NET sloučí obsah externího souboru se značkami v &lt;appSettings&gt; elementu. Modul runtime atribut souboru ignoruje, pokud zadaný soubor nebyl nalezen.

> [!WARNING]
> Zabezpečení – nepřidávejte vaše *tajných kódů .config* soubor do projektu nebo vrátit se změnami do správy zdrojového kódu. Ve výchozím nastavení, aplikace Visual Studio nastaví `Build Action` k `Content`, což znamená, že soubor je nasazená. Další informace najdete v části [Proč nejsou všechny soubory ve složce projektu nasazení?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Přestože lze použít pro jakoukoli příponu *tajných kódů .config* souboru, je nejvhodnější zajistit jeho *.config*, protože konfigurační soubory nejsou obsluhovány službou IIS. Všimněte si také, že *AppSettingsSecrets.config* soubor je dvě úrovně directory nahoru z *web.config* souboru, tak, aby byl zcela mimo adresář řešení. Přesunutím souborů mimo adresář řešení &quot;git přidat \* &quot; nepřidá do vašeho úložiště.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Práce s připojovací řetězce ve vývojovém prostředí

Visual Studio vytvoří nové projekty ASP.NET, které používají [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB byla vytvořena speciálně pro vývojové prostředí. To nebude vyžadovat heslo, proto není nutné dělat nic, aby se zabránilo tajné kódy z kontroly na do zdrojového kódu. Některé vývojové týmy používat plné verze SQL serveru (nebo jiné DBMS), které vyžadují heslo.

Můžete použít `configSource` atribut pro nahrazení celým `<connectionStrings>` značek. Na rozdíl od `<appSettings>` `file` atribut, který se sloučí kód, `configSource` atribut nahradí značky. Následující kód ukazuje `configSource` atribut *web.config* souboru:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Pokud používáte `configSource` atributu, jak je znázorněno výše na externí soubor, přesunout připojovací řetězce a sada Visual Studio vytvořit nový web, nebudete moci zjistit, použijete databázi, a nebude mít možnost konfigurace databáze při vám pu blikovat do Azure ze sady Visual Studio. Pokud používáte `configSource` atribut, můžete použít PowerShell k vytvoření a nasazení webu a databáze, nebo můžete vytvořit na webu a databáze na portálu před publikováním. [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) skriptu se vytvoří nový web a databáze.


> [!WARNING]
> Zabezpečení – na rozdíl od *AppSettingsSecrets.config* souboru, soubor řetězce externí připojení musí být ve stejném adresáři jako kořen *web.config* souboru, tak budete mít k opatření, aby vám nemusíte vrátit se změnami do zdrojového úložiště.


> [!NOTE]
> **Upozornění zabezpečení na soubor tajných kódů:** osvědčeným postupem nechcete použít produkční tajných kódů v vývoj a testování. Použití hesel produkčního prostředí v testovacím nebo vývojovém nevracení těchto tajných kódů.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>WebJobs konzolové aplikace

*App.config* soubor používaný konzolové aplikace nepodporuje relativní cesty, ale podporuje absolutní cesty. Můžete použít absolutní cestu k přesunutí tajné kódy z adresáře vašeho projektu. Následující kód ukazuje tajných kódů v *C:\secrets\AppSettingsSecrets.config* souborů a jiných citlivých dat v *app.config* souboru.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Tajné kódy nasazení do Azure

Při nasazení vaší webové aplikace do Azure, *AppSettingsSecrets.config* souboru se nenasadí (to znamená, co chcete). Může přejít na [portálu pro správu Azure](https://azure.microsoft.com/services/management-portal/) a nastavit ručně, to udělat:

1. Přejděte na [ https://portal.azure.com ](https://portal.azure.com)a přihlaste se pomocí přihlašovacích údajů Azure.
2. Klikněte na tlačítko **Procházet &gt; Web Apps**, pak klikněte na název vaší webové aplikace.
3. Klikněte na tlačítko **všechna nastavení &gt; nastavení aplikace**.

**Nastavení aplikace** a **připojovací řetězec** hodnoty přepsání stejného nastavení *web.config* souboru. V našem příkladu jsme nebyly nasazeny tato nastavení do Azure, ale pokud byly tyto klíče v *web.config* souboru nastavení zobrazí na portálu má přednost.

Osvědčeným postupem je použít [pracovních postupů DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) a používat [prostředí Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (nebo jiné rozhraní framework, jako [Chef](http://www.opscode.com/chef/) nebo [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) do automatizace nastavení tyto hodnoty v Azure. Následující skript Powershellu používá [Export CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) export šifrované tajné klíče na disk:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

Ve výše uvedené skriptu "Name" je název tajný klíč, jako například '&quot;FB\_AppSecret&quot; nebo "TwitterSecret". Můžete zobrazit soubor ".credential" vytvořen skriptem v prohlížeči. Následující fragment zkoušky každého souboru přihlašovacích údajů a nastaví tajných kódů pro pojmenovanou webovou aplikaci:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Zabezpečení – nezahrnují hesla a dalších tajných kódů v skriptu prostředí PowerShell to tedy popírá účel nasazení citlivá data pomocí skriptu prostředí PowerShell. [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) rutina poskytuje mechanismus zabezpečeného k získání hesla. Pomocí uživatelského rozhraní řádku může zabránit úniku heslo.


### <a name="deploying-db-connection-strings"></a>Nasazení databáze připojovacích řetězců

Připojovací řetězce databáze jsou zpracovány podobně jako nastavení aplikace. Pokud provádíte nasazení vaší webové aplikace ze sady Visual Studio, připojovací řetězec se nakonfigurují za vás. Můžete to ověřit na portálu. Doporučeným způsobem, jak nastavit připojovací řetězec je pomocí Powershellu. Příklad skriptu prostředí PowerShell vytvoří web a databáze a nastaví připojovací řetězec na webu stažení [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) z [knihovnu skriptů Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Poznámky pro jazyk PHP

Od páry klíč hodnota pro obě **nastavení aplikace** a **připojovací řetězce** jsou uložené v proměnné prostředí ve službě Azure App Service, vývojáře, které používají jakékoli webové aplikaci rozhraní (například PHP) můžete snadno Tyto hodnoty načtete. Zobrazit Stefan Schackow [weby Windows Azure: fungování řetězců aplikace a připojovacích řetězců](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) blogový příspěvek, který ukazuje fragment kódu PHP ke čtení nastavení aplikace a připojovacích řetězců.

## <a name="notes-for-on-premises-servers"></a>Poznámky k na místních serverech

Pokud provádíte nasazení na místní webové servery, může pomoct zabezpečené tajné kódy ve [šifrování konfigurační oddíly konfiguračních souborů](https://msdn.microsoft.com/library/ff647398.aspx). Jako alternativu můžete použít stejný přístup doporučuje pro Azure Websites: zachovat vývojové nastavení v konfiguračních souborech a použití hodnot proměnných prostředí pro výrobní nastavení. V takovém případě však musíte napsat kód aplikace pro funkce, které jsou v Azure Websites: načtení nastavení z proměnných prostředí a použijte tyto hodnoty místo nastavení konfiguračního souboru nebo můžete použít nastavení konfiguračního souboru při proměnné prostředí nebyly nalezeny.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

Příklad Powershellu skript, který se vytvoří webová aplikace a databáze, nastaví připojovací řetězec a nastavení aplikací, stahovat [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) z [knihovnu skriptů Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Zobrazit Stefan Schackow [webů Windows Azure: jak řetězců aplikace a připojení fungují řetězce](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Speciální díky Jiří Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) a Carlos Farre kontroly.
