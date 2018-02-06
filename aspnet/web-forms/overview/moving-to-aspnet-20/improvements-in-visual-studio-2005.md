---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: "Vylepšení v sadě Visual Studio 2005 | Microsoft Docs"
author: microsoft
description: "Visual Studio 2005 poskytuje webové aplikace vývojářům dlouhý seznam vylepšení a vylepšení, které webové projekty."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: aafc59980e807677d6023110d324365ce92bb5fc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/05/2018
---
<a name="improvements-in-visual-studio-2005"></a>Vylepšení v sadě Visual Studio 2005
====================
podle [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 poskytuje webové aplikace vývojářům dlouhý seznam vylepšení a vylepšení, které webové projekty.


Visual Studio 2005 poskytuje webové aplikace vývojářům dlouhý seznam vylepšení a vylepšení, které webové projekty. Jako výkonná jako Visual Studio .NET 2002 a 2003 se vyskytly mnoho stížností způsobem, že byly zpracovány webové projekty. Visual Studio 2005 přidá Chcete-li vyřešit tyto stížností velký počet nových funkcí. Pro ty, kteří raději způsob, Visual Studio .NET 2003 zpracování kompilace webových aplikací, najdete v části [projekty webových aplikací](https://go.microsoft.com/fwlink/?LinkId=57870).

Tento modul a obsahuje vylepšení v oblasti vytváření webového projektu, správu a vývoj. Modul novější dobře obsahuje vylepšení v oblasti vytváření webové projekty a jejich nasazení.

## <a name="frontpage-server-extensions"></a>Serverová rozšíření aplikace FrontPage

Visual Studio .NET 2002 a 2003 vyžaduje serverová rozšíření FrontPage na pole za účelem vytvoření nebo sestavení webové projekty. Vývojáři mají vybrat mezi dvěma režimy různý přístup (serverová rozšíření FrontPage nebo soubor režim přístupu), používá serverová rozšíření FrontPage k provádění úloh, jako je třeba nastavení kořenový adresář aplikace v IIS, atd.

Visual Studio 2005 odebere spoléhat na serverová rozšíření FrontPage pro místní projekty. Visual Studio 2005 teď přistupuje k metabázi služby IIS přímo místo používá serverová rozšíření FrontPage. Visual Studio 2005 také přidává podporu pro FTP, který umožňuje přístup k vzdálené projektu bez nutnosti serverová rozšíření FrontPage.

Pro tyto vývojáře, kteří chtějí používat serverová rozšíření FrontPage v jejich projektů je stále k dispozici možnost. Na základě silné zpětnou vazbu od komunity vývojářů ASP.NET, je však není požadavek.

> [!NOTE]
> Serverová rozšíření FrontPage se stále vyžadují pro vytvoření vzdáleného projektu, otevření, atd.


## <a name="aspnet-development-server"></a>Vývojový server ASP.NET

Visual Studio 2005 se dodává s názvem vývojový Server ASP.NET nový webový server. (Tento webový server byl dříve označované jako Cassini.)

Existuje několik výhod vývojový Server ASP.NET.

- Nyní je možné, kteří nejsou správci vyvíjet a ladit u webového serveru.
- Vývojový Server ASP.NET dynamicky mapuje virtuální adresáře na libovolné místo v systému souborů, který umožňuje flexibilní projektu umístění.
- Uživatelé v systému Windows XP Professional, kteří jsou již pomocí služby IIS, teď bude moct vytvářet nové webové aplikace, které nebude mít vliv na strukturu souboru nebo složky z jejich výchozí web ve službě IIS.

Chcete využít výhod vývojový Server ASP.NET není nutná žádná speciální konfigurace. Pokud webový projekt, který je hostován v systému souborů je ladit nebo procházení, Visual Studio 2005 automaticky spustí instanci vývojový Server ASP.NET na náhodných portu zpracovat požadavek.

Další informace se bude vztahovat na vývojový Server ASP.NET později v tomto modulu.

## <a name="improved-file-management"></a>Vylepšení správy

Soubor projektu (.vbproj pro VB.NET) a .csproj pro jazyk C# v sadě Visual Studio 2002 a 2003 uložené informace na všechny soubory ve webové aplikaci. V zobrazení Průzkumník řešení je založena na informace o souboru v souboru projektu. Z tohoto důvodu by Průzkumník řešení často zobrazit nesprávné informace v případech, kdy se používá externí editory. Visual Studio 2002 a 2003 by často přepsat změny souborů nebo nezobrazí nejnovější verzi souborů.

Visual Studio 2005 nepodporuje počítače do souboru projektu. Místo toho přečte informace souborů a složek přímo z disku, což vede přesné zobrazení souborů ve vašem projektu. Protože složce odkazy v sadě Visual Studio 2002 a 2003 nepředstavuje skutečnou složku ve webové aplikaci, Visual Studio 2005 také odebere složce odkazy v Průzkumníku řešení. Pro přístup k odkazy pro projekt v sadě Visual Studio 2005, používejte na stránkách vlastností projektu.

## <a name="creating-web-projects"></a>Vytváření webové projekty

Vývojáři webů mít mnoho nové možnosti, které jsou k dispozici pro vytvoření projektu v sadě Visual Studio 2005. Weby teď vytvořením kdekoli v systému souborů a můžete pak ladit nebo procházet pomocí nové vývojový Server ASP.NET. Vývojáři mohou také vytvářet nové webů pomocí protokolu FTP.

Kliknutím sem můžete zobrazit na video s návodem vytvoření webové projekty v sadě Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Open Full-Screen Video](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Projektů v systému souborů

Jak už jste viděli v video s návodem, můžete vytvářet weby v systému souborů v místním počítači nebo ve vzdáleném umístění prostřednictvím sdílené složky. Webové servery, které jsou vytvořené v systému souborů jsou Procházet a ladit pomocí vývojový Server ASP.NET.

> [!NOTE]
> Vývojový Server ASP.NET nemusí být úplně jasné pro zákazníky. Pokud je vytvoření webového projektu v systému souborů v IISs adresářovou strukturu (tj. c:/inetpub/wwwroot) webu stále procházet přes vývojový Server ASP.NET při spuštěn z prostředí Visual Studio 2005. Proto všechny konfigurace služby IIS (tj. metody ověřování) není použitelná.


Výchozí webový projekt také odebere mnoho režie podle pouze obsahuje stránku Default.aspx, default.cs souboru a k složce aplikace nebo _Data. Soubor web.config a speciální složky (tj. aplikace nebo _code) se přidají podle potřeby. Webový projekt zahrnuje jenom soubory a složky, které potřebujete.

### <a name="http-projects"></a>Projekty HTTP

Projekty HTTP může být buď projekty, které jsou vytvořené na místní web služby IIS nebo na vzdálený webový server. Výchozí umístění projektu je `http://localhost`. Pokud kliknete na tlačítko Procházet, existují dvě možnosti HTTP: místní a vzdálené lokality. Hlavní rozdíl v těchto dvou možností je metoda, ve kterém se zobrazují informace webu v dialogovém okně vyberte umístění a v tom, jak se soubory se zkopírují na webový server.

Možnost místní služba IIS načte informace o lokalitě z metabáze v místním počítači a kopírují soubory pomocí systému souborů. Možnost vzdálené lokality používá serverová rozšíření FrontPage a informace o lokalitě a soubory budou zkopírovány pomocí protokolu HTTP a volání vzdáleného volání Procedur rozšíření FrontPage serveru.

> [!NOTE]
> Soubor vs###/_tmp.htm a get/_aspx/_ver.aspx se už používají k určení informací o verzi.


Výchozí možnost HTTP je místní služba IIS. Tato možnost přečte metabáze služby IIS určit servery, které jsou k dispozici a umístění, do kterého chcete-li vytvořit obsah. Ve stromovém zobrazení můžete vybrat jinou složku nebo virtuální adresář. Můžete můžete také vytvořit nový virtuální adresář, označit složky jako aplikace, a také odstranit stávající virtuální adresáře z tohoto dialogového okna.


![Zvolte umístění dialogové okno](improvements-in-visual-studio-2005/_static/image1.gif)

**Obrázek 1**: Zvolte umístění dialogové okno


Na rozdíl od v dřívějších verzích sady Visual Studio, pokud zaškrtnete **použití Secure Sockets Layer** zaškrtávací políčko a certifikátu SSL neodpovídá adresu URL procházíte, zobrazí se výstraha zabezpečení dialogové okno s dotazem, pokud byste má pokračovat. Pomocí Visual Studio .NET 2003, je-li certifikát nebyl odpovídající jeden, vytváření projektu skončí s chybou.


![Certifikát SSL výstrahy týkající se zabezpečení](improvements-in-visual-studio-2005/_static/image2.gif)

**Obrázek 2**: výstraha zabezpečení týkající se certifikátu SSL


### <a name="note-on-host-headers"></a>Poznámka: na hlavičky hostitele

Pokud vytváříte webovou aplikaci v lokalitě vázána na konkrétní IP adresu, musíte zajistit, že je nakonfigurovaný hlavičku hostitele. Jinak, vytvoří sada Visual Studio na lokalitu `http://localhost`, ale pokud je web procházet nebo ladit z prostředí IDE nebude správně překlad adresy IP.

Pokud vyberete možnost vzdálené lokality, změní se dialogové okno a umožní vám zadat cílová adresa URL pro nový web. Tato adresa URL musí být na server, který má serverová rozšíření FrontPage povolena. Pokud chcete pracovat s vaší místní webový server používá serverová rozšíření FrontPage, můžete použít možnost vzdálené lokality a zadat místní adresu URL.


![Vytvoření webu na vzdáleném serveru](improvements-in-visual-studio-2005/_static/image1.jpg)

**Obrázek 3**: vytvoření webu na vzdáleném serveru


Při vytvoření aplikace ve vzdálené lokalitě přes SSL, pokud certifikát protokolu SSL neodpovídá, dialogové okno pro potvrzení se poněkud liší od dialogové okno zobrazí, když pomocí možnosti místní služba IIS.


![Výstrahy zabezpečení vzdálené lokality](improvements-in-visual-studio-2005/_static/image3.gif)

**Obrázek 4**: výstraha zabezpečení vzdálené lokality


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 představuje možnost vytvářet weby přes FTP. Pokud použijete tuto možnost, rozhraní IDE vytvoří soubory místně v dočasné složce uživatele a potom pomocí FTP k přesunutí souborů do umístění FTP.

> [!NOTE]
> Umístění dočasné složky je c:/dokumenty a nastavení nebo&lt;uživatele&gt;/Local nastavení nebo dočasné/VWDWebCache/&lt;Server&gt;/_&lt;název aplikace&gt;


Při použití volby FTP, zobrazí se dialogové okno Vybrat umístění. Můžete zadat požadované informace o připojení FTP do toto dialogové okno, jak je uvedeno níže.


![Výběr umístění dialogové okno pro FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Obrázek 5**: Zvolte umístění dialogové okno pro FTP


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Testovacího prostředí: Nastavení FTP lokality a vytvoření projektu

Následující kroky konfigurace serveru FTP tak, aby uživatel má umístění, které pouze jejich můžete uložit do přes FTP.

### <a name="install-the-ftp-service"></a>Nainstalovat službu FTP

1. Otevřete odebrat programy, vyberte možnost Přidat nebo odebrat součásti systému Windows
2. Vyberte Internetová informační služba (aplikační Server v systému Windows 2003) a klikněte na **podrobnosti**.
3. Zkontrolujte **protokol FTP (File Transfer) služby** a klikněte na tlačítko **OK**.
4. Klikněte na tlačítko **Další** k instalaci služby FTP.

### <a name="create-a-new-folder-for-content"></a>Vytvořte novou složku pro obsah

1. V Průzkumníku Windows, vytvořte novou složku s názvem **uživatel1** uvnitř c:/inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Nakonfiguruje složek a oprávnění u složek.

1. Otevření modulu snap-in služby IIS z nástroje pro správu. Nyní máte k serverům FTP složce pod uzlem název počítače.
2. Rozbalte položku **servery FTP**.
3. Klikněte pravým tlačítkem myši **výchozí server FTP**, vyberte **nový**, pak **virtuální adresář**, pak klikněte na tlačítko **Další**.
4. Zadejte **uživatel1** pro název virtuálního adresáře a klikněte na tlačítko **Další**.
5. Zadejte **c:/inetpub/wwwroot/User1** pro cestu a klikněte na tlačítko **Další**.
6. Klikněte na tlačítko **Další** a potom **Dokončit** dokončete průvodce.
7. Klikněte pravým tlačítkem myši **uživatel1** virtuálního adresáře v rámci výchozí server FTP a vyberte **vlastnosti**.
8. Zkontrolujte **zápisu** zaškrtávací políčko a klikněte na tlačítko **OK** zavřete dialogové okno.
9. Klikněte pravým tlačítkem na **výchozí server FTP** a vyberte **vlastnosti**.
10. Na **účtů zabezpečení** kartě a zrušte zaškrtnutí políčka **povolit anonymní připojení**.
11. Klikněte na tlačítko **Ano** v dialogovém okně s dotazem, pokud chcete pokračovat.
12. Klikněte na tlačítko **OK** zavřete dialogové okno.
13. Rozbalte **Default Web Site** pod **weby** uzlu.
14. Klikněte pravým tlačítkem myši **uživatel1** adresář a zaškrtněte možnost **vlastnosti**
15. V **nastavení aplikace** klikněte na tlačítko **vytvořit** označit složky jako aplikace.
16. Klikněte na tlačítko **OK** zavřete dialogové okno.
17. Zavřete modul snap-in Internetová informační služba.

### <a name="create-web-project"></a>Vytvoření webového projektu

1. Open Visual Studio 2005.
2. Z **soubor** nabídce vyberte možnost **nový web**.
3. V **umístění** rozevíracího seznamu vyberte **FTP**.
4. Klikněte na tlačítko **Procházet**.
5. Zadejte **localhost** v **Server** textové pole.
6. Zadejte **uživatel1** do textového pole adresáře.
7. Klikněte na tlačítko **otevřete**. Umístění FTP zadá do dialogu Nový web.
8. Click **OK**.
9. Zrušte zaškrtnutí políčka **anonymní přihlášení** v dialogovém okně přihlášení k serveru FTP zadejte své přihlašovací údaje a klikněte na tlačítko **OK**.
10. Jaká je adresa URL pro projekt? (Adresa URL pro projekt se zobrazí v Průzkumníku řešení.)
11. Z **sestavení** nabídce vyberte možnost **Sestavit web** nebo **sestavit řešení**.
12. Na stránce Default.aspx v Průzkumníku řešení klikněte pravým tlačítkem a vyberte **zobrazit v prohlížeči**.
13. V dialogovém okně požadována adresa URL webu, zadejte `http://localhost/user1` pro adresu URL a klikněte na tlačítko **OK**.

> [!NOTE]
> Pokud dojde k chybě označující nebylo možné načíst typ /_Default, ujistěte se, že používáte technologii ASP.NET 2.0 na váš web a ne starší verze. Můžete to udělat na kartě ASP.NET v Internetové informační službě.


## <a name="opening-web-projects"></a>Otevírání webové projekty

Otevírání webových projektů je podobná vytváření projektů. V následujících částech vyvolávající oblasti dohlížet na pro při práci v prostředí IDE. Popisuje také práci s webovými projekty pomocí protokolu HTTP a FTP.

Otevřete webový projekt, vyberte v nabídce Soubor otevřete web. Zobrazí se výzva s stejné dialogové okno Vybrat umístění popsané dříve a máte stejné čtyři možnosti k dispozici: systém souborů, místní služba IIS, FTP a vzdálené lokality.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Systém souborů

Jak je uvedeno dříve v tomto modulu, Visual Studio už používá soubor projektu. Proto pokud zvolíte možnost Otevřít web ze systému souborů, ve skutečnosti máte možnost výběru libovolné složky, který chcete, i když na složku, kterou zvolíte nebyl vytvořen jako webový projekt původně v sadě Visual Studio. Například můžete otevřít složku Dokumenty jako webový server a Visual Studio brouka otevřít a zobrazit vaše soubory, jak je uvedeno níže.


![Moje dokumenty otevřen jako webovou stránku](improvements-in-visual-studio-2005/_static/image3.jpg)

**Obrázek 6**: *Moje dokumenty* otevřen jako webovou stránku


Protože Visual Studio vytvoří jenom další soubory a složky, pokud je to nezbytné, žádné další soubory nebo složky se přidají do umístění, ve kterém můžete otevřít. Vedlejším účinkem této architektury je, že ho brání vnoření webů v systému souborů. Zvažte například následující adresářovou strukturu.

Webového projektu v C:/MyWebSite

Jiný webový projekt v C:/MyWebSite/vnořené

Když otevřete na webu na c:/MyWebSite, vnořené složky se zobrazí jako podsložku této aplikace.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Při otevírání webů pomocí protokolu HTTP, nastavení jsou přečtena buď z metabáze služby IIS (místní služba IIS) nebo používá serverová rozšíření FrontPage (vzdálený server.) Pokud jsou vnořené webové aplikace, tyto se zobrazí také ikonu, která označuje je jako aplikace. Pokud jste obeznámeni s práce s webovými aplikacemi v FrontPage, toto chování v sadě Visual Studio 2005 je podobné.

I když Visual Studio se zobrazí ikona pro aplikace, které jsou vnořené pod aplikace, který je aktuálně otevřený v prostředí IDE, nebude možné rozšířit jejich a zobrazit jejich obsah. Klikněte dvakrát na můžete, ale, aby jejich otevření. Když to uděláte, zobrazí se dialogové okno s požadavkem, abyste buď otevřete webovou aplikaci (a nahraďte aktuálně otevřené řešení) nebo přidat webové aplikace na vaše aktuální řešení.


![Dvakrát klikněte na ikonu vnořené aplikace nabídne vám toto dialogové okno](improvements-in-visual-studio-2005/_static/image4.jpg)

**Obrázek 7**: dvakrát klikněte na ikonu vnořené aplikace nabídne vám toto dialogové okno


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Server FTP

Když otevřete web přes FTP, soubory se zkopírovat všechny místně do dočasné složky. Úplná cesta pro umístění místní úložiště se zobrazí v podokně vlastností projektu a je vytvořen v následujícím formátu.

C:/dokumenty a nastavení nebo&lt;uživatele&gt;/Local nastavení nebo dočasné/VWDWebCache/&lt;Server&gt;/_&lt;název aplikace&gt;

Pokud používáte FTP, Visual Studio bude třeba zadat základní adresu URL pro váš projekt, ve kterém můžete vyhledat, jak je uvedeno níže. Pokud základní adresu URL nezadáte, Visual Studio zobrazí výzvu k jeho při prvním pokusu o procházení stránky na webu.


![Určení základní adresy URL pro servery FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Obrázek 8**: určení základní adresy URL pro servery FTP


## <a name="improvements-in-compilation"></a>Vylepšení v kompilaci

Práce s webové aplikace v sadě Visual Studio 2005 je výrazně rychlejší než v předchozích verzích. To je kvůli v žádné malou část změny v architektuře kompilace.

V sadě Visual Studio 2002 a 2003 byly zkompilovat webových aplikací do jednoho sestavení primární umístěný ve složce/Bin. V sadě Visual Studio 2005 byla přidána k aplikaci nebo _Code složce. Třídy a jiný kód, bez uživatelského rozhraní se přidají do složky, aplikace nebo _Code. Když Visual Studio vytvoří projekt, všechny soubory ve složce aplikace nebo _Code kompilovány do jednoho souboru App/_Code.dll. Výsledek této změny je mnohem rychlejší než v předchozích verzích následné sestavení.

> [!NOTE]
> Nástroj příkazového řádku nástroje MSBuild lze také vytvářet ASP.NET – webové aplikace. Tento nástroj se budeme v modulu 9.


Další vylepšení kompilace je nová možnost vytvoření stránky v nabídce sestavení. Tato funkce umožňuje vývojáři sestavit pouze aktuální stránku (spolu s, během a závislosti) tak, aby změny, které mohou být zkompilovány rychleji. Protože C# nenabízí pozadí kompilace pro účely aktualizace IntelliSense, atd., se budou využívat znatelně tuto funkci vzhledem k tomu, že bude možné pro technologii IntelliSense, která aktualizovat rychle jednoduše znovu sestavit jednu stránku.

Vlastnosti sestavení projektu umožňují konfigurovat typ sestavení, který se nachází před provedením úvodní stránce. Vývojářům můžete vytvořit pouze aktuální stránky tak, aby Visual Studio můžete spustit ladění aplikací rychleji po změně kódu.


![Akce sestavení úvodní stránky](improvements-in-visual-studio-2005/_static/image6.jpg)

**Obrázek 9**: akci spuštění stránky sestavení


Jiné skvělé vylepšení Architektura technologie ASP.NET a Visual Studio je v oblasti upravit a pokračovat. V sadě Visual Studio 2005 vývojáři můžou spustit ladění projektu a provádět změny kódu na projekt bez odpojení ladicího programu. Ve skutečnosti můžete oznámena spustit ladění projektu, přidejte novou třídu, přidat kód do třídy, přidejte kód na stránku, která vytvoří novou instanci třídy a provedení metody třídy, aniž by museli odpojování ladicího programu. Provádění nový kód je oznámena stejně snadná jako aktualizace v prohlížeči!

Kliknutím sem zobrazit video s návodem upravit a pokračovat funkce v sadě Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Open Full-Screen Video](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


Upravit a pokračovat funkce v technologii ASP.NET 2.0 robustní a Visual Studio 2005 je kvůli změně architektury pro aplikace ASP.NET. V technologii ASP.NET 1.x, aplikace vytvořené v aplikaci Visual Studio 2002 nebo 2003 byly zkompilovány do primárních sestavení, která byla uložená ve složce/Bin. Všechny třídy, stránkách atd pro aplikace se zkompiluje do tuto jednu knihovnu DLL. Potom v době běhu ASP.NET by všechny ovládací prvky, značek a kódu ASP.NET v rámci stránky zkompilovat a zkopírujte tyto knihovny DLL do dočasné složky ASP.NET.

V sadě Visual Studio 2005 pomocí technologie ASP.NET 2.0, kompilace dva modely popisují výše (jeden pro sadu Visual Studio) a jeden pro technologii ASP.NET v době běhu byly sloučeny do jednoho společný model kompilace. To znamená, že všechny problémy kompilace jsou nyní zachyceny během fáze vývoje místo v době běhu. Umožňuje také pro návrháře a podporu technologie IntelliSense pro funkce, jako je uživatelské ovládací prvky a stránky předlohy.

Kliknutím sem zobrazíte na video s návodem návrháře podpory pro uživatelské ovládací prvky.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Open Full-Screen Video](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Je-li uživatelský ovládací prvek odebrán ze stránky, @Register – direktiva zůstane ve značce a měla by být ručně odebrána, aby se zabránilo chybám analyzátor, je-li uživatelského ovládacího prvku odstraněna z webu.


Jiné zlepšování v modelu kompilace Visual Studio je funkce Publikovat Web. Protože funkce publikování překompiluje webu, vývojářům umožňuje využívat přidané výkon nebudou muset nic na vyžádání zkompilovat. Také překompiluje všechny zdrojového kódu ve složce aplikace nebo _Code do knihovny DLL tak, aby žádný zdrojový kód je nutné nasadit.


![Dialogové okno publikování webu](improvements-in-visual-studio-2005/_static/image7.jpg)

**Obrázek 10**: dialogové okno publikování webu


> [!NOTE]
> Nástroj aspnet/_compile.exe také slouží k předem zkompilovat webovou aplikaci ASP.NET. Tento nástroj se budeme v modulu 9.


Když budete publikovat webovou stránku, Předkompilované soubory jsou uloženy ve složce Temporary ASP.NET Files jak je uvedeno níže. Soubory s *Compiled* příponu souboru jsou soubory formátu XML, které definují závislosti pro konkrétní knihovny DLL. Všechny ovládací prvky webových formulářů nebo uživatele se zkompiluje do náhodných knihovny DLL, které začínají *aplikace nebo_webové /_*.

Pokud necháte *tento předkompilovaných webů aktualizovat* políčko zaškrtnuto, značek uvnitř své ovládací prvky webových formulářů a uživatel nebude předem kompilovaném do knihovny DLL, umožní vám provádět změny po nasazení. Pokud si přejete zamknout kód tak, aby nejsou povoleny změny nasazeným obsahem, zrušte zaškrtnutí tohoto políčka.

*Použít pevné názvy a jednoho sestavení* políčko umožňuje, aby se každý stránka kompiluje do sestavení s názvem pevné vypnutí dávkové kompilace. Ponechat toto pole není zaškrtnuto, můžete využít výhod dávkové kompilace.

*Povolení silné názvy na předkompilovaných sestavení* políčko umožňuje silné jméno – předkompilovaných sestavení.

> [!NOTE]
> V technologii ASP.NET 1.x, sestavení se silným názvem bylo možné nainstalovat do globální mezipaměti sestavení (GAC). V aplikaci ASP.NET 2.0 můžete se nevyžaduje instalace sestavení se silným názvem do mezipaměti GAC.


![Soubory předem kompilované aplikace ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Obrázek 11**: soubory předem kompilované aplikace ASP.NET


> [!NOTE]
> V aplikaci výše uvedené se žádný soubor web.config. Pokud by došlo, ho by mít byla volána *PrecompiledApp.config* po publikování webu lokality procesu.


## <a name="improvements-in-deployment"></a>Vylepšení v nasazení

Jako Visual Studio 2002 a 2003, Visual Studio 2005 nabízí funkce kopírování projektu. Ale funkci má byla zvýšenou úložnou v sadě Visual Studio 2005 a se nyní označuje jako Kopírovat webový server.

Dialogové okno Kopírovat web je rozdělit na levém rámec a pravém rámečku. Levý z nich se nazývá zdrojového webu a pravém rámečku se nazývá vzdáleného webového serveru. Jedna z věcí, které by mohly uvádět někteří vývojáři je, že webový server zobrazí v pravém rámečku není nutně vzdálené lokality. Může to být lokality v místním systému souborů nebo v místní instanci služby IIS. Kromě toho serveru zobrazí v levém rámečku není nutně zdrojového webu protože dialogové okno umožňuje publikovat ze vzdáleného webu *k* zdrojového webu.

Pokud kopírujete projektu vzdálený webový server, této lokality musí mít serverová rozšíření FrontPage na něm nainstalován. Pokud ne, musíte připojit pomocí protokolu FTP. Na druhé straně Pokud kopírujete projektu k místní instanci služby IIS, serverová rozšíření FrontPage nejsou požadovány.

> [!NOTE]
> Pokud pokusu o vytvoření nového webu v místní instanci služby IIS a instalují rozšíření FrontPage 2002 Server Extensions, zobrazí se chybová zpráva s oznámením, že vytváření webů, není podporován na serveru služby SharePoint. V takovém případě máte možnost serverová rozšíření FrontPage 2000 instalací nebo odebráním serverová rozšíření FrontPage.


Pro funkci kopírování webu na video s návodem, klikněte sem.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Open Full-Screen Video](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Vylepšení ladění

Existují čtyři klíčových vylepšení v ladění v sadě Visual Studio 2005.

- Ladění místně bez účtu správce je možné z pole.
- False ve výchozím nastavení je nyní atribut ladění pro element kompilace.
- Vzdálené ladění instalace a konfigurace je jednodušší než dřív.
- Nyní můžete ladit webu otevřít pomocí umístění služby FTP.

## <a name="debugging-as-a-non-administrator"></a>Ladění jako bez oprávnění správce

Přidání vývojový Server ASP.NET umožňuje jiným uživatelům než správcům snadno ladění aplikací ASP.NET okamžitě po nasazení. Pokud aplikace ASP.NET spuštěné v místním systému souborů je ladění, Visual Studio spustí vývojový Server ASP.NET v kontextu přihlášeného uživatele. Tento uživatel pak můžete ladit aplikace bez jakékoli dodatečné konfigurace.

## <a name="debug-is-false-by-default"></a>Ladění je False ve výchozím nastavení

V technologii ASP.NET 1.x, *ladění* atribut *kompilace* byl element v souboru web.config nastaven na *true* ve výchozím nastavení. Má vždy doporučujeme, aby vývojáři nastavte tento atribut na *false* před nasazením aplikace do produkčního prostředí, ale protože většina vývojářů není plně chápu důsledky ponechat atribut ladění nastaven na Hodnota TRUE, jednoduše ponechání jej jako-je.

Nejzávažnějšího problém s má atribut ladění je nastaven na hodnotu true, zakáže ASP.NETs dávkové kompilace modelu. Proto je každé stránce zkompilovat do samostatné knihovny DLL. Pokud webu, které aplikace se skládá z tisíce stránek (ne unheard z jakýmikoli prostředky), která znamená, vytvoří se několika tisíc malé knihovny DLL v příslušné aplikaci. I když tyto knihovny jsou malé velikost, nejsou načtena do žádné konkrétní umístění v paměti. Proto způsobit fragmentace v systémové paměti a může přispívat k OutOfMemoryException výskytů.

V aplikaci ASP.NET 2.0 atribut ladění ve výchozím nastavení má hodnotu false. Protože jste už viděli, kdy vývojář debugs aplikace ASP.NET v sadě Visual Studio 2005, bude vyzván k přidá soubor web.config s povoleným laděním. Díky tomu způsobuje stejné nevýhody, jež byly součástí ASP.NET 1.x, ale teď vývojář je jasně upozorněn, že atribut nastaven na hodnotu false před přesunutím aplikace do produkčního prostředí.

## <a name="remote-debugging-setup-and-configuration"></a>Vzdálené ladění instalací a konfigurací

V aplikaci Visual Studio 2002 nebo 2003, vzdálené ladění účinné Machine Debug Manager (mdm.exe) a proces vs7jit.exe. Z tohoto důvodu řešení vzdáleného ladění problémů s často byl černé políčko pro zákazníky a byl často není mnohem lepší pro PSS.

Visual Studio 2005 odebere spoléhat na mdm.exe a vs7jit.exe procesů. Místo toho nyní používá službu sledování vzdáleného ladění (msvsmon.exe).

Požadavek pro vzdálené ladění v sadě Visual Studio 2005 je poměrně jednoduché. Budete muset spustit msvsmon.exe na vzdáleném serveru před ladění. Sledování vzdáleného ladění můžete nainstalovat z disku CD Visual Studio nebo můžete jednoduše spustit msvsmon.exe ze sdílené složky bez vůbec nic. instalace na webovém serveru.

Při spuštění msvsmon.exe, je pravděpodobné, že bude stěžují porty blokována pro vzdálené ladění. Naštěstí můžete snadno odblokovat porty zprava. v dialogovém okně upozornění jak je uvedeno níže.


![Upozornění, že je brána Windows Firewall blokuje vzdálené ladění](improvements-in-visual-studio-2005/_static/image9.jpg)

**Obrázek 12**: oznámení, aby brána Windows Firewall je blokování vzdálené ladění


Jakmile mají odblokováno porty potřebné pro ladění, zobrazí se sledování vzdáleného ladění, jak je uvedeno níže. Z tohoto rozhraní můžete sledovat připojení a změnit oprávnění snadno ladění.


![Sledování vzdáleného ladění](improvements-in-visual-studio-2005/_static/image10.jpg)

**Obrázek 13**: sledování vzdáleného ladění


Je také možné vzdáleně ladit webové aplikace otevřít přes FTP. Kroky jsou stejné jako ty dříve uvedené. Nicméně budete muset zadat základní adresu URL pro procházení FTP projektu, jak je uvedeno výše v tomto modulu.

## <a name="lab-2"></a>Laboratoř 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Vzdálené ladění pomocí sady Visual Studio 2005

Tato laboratoř vás provede vzdáleného ladění pomocí sady Visual Studio 2005.

Kliknutím sem získáte na video s návodem tohoto testovacího prostředí.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Open Full-Screen Video](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Toto testovací prostředí vyžaduje, abyste měli dva počítače, jeden spuštěné Visual Studio 2005 a další spuštěné služby IIS 5 nebo novější.

1. Otevřete Visual Studio 2005 a vytvořit nový web na vzdáleném serveru.

> [!NOTE]
> Můžete vytvořit na webu na vzdálené instanci služby IIS nebo prostřednictvím protokolu FTP.


1. Ze vzdáleného webového serveru vyhledejte msvsmon.exe na vývojovém počítači pomocí cesty UNC a spustit ho.  
 Výchozí umístění pro msvsmon.exe je //server/c$/Program soubory nebo Microsoft Visual Studio 8/Common7/IDE/vzdáleného ladicího programu nebo x86.
2. Pokud se zobrazí výzva k odblokování porty pro vzdálené ladění, učinit.
3. Z vývojovém počítači otevřete kódu pro Default.aspx a nastavte zarážky v metodě stránky nebo _služba.
4. Spusťte ladění z vývojovém počítači.

Zarážce by měl dosáhl podle očekávání.

## <a name="aspnet-development-server"></a>Vývojový server ASP.NET

Jako weve již popsané Visual Studio 2005 se dodává s názvem vývojový Server ASP.NET webového serveru. (Vývojový Server ASP.NET se někdy označuje jako Cassini.) Tento webový server je pohodlný způsob, jak procházet a ladění webových aplikací spuštěných v systému souborů.

Vývojový Server ASP.NET je omezená webového serveru. Neumožňuje vzdáleného připojení, není možné všechny žádosti, z libovolného uživatele než uživatele, který webový server. Také nemá schopnost poskytovat služby na stránkách ASP. Zpracovává jenom ASP.NET prostředky a prostředky HTML (včetně obrázků, souborů CSS atd.).

Vývojový Server ASP.NET může být spuštěn prostřednictvím příkazového řádku spuštěním souboru WebDev.WebServer.exe nacházející se v c:/Windows/Microsoft.NET/Framework/v2.0./ */*  /  */*/*. Následující dialogové okno zobrazí parametry, které jsou k dispozici.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Obrázek 14**


> [!NOTE]
> Vývojový Server ASP.NET není podporován při spuštění explicitně prostřednictvím příkazového řádku.
