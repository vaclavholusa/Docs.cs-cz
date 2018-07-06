---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: c058d4314ea0bed87e22c0e6b3fa09b89b2423d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833352"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


## <a name="overview"></a>Přehled

Po počátečním nasazení pokračuje v práci údržbu a vývoj webu, a před dlouho budete chtít nasazení aktualizace. Tento kurz vás provede procesem nasazení aktualizace kódu aplikace. Aktualizaci, implementaci a nasazení v tomto kurzu nezahrnuje Změna databáze; uvidíte, co se liší o nasazení změn databází v dalším kurzu.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](troubleshooting.md).

## <a name="make-a-code-change"></a>Změně kódu

Jako jednoduchý příklad příkazu update pro vaši aplikaci, přidáte k **Instruktoři** stránce Seznam kurzy vedené vybrané instruktorem.

Při spuštění **Instruktoři** stránky, můžete si všimnout, že jsou **vyberte** odkazy v mřížce, ale jejich nic nedělají než zkontrolujte šedá zapnout pozadí řádku.

![Instruktoři stránky s výběrem](deploying-a-code-update/_static/image1.png)

Teď přidejte kód, který spouští, když **vyberte** dojde ke kliknutí na odkaz a zobrazí seznam kurzy vedené vybrané instruktorem.

1. V *Instructors.aspx*, přidejte následující kód bezprostředně po **ErrorMessageLabel** `Label` ovládacího prvku:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Spustit na stránku a vybrat instruktorem. Zobrazí seznam kurzy vedené této instruktorem.

    ![Stránka Instruktoři s výukové kurzy](deploying-a-code-update/_static/image2.png)
3. Zavřete prohlížeč.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Nasazení aktualizace kódu do testovacího prostředí

Než použijete profil publikování pro nasazení do testu, přípravném nebo produkčním prostředí budete muset změnit možnosti publikování databáze. Už nemusíte spouštět skripty nasazení udělení a dat pro databázi členství.

1. Otevřít **Publikovat Web** Průvodce pravým tlačítkem myši projekt ContosoUniversity a kliknutím na **publikovat**.
2. Klikněte na tlačítko **testovací** profil v **profilu** rozevíracího seznamu.
3. Klikněte na tlačítko **nastavení** kartu.
4. V části **objekt DefaultConnection** v **databází** části, zrušte **aktualizace databáze** zaškrtávací políčko.
5. Klikněte na tlačítko **profilu** kartu a potom klikněte na tlačítko **pracovní** profil v **profilu** rozevíracího seznamu.
6. Pokud se výzva, pokud chcete uložit změny provedené **testovací** profilu, klikněte na tlačítko **Ano**.
7. Stejnou změnu proveďte v pracovní profil.
8. Opakujte postup provést stejnou změnu v produkčním prostředí profilu.
9. Zavřít **publikování webu** průvodce.

Nasazení do testovacího prostředí je teď znovu publikovat jednoduché spuštění jedním kliknutím. Chcete-li tento proces urychlit, můžete použít **publikování webu jedním kliknutím** nástrojů.

1. V **zobrazení** nabídce zvolte **panely nástrojů** a pak vyberte **publikování webu jedním kliknutím**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. V **Průzkumníka řešení**, vyberte projekt ContosoUniversity.
3. **publikování webu jedním kliknutím** nástrojů, zvolte **testovací** profil publikování a pak klikněte na tlačítko **Publikovat Web** (na ikonu šipky směřující vlevo a vpravo).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio nasadí aktualizované aplikace a prohlížeč se automaticky otevře na domovskou stránku.
5. Spustit školitelů stránky a vyberte k ověření, že se aktualizace úspěšně nasadilo instruktorem.

Běžným způsobem provést také regresního testování (to znamená, že test zbytku lokality, abyste měli jistotu, že tato nová změna neměli přerušit všechny existující funkce). Ale pro účely tohoto kurzu budete tento krok přeskočit a přejít k nasazení aktualizací do pracovního a produkčního prostředí.

Při opětovném nasazování, Web Deploy automaticky určuje, které soubory se změnily a kopíruje jen změněné soubory na server. Ve výchozím nastavení nasazení webu používá data poslední změny na soubory k určení, které se nezměnila. Některé systémy správy zdrojového kódu změňte soubor data i při Neměňte obsah souboru. V takovém případě můžete chtít nakonfigurovat nasazení webu použití souboru kontrolní součty k určení, které soubory se změnily. Další informace najdete v tématu [Proč všechny soubory získat znovu nasadil Ačkoli jsem je nezměnili?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) v nejčastější dotazy k nasazení technologie ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Nastavit aplikaci offline během nasazení

Změna, kterou nasazujete nyní je jednoduchou změnu na jednu stránku. Ale někdy nasadit větší změny, nebo nasadit změny kódu a databáze a lokality se může chovat chybně Pokud uživatel požádá o stránku předtím, než se nasazení dokončí. Chcete-li uživatelům zabránit v přístupu na web, zatímco probíhá nasazení, můžete použít *aplikace\_offline.htm* souboru. Zadaný soubor s názvem *aplikace\_offline.htm* v kořenové složce vaší aplikace, služba IIS automaticky zobrazí tento soubor namísto spuštění vaší aplikace. Tak, aby se zabránilo přístupu během nasazování, kam si ukládáte *aplikace\_offline.htm* v kořenové složce spustit proces nasazení a pak odeberte *aplikace\_offline.htm* po úspěšné nasazení.

Můžete nakonfigurovat nasazení webu automatické přepnutí výchozí *aplikace\_offline.htm* souboru na serveru při spuštění nasazení a jeho odebrání po dokončení. Provedete to, že všechny budete muset provést je, přidejte následující element XML do souboru profilu (.pubxml) publikování:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Pro účely tohoto kurzu uvidíte, jak vytvořit a používat vlastní *aplikace\_offline.htm* souboru.

Pomocí *aplikace\_offline.htm* na pracovním webu není povinné, protože nemáte uživatele, kteří používají na pracovním webu. Ale je vhodné testovat vše, co tak, jak máte v plánu nasazení v produkčním prostředí pomocí přípravy.

### <a name="create-appofflinehtm"></a>Vytvoření aplikace\_offline.htm

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení a klikněte na tlačítko **přidat**a potom klikněte na tlačítko **nová položka**.
2. Vytvoření **stránku HTML** s názvem *aplikace\_offline.htm* (odstranit poslední "l" v *.html* rozšíření, které sada Visual Studio vytvoří ve výchozím nastavení).
3. Nahraďte kód šablony následujícím kódem:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Soubor uložte a zavřete.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Kopírování aplikací\_offline.htm do kořenové složky webu

Můžete použít jakýkoli nástroj FTP pro kopírování souborů do webové stránky. [Filezilly](http://filezilla-project.org/) je oblíbený nástroj FTP a zobrazí se v snímky obrazovky.

Pokud chcete použít nástroj FTP, potřebujete tři věci: adresa URL protokolu FTP, uživatelské jméno a heslo.

Adresa URL se zobrazí na stránce řídicího panelu na webu portálu pro správu Azure a uživatelské jméno a heslo pro FTP najdete v *.publishsettings* soubor, který jste předtím stáhli. Následující kroky ukazují, jak získat tyto hodnoty.

1. Na portálu Azure Management Portal, klikněte na tlačítko **weby** kartu a potom klikněte na pracovním webu.
2. Na **řídicí panel** stránky, posuňte se dolů najít název hostitele FTP v **rychlý přehled** oddílu.

    ![Název hostitele FTP](deploying-a-code-update/_static/image5.png)
3. Otevřete v přípravném *.publishsettings* soubor v poznámkovém bloku nebo jiného textového editoru.
4. Najít `publishProfile` – element pro profil FTP.
5. Kopírovat `userName` a `userPWD` hodnoty.

    ![Přihlašovací údaje serveru FTP](deploying-a-code-update/_static/image6.png)
6. Otevřete nástroj FTP a přihlaste se k adrese URL protokolu FTP.
7. Kopírování *aplikace\_offline.htm* složce řešení a */site/wwwroot* složky na pracovním webu.

    ![Zkopírujte app_offline](deploying-a-code-update/_static/image7.png)
8. Přejděte na adresu URL přípravného lokalitě. Uvidíte, že *aplikace\_offline.htm* stránky se nyní zobrazí místo domovské stránky.

    ![App_offline.htm v okně prohlížeče](deploying-a-code-update/_static/image8.png)

Nyní jste připraveni k nasazení do přípravného prostředí.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Nasazení aktualizace kódu do pracovního a produkčního prostředí

1. V **publikování webu jedním kliknutím** nástrojů, zvolte **pracovní** profil publikování a pak klikněte na tlačítko **Publikovat Web**.

    Visual Studio nasadí aktualizované aplikace a otevře prohlížeč na domovské stránce webu. *Aplikace\_offline.htm* soubor se zobrazí. Předtím, než můžete otestovat a ověřit úspěšné nasazení, je nutné odebrat *aplikace\_offline.htm* souboru.
2. Vraťte se do vašeho nástroje pro FTP a odstranit **aplikace\_offline.htm** z přípravného lokalitě.
3. V prohlížeči otevřete stránku Instruktoři na pracovním webu a vyberte instruktorem k ověření, že se aktualizace úspěšně nasadilo.
4. Postupujte stejným způsobem pro produkční prostředí, jako jste to udělali přípravy.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Zkontrolujte změny a nasaďte konkrétní soubory

Visual Studio 2012 také poskytuje schopnost nasadit jednotlivé soubory. Pro vybraný soubor můžete zobrazit rozdíly mezi místní verzi a verzi nasazené, nasaďte soubor do cílového prostředí nebo zkopírujte soubor do místní projekt v cílovém prostředí. V této části kurzu naleznete v tématu Jak používat tyto funkce.

### <a name="make-a-change-to-deploy"></a>Proveďte změnu nasazení

1. Otevřít *Content/Site.css*a najít bloku `body` značky.
2. Změňte hodnotu `background-color` z `#fff` k `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Zobrazení změn v okně Náhled publikování

Při použití **Publikovat Web** Průvodce Publikujte projekt, naleznete v tématu co se změny chystáte publikovat dvojitým kliknutím na soubor v **ve verzi Preview** okna.

1. Klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.
2. Z **profilu** rozevíracího seznamu, vyberte **Test** profil publikování.
3. Klikněte na tlačítko **ve verzi Preview**a potom klikněte na tlačítko **spustit Náhled**.
4. V **ve verzi Preview** podokně dvakrát klikněte na panel **Site.css**.

    ![Dvakrát klikněte na panel Site.css](deploying-a-code-update/_static/image9.png)

    **Náhled změn** dialogové okno zobrazí náhled změn, které se nasadí.

    ![Změny ve verzi Preview Site.css](deploying-a-code-update/_static/image10.png)

    Pokud dvakrát kliknete *Web.config* soubor, **náhled změn** dialogové okno demonstruje účinek sestavení transformace konfigurace a publikování profilu transformace. V tomto okamžiku jste neudělali cokoli, co způsobilo *Web.config* soubor na serveru, chcete-li změnit, takže byste měli vidět žádné změny. Ale **náhled změn** okno nesprávně ukazuje dvě změny. Zdá se, odeberou se dva prvky XML. Tyto prvky jsou přidány pomocí procesu publikování vyberete **spustit migrace Code First při spuštění aplikace** Code First třídy kontextu. Předtím, než se proces publikování přidá tyto prvky, takže to vypadá, že budou odebrány, i když se neodeberou, provádí se porovnání. Tato chyba bude opraven v budoucí verzi.
5. Klikněte na tlačítko **Zavřít**.
6. Klikněte na tlačítko **publikovat**.
7. Když prohlížeč otevře domovskou stránku Test webu, stiskněte CTRL + F5 způsobit pevné aktualizace Chcete-li zobrazit účinky změn šablon stylů CSS.

    ![Účinky změn šablon stylů CSS](deploying-a-code-update/_static/image11.png)
8. Zavřete prohlížeč.

### <a name="publish-specific-files-from-solution-explorer"></a>Publikovat konkrétní soubory z Průzkumníka řešení

Předpokládejme, že nemáte jako modré pozadí a chcete se vrátit na původní barvu. V této části budete obnovit původní nastavení a publikujte přímo z konkrétního souboru **Průzkumníka řešení**.

1. Otevřít *Content/Site.css* a obnovení `background-color` nastavení `#fff`.
2. V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *Content/Site.css* souboru.

    V místní nabídce ukazuje že tři možnosti publikování.

    ![Publikování možnosti z Průzkumníka řešení](deploying-a-code-update/_static/image12.png)
3. Klikněte na tlačítko **ve verzi Preview se změní na Site.css**.

    Otevře se okno zobrazit rozdíly mezi místního souboru a verze se v cílovém prostředí.

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **Site.css** znovu a klikněte na tlačítko **publikovat Site.css**.

    **Aktivita publikování webu** okno zobrazuje, že soubor byl publikován.

    ![Okno aktivity publikovat web](deploying-a-code-update/_static/image14.png)
5. Otevřete prohlížeč `http://localhost/contosouniversity` adresy URL a poté stiskněte klávesu CTRL + F5 a způsobit, že pevně aktualizaci, aby se projevily šablony CSS změnit.

    ![Domovská stránka s normální šablony stylů CSS](deploying-a-code-update/_static/image15.png)
6. Zavřete prohlížeč.

## <a name="summary"></a>Souhrn

Nyní jste viděli několik způsobů, jak nasadit aktualizace aplikace, který nezahrnuje Změna databáze a už víte, jak zobrazit náhled změn můžete ověřit, že toho, co se aktualizovalo čekáte. Stránka Instruktoři má nyní **výukové kurzy** oddílu.

![Stránka Instruktoři s výukové kurzy](deploying-a-code-update/_static/image16.png)

V dalším kurzu se dozvíte, jak nasadit databázi změnu: do databáze a na stránce Instruktoři přidáte pole Datum narození.

> [!div class="step-by-step"]
> [Předchozí](deploying-to-production.md)
> [další](deploying-a-database-update.md)
