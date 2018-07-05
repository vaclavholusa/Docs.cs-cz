---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení do produkčního prostředí | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: 5bc39162bc53dc27230f8ccc55266212a51b6380
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369642"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení do produkčního prostředí
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu nastavte si účet Microsoft Azure, vytvořte přípravného a produkčního prostředí a nasazení webové aplikace ASP.NET v přípravném a produkčním prostředí pomocí sady Visual Studio jedním kliknutím publikování funkce.

Pokud dáváte přednost, můžete nasadit do poskytovatele hostitelských služeb třetích stran. Většinu postupů popsaných v tomto kurzu jsou stejné pro poskytovatele hostingu nebo pro Azure, s tím rozdílem, že každý poskytovatel má své vlastní uživatelské rozhraní pro správu účtu a webové stránky. Můžete najít v poskytovateli hostingu [Galerie zprostředkovatelů](https://www.microsoft.com/web/hosting) na webu Microsoft.com.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat na stránce o řešení problémů v této sérii kurzů.

## <a name="get-a-microsoft-azure-account"></a>Získat účet Microsoft Azure

Pokud ještě nemáte účet Azure, můžete během několika minut vytvořit Bezplatný zkušební účet. Podrobnosti najdete v tématu [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Vytvořit testovací prostředí

> [!NOTE]
> Protože v tomto kurzu byla zapsána, Azure App Service přidali novou funkci k automatizaci mnoha procesy pro vytvoření přípravného a produkčního prostředí. Zobrazit [nastavení přípravných prostředí pro web apps ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Jak je vysvětleno v [nasadit testovací prostředí, najdete v kurzu](deploying-to-iis.md), nejvíce spolehlivé testovací prostředí je webová stránka na poskytovatele hostingu, který má stejně jako webové pracoviště. V mnoha poskytovatelé hostingu byste museli zvážit výhody tohoto proti významné další poplatky, ale v Azure můžete vytvořit další bezplatná webová aplikace jako pracovní aplikace. Budete potřebovat databázi a další výdaje za, která přes výdaje provozní databáze bude mít buď žádný nebo minimální. V Azure platíte velikost úložiště databáze, které používáte, a nikoli pro každou databázi, a bude minimální velikost dalšího úložiště, které budete používat v testovacím prostředí.

Jak je vysvětleno v [nasadit do testovacího prostředí kurzu](deploying-to-iis.md), do pracovního a produkčního prostředí se chystáte nasadit do jedné databáze dvě databáze. Pokud byste chtěli je oddělit, proces by stejné s tím rozdílem, že vytvoříte další databáze pro jednotlivá prostředí a vyberete řetězec správné cíl pro každou databázi, když vytvoříte profil publikování.

V této části kurzu vytvoříte webovou aplikaci a databázi pro testovací prostředí, a budete nasazení do přípravného prostředí a testování existuje ještě před vytvořením a nasazením do produkčního prostředí.

> [!NOTE]
> Následující kroky ukazují, jak vytvořit webovou aplikaci ve službě Azure App Service pomocí portálu pro správu Azure. V nejnovější verzi sady Azure SDK můžete také provést bez opuštění sady Visual Studio pomocí Průzkumníka serveru. V sadě Visual Studio 2013 můžete také vytvořit webovou aplikaci přímo z dialogového okna pro publikování. Další informace najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. V [portálu pro správu Azure](https://manage.windowsazure.com/), klikněte na tlačítko **Websites**a potom klikněte na tlačítko **nový**.
2. Klikněte na tlačítko **webu**a potom klikněte na tlačítko **vytvořit vlastní**.

    **Nový - vytvořit vlastní web** otevře se průvodce. **Vytvořit vlastní** Průvodce vám umožní vytvořit webovou stránku a databází ve stejnou dobu.
3. V **vytvořit web** kroku v průvodci zadat řetězec **adresy URL** pole použít jako jedinečná adresa URL pro vaši aplikaci prvku přípravného prostředí. Zadejte například ContosoUniversity-staging123 (včetně náhodných čísel na konci pro zajištění jeho jedinečnosti v případě, že se používá ContosoUniversity pracovní).

    Úplná adresa URL bude obsahovat co zadáte tady a příponu, která se zobrazí vedle textového pole.
4. V **oblasti** rozevírací seznam, vyberte oblast, která je nejblíže k vám.

    Toto nastavení určuje datové centrum, kterého webová aplikace se spustí v.
5. V **databáze** rozevíracím seznamu klikněte na položku **vytvořit novou databázi SQL**.
6. V **název připojovacího řetězce DB** okně ponechte výchozí hodnotu *objekt DefaultConnection*.
7. Klikněte na šipku, která ukazuje vpravo v dolní části seznamu.

    Je vidět na následujícím obrázku **vytvořit web** dialogové okno s ukázkové hodnoty v ní. Adresa URL a oblasti, kterou jste zadali, se liší.

    ![Vytvoření webu krok](deploying-to-production/_static/image1.png)

    Průvodce přejde **nastavení databáze** kroku.
8. V **název** zadejte *ContosoUniversity* plus náhodné číslo pro zajištění jeho jedinečnosti, například *ContosoUniversity123*.
9. V **Server** vyberte **nový Server služby SQL Database**.
10. Zadejte jméno správce a heslo.

    Tak nezadáváte existující jméno a heslo. Zadáváte nový název a heslo, které teď definujete pro pozdější použití při přístupu k databázi.
11. V **oblasti** vyberte stejnou oblast, kterou jste zvolili pro webovou aplikaci.

    Aby webový server a databázový server ve stejné oblasti poskytuje nejlepší výkon a minimalizuje náklady.
12. Kliknutím na značku zaškrtnutí v dolní části políčka potvrďte, že budete hotovi.

    Je vidět na následujícím obrázku **nastavení databáze** dialogové okno s ukázkové hodnoty v ní. Hodnoty, které jste zadali, se může lišit.

    ![Krok nastavení databáze webu nový - vytvořit pomocí Průvodce databáze](deploying-to-production/_static/image2.png)

    Na portálu pro správu vrátí na stránku pro Websites a **stav** sloupci se zobrazuje, že probíhá vytváření webové aplikace. Po chvíli (obvykle během méně než minutu) **stav** sloupci se zobrazuje, že webová aplikace byla úspěšně vytvořena. Na navigačním panelu na levé straně, počet webových aplikací, které máte ve vašem účtu se zobrazí vedle **Websites** ikonu a počet databází, které se zobrazí vedle **databází SQL** ikonu.

    ![Weby stránce portálu pro správu, vytvořit webovou stránku](deploying-to-production/_static/image3.png)

    Název vaší webové aplikace se bude lišit od aplikace příklad na obrázku.

## <a name="deploy-the-application-to-staging"></a>Nasaďte aplikaci do přípravného prostředí

Teď, když jste vytvořili webovou aplikaci a databázi pro testovací prostředí, můžete nasadit projekt na ni.

> [!NOTE]
> Tyto pokyny ukazují, jak vytvořit profil publikování stažením *.publishsettings* soubor, který funguje nejen pro Azure, ale také pro externí poskytovatele hostingu. Nejnovější sadu Azure SDK můžete také připojit přímo k Azure ze sady Visual Studio a zvolte ze seznamu webových aplikací, které máte ve svém účtu Azure. V sadě Visual Studio 2013, můžete přihlásit k Azure z **publikování webu** dialogové okno nebo **Průzkumníka serveru** okna. Další informace najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Stáhněte si soubor .publishsettings

1. Klikněte na název webové aplikace, kterou jste právě vytvořili.

    ![Klikněte na lokalitu, přejděte na řídicí panel](deploying-to-production/_static/image4.png)
2. V části **rychlý přehled** v **řídicí panel** klikněte na tlačítko **stáhnout profil publikování**.

    ![Profil publikování odkaz ke stažení](deploying-to-production/_static/image5.png)

    Tento krok stáhne soubor, který obsahuje všechna nastavení, které potřebujete k nasazení aplikace do webové aplikace. Tento soubor budete importovat do sady Visual Studio, takže není nutné tyto informace zadat ručně.
3. Uložit *.publishsettings* souboru ve složce, která se dá dostat z Visual Studio.

    ![ukládání souboru .publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Zabezpečení – *.publishsettings* soubor obsahuje vaše přihlašovací údaje (nekódovaný), které se používají ke správě vašich předplatných a služeb Azure. Osvědčené postupy zabezpečení pro tento soubor je dočasně ukládat mimo vašeho adresáře zdrojových souborů (například ve složce Libraries\Documents) a odstraňte ji. Po dokončení importu. Uživatel se zlými úmysly, který získá přístup k *.publishsettings* soubor můžete upravit, vytvářet a odstraňovat služeb Azure.

### <a name="create-a-publish-profile"></a>Vytvořte profil publikování

1. Ve Visual Studiu klikněte pravým tlačítkem na projekt ContosoUniversity v **Průzkumníka řešení** a vyberte **publikovat** v místní nabídce.

    **Publikování webu** otevře se průvodce.
2. Klikněte na tlačítko **profilu** kartu.
3. Klikněte na tlačítko **Import**.
4. Přejděte *.publishsettings* soubor jste předtím stáhli a potom klikněte na **otevřít**.

    ![Dialogové okno importu nastavení publikování](deploying-to-production/_static/image7.png)
5. V **připojení** klikněte na tlačítko **ověřit připojení** abyste měli jistotu, že jsou správné nastavení.

    Při připojení je potvrzená, se vedle položky zobrazí zelená značka zaškrtnutí **ověřit připojení** tlačítko.

    Pro některé poskytovatelům hostingu, po kliknutí na **ověřit připojení**, můžete se setkat **Chyba certifikátu** dialogové okno. Pokud tak učiníte, ověřte, že je název serveru, co očekáváte. Pokud je název serveru správný, vyberte **uložení tohoto certifikátu na budoucí relace sady Visual Studio** a klikněte na tlačítko **přijmout**. (Tato chyba znamená, že poskytovatel hostingu rozhodl vyhnout prostředky na nákup certifikátu SSL pro adresu URL, kterou nasazujete. Pokud chcete navázat zabezpečené připojení pomocí platného certifikátu, kontaktujte poskytovatele hostingových služeb.)
6. Klikněte na tlačítko **Další**.

    ![Ikona úspěšné připojení a tlačítko Další na kartě připojení Průvodce](deploying-to-production/_static/image8.png)
7. V **nastavení** kartu, rozbalte **možností publikování souboru**a pak vyberte **vyloučit soubory z aplikace\_složka dat**.

    Informace o dalších možností v části **možností publikování souboru**, najdete v článku [nasazení do služby IIS](deploying-to-iis.md) kurzu. Snímek obrazovky tohoto ukazuje výsledek tohoto kroku a následující kroky konfigurace databáze je na konci kroky konfigurace databáze.
8. V části **objekt DefaultConnection** v **databází** nakonfigurujte nasazení databáze pro databázi členství.
9. 1. Vyberte **aktualizace databáze**.

        **Vzdálený připojovací řetězec** pole pod **objekt DefaultConnection** vyplněné se připojovací řetězec ze souboru .publishsettings. Připojovací řetězec obsahuje přihlašovací údaje SQL serveru, které jsou uložené ve formátu prostého textu v *.pubxml* souboru. Pokud nechcete uložit je trvale existuje, můžete je odebrat z profilu publikování po nasazení databáze a uložit je do Azure. Další informace najdete v tématu [tom, jak zabezpečit vaši databázi technologie ASP.NET připojovací řetězce při nasazování do Azure ze zdroje](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) na blogu Scott Hanselman.
      2. Klikněte na tlačítko **konfigurovat aktualizace databáze**.
      3. V **konfigurace aktualizací databáze** dialogové okno, klikněte na tlačítko **přidat skript SQL**.
      4. V **přidat skript SQL** , přejděte na *aspnet-data-prod.sql* skript, který jste předtím uložili ve složce řešení a potom klikněte na tlačítko **otevřít**.
      5. Zavřít **konfigurace aktualizací databáze** dialogové okno.
10. V části **SchoolContext** v **databází** vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**.

    Visual Studio zobrazí **spustit migrace Code First** místo **aktualizace databáze** pro `DbContext` třídy. Pokud chcete použít poskytovatele dbDacFx místo migrace nasazení databáze, ke kterým přístup pomocí `DbContext` najdete v tématu [jak nasadit bez migrace Code First databázi?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) v nejčastějších Dotazech nasazení webové aplikace Visual Studio a ASP.NET na webu MSDN.

    **Nastavení** kartu teď vypadá jako v následujícím příkladu:

    ![Karta nastavení pro přípravu](deploying-to-production/_static/image9.png)
11. Proveďte následující kroky pro uložení profilu a přejmenujte ho na *pracovní*:

    1. Klikněte na tlačítko **profilu** kartu a potom klikněte na tlačítko **spravovat profily**.
    2. Import vytvořit dvě nové profily, jeden pro FTP a jeden pro nasazení webu. Nakonfigurovaný profil nasazení webu: přejmenování tohoto profilu *pracovní*.

        ![Přejmenovat profil do přípravného prostředí](deploying-to-production/_static/image10.png)
    3. Zavřít **Upravit profily publikování webových** dialogové okno.
    4. Zavřít **publikování webu** průvodce.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Konfigurace transformace profil publikování pro ukazatel prostředí

> [!NOTE]
> Tato část ukazuje, jak zřídit transformaci Web.config pro ukazatel prostředí. Vzhledem k tomu, se indikátor nachází ve `<appSettings>` element, můžete mít Další alternativou k určení transformace při nasazování do služby Azure App Service. Další informace najdete v tématu [nastavení zadáte Web.config v Azure](web-config-transformations.md#watransforms).


1. V **Průzkumníka řešení**, rozbalte **vlastnosti**a potom rozbalte **PublishProfiles**.
2. Klikněte pravým tlačítkem na *Staging.pubxml*a potom klikněte na tlačítko **přidat konfigurační transformace**.

    ![Přidat konfigurační transformaci pro přípravu](deploying-to-production/_static/image11.png)

    Visual Studio vytvoří *Web.Staging.config* transformační soubor a otevře jej.
3. V *Web.Staging.config* transformovat soubor, vložte následující kód bezprostředně po otevření `configuration` značky.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Když použijete pracovní profil publikování, nastaví Tato transformace na indikátor prostředí a "Produkční". V nasazené webové aplikace neuvidíte žádné přípony, jako je například "(vývoj)" nebo "(testovací)" po nadpis H1 "University společnosti Contoso".
4. Klikněte pravým tlačítkem myši *Web.Staging.config* souboru a klikněte na tlačítko **transformace ve verzi Preview** abyste měli jistotu, že transformace kódování vytvoří očekávané změny.

    **Náhled souboru Web.config** okno zobrazuje výsledek použití i *Web.Release.config* transformuje a *Web.Staging.config* transformace.

### <a name="prevent-public-use-of-the-test-app"></a>Zabránit veřejně přístupný test aplikace

Což je důležité pro pracovní aplikace je, že je na Internetu, ale nechcete, aby veřejně ji používat. Chcete-li minimalizovat pravděpodobnost, že lidé najít, který se použije, můžete použít jeden nebo více z následujících metod:

- Nastavte pravidla brány firewall, která umožňují přístup k pracovní aplikace jenom z IP adresy, které můžete použít k testování přípravy.
- Použijte obfuskovaný adresu URL, která by jinak nebylo možné uhodnout.
- Vytvoření *robots.txt* souboru k zajištění, že nesmí být z vyhledávací weby procházení k němu testovací aplikace a sestava odkazy ve výsledcích hledání.

První z těchto metod je nejúčinnější, ale není součástí v tomto kurzu, protože by vyžadovalo, můžete nasadit do cloudové služby Azure, namísto služby Azure App Service. Další informace o službách Cloud Services a omezení IP adres v Azure najdete v tématu [možnosti hostování výpočtů poskytované platformou Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) a [bloku konkrétní IP adresy v přístupu k webové roli](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Pokud provádíte nasazení do poskytovatele hostitelských služeb třetích stran, obraťte se na poskytovatele a zjistěte, jak implementovat omezení IP adres.

V tomto kurzu vytvoříte *robots.txt* souboru.

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **přidat novou položku**.
2. Vytvořte nový **textový soubor** s názvem *robots.txt*a vložte následující text:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent` Řádek říká vyhledávací weby, které v souboru pravidla se vztahují na všechny vyhledávací modul web prohledávací moduly (roboty), a `Disallow` řádek určuje, že by bylo žádné stránky na webu.

    Chcete vyhledávací weby snadněji katalogu produkční aplikace, takže je třeba vyloučit tento soubor z produkčního nasazení. Provedete to, že nakonfigurujete nastavení v produkční profil publikování po jeho vytvoření.

### <a name="deploy-to-staging"></a>Nasazení do přípravného prostředí

1. Otevřít **Publikovat Web** Průvodce pravým tlačítkem myši projekt Contoso University a kliknutím na **publikovat**.
2. Ujistěte se, že **pracovní** vybraný profil.
3. Klikněte na tlačítko **publikovat**.

    **Výstup** okno zobrazuje, jaké akce nasazení byly provedeny a oznámí úspěšné dokončení nasazení. K adrese URL nasazené webové aplikace se automaticky otevře výchozí prohlížeč.

## <a name="test-in-the-staging-environment"></a>Testování v testovacím prostředí

Všimněte si, že chybí ukazatel prostředí (neexistuje žádný "()" nebo "(vývoj)" po nadpis H1, který ukazuje, že *Web.config* transformace pro ukazatel prostředí bylo úspěšné.

![Domovská stránka pracovní](deploying-to-production/_static/image12.png)

Spustit **studenty** stránku a ověřte, zda má nasazené databáze se žádní studenti.

Spustit **Instruktoři** stránku a ověřte, že Code First naplnila databázi s instruktorem dat:

Vyberte **přidat studenty** z **studenty** přidat student nabídky a pak zobrazit nového objektu student do **studenty** stránku a ověřte, že můžete úspěšně zapisovat do databáze .

Z **kurzy** klikněte na **aktualizace kredity**. **Aktualizace kredity** stránka vyžaduje oprávnění správce, proto **přihlásit** zobrazí se stránka. Zadejte přihlašovací údaje účtu správce, které jste vytvořili dříve ("admin" a "prodpwd"). **Aktualizace kredity** se zobrazí stránka, která ověřuje, že účet správce, který jste vytvořili v předchozím kurzu byla správně nasazena do testovacího prostředí.

Neplatná adresa URL způsobí chybu, že ELMAH sledovat a následně požadovat zprávy o chybách ELMAH žádosti. Pokud provádíte nasazení do poskytovatele hostitelských služeb třetích stran, pravděpodobně zjistíte, že sestava je prázdná ze stejného důvodu, který byl prázdný v předchozím kurzu. Budete muset použít nástroje pro správu účtu poskytovatele hostingu nakonfigurovat oprávnění složky umožňující ELMAH zapisovat do složky protokolů.

Vytvořená aplikace je nyní spuštěna v cloudu ve webové aplikaci, která je stejně jako bude používat pro produkční prostředí. Vzhledem k tomu, že všechno funguje správně, je dalším krokem je nasazení do produkčního prostředí.

## <a name="deploy-to-production"></a>Nasazení do produkčního prostředí

Proces pro vytvoření webové aplikace v produkčním prostředí a nasazení do produkčního prostředí je stejná jako pracovní, s tím rozdílem, že je třeba vyloučit *robots.txt* z nasazení. K tomu budete upravovat soubor profilu publikování.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Vytvoření produkčního prostředí a produkční profil publikování

1. Vytvoření produkční webové aplikace a databáze v Azure, stejným způsobem, který jste použili pro přípravu.

    Při vytváření databáze, můžete chtít umístit na stejný server, který jste vytvořili dříve nebo vytvořit nový server.
2. Stáhněte si *.publishsettings* souboru.
3. Vytvořte profil publikování pomocí importu produkční *.publishsettings* souboru stejného postupu, který jste použili pro přípravu.

    Nezapomeňte nakonfigurovat skript nasazení dat. pod **objekt DefaultConnection** v **databází** část **nastavení** kartu.
4. Přejmenovat profil publikování *produkční*.
5. Konfigurace transformace profil publikování pro ukazatel prostředí, stejným způsobem, který jste použili pro pracovní...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Upravte soubor .pubxml vyloučit robots.txt

Soubory jsou pojmenovány profil publikování &lt;profilename&gt;*.pubxml* a jsou umístěny v *PublishProfiles* složky. *PublishProfiles* složky *vlastnosti* složky ve webové aplikaci C# v části projekt *Můj projekt* složky v projektu VB webové aplikace nebo v rámci *Aplikace\_Data* složky v projektu webové aplikace. Každý *.pubxml* soubor obsahuje nastavení, které se vztahují na jeden profil publikování. Hodnoty, které zadáte v Průvodci Publikovat Web se ukládají v těchto souborech a můžete je vytvořit nebo změnit nastavení, které nejsou k dispozici v Uživatelském rozhraní aplikace Visual Studio upravit.

Ve výchozím nastavení *.pubxml* soubory jsou zahrnuty v projektu, když vytvoříte profil publikování, ale můžete je vyloučit z projektu a je stále používají Visual Studio. Visual Studio vyhledá *PublishProfiles* složku pro *.pubxml* soubory, bez ohledu na to, zda jsou zahrnuty v projektu.

Pro každou *.pubxml* soubor existuje *. pubxml.user* souboru. *. Pubxml.user* soubor obsahuje šifrované heslo, pokud jste vybrali **uložit heslo** možnost a ve výchozím nastavení je vyloučený z projektu.

A *.pubxml* soubor obsahuje nastavení, které se týkají konkrétní Publikovat profil. Pokud chcete nakonfigurovat nastavení, které se vztahují na všechny profily, můžete vytvořit *. wpp.targets* souboru. Importuje tyto soubory do procesu sestavení *.csproj* nebo *.vbproj* souboru projektu, takže většinu nastavení, které můžete nakonfigurovat v souboru projektu se dá nakonfigurovat v těchto souborů. Další informace o *.pubxml* soubory a *. wpp.targets* soubory, naleznete v tématu [postupy: Úprava nastavení nasazení v souborech profil publikování (.pubxml) a. wpp.targets souboru v sadě Visual Studio Webové projekty](https://msdn.microsoft.com/library/ff398069.aspx).

1. V **Průzkumníka řešení**, rozbalte **vlastnosti** a rozbalte **PublishProfiles**.
2. Klikněte pravým tlačítkem na *Production.pubxml* a klikněte na tlačítko **otevřít**.

    ![Otevřete soubor .pubxml](deploying-to-production/_static/image13.png)
3. Klikněte pravým tlačítkem na *Production.pubxml* a klikněte na tlačítko **otevřít**.
4. Přidejte následující řádky bezprostředně před uzavírací `PropertyGroup` element:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Soubor .pubxml teď vypadá jako v následujícím příkladu:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Další informace o tom, jak vyloučit soubory a složky, najdete v části [můžu vyloučit určité soubory nebo složky z nasazení?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) v **webové nasazení – nejčastější dotazy pro Visual Studio a ASP.NET** na webové stránce MSDN.

### <a name="deploy-to-production"></a>Nasazení do produkčního prostředí

1. Otevřít **Publikovat Web** Průvodce Ujistěte se, že **produkční** profil publikování je vybrána a potom klikněte na tlačítko **spustit Náhled** na **Preview**kartu a ověřte, že *robots.txt* soubor nebude zkopírován do produkční aplikace.

    ![Náhled souborů, které mají být publikovány do produkčního prostředí](deploying-to-production/_static/image14.png)

    Projděte si seznam souborů, které budou zkopírovány. Uvidíte, že všechny *.cs* souborů, včetně *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, a  *Master.Designer.cs* soubory jsou vynechány. Veškerý tento kód byl zkompilován do *ContosoUniversity.dll* a *ContosUniversity.pdb* soubory, které najdete v *bin* složky. Protože pouze *.dll* , je potřeba spustit aplikaci a nastavili, že by měly být nasazeny pouze soubory potřebné ke spuštění aplikace, ne *.cs* soubory byly zkopírovány do cíle prostředí. *Obj* složky a *ContosoUniversity.csproj* a *. csproj.user* soubory jsou vynechány ze stejného důvodu.

    Klikněte na tlačítko **publikovat** k nasazení do produkčního prostředí.
2. Testování v produkčním prostředí, stejným způsobem, který jste použili pro přípravu.

    Všechno, co je stejný jako pracovní, s výjimkou adresy URL a absenci *robots.txt* souboru.

## <a name="summary"></a>Souhrn

Budete mít teď úspěšně nasadila a otestovala vaší webové aplikace a je k dispozici veřejně přes Internet.

![Domovská stránka výroby](deploying-to-production/_static/image15.png)

V dalším kurzu budete moct aktualizovat kód aplikace a nasadit změny do prostředí test, přípravném nebo produkčním prostředí.

> [!NOTE]
> Když vaše aplikace je používána v produkčním prostředí by měla implementace plánu obnovení. To znamená je by měly být pravidelně zálohování databází z produkční aplikace do zabezpečeného úložiště a by měl udržování několika generací tyto zálohy. Při aktualizaci databáze, měli byste zajistit záložní kopie z bezprostředně před provedením změny. Potom Pokud došlo k chybě a není objevit až po jeho nasazení do produkčního prostředí, stále budete moci obnovit databázi do stavu, ve kterém byl předtím, než začal být poškozen. Další informace najdete v tématu [Azure SQL Database zálohování a obnovení](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> V tomto kurzu systému SQL Server je edici, která nasazujete do Azure SQL Database. Během procesu nasazení se podobně jako jiné edice systému SQL Server, skutečné produkční aplikace můžou vyžadovat zvláštní kód pro Azure SQL Database v některých scénářích. Další informace najdete v tématu [pracovat se službou Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) a [výběru mezi SQL serverem a Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Předchozí](setting-folder-permissions.md)
> [další](deploying-a-code-update.md)
