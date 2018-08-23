---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace databáze SQL serveru – 11 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2018b58207365a0a3829f1f359d46d765aad0a77
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755030"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace databáze SQL serveru – 11, 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do modelu weby Windows Azure, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu se dozvíte, jak nasadit aktualizace databáze na úplné databáze serveru SQL Server. Vzhledem k tomu, že migrace Code First vykonává všechnu práci aktualizace databáze, je téměř shodné s co jste to udělali pro SQL Server Compact v procesu [nasazení aktualizace databáze](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) kurzu.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Přidá sloupec do tabulky

V této části kurzu provede změnit databázi a odpovídající změny kódu, otestujete je v sadě Visual Studio při přípravě na nasazení do testovacího a produkčního prostředí. Tato změna vyžaduje přidání `OfficeHours` sloupec, který se `Instructor` entity a zobrazení nových informací v **Instruktoři** webové stránky.

Otevřete v projektu ContosoUniversity.DAL *Instructor.cs* a přidejte následující vlastnost mezi `HireDate` a `Courses` vlastnosti:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Aktualizace inicializátor třídy tak, aby se nasazení nasazuje nový sloupec s daty testu. Otevřít *Migrations\Configuration.cs* a nahraďte blok kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který obsahuje nový sloupec:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidat nové pole šablony pro office hodin těsně před uzavírací `</Columns>` značky v prvním `GridView` ovládacího prvku:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Sestavte řešení.

Otevřít **Konzola správce balíčků** okna a vyberte ContosoUniversity.DAL jako **výchozí projekt**.

Zadejte následující příkazy:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Spusťte aplikaci a vyberte **Instruktoři** stránky. Na stránce trvá o něco déle než obvykle. Chcete načíst, protože rozhraní Entity Framework znovu vytvoří databázi a nasazení se nasazuje s testovací data.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Nasazení aktualizace databáze do testovacího prostředí

Při použití migrace Code First metodu pro nasazení změn databáze na SQL serveru je stejná jako SQL Server Compact. Nicméně je nutné změnit testovací profil publikování, protože stále byla nastavená pro migraci ze systému SQL Server Compact do systému SQL Server.

Prvním krokem je odebrat transformace řetězec připojení, které jste vytvořili v předchozím kurzu. Tyto jsou už je nepotřebujete vzhledem k tomu, že zadáte transformace připojovací řetězec do profilu publikování, jako jste to udělali dříve, než jste nakonfigurovali **balení/publikování kódu SQL** kartu pro migraci na SQL Server.

Otevřít *Web.Test.config* souboru a odebrat `connectionStrings` elementu. Jediný zbývající transformace v *Web.Test.config* je soubor `Environment` hodnota v `appSettings` elementu.

Nyní můžete aktualizovat profil publikování a publikovat do testovacího prostředí.

Otevřít **Publikovat Web** průvodce a pak přepnout do **profilu** kartu.

Vyberte **Test** profil publikování.

Vyberte **nastavení** kartu.

Klikněte na tlačítko **povolit nová vylepšení publikování databáze**.

V poli připojovací řetězec pro **SchoolContext**, zadejte stejnou hodnotu, která jste použili v *Web.Test.config* transformační soubor v předchozím kurzu:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**. (Ve vaší verzi sady Visual Studio může být označené jako zaškrtávací políčko **použít migrace Code First**.)

V poli připojovací řetězec pro **objekt DefaultConnection**, zadejte stejnou hodnotu, která jste použili v *Web.Test.config* transformační soubor v předchozím kurzu:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Ponechte **aktualizace databáze** vymazána.

Klikněte na tlačítko **publikovat**.

Visual Studio nasadí změny kódu do testovacího prostředí a otevře prohlížeč na domovskou stránku společnosti Contoso University.

Vyberte stránku Instruktoři.

Při spuštění aplikace na této stránce, pokusí se o přístup k databázi. Migrace Code First zkontroluje, jestli je aktuální databáze a zjistí, že zatím nebyla použita nejnovější migrace. Migrace Code First použije nejnovější migrace, spustí `Seed` metoda a pak na stránce běží normálně. Zobrazí se nový sloupec Office Hours s dosazená data.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Nasazení aktualizace databáze do produkčního prostředí

Budete muset změnit také profil publikování pro produkční prostředí. V tomto případě budete odeberte existující profil a vytvořit nové importováním souboru .publishsettings aktualizované. Aktualizovaný soubor bude obsahovat připojovací řetězec pro databázi serveru SQL Server na Cytanium.

Jak jste viděli, kdy jste nasadili do testovacího prostředí, které už nepotřebujete transformace řetězec připojení v *Web.Production.config* transformačního souboru. Otevřete soubor a odebrat `connectionStrings` elementu. Zbývající transformace jsou pro `Environment` hodnotu `appSettings` element a `location` element, který omezuje přístup k Elmah zprávy o chybách.

Než vytvoříte nový profil publikování pro produkční prostředí, stažení souboru .publishsettings aktualizované stejně jako jste to udělali dříve v [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu. (V Ovládacích panelech Cytanium, klikněte na tlačítko **weby**a potom klikněte na tlačítko **contosouniversity.com** webu. Vyberte **publikování na webu** kartu a potom klikněte na tlačítko **stáhnout profil publikování pro tento web**.) Z důvodů, proč to děláte je ke sbírání připojovací řetězec databáze v souboru .publishsettings. Připojovací řetězec nebyl k dispozici při prvním stažený soubor, protože stále používali SQL Server Compact a kdyby databáze serveru SQL Server na Cytanium zatím nevytvořili.

Nyní můžete aktualizovat profil publikování a publikovat do produkčního prostředí.

Otevřít **Publikovat Web** průvodce a pak přepnout do **profilu** kartu.

Klikněte na tlačítko **spravovat profily**a pak odstranit profil produkčního prostředí.

Zavřít **publikování webu** průvodce a uložte tuto změnu.

Otevřít **Publikovat Web** průvodce znovu a pak klikněte na tlačítko **Import**.

Na **připojení** kartu, změňte **cílovou adresu URL** na odpovídající hodnotu, pokud používáte dočasné adresy URL.

Klikněte na tlačítko **Další**.

Na **nastavení** klikněte na tlačítko **povolit nová vylepšení publikování databáze**.

V rozevíracím seznamu připojovací řetězec pro **SchoolContext**, vyberte Cytanium připojovací řetězec.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Vyberte **provést Code First (spuštěno při spuštění aplikace)**.

V rozevíracím seznamu připojovací řetězec pro **objekt DefaultConnection**, vyberte Cytanium připojovací řetězec.

Vyberte **profilu** klikněte na tlačítko **spravovat profily**a přejmenování profilu "Produkční" z "contosouniversity.com – nástroj nasazení webu".

Profil publikování se uložit změnu zavřít a znovu ji spusťte.

Klikněte na tlačítko **publikovat**. (Pro skutečné produkční webovou stránku, zkopírujte *aplikace\_offline.htm* do produkčních a vložit ho do složky projektu před publikováním, odeberte ho po dokončení nasazení.)

Visual Studio nasadí změny kódu do testovacího prostředí a otevře prohlížeč na domovskou stránku společnosti Contoso University.

Vyberte stránku Instruktoři.

Migrace Code First aktualizuje databázi stejným způsobem jako fungovala v testovacím prostředí. Zobrazí se nový sloupec Office Hours s dosazená data.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Teď úspěšně jste nasadili aktualizaci aplikace, která zahrnuté změny databáze, použití databáze systému SQL Server.

## <a name="more-information"></a>Další informace

Dokončení tohoto postupu Tato série kurzů o nasazení webové aplikace ASP.NET do poskytovatele hostitelských služeb třetích stran. Další informace o některém z témat uvedených v následujících kurzech najdete v článku [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) na webové stránce MSDN.

## <a name="acknowledgements"></a>Potvrzení

Chci vám chceme poděkovat následující uživatelů, kteří provedli významné příspěvky k obsahu v této sérii kurzů:

- [Alberto Poblacion, MVP &amp; MCT, Španělsko](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, vývoj pro platformu dat MVP, Spojené státy
- Nepříznivých Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Itálie](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [SAYED Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Srbsko](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [další](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
