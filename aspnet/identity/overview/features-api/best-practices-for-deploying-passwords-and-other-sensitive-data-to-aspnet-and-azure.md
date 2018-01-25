---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: "Osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a službě Azure App Service | Microsoft Docs"
author: Rick-Anderson
description: "Tento kurz ukazuje, jak můžete kódu bezpečně ukládala a přístup k zabezpečeným informacím. Většina důležité je, že by nikdy neukládají hesla nebo dalších esílat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 995d9a088e3095f36a01d2adb19ec08e6a6d1b3e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a službě Azure App Service
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> Tento kurz ukazuje, jak můžete kódu bezpečně ukládala a přístup k zabezpečeným informacím. Nejdůležitější bod je ve zdrojovém kódu by nikdy neukládají hesla nebo dalších citlivých dat, a tajné klíče produkční byste neměli používat v režimu pro vývoj a testování.
> 
> Ukázkový kód je jednoduchý konzolovou aplikaci webové úlohy a aplikace ASP.NET MVC, která potřebuje přístup k databáze připojovací řetězec heslo, Twilio, Google a sendgrid vám umožňuje zabezpečovací klíče.
> 
> Místní nastavení a PHP je taky uvedený.


- [Práce s hesly ve vývojovém prostředí](#pwd)
- [Práce s připojovací řetězce ve vývojovém prostředí](#con)
- [Aplikace konzoly webové úlohy](#wj)
- [Nasazení tajných klíčů do Azure](#da)
- [Poznámky pro místní a PHP](#not)
- [Další zdroje informací](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Práce s hesly ve vývojovém prostředí

Kurzy často zobrazit citlivá data ve zdrojovém kódu, zpravidla s výstrahou, že by nikdy ukládat citlivá data ve zdrojovém kódu. Například Moje [aplikaci ASP.NET MVC 5 s SMS a e-mailu 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) kurzu se dozvíte, následující *web.config* souboru:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config* souboru je zdrojový kód, takže těchto tajných klíčů by měly být nikdy uložené v tomto souboru. Naštěstí `<appSettings>` element má `file` atribut, který umožňuje zadat externí soubor, který obsahuje nastavení konfigurace pro citlivé aplikace. Vaše tajné klíče můžete přesunout na externí soubor tak dlouho, dokud externí soubor nastavení není kontrolován do vašeho zdrojového stromu. Například v následující kód, soubor *AppSettingsSecrets.config* obsahuje všechny tajné klíče aplikace:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Kód v souboru externí (*AppSettingsSecrets.config* v této ukázce), je stejný kód nalezena v *web.config* souboru:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Modul runtime ASP.NET sloučí obsah externího souboru se značkami v &lt;appSettings&gt; elementu. Modul runtime atribut souboru ignoruje, pokud zadaný soubor nelze nalézt.

> [!WARNING]
> Zabezpečení – nepřidávejte vaše *tajné klíče .config* souboru do projektu nebo zkontrolujte do správy zdrojového kódu. Ve výchozím nastavení, Visual Studio nastaví `Build Action` k `Content`, což znamená, že soubor je nasazená. Další informace najdete v části [proč si všechny soubory ve složce projektu nasadí?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Přestože je možné použít jakékoli rozšíření pro *.config tajné klíče* souboru, je nejvhodnější zajistit jeho *.config*, protože konfigurační soubory nejsou obsloužených služby IIS. Všimněte si také, že *AppSettingsSecrets.config* soubor je dvě úrovně directory si z *web.config* souboru, takže je zcela mimo adresář řešení. Přesunutím souboru mimo adresář řešení &quot;git přidat \* &quot; nebude přidejte do úložiště.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Práce s připojovací řetězce ve vývojovém prostředí

Visual Studio vytvoří nové projekty ASP.NET, které používají [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB byla vytvořena speciálně pro vývojové prostředí. Není třeba heslo, proto nemusíte dělat nic, aby se zabránilo tajné klíče z kontroly do zdrojového kódu. Některé vývojové týmy pomocí plné verze systému SQL Server (nebo jiné databázového systému), které vyžadují heslo.

Můžete použít `configSource` atribut nahradit celý `<connectionStrings>` značek. Na rozdíl od `<appSettings>` `file` atribut, který sloučí kód, `configSource` atribut nahradí značku. Následující kód ukazuje `configSource` atribut *web.config* souboru:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Pokud použijete `configSource` atribut jako v příkladu nahoře přesunout připojovací řetězce pro externí soubor a mít Visual Studio vytvořte novou webovou stránku, nebude moci zjistit, používáte-li databázi, a nebude dostupná možnost konfigurace databáze při jste celofiremnímu vat do Azure ze sady Visual Studio. Pokud používáte `configSource` atribut, můžete použít PowerShell k vytvoření a nasazení webu a databáze, nebo můžete vytvořit na webu a databázi portálu před publikováním. [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) skript se vytvoří nový web a databáze.


> [!WARNING]
> Zabezpečení – na rozdíl od *AppSettingsSecrets.config* souboru, soubor řetězce externí připojení musí být ve stejném adresáři jako kořenová *web.config* souboru, takže budete muset provést opatření, aby vám Nekontrolovat ho do úložiště zdrojů.


> [!NOTE]
> **Upozornění zabezpečení v souboru tajných klíčů:** osvědčeným postupem nechcete použít produkční tajných klíčů v testovacích a vývojových. Pomocí produkčního hesla v testovacím nebo vývojovém nevracení těchto tajných klíčů.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Aplikace konzoly webové úlohy

*App.config* soubor používaný konzolovou aplikaci nepodporuje relativní cesty, ale podporuje absolutní cesty. Absolutní cesta můžete přesunout vaše tajné klíče z adresáře projektu. Následující kód ukazuje tajných klíčů v *C:\secrets\AppSettingsSecrets.config* soubor a bez citlivá data v *app.config* souboru.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Nasazení tajných klíčů do Azure

Při nasazení webové aplikace do Azure, *AppSettingsSecrets.config* soubor nebude možné nasadit (to znamená, co chcete použít). Můžete může přejít [portálu pro správu Azure](https://azure.microsoft.com/services/management-portal/) a je nastavit ručně, k tomu:

1. Přejděte na [https://portal.azure.com](https://portal.azure.com)a přihlaste se pomocí přihlašovacích údajů Azure.
2. Klikněte na tlačítko **Procházet &gt; webové aplikace**, pak klikněte na název vaší webové aplikace.
3. Klikněte na tlačítko **všechna nastavení &gt; nastavení aplikace**.

**Nastavení aplikace** a **připojovací řetězec** hodnoty přepsání stejné nastavení *web.config* souboru. V našem příkladu jsme nebyla nasazena tato nastavení do Azure, ale pokud byly tyto klíče v *web.config* souboru nastavení zobrazí na portálu by mají přednost před.

Osvědčeným postupem je podle [DevOps pracovního postupu](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) a používat [prostředí Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (nebo jiné framework jako [Chef](http://www.opscode.com/chef/) nebo [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) na automatizovat nastavení tyto hodnoty v Azure. Následující skript Powershellu využívá [Export CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) export šifrované tajné klíče na disk:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

Ve výše uvedené skriptu "Název" je název tajný klíč, jako například '&quot;FB\_AppSecret&quot; nebo "TwitterSecret". Můžete zobrazit soubor ".credential" vytvořený skript v prohlížeči. Následující fragment testuje každý soubor přihlašovacích údajů a nastaví tajných klíčů s názvem webové aplikace:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Zabezpečení – neobsahují hesla nebo jiné tajné ve skriptu prostředí PowerShell, díky Ano popírá svůj účel nasazení citlivá data pomocí skriptu prostředí PowerShell. [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) rutiny poskytuje zabezpečené mechanismus k získání hesla. Pomocí uživatelského rozhraní řádku můžete zabránit úniku heslo.


### <a name="deploying-db-connection-strings"></a>Nasazení DB-řetězce připojení

DB připojovací řetězce jsou zpracovávány podobně nastavení aplikace. Pokud nasadíte webovou aplikaci ze sady Visual Studio, nakonfigurují se pro vás připojovací řetězec. Můžete to ověřit na portálu. Doporučeným způsobem, jak nastavit připojovací řetězec je v prostředí PowerShell. Příklad skriptu prostředí PowerShell vytvoří web a databáze a nastaví připojovací řetězec na webu stažení [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) z [knihovny Azure skriptu](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Poznámky pro jazyk PHP

Od páry klíč hodnota pro obě **nastavení aplikace** a **připojovací řetězce** jsou uložené v proměnné prostředí v Azure App Service, vývojáři, které používají všechny webové aplikace rozhraní (například PHP) můžete snadno načtení těchto hodnot. V tématu Stefan Schackow [weby systému Windows Azure: fungování řetězců aplikace a připojovacích řetězců](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) příspěvek blogu, který je ukázka PHP číst nastavení aplikace a připojovací řetězce.

## <a name="notes-for-on-premises-servers"></a>Poznámky pro místní servery

Pokud nasazujete do místní webové servery, můžete pomoct zabezpečené tajné klíče podle [šifrování konfigurační oddíly konfiguračních souborů](https://msdn.microsoft.com/library/ff647398.aspx). Jako alternativu, můžete použít ve stejný přístup doporučené pro weby Azure: udržovat – vývojové nastavení v konfiguračních souborech a používat hodnot proměnných prostředí pro výrobní nastavení. V takovém případě však máte napsat kód aplikace pro funkce, které jsou automatické v weby Azure: nastavení načíst z proměnné prostředí a použijte tyto hodnoty místo souboru nastavení konfigurace nebo použijte nastavení konfiguračního souboru při nejsou nalezeny proměnné prostředí.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

Příkladem prostředí PowerShell pro skript, který vytvoří webovou aplikaci + databáze, nastaví připojovací řetězec a nastavení aplikace, stažení [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) z [knihovny Azure skriptu](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Najdete v části Stefan Schackow [Azure webových serverů Windows: jak řetězců aplikace a připojovací řetězce pracovní](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Zvláštní díky Jiří Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) a Carlos Farre kontroly.
