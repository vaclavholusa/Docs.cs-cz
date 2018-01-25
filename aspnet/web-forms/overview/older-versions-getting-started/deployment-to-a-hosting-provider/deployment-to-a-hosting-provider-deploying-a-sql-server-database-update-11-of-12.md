---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizaci databáze SQL Server - 11 12 | Microsoft Docs"
author: tdykstra
description: "Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: aeec69c7373a111d30e8f32a374a9f02fb4c080a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizaci databáze SQL Server - 11 12
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit na weby systému Windows Azure, najdete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu se dozvíte, jak nasadit aktualizace databáze úplnou databázi serveru SQL Server. Vzhledem k tomu, že migrace Code First vykonává všechnu práci aktualizace databáze, je téměř identický nebyla pro SQL Server Compact v procesu [nasazení aktualizace databáze](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) kurzu.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Přidá sloupec do tabulky

V této části kurzu budete mít databázi změnit a odpovídající změn kódu v rámci přípravy jejich nasazení do testovací a produkční prostředí je testování v sadě Visual Studio. Tato změna vyžaduje přidání `OfficeHours` sloupec, který se `Instructor` entitu a zobrazení nové informace o v **vyučující** webové stránky.

Otevřete v projektu ContosoUniversity.DAL *Instructor.cs* a přidejte následující vlastnost mezi `HireDate` a `Courses` vlastnosti:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Aktualizace třídy pro inicializátoru tak, aby ho doplňuje pro nový sloupec s testovacích datech. Otevřete *Migrations\Configuration.cs* a nahraďte bloku kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který zahrnuje nový sloupec:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidat nové šablony pole pro čas office těsně před uzavírací `</Columns>` značky v prvním `GridView` ovládacího prvku:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Sestavte řešení.

Otevřete **Konzola správce balíčků** okno a vyberte ContosoUniversity.DAL jako **výchozí projekt**.

Zadejte následující příkazy:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Spusťte aplikaci a vyberte **vyučující** stránky. Stránce trvá trochu déle než obvykle načíst, protože rozhraní Entity Framework znovu vytvoří databázi a doplňuje s testovacích datech.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Nasazení aktualizace databáze do testovacího prostředí

Pokud používáte migrace Code First, je stejné jako pro systém SQL Server Compact metodu pro nasazení změn databáze do systému SQL Server. Ale budete muset změnit Test profil publikování, protože stále jsou nastaveny na migraci ze systému SQL Server Compact do systému SQL Server.

Prvním krokem je odebrat Transformace řetězců připojení, které jste vytvořili v předchozí kurzu. Tyto už nejsou potřeba vzhledem k tomu, že specifikujete Transformace řetězců připojení v profil publikování, jako jste to udělali předtím, než jste nakonfigurovali **balení/publikování kódu SQL** kartě pro migraci do systému SQL Server.

Otevřete *Web.Test.config* souborů a odebrat `connectionStrings` elementu. Jenom zbývající transformací v *Web.Test.config* soubor `Environment` hodnotu `appSettings` element.

Nyní můžete aktualizovat profil publikování a publikovat do testovacího prostředí.

Otevřete **Publikovat Web** průvodce a potom přepnout na **profil** kartě.

Vyberte **Test** profilu publikování.

Vyberte **nastavení** kartě.

Klikněte na tlačítko **povolit novou databázi publikování vylepšení**.

V poli připojovací řetězec pro **SchoolContext**, zadejte stejnou hodnotu, která jste použili v *Web.Test.config* transformace souborů v předchozích kurzu:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**. (Ve vaší verzi sady Visual Studio může být označené políčko **použít migrace Code First**.)

V poli připojovací řetězec pro **objekt DefaultConnection**, zadejte stejnou hodnotu, která jste použili v *Web.Test.config* transformace souborů v předchozích kurzu:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Nechte **aktualizace databáze** vymazán.

Klikněte na tlačítko **publikování**.

Visual Studio nasadí změny kódu do testovacího prostředí a otevře prohlížeč na domovskou stránku univerzity Contoso.

Vyberte stránku vyučující.

Při spuštění aplikace na této stránce, pokusí se o přístup k databázi. Migrace Code First zkontroluje, zda je aktuální databáze a zjistí, že ještě nebyla použita nejnovější migrace. Migrace Code First použije nejnovější migrace, běží `Seed` metoda a stránky spustí normálně. Zobrazí nový sloupec Office hodin se dosazená data.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Nasazení aktualizace databáze do produkčního prostředí

Budete muset změnit také profil publikování pro produkční prostředí. V takovém případě budete odeberte existující profil a vytvořit novou importováním souboru .publishsettings aktualizované. Aktualizovaný soubor bude obsahovat připojovací řetězec pro databázi systému SQL Server v Cytanium.

Jak už jste viděli při nasazení do testovacího prostředí, již nepotřebujete transformací řetězec připojení v *Web.Production.config* soubor transformace. Otevřete soubor a odebrat `connectionStrings` elementu. Zbývající transformace jsou pro `Environment` hodnotu `appSettings` elementu a `location` element, který omezuje přístup Elmah chybových zpráv.

Než vytvoříte nový profil publikování pro produkční prostředí, stažení souboru .publishsettings aktualizované stejně jako jste to udělali v [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu. (V Ovládacích panelech Cytanium, klikněte na tlačítko **weby**a klikněte **contosouniversity.com** webu. Vyberte **publikování na webu** a pak klikněte **stáhnout profil publikování pro tento web**.) Důvod, proč byste to je vyzvednutí připojovací řetězec databáze v souboru .publishsettings. Připojovací řetězec nebyl k dispozici při prvním stažený soubor, protože stále používáte systém SQL Server Compact a kdyby databázi systému SQL Server v Cytanium dosud vytvořena.

Nyní můžete aktualizovat profil publikování a publikovat do produkčního prostředí.

Otevřete **Publikovat Web** průvodce a potom přepnout na **profil** kartě.

Klikněte na tlačítko **spravovat profily**a pak odstraňte produkční profil.

Zavřít **Publikovat Web** průvodce uložte tuto změnu.

Otevřete **Publikovat Web** průvodce znovu a pak klikněte na tlačítko **Import**.

Na **připojení** změňte **cílová adresa URL** na odpovídající hodnotu, pokud používáte dočasnou adresu URL.

Klikněte na tlačítko **Další**.

Na **nastavení** , klikněte na **povolit novou databázi publikování vylepšení**.

V rozevíracím seznamu řetězec připojení pro **SchoolContext**, vyberte Cytanium připojovací řetězec.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Vyberte **migrace provést Code First (spuštěno při spuštění aplikace)**.

V rozevíracím seznamu řetězec připojení pro **objekt DefaultConnection**, vyberte Cytanium připojovací řetězec.

Vyberte **profil** , klikněte na **spravovat profily**a přejmenujte jej z "contosouniversity.com - nasazení webu" na "Výroba".

Profil publikování uložte tuto změnu zavřete a pak znovu otevřete.

Klikněte na tlačítko **publikování**. (Pro skutečné produkční web, zkopírujte *aplikace\_offline.htm* do produkčního prostředí a vložit jej do složky projektu před publikováním, pak ji odebrat po dokončení nasazení.)

Visual Studio nasadí změny kódu do testovacího prostředí a otevře prohlížeč na domovskou stránku univerzity Contoso.

Vyberte stránku vyučující.

Migrace Code First aktualizuje databázi stejným způsobem jako v testovacím prostředí. Zobrazí nový sloupec Office hodin se dosazená data.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Teď úspěšně jste nasadili aktualizaci aplikace, která zahrnuté změně databáze použití databáze systému SQL Server.

## <a name="more-information"></a>Další informace

Dokončení této série kurzů k nasazení do hostujícího zprostředkovatele třetí strany webovou aplikaci ASP.NET. Další informace o všech témat zahrnutých v těchto kurzech najdete v tématu [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) na webu MSDN.

## <a name="acknowledgements"></a>Potvrzení

Děkujeme, že následující osob, které významně přispěli k obsahu tento kurz řady chci:

- [Alberto Poblacion, MVP &amp; MCT, Španělsko](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, vývoj platformy Data MVP, Spojené státy
- Harsh Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Jan Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Itálie](http://www.iamraf.net/)
- [Rick Anderson Microsoft](https://blogs.msdn.com/b/rickandy/)
- [SAYED Hashimi společnost Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

>[!div class="step-by-step"]
[Předchozí](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[další](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
