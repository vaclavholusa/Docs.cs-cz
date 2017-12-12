---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: "Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu | Microsoft Docs"
author: tdykstra
description: "Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 10da2b5013ae1348b69ea4f456d81bb4c4b73df6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace kódu
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010. Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).


## <a name="overview"></a>Přehled

Po počátečním nasazení pokračuje v práci údržbu a vývoj vašeho webu a před dlouho bude chtít nasadit aktualizace. Tento kurz vás provede procesem nasazení aktualizace do kódu aplikace. Aktualizace, která můžete implementovat a nasazení v tomto kurzu nezahrnuje změnu databáze; Zobrazí se, co se liší o nasazení změn databáze v dalším kurzu.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](troubleshooting.md).

## <a name="make-a-code-change"></a>Ujistěte se, změňte kód

Jako jednoduchý příklad aktualizace do vaší aplikace, přidáte do **vyučující** stránky Seznam kurzů výukové podle vybrané lektorem.

Pokud spustíte **vyučující** stránky, můžete si všimnout, že existují **vyberte** odkazy v mřížce, ale přitom nedělají nic jakoukoli jinou hodnotu než zkontrolujte šedá řádek pozadí zapnout.

![Vyučující stránky s výběrem](deploying-a-code-update/_static/image1.png)

Nyní přidáte kód, který běží, když **vyberte** po kliknutí na odkaz a zobrazí seznam kurzů výukové podle vybrané lektorem.

1. V *Instructors.aspx*, přidejte následující kód bezprostředně po **ErrorMessageLabel** `Label` ovládacího prvku:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Spuštění stránky a vyberte lektorem. Zobrazí seznam kurzů výukové podle této lektorem.

    ![Stránka vyučující s kurzy výukové](deploying-a-code-update/_static/image2.png)
3. Zavřete prohlížeč.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Kód aktualizace nasadit do testovacího prostředí

Předtím, než vaše profilů publikování můžete použít k nasazení na testovací, přípravné nebo produkční prostředí, budete muset změnit možnosti publikování databáze. Už nemusí spouštět skripty nasazení grant a dat pro databázi členství.

1. Otevřete **Publikovat Web** Průvodce pravým tlačítkem na projekt ContosoUniversity a kliknutím na **publikovat**.
2. Klikněte na tlačítko **Test** profilu v **profil** rozevíracího seznamu.
3. Klikněte **nastavení** kartě.
4. V části **objekt DefaultConnection** v **databáze** část, zrušte **aktualizace databáze** zaškrtávací políčko.
5. Klikněte na tlačítko **profil** a pak klikněte **pracovní** profilu v **profil** rozevíracího seznamu.
6. Když se zobrazí výzva Pokud chcete uložit změny provedené **Test** profilu, klikněte na tlačítko **Ano**.
7. Proveďte požadovanou změnu stejné v pracovní profil.
8. Opakujte tento proces zpřístupnění stejné změny v produkčním profilu.
9. Zavřít **Publikovat Web** průvodce.

Nasazení do testovacího prostředí se nyní znovu publikovat jednoduché spuštění jedním kliknutím. Chcete-li tento proces rychlejší, můžete použít **publikování webu jedním kliknutím** panelu nástrojů.

1. V **zobrazení** nabídce zvolte **panely nástrojů** a pak vyberte **publikování webu jedním kliknutím**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. V **Průzkumníku**, vyberte ContosoUniversity projekt.
3. **publikování webu jedním kliknutím** nástrojů, vyberte **Test** profil publikování a pak klikněte na **Publikovat Web** (ikona s dvojice šipek, které ukazují doleva a doprava).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio nasadí aktualizovanou aplikaci a prohlížeč se automaticky otevře na domovskou stránku.
5. Spuštění stránky vyučující a vyberte lektorem k ověření, že aktualizace byla úspěšně nasazena.

Za normálních okolností byste také provést testování regrese (tedy testovací zbytek lokality a ujistěte se, že nové změny nebyla rozdělit žádné existující funkce). Ale pro účely tohoto kurzu budete tento krok přeskočit a přejít k aktualizace nasadit do pracovní a provozní.

Při opětovném, Web Deploy automaticky určuje, které soubory se změnili a změnit pouze kopie souborů na server. Ve výchozím nastavení nasazení webu pomocí poslední změny dat na soubory určuje ty, které se změnily. Některé systémy správy zdrojového souboru data změnit i když neměnit obsah souboru. V takovém případě můžete chtít nakonfigurovat nasazení webu použít kontrolní součty souborů k určení, které soubory se změnily. Další informace najdete v tématu [Proč všechny soubory získat znovu nasazena i když nebyla je změnit?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#use_checksum) v části Nejčastější dotazy pro nasazení technologie ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Trvat aplikace do režimu offline během nasazení

Změna, kterou nasazujete nyní je jednoduchá změna na jednu stránku. Ale někdy nasadit větší změny, nebo nasazení změn kódu a databáze a lokality mohou chovat správně, pokud uživatel požádá o na stránce před dokončení nasazení. Chcete-li zabránit uživatelům v přístupu na web, když probíhá nasazení, můžete použít *aplikace\_offline.htm* souboru. Když vložíte soubor s názvem *aplikace\_offline.htm* kořenové složky vaší aplikace, služba IIS automaticky zobrazí tento soubor namísto spuštění vaší aplikace. Tak, aby se zabránilo přístup při nasazení, můžete zadat *aplikace\_offline.htm* v kořenové složce, spustí proces nasazení a potom odeberte *aplikace\_offline.htm* po úspěšné nasazení.

Můžete nakonfigurovat nasazení webu se automaticky umístí výchozí *aplikace\_offline.htm* souboru na serveru, když se spustí, nasazení a jeho odebrání po dokončení. Chcete, aby všechny je nutné provést je publikování souboru profilu (.pubxml) přidejte následující element XML:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

V tomto kurzu uvidíte, jak vytvořit a používat vlastní *aplikace\_offline.htm* souboru.

Pomocí *aplikace\_offline.htm* v pracovní lokalitě není povinné, protože nemáte uživatele, kteří používají pracovní lokality. Ale je dobrým zvykem pomocí pracovní proveďte kompletní testování způsobem, ke kterému plánujete nasadit v provozním prostředí.

### <a name="create-appofflinehtm"></a>Vytvoření aplikace\_offline.htm

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení a klikněte na **přidat**a potom klikněte na **novou položku**.
2. Vytvoření **stránku HTML** s názvem *aplikace\_offline.htm* (odstranit konečné "l" v *.html* rozšíření, která sada Visual Studio vytvoří ve výchozím nastavení).
3. Nahraďte kód šablony následující kód:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Soubor uložte a zavřete.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Kopie aplikace\_offline.htm ke kořenové složce webové stránky

Jakýkoli nástroj FTP můžete kopírovat soubory na web. [FileZilla](http://filezilla-project.org/) je nástroj oblíbených FTP a zobrazí se v snímky obrazovky.

Pomocí nástroje FTP, budete potřebovat tři věci: adresy URL FTP, uživatelské jméno a heslo.

Adresa URL se zobrazí na stránce řídicího panelu webu v Azure Management Portal a uživatelské jméno a heslo pro FTP najdete v *.publishsettings* soubor, který jste předtím stáhli. Následující kroky ukazují, jak získat tyto hodnoty.

1. V portálu pro správu Azure, klikněte na **weby** a pak klikněte na pracovní webu.
2. Na **řídicí panel** stránky, posuňte se dolů najít název hostitele FTP v **rychlý přehled** části.

    ![Název hostitele FTP](deploying-a-code-update/_static/image5.png)
3. Otevřete pracovní *.publishsettings* souboru v programu Poznámkový blok nebo jiném textovém editoru.
4. Najít `publishProfile` element profilu FTP.
5. Kopírování `userName` a `userPWD` hodnoty.

    ![Přihlašovací údaje serveru FTP](deploying-a-code-update/_static/image6.png)
6. Otevřete nástroj FTP a přihlaste se k adresy URL FTP.
7. Kopírování *aplikace\_offline.htm* ze složky řešení */web/wwwroot* složky na pracovní webu.

    ![Zkopírujte app_offline](deploying-a-code-update/_static/image7.png)
8. Přejděte na adresu URL pracovní lokality. Uvidíte, že *aplikace\_offline.htm* stránky se nyní zobrazí namísto domovské stránky.

    ![App_offline.htm v okně prohlížeče](deploying-a-code-update/_static/image8.png)

Nyní jste připraveni k nasazení do přípravy.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Kód aktualizace nasadit do přípravné nebo produkční prostředí

1. V **publikování webu jedním kliknutím** nástrojů, vyberte **pracovní** profil publikování a pak klikněte na **Publikovat Web**.

    Visual Studio nasadí aktualizovanou aplikaci a otevře prohlížeč na domovskou stránku. *Aplikace\_offline.htm* souboru se zobrazí. Než můžete otestovat a ověřit úspěšné nasazení, je třeba odstranit *aplikace\_offline.htm* souboru.
2. Vraťte se na vaše nástroje FTP a odstranit **aplikace\_offline.htm** z pracovní lokality.
3. V prohlížeči otevřít stránku vyučující v pracovní lokality a vyberte lektorem k ověření, že aktualizace byla úspěšně nasazena.
4. Postupujte stejným způsobem pro produkční prostředí, jako jste to udělali pracovní.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Kontrola změn a nasazení konkrétní soubory

Visual Studio 2012 také poskytuje schopnost nasadit jednotlivé soubory. Pro vybraný soubor můžete zobrazit rozdíly mezi místní verze a nasazené verzi, nasaďte soubor na cílovém prostředí nebo zkopírujte soubor do místní projekt z cílového prostředí. V této části kurzu najdete v části používání těchto funkcí.

### <a name="make-a-change-to-deploy"></a>Změňte k nasazení

1. Otevřete *Content/Site.css*a najít bloku `body` značky.
2. Změňte hodnotu `background-color` z `#fff` k `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Zobrazení změn v okně Náhled publikování

Při použití **Publikovat Web** Průvodce publikování projektu, uvidíte, jaké změny budou publikována dvojitým kliknutím na soubor v **Preview** okno.

1. Klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.
2. Z **profil** rozevíracího seznamu, vyberte **testovací** profilu publikování.
3. Klikněte na tlačítko **Preview**a potom klikněte na **spustit Náhled**.
4. V **Preview** podokně dvakrát klikněte na **Site.css**.

    ![Klikněte dvakrát na Site.css](deploying-a-code-update/_static/image9.png)

    **Zobrazení náhledu změn** dialogové okno se zobrazí ukázka změny, které se nasadí.

    ![Zobrazení náhledu změn na Site.css](deploying-a-code-update/_static/image10.png)

    Pokud dvakrát kliknete *Web.config* souboru **zobrazení náhledu změn** dialogové okno zobrazuje účinku buildu transformace konfigurace a publikování profil transformace. V tomto okamžiku jste to neudělali všechno, co by *Web.config* soubor na serveru, pokud chcete změnit, takže byste měli vidět žádné změny. Ale **zobrazení náhledu změn** v okně se nesprávně zobrazí dvě změny. Zdá se, se odeberou dva elementy XML. Tyto prvky jsou přidávány proces publikování, když vyberete **spustit migrace Code First při spuštění aplikace** pro třídu kontextu Code First. Porovnání se provádí před proces publikování přidá tyto prvky, takže to vypadá, budou se odstraněny i když se neodeberou. Tato chyba bude opraven v budoucí verzi.
5. Klikněte na tlačítko **Zavřít**.
6. Klikněte na tlačítko **publikování**.
7. Když se otevře prohlížeč na domovské stránce webu testovací, stiskněte CTRL + F5 a způsobit pevný aktualizaci, aby se projevily změny šablon stylů CSS.

    ![Vliv změny šablon stylů CSS](deploying-a-code-update/_static/image11.png)
8. Zavřete prohlížeč.

### <a name="publish-specific-files-from-solution-explorer"></a>Publikování konkrétní soubory v Průzkumníku řešení

Předpokládejme, že nemáte jako modré pozadí a chcete obnovit původní barvy. V této části budete obnovit původní nastavení tím, že publikujete konkrétní soubor přímo z **Průzkumníku řešení**.

1. Otevřete *Content/Site.css* a obnovení `background-color` nastavení `#fff`.
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Content/Site.css* souboru.

    V místní nabídce zobrazuje že tři možnosti publikovat.

    ![Publikování možnosti v Průzkumníku řešení](deploying-a-code-update/_static/image12.png)
3. Klikněte na tlačítko **Preview změny Site.css**.

    Otevře se okno zobrazíte rozdíly mezi místního souboru a verze se v cílovém prostředí.

    ![Diff – obsah nebo Site.css](deploying-a-code-update/_static/image13.png)
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Site.css** znovu a klikněte na tlačítko **publikování Site.css**.

    **Aktivity publikování webového** ukazuje, že soubor byl publikován.

    ![Okno aktivity publikovat web](deploying-a-code-update/_static/image14.png)
5. Otevřete prohlížeč na `http://localhost/contosouniversity` adresu URL a potom stiskněte klávesu CTRL + F5 a způsobit, že pevně aktualizovat Chcete-li zobrazit účinku CSS změnit.

    ![Domovská stránka s normální šablon stylů CSS](deploying-a-code-update/_static/image15.png)
6. Zavřete prohlížeč.

## <a name="summary"></a>Souhrn

Nyní jste viděli několik způsobů, jak nasadit aktualizace aplikace, která nezahrnuje změnu databáze a už víte, jak zobrazení náhledu změn k ověření toho, co se aktualizovalo očekávat. Stránka vyučující má nyní **výukové kurzy** části.

![Stránka vyučující s kurzy výukové](deploying-a-code-update/_static/image16.png)

V dalším kurzu se dozvíte, jak nasadit změnu databáze: přidáte datum narození pole do databáze a na stránku vyučující.

>[!div class="step-by-step"]
[Předchozí](deploying-to-production.md)
[další](deploying-a-database-update.md)
