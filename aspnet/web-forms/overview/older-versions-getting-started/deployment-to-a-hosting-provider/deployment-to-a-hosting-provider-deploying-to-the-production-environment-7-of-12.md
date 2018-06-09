---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení do produkčního prostředí - 7 12 | Microsoft Docs'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: ab3b7ba332deddae7d04fc37c7aabc72bdb2d17e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "30889680"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení do produkčního prostředí - 7 12
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu nastavíte účet s poskytovatele hostitelských služeb a nasazení vaší ASP.NET funkce publikování webové aplikace do produkčního prostředí pomocí sady Visual Studio jedním kliknutím.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Výběr poskytovatele hostitelských služeb

Pro aplikace společnosti Contoso univerzity a tento kurz řady budete potřebovat zprostředkovatele, který podporuje technologii ASP.NET 4 a nasazení webu. Konkrétní hostingové společnosti jste vybrali, takže může kurzů k objasnění dokončení prostředí nasazení na živý web. Každý hostující společnost poskytuje různých funkcí a možností nasazení serverů se poněkud liší. Proces popsaný v tomto kurzu se ale typické pro celý proces. Poskytovatel hostingu používá pro účely tohoto kurzu, Cytanium.com, je jedním z mnoha, které jsou k dispozici, a jeho použití v tomto kurzu nepředstavuje ke schválení nebo doporučení.

Jakmile budete připraveni k výběru vlastního poskytovatele hostitelských služeb, můžete porovnat funkce a ceny v [Galerie zprostředkovatelů](https://www.microsoft.com/web/hosting) na webu http://www.microsoft.com/web/gallery/install.aspx?appid=IISExpress.

## <a name="creating-an-account"></a>Vytvoření účtu

Vytvořte účet v vybraného zprostředkovatele. Pokud podpora úplnou databázi systému SQL Server je přidání velmi, není potřeba vyberte ho v tomto kurzu, ale budete ho potřebovat [migrace do systému SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) kurz později z této série.

Tyto kurzy nemáte zaregistrovat nový název domény. Můžete otestovat ověření úspěšného nasazení pomocí dočasné adresy URL, které jsou přiřazeny k lokalitě zprostředkovatelem.

Po vytvoření účtu se obvykle zobrazit uvítací e-mail, který obsahuje všechny informace, které potřebujete k nasazení a správě vaší lokality. Informace, které vám pošle poskytovatele hostingových služeb, bude vypadat podobně jako informace zobrazené v tomto poli. Uvítací e-mail Cytanium, odeslaný na nový účet vlastníky obsahuje následující informace:

- Adresa URL pro poskytovatele ovládací prvek panel lokality, kde můžete spravovat nastavení pro svůj web. ID a heslo, které jste zadali jsou zahrnuty v této části Uvítacího e-mailu referenční. (Obojí, byl změněn na ukázku hodnotu pro tento obrázek.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Výchozí verze rozhraní .NET Framework a informace o tom, jak ho změnit. Mnoho hostování lokality výchozí nastavení 2.0, který funguje s aplikacemi technologie ASP.NET, cílených na rozhraní .NET Framework 2.0, 3.0 nebo 3.5. Contoso univerzity je ale aplikace pomocí rozhraní .NET Framework 4, budete muset změnit toto nastavení. (Pro aplikace ASP.NET 4.5 by použít nastavení rozhraní .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Dočasné adresa URL, který můžete použít pro přístup vašeho webu. Když tento účet byl vytvořen, "contosouniversity.com" byl zadán jako stávající název domény. Proto je dočasný adresa URL `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informace o tom, jak nastavit databáze a připojovací řetězce, které potřebujete, aby bylo možné přistupovat k nim:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informace o nástrojích a nastavení pro nasazení webu. (E-mailu z Cytanium také uvádí WebMatrix, který je zde vynechaný.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Nastavení verze rozhraní .NET Framework

Uvítací e-mail Cytanium zahrnuje odkaz na pokyny, jak změnit verzi rozhraní .NET Framework. Tyto pokyny popisují, že to lze provést prostřednictvím ovládacího panelu Cytanium. Ovládací prvek panel lokalit, které vypadají máte jiných poskytovatelů, nebo se může vyzvat vás k tomu jiným způsobem.

Přejděte na adresu URL ovládacích panelech. Až se přihlásíte svoje uživatelské jméno a heslo, najdete v části Ovládací panely.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

V **hostování prostory** , podržte ukazatel nad ikonu Web a vyberte položku **weby** z nabídky.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

V **weby** pole, klikněte na tlačítko **contosouniversity.com** (název serveru, který jste použili při vytvoření účtu).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

V **webu vlastnosti** vyberte **rozšíření** kartě.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Změnit ASP.NET z **2.0 integrovaného kanálu** k **4.0 (integrovaného kanálu)** a potom klikněte na **aktualizace**.

## <a name="publishing-to-the-hosting-provider"></a>Publikování do poskytovatele hostitelských služeb

Uvítací e-mail od poskytovatele hostitelských služeb obsahuje všechna nastavení, které potřebujete k publikování tohoto projektu a tyto informace mohou zadat ručně do profilu publikování. Ale budete používat jednodušší a méně náchylný metoda konfigurace nasazení k poskytovateli: Stáhněte si *.publishsettings* souboru a importujte ho do profilu publikování.

V prohlížeči přejděte do ovládacích panelů Cytanium a vyberte **webové** a pak vyberte **webů.**

![Ovládací panely výběrem webů](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Vyberte **contosouniversity.com** webu.

![Výběr contosouniversity.com ovládací panely](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Vyberte **publikování na webu** kartě.

![Karta publikování na webu Panel ovládacího prvku](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Vytvoření pověření pro zadáním uživatelského jména a hesla pro publikování webů. Můžete zadat stejné přihlašovací údaje, které používáte k přihlášení na ovládacích panelech. Pak klikněte na tlačítko **povolit**.

![Ovládací panely vytvořit přihlašovací údaje pro publikování](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Klikněte na tlačítko **stáhnout profil publikování pro tento web**.

![Ovládací prvek Panel stažení profilu publikování](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Po zobrazení výzvy k otevření nebo uložení souboru, uložte ho.

![Uložte soubor profilu publikování](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

V **Průzkumníku řešení** v sadě Visual Studio, klikněte pravým tlačítkem na projekt ContosoUniversity a vyberte **publikovat**. **Publikovat Web** otevře se dialogové okno na **Preview** kartě s **Test** profil vybrat, protože se poslední profil, který jste použili.

Vyberte **profil** a pak klikněte **Import**.

![Webové Průvodce importem tlačítko Publikovat](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

V **importu nastavení publikování** dialogové okno, vyberte *.publishsettings* souboru, který jste stáhli a klikněte na tlačítko **otevřete**. Průvodce přejde na kartu připojení s všechna pole vyplněna.

![Publikování webové kartě připojení Průvodce](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Soubor .publishsettings vloží plánované trvalé adresa URL webu do pole cílová adresa URL, ale pokud jste si nezakoupili tuto doménu ještě, nahraďte hodnotu dočasnou adresu URL. V tomto příkladu se adresa URL je  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* Jediným účelem: Toto políčko je určit, jaké adresy URL v prohlížeči se otevře s automaticky po úspěšně po nasazení. Pokud pole ponecháte prázdné, pouze důsledkem je, že v prohlížeči se automaticky spouštět nebude po nasazení.

Klikněte na tlačítko **ověřit připojení** k ověřte, zda jsou nastavení správná, a můžete připojit k serveru. Jak už jste viděli dříve, zelená značka zaškrtnutí ověřuje, že je připojení úspěšné.

Po kliknutí na tlačítko ověřit připojení, můžete se setkat **Chyba certifikátu** dialogové okno. Pokud tak učiníte, ověřte, zda název serveru očekávat. Je-li, vyberte **uložit tento certifikát pro budoucí relace sady Visual Studio** a klikněte na tlačítko **přijmout**. (Tato chyba znamená, že poskytovatel hostingu rozhodl vyhnout nákladů na zakoupení certifikátu protokolu SSL pro adresu URL, kterou nasazujete. Pokud dáváte přednost navázat zabezpečené připojení pomocí platného certifikátu, obraťte se na svého poskytovatele hostingu.)

![Chyba certifikátu](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Klikněte na tlačítko **Další**.

V **databáze** části **nastavení** zadejte stejné hodnoty, které jste zadali pro Test profilu publikování. Připojovací řetězce, které potřebujete najdete v rozevíracích seznamech.

- V poli připojovací řetězec pro **SchoolContext,** vyberte `Data Source=|DataDirectory|School-Prod.sdf`
- V části **SchoolContext**, vyberte **použít migrace Code First**.
- V poli připojovací řetězec pro **objekt DefaultConnection**, vyberte možnost `Data Source=|DataDirectory|aspnet-Prod.sdf`
- V části **objekt DefaultConnection**, nechte **aktualizace databáze** vymazán.

![Karta nastavení průvodce webové publikování](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Klikněte na tlačítko **Další**.

V **Preview** , klikněte na **spustit Náhled** zobrazíte seznam souborů, které budou zkopírovány. Zobrazí seznam, který jste předtím viděli při nasazení pro službu IIS v místním počítači.

Před publikováním, změňte název profilu tak, aby se použijí souboru Web.Production.config transformace. Vyberte **profil** a klikněte na **spravovat profily**.

![Publikování webové průvodce spravovat profily](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

V **Upravit profily publikování webové** dialogové okno pole, vyberte profil, výroby, klikněte na **přejmenovat**a změňte název profilu do produkčního prostředí. Pak klikněte na tlačítko **Zavřít**.

![Profily publikování webové dialogové okno Upravit](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Klikněte na tlačítko **publikování**.

Aplikace se publikuje pro poskytovatele hostingu. Zobrazuje výsledek **výstup** okno.

![Výstup – okno po nasazení](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Automaticky otevře v prohlížeči adresu URL, kterou jste zadali v **cílová adresa URL** pole na **připojení** kartě **Publikovat Web** průvodce. Zobrazí domovské stránce stejný jako při spuštění webu v sadě Visual Studio, s výjimkou nyní neexistuje žádná "(testovací)" nebo "(vývoj)" prostředí indikátoru v záhlaví okna. Znamená to, že indikátor prostředí *Web.config* transformace fungovala správně.

> [!NOTE]
> Pokud stále vidět "(testovací)" v záhlaví, odstraňte *obj* složku z projektu ContosoUniversity a znovu ho zaveďte. V předběžné verze softwaru dřív aplikovaná transformaci souboru (Web.Test.config) může získat použít znovu i když používáte produkční profil.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Před spuštěním stránky, který způsobí, že přístup k databázi, ujistěte se, že Elmah bude moci přihlásit všechny chyby, ke kterým dochází.

## <a name="setting-folder-permissions-for-elmah"></a>Nastavení oprávnění ke složce pro Elmah

Jak si pamatovat z předchozí kurzu této série, musí se ujistěte, že aplikace má oprávnění k zápisu pro složku v aplikaci, kde Elmah ukládá soubory protokolu chyb. Při nasazení pro službu IIS místně v počítači, můžete nastavit oprávnění ručně. V této části se zobrazí, nastavení oprávnění na Cytanium. (Někteří poskytovatelé hostingu nemusí povolit k tomu, nabízejí jeden nebo více předem definovaných složek s oprávněními k zápisu. V takovém případě je třeba změnit aplikace pomocí zadané složky.)

V Ovládacích panelech Cytanium můžete nastavit oprávnění složky. Přejděte na ovládací prvek panelu Adresa URL a vyberte **Správce souborů**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

V **Správce souborů** vyberte **contosouniversity.com** a potom **wwwrooot** zobrazíte kořenové složce aplikace. Klepněte na ikonu zámku vedle **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

V **soubor**/**složkám** vyberte **čtení** a **zápisu** zaškrtávací políčka pro  **contosouniversity.com** a klikněte na tlačítko **nastavit oprávnění**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Ujistěte se, že Elmah má oprávnění k zápisu do *Elmah* složky způsobuje chybu a pak zobrazením zprávy o chybách Elmah. Neplatná adresa URL jako žádosti *Studentsxxx.aspx*. Jak tady vidíte *GenericErrorPage.aspx* stránky. Klikněte **Odhlásit** propojit a poté spusťte *Elmah.axd*. Můžete získat **protokolu v** nejprve stránky, která ověří, jestli *Web.config* transformace úspěšně přidal Elmah autorizace. Po přihlášení, zobrazí sestavu, zobrazí se chyba, kterou právě způsobil.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Testování v produkčním prostředí

Spustit **studenty** stránky. Aplikace se pokusí o přístup k databázi školy poprvé, která aktivuje migrace Code First k vytvoření databáze. Při zobrazení stránky po okamžiku zpoždění, ukazuje, že neexistují žádné studenty.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Spustit **vyučující** a ověřte, že počáteční data úspěšně vložena lektorem dat v databázi.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Jako jste to udělali v testovacím prostředí, chcete ověřit, že aktualizace databáze fungovat v produkčním prostředí, ale obvykle nechcete zadat testovací data do provozní databáze. V tomto kurzu budete používat stejným způsobem, jakým jste to udělali v testu. Ale v reálné aplikaci můžete chtít najít metodu, která ověří, že databáze jsou aktualizace úspěšná bez zavedení testovací data do provozní databáze. V některých aplikacích může být potřeba přidání dat a poté jej odstraňte.

Přidat student a zobrazte data, které jste zadali v **studenty** a ověřte, že můžete aktualizovat data v databázi.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Ověřte, že se autorizační pravidla správně funguje tak, že vyberete **aktualizace kredity** z **kurzy** nabídky. **Protokolu v** zobrazí se stránka. Zadejte přihlašovací údaje účtu správce, klikněte na tlačítko **protokolu v**a **aktualizace kredity** zobrazí se stránka.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Pokud je přihlášení úspěšné, **aktualizace kredity** zobrazí se stránka. To znamená, že databáze členství technologie ASP.NET (pomocí účtu správce jednoho) byl úspěšně nasazen.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Teď úspěšně nasadit a otestovat váš web a je k dispozici veřejně přes Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Vytváření spolehlivější testovacího prostředí

Jak je popsáno v [nasazení v prostředí testovací](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) kurzu nejspolehlivější testovacího prostředí bude druhého účtu u poskytovatele hostingu, který má stejně jako výrobní účtu. To může být dražší než pomocí místní služby IIS jako testovacího prostředí vzhledem k tomu, že budete muset zaregistrovat druhý hostování účet. Ale pokud zabraňuje produkční lokality chyby nebo výpadkům, můžete se rozhodnout, že je vhodné náklady.

Většina procesu vytváření a nasazení do testovací účet je podobná co již bylo provedeno nasazení do produkčního prostředí:

- Vytvoření *Web.config* soubor transformace.
- Vytvoření účtu u poskytovatele hostingu.
- Vytvořte nový profil publikování a nasazení do testovací účet.

### <a name="preventing-public-access-to-the-test-site"></a>Brání veřejný přístup k webu testu

Důležitá poznámka pro testovací účet se bude za provozu v síti Internet, ale nechcete, aby veřejné ji použít. Kvůli zachování webu uživatelů můžete použít jeden nebo více z následujících metod:

- Obraťte se na poskytovatele hostingu pravidel brány firewall, které umožňují přístup k webu testování pouze z IP adresy, které můžete použít pro testování.
- Skrytí adresu URL, takže není podobná veřejné adresy URL.
- Použití *robots.txt* souboru zajistit, že nebude být z vyhledávacích webů Procházet k němu testovací lokality a sestava odkazy ve výsledcích hledání.

První z těchto metod je samozřejmě nejbezpečnější, ale pro tento postup je specifická pro každého poskytovatele hostitelských služeb a nebudou uvedena v tomto kurzu. Pokud je uspořádat u svého poskytovatele hostingu povolit pouze vaše IP adresa a přejděte na adresu URL účtu test, teoreticky nemáte si dělat starosti vyhledávače procházení ho. Ale i v takovém případě nasazení *robots.txt* souboru je vhodné jako zálohu, v případě, že daného pravidla brány firewall je někdy omylem vypnutý.

*Robots.txt* soubor ocitne ve složce projektu a měli následující text:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent` Řádek říká vyhledávací weby, které pravidla v souboru platí pro všechny vyhledávací modul webové prohledávací moduly (robotů), a `Disallow` řádku určuje, že žádné stránky na webu Procházet.

Budete ho zřejmě chtít vyhledávací weby snadněji katalogu provozního webu, takže je třeba vyloučit tento soubor z produkčního nasazení. To lze provést, najdete v části **můžete vyloučit konkrétní soubory nebo složky z nasazení?** v [ASP.NET webové aplikace projektu nasazení – nejčastější dotazy](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Zajistěte, aby zadat vyloučení pouze provozní profilu publikování.

Vytvoření druhého hostování účtu je přístup k práci s testovacím prostředí, která není povinný, ale může být vhodné vytvářet. V následujících kurzech můžete pokračovat v používání služby IIS jako testovacího prostředí.

V dalším kurzu budete aktualizaci kódu aplikace a nasazení změny do testovací a produkční prostředí.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [další](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
