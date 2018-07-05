---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení do produkčního prostředí – 7 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 26a832fd336f886ba1ddfb1682930afa4df56c58
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383633"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení do produkčního prostředí – 7 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu, můžete nastavit účet u nějakého poskytovatele hostingu a nasadit vaše ASP.NET funkce publikovat webovou aplikaci do produkčního prostředí pomocí sady Visual Studio jedním kliknutím.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Výběr poskytovatele hostingu

Aplikace Contoso University a v této sérii kurzů je nutné poskytovatele, který podporuje technologii ASP.NET 4 a nasazení webu. Konkrétní hostingové společnosti byla vybrána tak, aby v kurzech může ukazuje kompletní prostředí nasazení na živý web. Každý hostingové společnosti poskytuje různé funkce a prostředí nasazení na servery se mírně liší. Proces popsaný v tomto kurzu ale typické pro celý proces. Poskytovatel hostingu použitou v tomto kurzu Cytanium.com, je jedním z mnoha, které jsou k dispozici a jeho použití v tomto kurzu nepředstavuje k potvrzení nebo doporučení.

Jakmile budete připraveni k výběru vlastního poskytovatele hostitelských služeb, můžete porovnat funkce a ceny v [Galerie zprostředkovatelů](https://www.microsoft.com/web/hosting) na webu Microsoft.com/web.

## <a name="creating-an-account"></a>Vytvoření účtu

Vytvoření účtu na vybraného zprostředkovatele. Pokud podpora úplné databáze serveru SQL Server je přidání dalších, není potřeba vyberte pro účely tohoto kurzu, ale budete potřebovat pro [migrace na SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) později v této sérii kurzů.

Pro tyto kurzy nemusíte registrovat nový název domény. Můžete otestovat a ověřit úspěšné nasazení pomocí dočasné adresy URL, které jsou přiřazeny k lokalitě poskytovatele.

Po vytvoření účtu se obvykle zobrazí uvítací e-mail, který obsahuje všechny informace, které potřebujete k nasazení a spravujte svůj web. Informace, které vám pošle váš poskytovatel hostingu bude podobný co je znázorněna zde. Cytanium Uvítacího e-mailu, které je odesláno nové vlastníci účtu obsahuje následující informace:

- Adresa URL poskytovatele řízení panelu webu, kde můžete spravovat nastavení pro váš web. ID a heslo, které jste zadali, jsou zahrnuté v této části Uvítacího e-mailu pro snadné odkazování. (Obojí, byly změněny na ukázku hodnotu pro tento obrázek.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Výchozí verze rozhraní .NET Framework a informace o tom, jak ho změnit. Mnoho hostování webů výchozí 2.0, který funguje s aplikacemi ASP.NET, které jsou cíleny rozhraní .NET Framework 2.0, 3.0 nebo 3.5. Contoso University je ale aplikace rozhraní .NET Framework 4, budete muset toto nastavení změnit. (Pro technologii ASP.NET 4.5 aplikaci použijete nastavení rozhraní .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Dočasné adresa URL, která vám umožní přístup k webovému serveru. Když tento účet byl vytvořen, "contosouniversity.com" byl zadán jako existující název domény. Proto je dočasný adresa URL `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informace o tom, jak nastavit databází a připojovací řetězce, které potřebujete, aby bylo možné přistupovat k nim:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informace o nástrojích a nastavení pro nasazení webu. (E-mailů z Cytanium také uvádí služby WebMatrix, což je tady jsme ji vynechali.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Nastavení verze rozhraní .NET Framework

Cytanium uvítací e-mail obsahuje odkaz na pokyny o tom, jak změnit verzi rozhraní .NET Framework. Tyto pokyny vysvětlují, že to lze provést prostřednictvím ovládacího panelu Cytanium. Ostatní poskytovatelé, kteří mají ovládací prvky panel stránky, které vypadají jinak nebo se možná dáte pokyn, aby vám k tomu jiným způsobem.

Přejděte na adresu URL ovládacího panelu. Po přihlášení pomocí uživatelského jména a hesla, najdete v Ovládacích panelech.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

V **hostování prostory** podržením ukazatele nad ikonou Web a vyberte **weby** z nabídky.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

V **weby** klikněte **contosouniversity.com** (název webu, který jste použili při vytváření účtu).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

V **vlastnosti webu** vyberte **rozšíření** kartu.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Změnit ASP.NET z **2.0 integrovaného kanálu** k **4.0 (integrovaného kanálu)** a potom klikněte na tlačítko **aktualizace**.

## <a name="publishing-to-the-hosting-provider"></a>Publikování do poskytovatele hostitelských služeb

Uvítacího e-mailu od poskytovatele hostitelských služeb zahrnuje všechna nastavení, které potřebujete k publikování projektu a tyto informace můžete zadat ručně do profilu publikování. Ale použijeme jednodušší a méně náchylný metoda konfigurace nasazení tak, aby zprostředkovatel: Stáhněte si *.publishsettings* soubor a naimportujte ho do profilu publikování.

V prohlížeči přejděte do ovládacích panelů Cytanium a vyberte **webové** a pak vyberte **webů.**

![Ovládací Panel výběru webů](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Vyberte **contosouniversity.com** webu.

![Ovládací Panel výběru contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Vyberte **publikování na webu** kartu.

![Publikování na webu Panel ovládacího prvku karta](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Vytvořte přihlašovací údaje pro tak, že zadáte uživatelské jméno a heslo pro publikování webů. Můžete zadat stejné přihlašovací údaje, které používáte k přihlášení do ovládacích panelů. Pak klikněte na tlačítko **povolit**.

![Ovládací Panel vytvořit přihlašovací údaje pro publikování](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Klikněte na tlačítko **stáhnout profil publikování pro tento web**.

![Ovládací prvek Panel stáhnout profil publikování](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Když se zobrazí výzva k otevření nebo uložení souboru, uložte ho.

![Uložte soubor profilu publikování](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

V **Průzkumníka řešení** ve Visual Studiu klikněte pravým tlačítkem na projekt ContosoUniversity a vyberte **publikovat**. **Publikovat Web** dialogové okno se otevře na **ve verzi Preview** kartu s **Test** profil vybrat, protože to je poslední profil, který jste použili.

Vyberte **profilu** kartu a potom klikněte na tlačítko **Import**.

![Web Průvodce importem tlačítko Publikovat](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

V **Import nastavení publikování** dialogové okno, vyberte *.publishsettings* soubor, který jste stáhli a klikněte na tlačítko **otevřít**. Průvodce přejde na kartě připojení s všechna pole vyplněna.

![Publikování webových kartě připojení Průvodce](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Soubor .publishsettings vloží do pole Adresa URL cílové plánované trvalé adresy URL pro web, ale pokud jste si nezakoupili tuto doménu ještě, nahraďte hodnotu dočasné adresy URL. V tomto příkladu je adresa URL  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* Jediným účelem: Toto políčko je k určení, jaké adresy URL v prohlížeči se otevře na automaticky po úspěšně po nasazení. Pokud je necháte prázdnou, pouze důsledkem je, že prohlížeč se nespustí automaticky po nasazení.

Klikněte na tlačítko **ověřit připojení** k ověření, že je nastavení správné a můžete připojit k serveru. Protože jste předtím viděli, zelená značka zaškrtnutí ověřuje, že je připojení úspěšné.

Po kliknutí na ověřit připojení, může se zobrazit **Chyba certifikátu** dialogové okno. Pokud tak učiníte, ověřte, že je název serveru, co očekáváte. Pokud se jedná, vyberte **uložení tohoto certifikátu na budoucí relace sady Visual Studio** a klikněte na tlačítko **přijmout**. (Tato chyba znamená, že poskytovatel hostingu rozhodl vyhnout prostředky na nákup certifikátu SSL pro adresu URL, kterou nasazujete. Pokud chcete navázat zabezpečené připojení pomocí platného certifikátu, kontaktujte poskytovatele hostingových služeb.)

![Chyba certifikátu](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Klikněte na tlačítko **Další**.

V **databází** část **nastavení** kartu, zadejte stejné hodnoty, které jste zadali pro Test profil publikování. V rozevíracích seznamech najdete připojovací řetězce, které potřebujete.

- V poli připojovací řetězec pro **SchoolContext,** vyberte `Data Source=|DataDirectory|School-Prod.sdf`
- V části **SchoolContext**vyberte **použít migrace Code First**.
- V poli připojovací řetězec pro **objekt DefaultConnection**vyberte `Data Source=|DataDirectory|aspnet-Prod.sdf`
- V části **objekt DefaultConnection**, ponechte **aktualizace databáze** vymazána.

![Karta nastavení Průvodce Web publikovat](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Klikněte na tlačítko **Další**.

V **ve verzi Preview** klikněte na tlačítko **spustit Náhled** zobrazíte seznam souborů, které budou zkopírovány. Zobrazí seznam, který jste předtím viděli při nasazení do služby IIS v místním počítači.

Před publikováním, změňte název profilu tak, aby vaše Web.Production.config transformační soubor se použije. Vyberte **profilu** kartě a klikněte na tlačítko **spravovat profily**.

![Publikování webové průvodce spravovat profily](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

V **Upravit profily publikování webových** dialogové okno Vyberte profil produkčního prostředí, klikněte na tlačítko **přejmenovat**a změňte název profilu do produkčního prostředí. Pak klikněte na tlačítko **Zavřít**.

![Profily publikování webových dialogové okno Upravit](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Klikněte na tlačítko **publikovat**.

Aplikace je publikována pro poskytovatele hostingu. Zobrazuje výsledek **výstup** okna.

![Okno výstup po nasazení](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Automaticky otevře v prohlížeči na adresu URL, kterou jste zadali v **cílovou adresu URL** pole na **připojení** karty **publikování webu** průvodce. Zobrazit domovské stránce stejný jako při spuštění webu v sadě Visual Studio, s výjimkou nyní neexistuje žádný "()" nebo ukazatel prostředí "(vývoj)" v záhlaví okna. Znamená to, že ukazatel prostředí *Web.config* transformace fungoval správně.

> [!NOTE]
> Pokud se stále zobrazuje "(testovací)" v záhlaví, odstranit *obj* složku z ContosoUniversity projektu a opětovné nasazení. V předběžné verze softwaru dříve použitých transformačního souboru (Web.Test.config) může použije znovu i když použijete profil produkčního prostředí.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Předtím, než spustíte stránku, která způsobí, že přístup k databázi, ujistěte se, že bude Elmah může zapisovat do protokolu všechny chyby, ke kterým dochází.

## <a name="setting-folder-permissions-for-elmah"></a>Nastavení oprávnění ke složce pro Elmah

Jak si pamatovat z předchozího kurzu v této sérii, musí se ujistěte, že aplikace má oprávnění k zápisu pro složku ve vaší aplikaci, kde Elmah ukládá soubory protokolu chyb. Při nasazení do služby IIS místně v počítači, tato oprávnění můžete nastavit ručně. V této části uvidíte, jak nastavit oprávnění na Cytanium. (Někteří poskytovatelé hostingu nemusí umožňovat můžete k tomu, nabízejí jeden nebo více předem definovaných složek s oprávněním pro zápis. V takovém případě je třeba změnit aplikace pomocí zadaných složek.)

V Ovládacích panelech Cytanium můžete nastavit oprávnění pro složky. Přejděte na ovládací prvek panelu adresy URL a vyberte **Správce souborů**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

V **Správce souborů** vyberte **contosouniversity.com** a potom **wwwrooot** zobrazíte kořenové složky vaší aplikace. Klikněte na ikonu zámku vedle **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

V **souboru**/**oprávnění složky** okna, vyberte **čtení** a **zápisu** zaškrtávací políčka pro  **contosouniversity.com** a klikněte na tlačítko **nastavit oprávnění**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Ujistěte se, že Elmah oprávnění k zápisu *Elmah* složky způsobí chybu a pak zobrazením Elmah zprávy o chybách. Neplatná adresa URL jako požadavku *Studentsxxx.aspx*. Jak vidíte, *GenericErrorPage.aspx* stránky. Klikněte na tlačítko **Odhlásit** propojit a pak spusťte *Elmah.axd*. Získáte **přihlásit** stránce první, který ověří, jestli *Web.config* transformace byla úspěšně přidána Elmah autorizace. Po přihlášení se zobrazí sestavu, zobrazí se chyba, kterou právě způsobila.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Testování v produkčním prostředí

Spustit **studenty** stránky. Aplikace se pokusí o přístup k databázi School poprvé, aktivuje se migrace Code First k vytvoření databáze. Na stránce se zobrazí po prodlevě upozorňování, zobrazí, že neexistují žádní studenti.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Spustit **Instruktoři** stránku a ověřte, že počáteční data kurzů vedených data úspěšně vložena do databáze.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Stejně jako v testovacím prostředí, budete chtít ověřit, že aktualizace databáze fungovat v produkčním prostředí, ale obvykle nechcete zadat testovací data do provozní databáze. Pro účely tohoto kurzu budete používat stejné metody, které jste provedli v testu. Ale v reálné aplikaci můžete chtít najít metodu, která ověřuje, že databáze jsou aktualizace úspěšná bez vnášení testovací data do provozní databáze. V některých aplikacích může být praktické přidat nějakou položku a pak ji odstraňte.

Přidat student a zobrazte data zadaná v **studenty** stránku a ověřte, že můžete aktualizovat data v databázi.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Ověřte, že jsou pravidla autorizace správně funguje tak, že vyberete **aktualizace kredity** z **kurzy** nabídky. **Přihlásit** zobrazí se stránka. Zadejte svoje přihlašovací údaje účtu správce, klikněte na tlačítko **přihlásit**a **aktualizace kredity** zobrazí se stránka.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Pokud je přihlášení úspěšné, **aktualizace kredity** zobrazí se stránka. To znamená, že databáze členství technologie ASP.NET (pomocí účtu správce jednoho) se úspěšně nasadilo.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Budete mít teď úspěšně nasadila a otestovala vašeho webu a je k dispozici veřejně přes Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Vytváření spolehlivější testovacího prostředí

Jak je vysvětleno v [nasazení do prostředí Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) kurzu nejspolehlivější testovacího prostředí bude druhého účtu na poskytovatele hostingu, který má stejně jako produkčního účtu. To může být nákladnější než použití místní služby IIS jako testovacího prostředí, protože budete muset zaregistrovat druhý hostitelský účet. Ale pokud zabraňuje produkční lokality chyby nebo výpadky, můžete se rozhodnout, že se vyplatí.

Většina procesů pro vytváření a nasazování do testovací účet je podobný co již bylo provedeno nasazení do produkčního prostředí:

- Vytvoření *Web.config* transformačního souboru.
- Vytvoření účtu u poskytovatele hostingu.
- Vytvořte nový profil publikování a nasazení do testovacího účtu.

### <a name="preventing-public-access-to-the-test-site"></a>Brání veřejný přístup k webu testu

Což je důležité pro testovací účet je, že je na Internetu, ale nechcete, aby veřejně ji používat. Zachovat privátní lokality můžete použít jeden nebo více z následujících metod:

- Obraťte se na poskytovatele hostingu k nastavení pravidla brány firewall, která umožňují přístup k webu testování jenom z IP adresy, které používáte pro účely testování.
- Skrýt adresu URL tak, že není podobně jako adresa URL veřejného webu.
- Použití *robots.txt* soubor tak, aby, nesmí být z vyhledávací weby procházení k němu testu webu a sestavy odkazy ve výsledcích hledání.

První z těchto metod je samozřejmě nejbezpečnější, ale postup, který je specifický pro každého poskytovatele hostitelských služeb a nesmí být zahrnuté v tomto kurzu. Je-li uspořádat u poskytovatele hostingu umožňuje pouze vaši IP adresu a přejděte do testovací adresa URL účtu, teoreticky nemusíte starat o procházení ho vyhledávací weby. Ale i v takovém případě nasazení *robots.txt* souboru je vhodné jako záložní v případě, že toto pravidlo brány firewall je někdy omylem vypnutý.

*Robots.txt* soubor přejde do složky projektu a v něm musí mít následující text:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent` Řádek říká vyhledávací weby, které v souboru pravidla se vztahují na všechny vyhledávací modul web prohledávací moduly (roboty), a `Disallow` řádek určuje, že by bylo žádné stránky na webu.

Budete zřejmě chtít vyhledávací weby snadněji katalogu svoje produkční lokality, takže je třeba vyloučit tento soubor z produkčního nasazení. To mohli udělat, najdete v článku **můžu vyloučit určité soubory nebo složky z nasazení?** v [technologie ASP.NET webové aplikace projektu nasazení – nejčastější dotazy](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Ujistěte se, že zadáte vyloučení jenom pro produkční profil publikování.

Vytvoření druhého hostování účtu je takový přístup k práci s testovací prostředí, které se nevyžaduje, ale může být vhodné přidání výdajů. V následujících kurzech se budete dál používat službu IIS jako testovacího prostředí.

V dalším kurzu budete moct aktualizovat kód aplikace a nasadit změny na testovacím a produkčním prostředí.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [další](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
