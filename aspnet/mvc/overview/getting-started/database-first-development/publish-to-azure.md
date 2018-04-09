---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publikování MVC databáze první lokalitu v Azure | Microsoft Docs
author: tfitzmac
description: Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 839bbceba6f0e098303facd40dbb1496bd449ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a>Publikování MVC databáze první lokalitu v Azure
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze. Generovaný kód odpovídá sloupců v tabulce databáze.
> 
> Tato část řady se zaměřuje na publikování webové aplikace a databáze do Azure. Další informace v tomto tématu se dozvíte o publikování webové aplikace a databáze, ale ve skutečnosti provádět kroky, které je třeba spustit na začátku tohoto kurzu. V tématu [Začínáme](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Nasazení webové aplikace v Azure

Účet Azure k dokončení tohoto kurzu potřebujete:

- Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.
- Můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.

K publikování webové aplikace, klikněte pravým tlačítkem na projekt a vyberte **publikovat**.

![publikování webu](publish-to-azure/_static/image1.png)

Vyberte weby Microsoft Azure.

![Vyberte Azure](publish-to-azure/_static/image2.png)

Pokud nejste přihlášení do Azure, zadejte přihlašovací údaje účtu Azure. Pak vyberte nový vytvořte novou webovou aplikaci.

![nové lokality](publish-to-azure/_static/image3.png)

Vytvořte jedinečný název pro vaši webovou aplikaci. Budete vědět, že že název je jedinečný. Pokud se zobrazí zelená značka zaškrtnutí vpravo od pole název. Vyberte oblast pro vaši webovou aplikaci. Vyberte **vytvořit nový server** pro databázi a zadejte uživatelské jméno a heslo pro tento nový server databáze. Po dokončení klikněte na tlačítko **vytvořit**.

![Vytvoření webu](publish-to-azure/_static/image4.png)

Vaše připojení hodnoty jsou nyní všechny nastavené. Můžete ponechat tyto hodnoty beze změny.

![Připojení](publish-to-azure/_static/image5.png)

Klikněte na tlačítko **Další**.

Pro nastavení, Všimněte si, že dvě připojení databáze zadávají - ApplicationDBContext a ContosoUniversityDataEntities. ApplicationDBContext je připojení pro tabulky účtu uživatele. Tyto hodnoty jenom zobrazit připojovací řetězce pro databáze. Neznamená to, že tyto databáze bude publikována při publikování webu. Po dokončení publikování webové aplikace, budete publikovat projekt databáze.

![](publish-to-azure/_static/image6.png)

Na znak výpustky (...) vedle připojení k databázi zobrazuje podrobnosti o připojovacím řetězci. Klikněte na tlačítko se třemi tečkami vedle ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Poznamenejte si název databázového serveru a databáze. Název serveru je náhodně vygenerované. Název databáze je jednoduchý název vaší lokality s  **\_db** připojí. Oba názvy bude potřebovat později při publikování databáze.

Klikněte na tlačítko **OK** zavřete okno databáze připojovací řetězec.

V okně Publikovat Web, klikněte na **Další** zobrazíte ve verzi preview.

![](publish-to-azure/_static/image8.png)

Klikněte na tlačítko Spustit Náhled zobrazíte seznam souborů k publikování. Vzhledem k tomu, že je to první, kdy jste publikovali této lokality, je v seznamu všechny relevantní soubory v projektu.

Klikněte na tlačítko **publikování**.

V podokně výstupu se zobrazí výsledek publikace.

![](publish-to-azure/_static/image9.png)

Po publikaci web je immedialely spuštění ve webovém prohlížeči. Po nasazení vašeho webu a můžete zaregistrovat nového uživatele na web; ale tabulek v projektu ContosoUniversityData zatím nebyly publikovány. Pokud kliknete na seznam studenty odkaz dojde k chybě.

## <a name="publish-database-to-sql-azure"></a>Publikování databáze SQL Azure

Před publikováním vaší databáze, musí se ujistěte, že váš místní počítač může připojit k serveru databáze. Brány firewall na serveru databáze omezuje, které počítače může připojit k databázi. Je nutné přidat IP adresu počítače na povolené IP adresy pro bránu firewall.

Přihlášení k účtu Azure prostřednictvím portálu Azure.

Vyberte nové databáze a vyberte **spravovat**.

![Správa](publish-to-azure/_static/image10.png)

Je nutné nakonfigurovat databázový server umožňuje připojení z vašeho počítače. Když vyberete spravovat, jste vyzváni k přidejte aktuální IP adresu, jak umožnit databázového serveru. Vyberte možnost Ano.

![Přidat ip adresu](publish-to-azure/_static/image11.png)

Je pravděpodobné, že IP adresu, kterou jste přidali v předchozím kroku není jedinou IP adresu, kterou je nutné nakonfigurovat pro připojení. Pokuste se přihlásit k databázi a zda připojení správně nastaven. Zadejte uživatele a heslo, které jste vytvořili dříve.

![přihlášení](publish-to-azure/_static/image12.png)

Pokud se zobrazí chybovou zprávu, budete muset přidat další IP adresu. Klikněte na tlačítko chybovou zprávu zobrazíte další podrobnosti o chybě. V podrobnostech uvidíte IP adresu, která je nutné přidat. Všimněte si tuto IP adresu.

![není povolené.](publish-to-azure/_static/image13.png)

Zavřete toto okno přihlášení a vrátit na portál Azure.

Přejděte na řídicí panel pro vaši databázi. Klikněte na tlačítko **Spravovat povolené IP adresy**.

![Správa ip adres](publish-to-azure/_static/image14.png)

Nyní je nutné přidat IP adresu z chybová zpráva. Buď změnit rozsah povolených IP adres zahrnout jeden z chybová zpráva, nebo přidejte tuto IP adresu jako samostatný záznam.

![Přidání nové adresy](publish-to-azure/_static/image15.png)

Uložte změny do povolené IP adresy.

Kliknutím na spravovat a zkuste se znovu přihlásit k databázi. Potřebujete Počkejte několik minut, než povolené IP adresy pro bránu firewall správně nakonfigurováno. Když se můžete úspěšně přihlásit databáze, jste dokončili nastavení připojení k databázi.

Toto okno správy můžete nechat otevřené, protože zkontroluje výsledek nasazení databáze za chvíli.

Vraťte se k projektu databáze. Klikněte pravým tlačítkem na projekt a vyberte **publikovat**.

![publikování databáze](publish-to-azure/_static/image16.png)

V okně publikování databáze vyberte **upravit**.

![Upravit](publish-to-azure/_static/image17.png)

Zadejte název databázového serveru a pověření pro ověření serveru. Po zadání přihlašovacích údajů, vyberte databázi, kterou jste vytvořili z seznam dostupných databází. Ve výchozím nastavení Visual Studio nastaví název pole databáze na název projektu, který nemusí být stejný jako databázi, kterou jste vytvořili.

![](publish-to-azure/_static/image18.png)

Klikněte na tlačítko OK.

Budete pravděpodobně chtít uložit tento profil, takže aktualizace v budoucnu můžete publikovat bez nutnosti opětovného zadání všechny informace o připojení. Vyberte **vytvořit profil**.

![Uložit profil](publish-to-azure/_static/image19.png)

Vytvoří soubor v projektu s názvem **ContosoUniversityData.publish.xml**. Při příštím publikování databáze v Azure, jednoduše načíst tento profil.

Nyní, klikněte na tlačítko **publikovat** k vytvoření databáze v Azure.

![Publikování](publish-to-azure/_static/image20.png)

Po spuštění nějakou dobu, se zobrazí publikování výsledků.

![výsledky](publish-to-azure/_static/image21.png)

Nyní můžete přejít zpět na portálu pro správu pro vaši databázi. Aktualizujte zobrazení návrhu a Všimněte si, že byly nasazeny 3 tabulky s předem vyplněné data.

![nové tabulky](publish-to-azure/_static/image22.png)

Nyní jste připraveni k otestování webové aplikace, která je nasazena do Azure. Přejděte na webovou aplikaci v Azure (například http://contosositeexample.azurewebsites.net/). Klikněte na odkaz pro seznam studenty a měli byste vidět zobrazení index pro studenty.

![Zobrazení](publish-to-azure/_static/image23.png)

V některých případech databáze a připojení potřebují trochu čas musí být správně nakonfigurovaná. Pokud obdržíte chybu, počkejte několik minut a potom akci opakujte.

## <a name="conclusion"></a>Závěr

Tato řada zadat jednoduchý příklad způsob generování kódu z existující databázi, která umožňuje uživatelům upravit, aktualizovat, vytvářet a odstraňovat data. Použije pro vytvoření projektu ASP.NET MVC 5, rozhraní Entity Framework a generování uživatelského rozhraní ASP.NET.

Příklad úvodní Code First vývoj naleznete v části [Začínáme s ASP.NET MVC 5](../introduction/getting-started.md).

Pokročilejší příklad najdete v tématu [vytváření datového modelu Entity Framework pro aplikace ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Všimněte si, že rozhraní API DbContext, který používáte pro práci s daty v první databáze je stejný jako rozhraní API, které používáte pro práci s daty v Code First. I v případě, že máte v úmyslu použít Database First, můžete naučit pro zpracování složitějších scénáře, jako je čtení a aktualizaci související data, zpracování konfliktů souběžnosti, a tak dále z Code First kurzu. Jediný rozdíl spočívá v tom, jak vytvořit databázi, třída kontextu a tříd entit.

> [!div class="step-by-step"]
> [Předchozí](enhancing-data-validation.md)
