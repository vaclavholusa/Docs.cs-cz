---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: "Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení do produkčního prostředí | Microsoft Docs"
author: tdykstra
description: "Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: 2c49e7f6925b1ca172642747c5052ba97d70d036
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení do produkčního prostředí
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010. Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu nastavit účet Microsoft Azure, vytvořit pracovní a provozní prostředí a nasadit vaši webovou aplikaci ASP.NET do pracovní a provozní prostředí pomocí sady Visual Studio jedním kliknutím publikování funkce.

Pokud dáváte přednost, můžete nasadit do hostujícího zprostředkovatele třetí strany. Většinu postupů popsaných v tomto kurzu jsou stejné pro poskytovatele hostingu nebo pro Azure, s tím rozdílem, že každý poskytovatel má svou vlastní uživatelské rozhraní pro správu účtu a webu. Můžete najít poskytovatele hostingu v [Galerie zprostředkovatelů](https://www.microsoft.com/web/hosting) na webu Microsoft.com.

Upozornění: Pokud zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat na stránku Poradce při potížích s Tento kurz řady.

## <a name="get-a-microsoft-azure-account"></a>Získat účet Microsoft Azure

Pokud účet Azure nemáte, můžete si během několika minut vytvořit Bezplatný zkušební účet. Podrobnosti najdete v tématu [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Vytvořit pracovní prostředí

> [!NOTE]
> Vzhledem k tomu, že byla zapsána v tomto kurzu, Azure App Service přidat novou funkci k automatizaci mnoha procesy pro vytvoření pracovní a provozní prostředí. V tématu [nastavení přípravných prostředí pro webové aplikace v Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Jak je popsáno v [nasadit do tohoto kurzu testovací prostředí](deploying-to-iis.md), nejvíce spolehlivé testovací prostředí je webová stránka u poskytovatele hostingu, který má stejně jako webové pracoviště. V mnoha poskytovatelům hostingu museli byste zvážit výhody tohoto proti významné další náklady, ale v Azure můžete vytvořit další volné webové aplikace jako pracovní aplikace. Musíte taky databázi a další náklady pro tento přes nákladů na vaši produkční databázi bude mít buď none nebo minimální. V Azure platíte pro velikost úložiště databáze, které používáte, nikoli pro každou databázi, a množství dalšího úložiště, který budete používat při přípravě bude minimální.

Jak je popsáno v [nasadit do testovacího prostředí kurzu](deploying-to-iis.md)v přípravné nebo produkční prostředí, které se chystáte nasadit dvě databáze do jedné databáze. Pokud jste chtěli je oddělit, proces bude stejné s tím rozdílem, že byste vytvořit další databázi pro každé prostředí a byste vybrali správnou cílovou řetězec pro každou databázi, když vytvoříte profil publikování.

V této části kurzu vytvoříte webovou aplikaci a databázi pro použití při pracovní prostředí, a budete nasadit do přípravy a testování existuje před vytvoření a nasazení do produkčního prostředí.

> [!NOTE]
> Následující kroky ukazují, jak vytvořit webovou aplikaci v Azure App Service pomocí portálu správy Azure. V nejnovější verzi sady Azure SDK to můžete provést taky aniž byste museli opustit pomocí Průzkumníka serveru Visual Studio. V sadě Visual Studio 2013 můžete také vytvořit webovou aplikaci přímo z dialogového okna Publikovat. Další informace najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. V [portálu pro správu Azure](https://manage.windowsazure.com/), klikněte na tlačítko **weby**a potom klikněte na **nový**.
2. Klikněte na tlačítko **webu**a potom klikněte na **vytvořit vlastní**.

    **Webu nový - vytvořit vlastní** otevře se průvodce. **Vytvořit vlastní** průvodce můžete vytvořit webovou stránku a databáze ve stejnou dobu.
3. V **vytvořit web** krok v průvodci zadejte řetězec ve **URL** pole, které chcete použít jako jedinečnou adresu URL pro vaši aplikaci je pracovní prostředí. Zadejte například ContosoUniversity-staging123 (včetně náhodných čísel na konci zajistit její jedinečnost v případě, že nebyla provedena ContosoUniversity pracovní).

    Úplnou adresu URL bude obsahovat co zadáte a příponu, která se zobrazí vedle textového pole.
4. V **oblast** rozevíracího seznamu vyberte oblast, která je k vám nejblíže.

    Toto nastavení určuje, které datového centra vaší webové aplikace se spustí v.
5. V **databáze** rozevíracího seznamu vyberte **vytvořit novou databázi SQL**.
6. V **název připojovacího řetězce DB** pole, ponechte výchozí hodnotu *objekt DefaultConnection*.
7. Klikněte na šipku, která odkazuje na vpravo v dolní části pole.

    Následující obrázek ukazuje **vytvořit web** dialogové okno s ukázkové hodnoty v ní. Adresa URL a oblasti, které jste zadali, se liší.

    ![Vytvoření webu krok](deploying-to-production/_static/image1.png)

    Průvodce přejde **zadejte nastavení databáze** krok.
8. V **název** zadejte *ContosoUniversity* plus náhodné číslo, aby byla zajištěna jedinečnost, například *ContosoUniversity123*.
9. V **Server** vyberte **nový Server databáze SQL**.
10. Zadejte jméno správce a heslo.

    Nezadáváte existující jméno a heslo. Zadáváte nové jméno a heslo, které definujete teď pro pozdější použití při přístupu k databázi.
11. V **oblast** vyberte stejné oblasti, kterou jste zvolili pro webovou aplikaci.

    Zachování webový server a databázový server ve stejné oblasti získáte nejlepší výkon a minimalizuje výdaje.
12. Kliknutím na značku zaškrtnutí v dolní části políčka potvrďte, že budete hotovi.

    Následující obrázek ukazuje **zadejte nastavení databáze** dialogové okno s ukázkové hodnoty v ní. Hodnoty, které jste zadali, se může lišit.

    ![Krok nastavení databáze webu nový - vytvořit pomocí Průvodce databáze](deploying-to-production/_static/image2.png)

    Vrátí portálu pro správu na stránku weby a **stav** sloupci se zobrazuje, že probíhá vytváření webové aplikace. Po chvíli (obvykle během méně než minuty) **stav** sloupci se zobrazuje, že webová aplikace byla úspěšně vytvořena. V navigačním panelu na levé straně, počet webových aplikací, které máte ve vašem účtu se zobrazí vedle **weby** ikonu a počet databází, se zobrazí vedle **databází SQL** ikonu.

    ![Stránka webové stránky portálu pro správu, vytvořit webovou stránku](deploying-to-production/_static/image3.png)

    Název vaší webové aplikace se budou lišit od aplikace příklad na obrázku.

## <a name="deploy-the-application-to-staging"></a>Nasaďte aplikaci do pracovní

Teď, když jste vytvořili webovou aplikaci a databázi pro pracovní prostředí, můžete nasadit projektu do ní.

> [!NOTE]
> Tyto pokyny ukazují, jak vytvořit profil publikování stažením *.publishsettings* souboru, který funguje není pouze pro Azure, ale i pro poskytovatele hostingu třetích stran. Nejnovější sadu Azure SDK můžete také připojit přímo k Azure ze sady Visual Studio a zvolte ze seznamu webových aplikací, které máte v účtu Azure. Ve Visual Studiu 2013 se můžete přihlásit k Azure z **publikování webu** dialogové okno nebo z **Průzkumníka serveru** okno. Další informace najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Stáhněte si soubor .publishsettings

1. Klikněte na název webové aplikace, kterou jste právě vytvořili.

    ![Klikněte na lokalitu, přejdete na řídicí panel](deploying-to-production/_static/image4.png)
2. V části **rychlého přehledu** v **řídicí panel** , klikněte na **stažení profilu publikování**.

    ![Stáhněte profil publikování odkazu](deploying-to-production/_static/image5.png)

    Tento krok stáhne soubor, který obsahuje všechna nastavení, které potřebujete k nasazení aplikace do webové aplikace. Tento soubor je budete importovat do sady Visual Studio, nemusíte tyto informace zadat ručně.
3. Uložit *.publishsettings* souboru ve složce, která se zobrazí ze sady Visual Studio.

    ![ukládání souboru .publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Zabezpečení – *.publishsettings* soubor obsahuje vaše pověření (nekódovaných), které se používají ke správě vašich předplatných Azure a služby. Nejlepším způsobem zabezpečení pro tento soubor je dočasně uložit mimo adresáře zdroje (například ve složce Libraries\Documents) a poté jej odstraňte po dokončení importu. Uživatel se zlými úmysly, který získá přístup k *.publishsettings* soubor můžete upravit, vytvářet a odstraňovat služeb Azure.

### <a name="create-a-publish-profile"></a>Vytvoření profilu publikování

1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt ContosoUniversity **Průzkumníku řešení** a vyberte **publikovat** v místní nabídce.

    **Publikovat Web** otevře se průvodce.
2. Klikněte **profil** kartě.
3. Klikněte na tlačítko **Import**.
4. Přejděte na *.publishsettings* souboru jste předtím stáhli a potom klikněte na **otevřete**.

    ![Dialogové okno importu nastavení publikování](deploying-to-production/_static/image7.png)
5. V **připojení** , klikněte na **ověřit připojení** a ujistěte se, zda jsou správné nastavení.

    Při připojení byl ověřen, je vedle zobrazí zelená značka zaškrtnutí **ověřit připojení** tlačítko.

    Pro některé zprostředkovatelů hostování, po kliknutí na tlačítko **ověřit připojení**, můžete se setkat **Chyba certifikátu** dialogové okno. Pokud tak učiníte, ověřte, zda název serveru očekávat. Pokud je název serveru správný, vyberte **uložit tento certifikát pro budoucí relace sady Visual Studio** a klikněte na tlačítko **přijmout**. (Tato chyba znamená, že poskytovatel hostingu rozhodl vyhnout nákladů na zakoupení certifikátu protokolu SSL pro adresu URL, kterou nasazujete. Pokud dáváte přednost navázat zabezpečené připojení pomocí platného certifikátu, obraťte se na svého poskytovatele hostingu.)
6. Klikněte na tlačítko **Další**.

    ![Ikona úspěšné připojení a tlačítko Další na kartě připojení Průvodce](deploying-to-production/_static/image8.png)
7. V **nastavení** rozbalte **možností publikování souboru**a potom vyberte **vyloučit soubory z aplikace\_složky dat**.

    Informace o dalších možnostech pod **možností publikování souboru**, najdete v článku [nasazení pro službu IIS](deploying-to-iis.md) kurzu. Snímek obrazovky této zobrazuje výsledek tento krok a následující kroky konfigurace databáze je na konci kroky konfigurace databáze.
8. V části **objekt DefaultConnection** v **databáze** nakonfigurujte nasazení databáze pro databázi členství.
9. 1. Vyberte **aktualizace databáze**.

        **Vzdáleného připojovací řetězec** pole přímo pod **objekt DefaultConnection** obsahuje připojovací řetězec ze souboru .publishsettings. Připojovací řetězec obsahuje pověření systému SQL Server, které jsou uloženy v prostém textu v *.pubxml* souboru. Pokud nechcete uložit je trvale existuje, můžete je odebrat z profilu publikování po nasazení databáze a uložit je do Azure. Další informace najdete v tématu [jak zabezpečit vaše databáze ASP.NET připojovací řetězce při nasazení do Azure ze zdroje](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) na Scotta Hanselmana.
    2. Klikněte na tlačítko **konfigurovat aktualizace databáze**.
    3. V **konfigurovat aktualizace databáze** dialogové okno, klikněte na tlačítko **přidat skript SQL**.
    4. V **přidat skript SQL** pole, přejděte *aspnet. data prod.sql* skript, který jste předtím uložili ve složce řešení a potom klikněte na **otevřete**.
    5. Zavřít **konfigurovat aktualizace databáze** dialogové okno.
10. V části **SchoolContext** v **databáze** vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**.

    Visual Studio zobrazí **spustit migrace Code First** místo **aktualizace databáze** pro `DbContext` třídy. Pokud chcete použít poskytovatele dbDacFx místo migrace nasazení databáze, které máte přístup k pomocí `DbContext` třídy najdete v tématu [jak nasadit Code First databáze bez migrace?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#deploy_code_first_without_migrations) v nejčastějších Dotazech webové nasazení pro Visual Studio a ASP.NET na webu MSDN.

    **Nastavení** kartě teď vypadá jako v následujícím příkladu:

    ![Karta nastavení pro pracovní](deploying-to-production/_static/image9.png)
11. Proveďte následující kroky pro uložení profilu a přejmenujte ji na *pracovní*:

    1. Klikněte **profil** a pak klikněte **spravovat profily**.
    2. Import vytvoří dvě nové profily, jeden pro FTP a jeden pro nasazení webu. Nakonfigurovaný profil nasazení webu: přejmenování tohoto profilu můžete *pracovní*.

        ![Přejmenujte pracovní profil](deploying-to-production/_static/image10.png)
    3. Zavřít **Upravit profily publikování webové** dialogové okno.
    4. Zavřít **Publikovat Web** průvodce.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Konfigurace transformace profil publikování pro prostředí indikátoru

> [!NOTE]
> V této části ukazuje, jak nastavit transformaci Web.config pro prostředí ukazatel. Protože je ve `<appSettings>` element, můžete mít Další alternativou k určení transformace, pokud nasazujete do služby Azure App Service. Další informace najdete v tématu [Web.config zadání nastavení v Azure](web-config-transformations.md#watransforms).


1. V **Průzkumníku řešení**, rozbalte položku **vlastnosti**a potom rozbalte **PublishProfiles**.
2. Klikněte pravým tlačítkem na *Staging.pubxml*a potom klikněte na **přidat transformace konfigurace**.

    ![Přidejte konfigurační transformaci pro pracovní](deploying-to-production/_static/image11.png)

    Visual Studio vytvoří *Web.Staging.config* transformační soubor a otevře ji.
3. V *Web.Staging.config* transformační soubor, vložte následující kód bezprostředně po otevření `configuration` značky.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Pokud použijete pracovní profil publikování, nastaví Tato transformace na indikátor prostředí a "Produkčnímu". V nasazené webové aplikace neuvidíte žádné přípony, jako je například "(vývoj)" nebo "(testovací)" po nadpis H1 "Univerzity Contoso".
4. Klikněte pravým tlačítkem myši *Web.Staging.config* souboru a klikněte na tlačítko **Preview transformace** a ujistěte se, že transformace můžete programového vytváří očekávané změny.

    **Web.config Preview** v okně se zobrazí výsledek použití i *Web.Release.config* transformuje a *Web.Staging.config* transformace.

### <a name="prevent-public-use-of-the-test-app"></a>Zabránit veřejné použití testování aplikace

Důležitá poznámka pro pracovní aplikace je, že bude za provozu v síti Internet, ale nechcete, aby veřejné pro použití. Chcete-li minimalizovat pravděpodobnost, že uživatelé budou najít a použít ho, můžete použít jeden nebo více z následujících metod:

- Nastavit pravidla brány firewall, které umožňují přístup do pracovní aplikace jenom z IP adresy, které můžete použít k testování pracovní.
- Použijte zkomolené adresu URL, které by bylo možné snadno uhodnout.
- Vytvoření *robots.txt* souboru zajistit, že nebude být z vyhledávacích webů Procházet k němu testovací aplikace a sestava odkazy ve výsledcích hledání.

První z těchto metod je co nejúčinnější, ale není zahrnutý v tomto kurzu, protože by vyžadovaly, která můžete nasadit na cloudové služby Azure místo Azure App Service. Další informace o službách Cloud Services a omezení IP adres v Azure najdete v tématu [možnosti hostování výpočtů poskytované platformou Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) a [bloku IP adres v přístupu k webové Role](https://msdn.microsoft.com/en-us/library/windowsazure/jj154098.aspx). Pokud provádíte nasazení do hostujícího zprostředkovatele třetí strany, obraťte se na poskytovatele a zjistěte, jak implementovat omezení IP adres.

V tomto kurzu vytvoříte *robots.txt* souboru.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na **přidat novou položku**.
2. Vytvořte novou **textový soubor** s názvem *robots.txt*a uveďte následující text:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent` Řádek říká vyhledávací weby, které pravidla v souboru platí pro všechny vyhledávací modul webové prohledávací moduly (robotů), a `Disallow` řádku určuje, že žádné stránky na webu Procházet.

    Chcete vyhledávací weby snadněji katalogu produkční aplikace, takže je třeba vyloučit tento soubor z produkčního nasazení. Uděláte to, že nakonfigurujete nastavení v produkční profilu publikování při jeho vytvoření.

### <a name="deploy-to-staging"></a>Nasazení do přípravy

1. Otevřete **Publikovat Web** Průvodce pravým tlačítkem na projekt univerzity Contoso a kliknutím na **publikovat**.
2. Ujistěte se, že **pracovní** vybraný profil.
3. Klikněte na tlačítko **publikování**.

    **Výstup** okno zobrazuje, jaké akce nasazení byly provedeny a hlásí úspěšné dokončení nasazení. Na adresu URL nasazené webové aplikace se automaticky otevře výchozí prohlížeč.

## <a name="test-in-the-staging-environment"></a>Testování v testovacím prostředí

Všimněte si, že indikátor prostředí je chybí (neexistuje žádné "(testovací)" nebo "(vývoj)" po nadpis H1, které ukazuje, že *Web.config* transformaci pro prostředí indikátoru byla úspěšná.

![Pracovní domovské stránky](deploying-to-production/_static/image12.png)

Spustit **studenty** a ověřte, že nasazené databáze nemá žádné studenty.

Spustit **vyučující** a ověřte, že Code First nasadí databáze s lektorem dat:

Vyberte **přidat studenty** z **studenty** nabídce Přidat student a zobrazte nový student v **studenty** a ověřte, že můžete úspěšně zapisovat do databáze .

Z **kurzy** klikněte na tlačítko **aktualizace kredity**. **Aktualizace kredity** stránka vyžaduje oprávnění správce, proto **protokolu v** zobrazí se stránka. Zadejte přihlašovací údaje účtu správce, který jste vytvořili dříve ("admin" a "prodpwd"). **Aktualizace kredity** se zobrazí stránka, která ověřuje, že účet správce, který jste vytvořili v předchozí kurzu byla správně nasazena do testovacího prostředí.

Neplatná adresa URL způsobí chybu, že ELMAH sledovat a potom si vyžádají zprávy o chybách ELMAH žádost. Pokud provádíte nasazení do hostujícího zprostředkovatele třetí strany, pravděpodobně zjistíte, že sestava je prázdná ze stejného důvodu, který byl v předchozím kurzu prázdné. Budete muset použít nástroje pro správu účtu poskytovatele hostingu ke konfiguraci oprávnění složky povolit ELMAH k zápisu do složky protokolů.

Aplikace, kterou jste vytvořili je nyní spuštěna v cloudu ve webové aplikaci, která je stejně jako bude používat pro produkční prostředí. Vzhledem k tomu, že všechno funguje správně, je dalším krokem je nasadit do produkčního prostředí.

## <a name="deploy-to-production"></a>Nasazení do produkčního prostředí

Proces vytvoření produkční webové aplikace a nasazení do produkčního prostředí je stejné jako pracovní, s tím rozdílem, že je potřeba vyloučit *robots.txt* z nasazení. K tomu budete upravovat soubor profilu publikování.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Vytvoření produkčního prostředí a provozní profilu publikování

1. Vytvoření produkční webové aplikace a databáze v Azure, stejný postup, který jste použili pro přípravu.

    Když vytvoříte databázi, můžete uvedete na stejném serveru, který jste vytvořili dříve, nebo vytvořit nový server.
2. Stažení *.publishsettings* souboru.
3. Vytvořit profil publikování importováním provozní *.publishsettings* soubor, stejným způsobem, který jste použili pro přípravu.

    Nezapomeňte nakonfigurovat skript nasazení dat v rámci **objekt DefaultConnection** v **databáze** části **nastavení** kartě.
4. Přejmenování profilu publikování k *produkční*.
5. Konfigurace transformace profil publikování pro prostředí indikátoru, stejný postup, který jste použili pro pracovní...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Upravte soubor .pubxml vyloučit robots.txt

Soubory jsou pojmenované profilu publikování &lt;profilename&gt;*.pubxml* a jsou umístěny v *PublishProfiles* složky. *PublishProfiles* složky *vlastnosti* složky ve webové aplikaci C# projektu v části *Můj projekt* složky v projektu jazyka Visual Basic webové aplikace, nebo pod *Aplikace\_Data* složky v projektu webové aplikace. Každý *.pubxml* soubor obsahuje nastavení, které se vztahují na jeden profil publikování. Hodnoty, které zadáte v Průvodci publikování webu jsou uložené v těchto souborech a upravovat vytvořit nebo změnit nastavení, které nejsou k dispozici ve Visual Studiu.

Ve výchozím nastavení *.pubxml* soubory jsou zahrnuty v projektu, když vytvoříte profil publikování, ale můžete je vyloučit z projektu a je stále používají Visual Studio. Visual Studio hledá v *PublishProfiles* složku pro *.pubxml* soubory, bez ohledu na to, jestli jsou zahrnuté v projektu.

Pro každou *.pubxml* soubor existuje *. pubxml.user* souboru. *. Pubxml.user* soubor obsahuje zašifrované heslo, pokud jste vybrali **uložit heslo** možnost a ve výchozím nastavení je vyloučena z projektu.

A *.pubxml* soubor obsahuje nastavení, které se týkají profil konkrétní publikování. Pokud chcete nakonfigurovat nastavení, které se vztahují na všechny profily, můžete vytvořit *. wpp.targets* souboru. Proces sestavení importuje tyto soubory do *.csproj* nebo *.vbproj* soubor projektu, takže většina nastavení, které můžete nakonfigurovat v souboru projektu se dá nakonfigurovat v těchto souborů. Další informace o *.pubxml* soubory a *. wpp.targets* soubory, najdete v části [postup: Upravit nastavení nasazení v souborech profil publikování (.pubxml) a. wpp.targets souborů v sadě Visual Studio Webové projekty](https://msdn.microsoft.com/en-us/library/ff398069.aspx).

1. V **Průzkumníku řešení**, rozbalte položku **vlastnosti** a rozbalte **PublishProfiles**.
2. Klikněte pravým tlačítkem na *Production.pubxml* a klikněte na tlačítko **otevřete**.

    ![Otevřete soubor .pubxml](deploying-to-production/_static/image13.png)
3. Klikněte pravým tlačítkem na *Production.pubxml* a klikněte na tlačítko **otevřete**.
4. Přidejte následující řádky bezprostředně před uzavírací `PropertyGroup` element:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Soubor .pubxml teď vypadá jako v následujícím příkladu:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Další informace o tom, jak vyloučit soubory a složky najdete v tématu [můžete vyloučit konkrétní soubory nebo složky z nasazení?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) v **webové nasazení – nejčastější dotazy pro Visual Studio a ASP.NET** na webu MSDN.

### <a name="deploy-to-production"></a>Nasazení do produkčního prostředí

1. Otevřete **Publikovat Web** průvodce, zkontrolujte, zda **produkční** profilu publikování je vybrána a potom klikněte na **spustit Náhled** na **Preview**a ověřte, zda *robots.txt* souboru se nezkopírují na produkční aplikaci.

    ![Verze Preview soubory k publikování do produkčního prostředí](deploying-to-production/_static/image14.png)

    Zkontrolujte seznam souborů, které budou zkopírovány. Uvidíte, všechny *.cs* soubory, včetně *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, a  *Master.Designer.cs* soubory byly vynechány. Všechny tento kód byl zkompilován do *ContosoUniversity.dll* a *ContosUniversity.pdb* soubory, které se nachází ve *bin* složky. Protože pouze *.dll* je nutný k spuštění aplikace a dříve zadaný, by měly být nasazeny pouze soubory potřebné ke spuštění aplikace, ne *.cs* soubory byly zkopírovány do cílového umístění prostředí. *Obj* složky a *ContosoUniversity.csproj* a *. csproj.user* soubory byly vynechány ze stejného důvodu.

    Klikněte na tlačítko **publikovat** k nasazení do produkčního prostředí.
2. Testování v produkčním prostředí, stejného postupu, které jste použili pro přípravu.

    Všechno, co je stejný jako pracovní s výjimkou adresu URL a absenci *robots.txt* souboru.

## <a name="summary"></a>Souhrn

Teď úspěšně nasadit a otestovat vaší webové aplikace a je k dispozici veřejně přes Internet.

![Produkční domovské stránky](deploying-to-production/_static/image15.png)

V dalším kurzu budete aktualizovat kód aplikace a nasazení změn do prostředí testovací, přípravné nebo produkční prostředí.

> [!NOTE]
> Když aplikaci právě používá v produkčním prostředí by měla implementace plánu obnovení. To znamená můžete by měl být pravidelně zálohování databází z na produkční aplikaci do umístění zabezpečeného úložiště a musí zachovat několika generací takové záloh. Při aktualizaci databáze byste měli vytvořit záložní kopii z bezprostředně před provedením změny. Poté Pokud došlo k chybě a nebudete zjistit až po jeho nasazení do produkčního prostředí, bude stále budete moci obnovit databázi do stavu, ve kterém se nacházel před jeho poškozením. Další informace najdete v tématu [zálohy databáze SQL Azure a obnovení](https://msdn.microsoft.com/en-us/library/windowsazure/jj650016.aspx).


> [!NOTE]
> V tomto kurzu systému SQL Server je edici, která nasazujete do Azure SQL Database. Během procesu nasazení je podobná jiné edice systému SQL Server, skutečné produkční aplikace by mohla vyžadovat zvláštní kód pro Azure SQL Database v některých scénářích. Další informace najdete v tématu [práce s Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) a [výběr mezi SQL serverem a Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).

>[!div class="step-by-step"]
[Předchozí](setting-folder-permissions.md)
[další](deploying-a-code-update.md)
