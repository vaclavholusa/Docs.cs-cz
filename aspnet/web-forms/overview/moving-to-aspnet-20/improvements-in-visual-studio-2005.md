---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Vylepšení v sadě Visual Studio 2005 | Dokumentace Microsoftu
author: microsoft
description: Visual Studio 2005 poskytuje vývojáře webových aplikací s dlouhým seznamem vylepšení a rozšíření pro webové projekty.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 60259ceb99de536410aa5f53db64fb2dca68bf66
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754268"
---
<a name="improvements-in-visual-studio-2005"></a>Vylepšení v sadě Visual Studio 2005
====================
podle [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 poskytuje vývojáře webových aplikací s dlouhým seznamem vylepšení a rozšíření pro webové projekty.


Visual Studio 2005 poskytuje vývojáře webových aplikací s dlouhým seznamem vylepšení a rozšíření pro webové projekty. Výkonné jsou Visual Studio .NET 2002 a 2003, došlo k mnoha stížností způsobem, že byly zpracovány webové projekty. Visual Studio 2005 přidá řeší tyto stížností velký počet nových funkcí. Pro ty, kteří dávají přednost tak, že Visual Studio .NET 2003 zpracování kompilace webových aplikací, najdete v článku [projekty webových aplikací](https://go.microsoft.com/fwlink/?LinkId=57870).

V tomto modulu zahrnují také vylepšení vytváření webového projektu, správu a vývoj. Novější modul a obsahuje vylepšení vytváření webových projektů a jejich nasazování.

## <a name="frontpage-server-extensions"></a>Rozšíření serveru FrontPage

Visual Studio .NET 2002 a 2003 vyžaduje rozšíření serveru FrontPage v poli, aby bylo možné vytvářet nebo sestavovat webové projekty. Možnost volby mezi dvěma režimy různý přístup (soubor nebo rozšíření serveru FrontPage režim přístupu) mají vývojáři, používá rozšíření serveru FrontPage provádět úkoly, jako je nastavení kořenový adresář aplikace v IIS atd.

Visual Studio 2005 odebere závislost na rozšíření serveru FrontPage pro místních projektů. Visual Studio 2005 teď přistupuje k metabázi služby IIS přímo namísto použití rozšíření serveru FrontPage. Visual Studio 2005 také přidává podporu pro FTP, který umožňuje vzdálený projekt bez nutnosti rozšíření serveru FrontPage.

Pro vývojáře, kteří chtějí používat rozšíření serveru FrontPage ve svých projektech je stále k dispozici možnost. Ale na základě silné zpětnou vazbu od komunity vývojářů technologie ASP.NET, není povinné.

> [!NOTE]
> Rozšíření serveru FrontPage se stále vyžadují pro vytvoření vzdáleného projektu, otevření, atd.


## <a name="aspnet-development-server"></a>Vývojový server ASP.NET

Visual Studio 2005 se dodává s názvem serveru ASP.NET Development Server nový webový server. (Tento webový server se dřív označovalo jako Cassini.)

Existuje několik výhod serveru ASP.NET Development Server.

- Nyní je možné, kteří nejsou správci vyvíjet a ladit na webovém serveru.
- Virtuální adresáře serveru ASP.NET Development Server mapuje dynamicky do libovolného umístění v systému souborů umožňuje flexibilní projektu umístění.
- Uživatelé systému Windows XP Professional, kteří již používají službu IIS teď budou moct vytvořit nové webové aplikace, které ovlivňují strukturu souboru nebo složky z jejich výchozí webový server ve službě IIS.

Abyste mohli využívat serveru ASP.NET Development Server není nutná žádná zvláštní konfigurace. Když webový projekt, který je hostován v systému souborů se ladit nebo procházet, Visual Studio 2005, automaticky spustí instanci serveru ASP.NET Development Server na náhodný port ke zpracování požadavku.

Další informace se bude vztahovat na serveru ASP.NET Development Server později v tomto modulu.

## <a name="improved-file-management"></a>Vylepšení správy

Soubor projektu (.vbproj pro VB.NET) a csproj pro jazyk C# v sadě Visual Studio 2002 a 2003, uloženy informace u všech souborů ve webové aplikaci. Zobrazení Průzkumníka řešení je založena na informace o souboru v souboru projektu. Z tohoto důvodu by Průzkumník řešení často zobrazit nesprávné informace v případech, ve kterém byly použity externích editorech. Visual Studio 2002 a 2003 by často přepsat změny souborů nebo nejnovější verzi souborů se nezobrazí.

Visual Studio 2005 se hned do souboru projektu. Místo toho přečte informace, souborům a složkám přímo z disku a výsledkem je přesné zobrazení souborů ve vašem projektu. Protože složka s odkazy v sadě Visual Studio 2002 a 2003 nepředstavuje skutečný složky ve webové aplikaci, Visual Studio 2005 na složku odkazy taky odebere z Průzkumníka řešení. Pro přístup k odkazy pro váš projekt v sadě Visual Studio 2005, používejte stránky vlastností pro projekt.

## <a name="creating-web-projects"></a>Vytváření webových projektů

Weboví vývojáři mají spousta nových možností, které jsou k dispozici pro vytvoření projektu v sadě Visual Studio 2005. Web sites teď je možné vytvářet kdekoli v systému souborů a pak můžete ladit nebo procházet pomocí nového serveru ASP.NET Development Server. Vývojáři mohou také vytvářet nové weby pomocí FTP.

Kliknutím sem zobrazíte video s návodem, vytváření webových projektů v sadě Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Otevřít Video na celou obrazovku](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Projekty systému souborů

Jak už jste viděli v video s názorným postupem, můžete vytvářet weby v systému souborů na místním počítači nebo ve vzdáleném umístění prostřednictvím sdílené složky. Webové stránky, které jsou vytvořeny v systému souborů jsou Procházet a ladit pomocí serveru ASP.NET Development Server.

> [!NOTE]
> Vývojový Server ASP.NET může způsobit jisté zmatení pro zákazníky. Pokud webový projekt je vytvořen v systému souborů do struktury adresářů IISs (například c:/inetpub/wwwroot), na webu společnosti stále procházet přes vývojový Server ASP.NET při spuštění z v rámci sady Visual Studio 2005. Proto všechny konfigurace služby IIS (tj. metody ověřování) se nevztahuje.


Výchozí webový projekt odebere také mnohem zatížení podle pouze obsahuje stránku Default.aspx default.cs soubor a složku aplikace/_Data. Soubor web.config a speciální složky (například aplikace/_fragmenty) jsou přidány jako nejsou potřeba. Webový projekt obsahuje pouze soubory a složky, které potřebujete.

### <a name="http-projects"></a>Projekty HTTP

Projekty, které jsou vytvořeny na web místní služby IIS nebo na vzdálený web může být buď HTTP projekty. Výchozí umístění projektu `http://localhost`. Pokud kliknete na tlačítko Procházet, existují dvě možnosti protokolu HTTP: místní a vzdálené lokality. Hlavní rozdíl v těchto dvou možností je metoda, ve kterém se zobrazí informace o webu v dialogovém okně Zvolit umístění a v tom, jak soubory se zkopírují na webový server.

Možnost místní služba IIS načte informace o lokalitě z metabáze na místním počítači a soubory se zkopírují pomocí systému souborů. Možnost vzdálené lokality používá rozšíření serveru FrontPage a informací o lokalitě a soubory se zkopírují pomocí protokolu HTTP a volání RPC rozšíření serveru FrontPage.

> [!NOTE]
> Get/_aspx/_ver.aspx a vs###/_tmp.htm souboru se už používají k určení informací o verzi.


Výchozí možnost HTTP je místní služba IIS. Tato možnost čtení metabáze služby IIS určit weby, které jsou k dispozici a oblasti, ve kterém chcete vytvořit obsah. Výběrem ve stromovém zobrazení můžete vybrat jinou složku nebo virtuálního adresáře. Je můžete také vytvořit nový virtuální adresář, označit složky jako aplikace, jakož i odstranit existující virtuální adresáře z tohoto dialogového okna.


![Zvolte umístění](improvements-in-visual-studio-2005/_static/image1.gif)

**Obrázek 1**: Zvolte umístění


Na rozdíl od v dřívějších verzích sady Visual Studio, pokud zaškrtnete **použít zabezpečení SSL** zaškrtávací políčko a certifikát protokolu SSL se neshoduje s URL procházení, zobrazí se výstraha zabezpečení dialogové okno s dotazem, pokud byste byli Chcete-li pokračovat, jako jsou. Pomocí Visual Studio .NET 2003, pokud certifikát není odpovídající jedné, vytvoření projektu selže.


![Certifikát SSL výstrahy týkající se zabezpečení](improvements-in-visual-studio-2005/_static/image2.gif)

**Obrázek 2**: výstraha zabezpečení týkající se certifikátu SSL


### <a name="note-on-host-headers"></a>Poznámka: na hlavičky hostitele

Pokud vytváříte webovou aplikaci na webu vázán na konkrétní IP adresu, musíte zajistit konfiguraci hlavičku hostitele. Jinak, vytvoří Visual Studio na webu `http://localhost`, ale když je web procházet nebo ladit z integrovaného vývojového prostředí nebude správně přeložit IP adresu.

Pokud vyberete možnost vzdálené lokality, dialogové okno se změní na umožňují zadat cílovou adresu URL pro nový web. Tato adresa URL musí být na serveru, který má povolené rozšíření serveru FrontPage. Pokud chcete pracovat s vaší místní webový server pomocí rozšíření serveru FrontPage, můžete použít možnost vzdálené lokality a zadat místní adresu URL.


![Vytvoření webu na vzdáleném serveru](improvements-in-visual-studio-2005/_static/image1.jpg)

**Obrázek 3**: vytvoření webu na vzdáleném serveru


Při vytváření aplikace ve vzdálené lokalitě přes SSL, pokud certifikát protokolu SSL se neshoduje s potvrzovací dialogové okno se mírně liší od dialogové okno zobrazí, když pomocí možnosti místní služby IIS.


![Výstraha zabezpečení vzdáleného webu](improvements-in-visual-studio-2005/_static/image3.gif)

**Obrázek 4**: výstraha zabezpečení vzdáleného webu


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 představuje možnost vytvářet weby přes protokol FTP. Když použijete tuto možnost, rozhraní IDE místně vytváří soubory ve složce temp uživatele a potom pomocí FTP přesuňte soubory do umístění serveru FTP.

> [!NOTE]
> Umístění složky temp je c:/Documents and Settings /&lt;uživatele&gt;/místní nastavení/Temp/VWDWebCache/&lt;Server&gt;/_&lt;název aplikace&gt;


Při použití možnosti FTP, zobrazí se dialogové okno Zvolit umístění. Zadejte požadované informace o připojení FTP do tohoto dialogového okna, jak je znázorněno níže.


![Zvolte umístění pro službu FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Obrázek 5**: Zvolte umístění pro službu FTP


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Testovacího prostředí: Nastavení FTP lokalit a vytvoření projektu

Následující kroky konfigurace serveru FTP tak, aby uživatel měl umístění, ke kterému pouze jejich nahrání do přes protokol FTP.

### <a name="install-the-ftp-service"></a>Služba FTP instaluje

1. Otevřete přidat nebo odebrat programy, vyberte Přidat nebo odebrat součásti Windows
2. Vyberte Internetové informační služby (aplikační Server na Windows serveru 2003) a klikněte na tlačítko **podrobnosti**.
3. Zkontrolujte **služby protokolu FTP (File Transfer)** a klikněte na tlačítko **OK**.
4. Klikněte na tlačítko **Další** k instalaci služby FTP.

### <a name="create-a-new-folder-for-content"></a>Vytvořte novou složku pro obsah

1. V Průzkumníku Windows, vytvořte novou složku s názvem **User1** uvnitř c:/inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Nakonfigurujte složek a oprávnění pro složky.

1. Otevřete modul snap-in Internetové informační služby z nástroje pro správu. Nyní máte složku servery FTP podle uzlu název počítače.
2. Rozbalte **servery FTP**.
3. Klikněte pravým tlačítkem myši **výchozí server FTP**, vyberte **nový**, pak **virtuální adresář**, klikněte na **Další**.
4. Zadejte **User1** název virtuálního adresáře a klikněte na **Další**.
5. Zadejte **c:/inetpub/wwwroot/User1** cestu a klikněte na **Další**.
6. Klikněte na tlačítko **Další** a potom **Dokončit** kroky průvodce dokončete.
7. Klikněte pravým tlačítkem myši **User1** virtuální adresář v rámci výchozí server FTP a vyberte **vlastnosti**.
8. Zkontrolujte **zápisu** zaškrtávací políčko a klikněte na tlačítko **OK** zavřete dialogové okno.
9. Klikněte pravým tlačítkem na **výchozí server FTP** a vyberte **vlastnosti**.
10. Na **účty zabezpečení** kartu, zrušte zaškrtnutí políčka **povolit anonymní připojení**.
11. Klikněte na tlačítko **Ano** v dialogovém okně s dotazem, jestli chcete pokračovat.
12. Klikněte na tlačítko **OK** zavřete dialogové okno.
13. Rozbalte **výchozí webový server** pod **weby** uzlu.
14. Klikněte pravým tlačítkem myši **User1** adresář a zaškrtnout možnost **vlastnosti**
15. V **nastavení aplikace** klikněte na tlačítko **vytvořit** k označení složce jako aplikace.
16. Klikněte na tlačítko **OK** zavřete dialogové okno.
17. Zavřete modul snap-in Internetová informační služba.

### <a name="create-web-project"></a>Vytvoření webového projektu

1. Otevřít Visual Studio 2005.
2. Z **souboru** nabídce vyberte možnost **nový web**.
3. V **umístění** rozevíracího seznamu vyberte **FTP**.
4. Klikněte na tlačítko **Procházet**.
5. Zadejte **localhost** v **Server** textového pole.
6. Zadejte **User1** v textovém poli adresáře.
7. Klikněte na tlačítko **otevřít**. Umístění FTP, zadáme do dialogového okna Nový web.
8. Klikněte na tlačítko **OK**.
9. Zrušte zaškrtnutí políčka **Ymní přihlášení** v dialogovém okně FTP přihlášení zadejte své přihlašovací údaje a klikněte na tlačítko **OK**.
10. Jaká je adresa URL projektu? (Adresa URL pro projekt se zobrazí v Průzkumníku řešení.)
11. Z **sestavení** nabídce vyberte možnost **Sestavit web** nebo **sestavit řešení**.
12. Klikněte pravým tlačítkem myši na Default.aspx v Průzkumníku řešení a vyberte **zobrazit v prohlížeči**.
13. V dialogovém okně vyžaduje adresu URL webu, zadejte `http://localhost/user1` pro adresu URL a klikněte na tlačítko **OK**.

> [!NOTE]
> Pokud se zobrazí chyba oznamující nebylo možné načíst typ /_Default, ujistěte se, že používáte technologii ASP.NET 2.0 na váš web a ne starší verze. Můžete to udělat na kartě technologie ASP.NET v Internetové informační služby.


## <a name="opening-web-projects"></a>Otevírání webových projektů

Otevírání webových projektů je podobné jako vytvoření projektů. V dalších částech upozorňujte na oblasti, jak dohlížet navýšení kapacity pro při práci v rámci rozhraní IDE. Věnuje se také práci s projekty Web pomocí protokolu HTTP a protokolu FTP.

Otevřete webový projekt, vyberte v nabídce Soubor otevřete webovou stránku. Zobrazí se výzva s stejné dialogového okna zvolte umístění popsané dříve a mají stejné čtyři dostupné možnosti: systém souborů, místní služby IIS, FTP a vzdálené lokality.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Systém souborů

Jak je uvedeno dříve v tomto modulu, Visual Studio už používá soubor projektu. Proto pokud budete chtít otevřít web ze systému souborů, ve skutečnosti máte možnost vybrat libovolnou složku, pro kterou chcete, i v případě, že složka, kterou zvolíte nebyl vytvořen jako webový projekt zpočátku v sadě Visual Studio. Například můžete také otevřít složku Dokumenty jako webový server a Visual Studio využívá elastic otevřít a zobrazit soubory, jak je znázorněno níže.


![Dokumenty otevřít webovou stránku](improvements-in-visual-studio-2005/_static/image3.jpg)

**Obrázek 6**: *dokumenty* otevřít webovou stránku


Vzhledem k tomu, že sada Visual Studio pouze vytvoří další soubory a složky, pokud je to nezbytné, žádné další soubory nebo složky přidají do umístění, které můžete otevřít. Vedlejším účinkem této architektury je, že to brání vnoření webů v systému souborů. Představte si třeba následující adresářovou strukturu.

Webový projekt na C:/MyWebSite

Jiný webový projekt na C:/MyWebSite/vnořené

Při otevření webové stránky na c:/MyWebSite vnořené složky se zobrazí jako dílčí složku této aplikace.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Při otevírání webů přes protokol HTTP, nastavení jsou přečtena z metabáze služby IIS (místní služby IIS) nebo pomocí rozšíření serveru FrontPage (vzdálené lokality.) Pokud jsou vnořené webové aplikace, se také zobrazí s ikonou je identifikuje jako aplikace. Pokud jste se seznámili s práce s webovými aplikacemi v FrontPage, se podobá chování v sadě Visual Studio 2005.

I když Visual Studio se zobrazí ikona pro aplikace, které jsou vnořené pod aplikace, která je aktuálně otevřen v integrovaném vývojovém prostředí, nebude možné rozšířit je chcete zobrazit jejich obsah. Můžete však poklikejte na je tak, aby je. Když použijete, zobrazí se dialogové okno s výzvou k buď otevřete webovou aplikaci (a nahraďte aktuálně otevřené řešení) nebo přidat webovou aplikaci s aktuálním řešením.


![Dvojitým kliknutím ikonu vnořené aplikace zobrazí toto dialogové okno](improvements-in-visual-studio-2005/_static/image4.jpg)

**Obrázek 7**: dvojitým kliknutím ikonu vnořené aplikace zobrazí toto dialogové okno


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Server FTP

Při otevření webu přes protokol FTP souborů všechny zkopírují místně do dočasné složky. Úplná cesta k umístění místního úložiště se zobrazí v podokně vlastností projektu a je vytvořen v následujícím formátu.

C:/Documents and Settings /&lt;uživatele&gt;/místní nastavení/Temp/VWDWebCache/&lt;Server&gt;/_&lt;název aplikace&gt;

Při použití protokolu FTP, Visual Studio bude třeba zadat základní adresu URL pro váš projekt tak, aby ho můžete procházet, jak je znázorněno níže. Pokud základní adresu URL nezadáte, Visual Studio se vás o ně požádáme poprvé pokusí procházet stránku na webu.


![Určení základní adresy URL pro servery FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Obrázek 8**: určení základní adresy URL pro servery FTP


## <a name="improvements-in-compilation"></a>Vylepšení v kompilaci

Práce s webovými aplikacemi v sadě Visual Studio 2005 je výrazně rychlejší než předchozí verze. Toto je kvůli žádné malou část změny v architektuře kompilace.

V sadě Visual Studio 2002 a 2003 webové aplikace byly zkompilovány do jediného primární sestavení, které se nacházejí ve složce/Bin. V sadě Visual Studio 2005 byla přidána k aplikaci/_fragmenty složce. Třídy a jiný kód bez uživatelského rozhraní se přidají do složky aplikace/_fragmenty. Když Visual Studio vytvoří projekt, všechny soubory ve složce aplikace/_fragmenty jsou kompilovány do jediného souboru App/_Code.dll. Výsledkem této změny je, že následující sestavení jsou mnohem rychlejší než v předchozích verzích.

> [!NOTE]
> Nástroj příkazového řádku MSBuild lze použít také k vytváření aplikací ASP.NET Web. Tento nástroj se budeme v modulu 9.


Další vylepšení kompilace je nová možnost sestavit stránku v nabídce sestavení. Tato funkce umožňuje vývojářům znovu sestavit pouze aktuální stránku (spolu s, kurzů a závislostí) tak, aby změny mohou být zkompilovány rychleji. Protože C# nenabízí kompilace na pozadí pro účely aktualizace IntelliSense, atd., se budou využívat nesmírně tuto funkci vzhledem k tomu, že vám umožní pro technologii IntelliSense, abychom rychle aktualizovat pomocí jednoduše znovu sestavit jednu stránku.

Vlastnosti sestavení pro projekt umožňují konfigurovat typ sestavení, který se nachází před provedením úvodní stránku. Vývojáři se můžete rozhodnout vytvářet pouze aktuální stránku tak, aby Visual Studio můžete spustit, rychlejší ladění aplikací po změně kódu.


![Úvodní stránka akce sestavení](improvements-in-visual-studio-2005/_static/image6.jpg)

**Obrázek 9**: úvodní stránka akce sestavení


Další skvělou vylepšení sady Visual Studio a architektura ASP.NET je v oblasti upravit a pokračovat. V sadě Visual Studio 2005 můžou vývojáři spuštění ladění projektu a provádět změny kódu v projektu bez odpojuje ladicí program. Ve skutečnosti můžete doslova spuštění ladění projektu, přidejte novou třídu, přidejte kód do třídy, přidejte kód na stránku, která vytvoří novou instanci této třídy a provedení metody třídy, aniž by odpojuje ladicí program. Spouštění nový kód je doslova stejně jednoduše jako si aktualizaci prohlížeče!

Kliknutím zde můžete zobrazit na video s návodem úpravy a pokračovat funkce v sadě Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Otevřít Video na celou obrazovku](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


Robustní upravit a pokračovat funkce v technologii ASP.NET 2.0 a Visual Studio 2005 je z důvodu architektury změny pro aplikace ASP.NET. V technologii ASP.NET 1.x, aplikace vytvořené v aplikaci Visual Studio 2002/2003 byly zkompilovány do primární sestavení, která byla uložená ve složce/Bin. Všechny třídy, stránkách atd pro aplikace, které byly zkompilovány do tohoto jednu knihovnu DLL. Pak za běhu, technologie ASP.NET by všechny ovládací prvky, značek a kódu ASP.NET v rámci stránky zkompilovat a zkopírování těchto knihoven DLL do dočasné složky ASP.NET.

V sadě Visual Studio 2005 pomocí technologie ASP.NET 2.0, modely dvě kompilace popisují výše (jeden pro Visual Studio) a jeden pro technologii ASP.NET v době běhu byly sloučeny do jednoho modelu common kompilace. To znamená, že všechny problémy při kompilaci jsou nyní zachycena ve fázi vývoje místo za běhu. Umožňuje také pro návrháře a podporu technologie IntelliSense pro funkce, jako je uživatelské ovládací prvky a hlavní stránky.

Kliknutím sem zobrazíte na video s návodem návrhářské podpory pro uživatelské ovládací prvky.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Otevřít Video na celou obrazovku](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Pokud uživatelský ovládací prvek se odebere ze stránky, @Register – direktiva zůstává v kódu a měly by se odebrat ručně vyhnout chyby analyzátoru, pokud uživatelský ovládací prvek je odstraněn z webu.


Další vylepšení v modelu kompilace Visual Studio je funkce Publikovat Web. Protože předkompiluje funkci Publikovat na webu, vývojáři mohli využít přidání výkonu bez nutnosti kompilace nic na vyžádání. Také předkompiluje s veškerým zdrojovým kódem ve složce aplikace/_fragmenty do knihovny DLL tak, aby žádný zdrojový kód je nutné nasadit.


![Dialogové okno publikování webu](improvements-in-visual-studio-2005/_static/image7.jpg)

**Obrázek 10**: dialogové okno publikování webu


> [!NOTE]
> Nástroj aspnet/_compile.exe lze také provést předkompilaci webovou aplikaci ASP.NET. Tento nástroj se budeme v modulu 9.


Když publikujete webovou stránku, Předkompilované soubory jsou uloženy ve složce dočasných souborů ASP.NET jak je znázorněno níže. Soubory s *Compiled* přípona souboru jsou soubory formátu XML, které definují závislosti pro konkrétní knihovny DLL. Všechny ovládací prvky webového formuláře nebo uživatele jsou kompilovány do náhodných knihovny DLL, které začínají *aplikace /_Web /_*.

Necháte-li *povolit tomuto předkompilovanému webu umožnit aktualizaci modelové* zaškrtávací políčko zaškrtnuto, kód uvnitř své ovládací prvky webových formulářů a uživatel nebude předem kompilovaných do knihovny DLL, umožní vám provádět změny po nasazení. Pokud chcete zamezit kód tak, aby nejsou povoleny změny nasazeným obsahem, zrušte zaškrtnutí tohoto políčka.

*Použít pevné nování a jednostránkové sestavení* checkbox umožňuje, aby každá stránka je zkompilovat do sestavení s názvem oprava vypnutí dávkové kompilace. Opuštění toto políčko není zaškrtnuto, můžete využít výhod dávkové kompilace.

*Povolení silné názvy předkompilovaných sestavení* zaškrtávací políčko umožní se silným názvem předkompilovaných sestavení.

> [!NOTE]
> V technologii ASP.NET 1.x, sestavení se silným názvem museli nainstalovat do globální mezipaměti sestavení (GAC). V technologii ASP.NET 2.0 můžete nejsou nutné pro instalaci sestavení se silným názvem do mezipaměti GAC.


![Předem kompilovaných souborů aplikace ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Obrázek 11**: předem kompilovaných souborů aplikace ASP.NET


> [!NOTE]
> Ve výše uvedené aplikace byla žádný soubor web.config. Pokud by došlo, je by byly volány *PrecompiledApp.config* po publikování webu serveru procesu.


## <a name="improvements-in-deployment"></a>Vylepšení v nasazení

Jako s Visual Studio 2002 a 2003, nabízí Visual Studio 2005 funkce Kopírovat projekt. Ale tato funkce se se zvýšenou úložnou v sadě Visual Studio 2005 a se teď nazývá Kopírovat web.

Dialogové okno Kopírovat web je rozdělený do blok levé a pravé rámce. Levý rámec se nazývá zdrojového webu a správné rámce je na vzdáleném webu. Jednou z věcí, které by mohly uvádět někteří vývojáři je, že lokality zobrazí v pravém rámečku není nutně vzdálené lokality. Může to být web v místním systému souborů nebo v místní instanci služby IIS. Kromě toho web zobrazí v levém podokně není nutně zdrojového webu protože dialogového okna můžete publikovat ze vzdáleného webu *k* zdrojového webu.

Pokud kopírujete projekt na vzdálený web, této lokality musí mít na něj nainstalovat rozšíření serveru FrontPage. Pokud tomu tak není, musíte připojit pomocí protokolu FTP. Na druhé straně Pokud kopírujete projekt k místní instanci služby IIS, rozšíření serveru FrontPage nejsou nutné.

> [!NOTE]
> Pokud se pokusíte vytvořit novou webovou stránku v místní instanci služby IIS a jsou nainstalovaná rozšíření FrontPage 2002 Server Extensions, zobrazí se chybová zpráva s oznámením, že se na Sharepointovém serveru nepodporuje vytváření webů. V takovém případě máte možnost instalace rozšíření serveru FrontPage 2000 nebo odebírání rozšíření serveru FrontPage.


Video návod k funkci kopírování webu, klikněte sem.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Otevřít Video na celou obrazovku](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Vylepšení ladění

Existují čtyři klíčových vylepšení při ladění v sadě Visual Studio 2005.

- Ladění místně jako bez oprávnění správce je možné úprav.
- Atribut ladění pro Compilation element je teď ve výchozím nastavení hodnotu false.
- Vzdálené ladění, nastavení a konfigurace je jednodušší než dřív.
- Teď můžete ladit webovou stránku otevřít pomocí umístění služby FTP.

## <a name="debugging-as-a-non-administrator"></a>Ladění jako bez oprávnění správce

Přidání serveru ASP.NET Development Server umožňuje nejsou správci snadno ladit aplikace ASP.NET předem připravené. Při ladění aplikace ASP.NET běžící v místním systému souborů sady Visual Studio spustí vývojový Server ASP.NET v kontextu přihlášeného uživatele. Tento uživatel potom můžete ladit tuto aplikaci bez další konfigurace.

## <a name="debug-is-false-by-default"></a>Ladění je ve výchozím nastavení

V technologii ASP.NET 1.x, *ladění* atribut *kompilace* element v souboru web.config byl nastaven na hodnotu *true* ve výchozím nastavení. Se vždy doporučuje, aby vývojáři tento atribut nastavte na *false* před nasazením aplikace do produkčního prostředí, ale protože většina vývojáři plně nerozumí důsledků byste museli opustit ladicí atribut nastavený na Hodnota TRUE, se jednoduše nacházela jako-je.

Nejzávažnější problém mít atribut ladění je nastavenou na hodnotu true, zakáže modelu ASP.NETs dávkové kompilace. Proto je každé stránky kompilovány do samostatné knihovny DLL. Pokud se k webu, které aplikace se skládá z tisíců stránky (ne mám jsou jakýmikoli prostředky), který znamená, že několik tisíc malé knihoven DLL, vytvoří se v příslušné aplikaci. I když tyto knihovny jsou malé velikosti, nejsou načtena do libovolné konkrétní místo v paměti. Proto způsobit fragmentaci v systémové paměti a může přispět k OutOfMemoryException výskytů.

V technologii ASP.NET 2.0 atribut ladění ve výchozím nastavení na hodnotu false. Jak jste už viděli, kdy vývojář ladí aplikaci technologie ASP.NET v sadě Visual Studio 2005, zobrazí se výzva k přidání souboru web.config s povoleným laděním. To s sebou nese náklady stejné nevýhody, které byly k dispozici v technologii ASP.NET 1.x, ale teď vývojář je zřetelně zobrazí upozornění, že atribut by se měla obnovit na hodnotu false před přesunutím aplikaci do produkčního prostředí.

## <a name="remote-debugging-setup-and-configuration"></a>Nastavení vzdáleného ladění a konfigurace

Vzdálené ladění ve Visual Studio 2002/2003, spoléhal na počítač ladění správce (mdm.exe) a proces vs7jit.exe. Proto vzdáleného ladění potíží často byl černé políčko pro zákazníky a nebylo často mnohem vyšší u služby PSS.

Visual Studio 2005 odebere závislost na mdm.exe a vs7jit.exe procesy. Místo toho teď používá službu sledování vzdáleného ladění (msvsmon.exe).

Požadavek na vzdáleného ladění sady Visual Studio 2005 je poměrně jednoduché. Potřebujete ke spuštění msvsmon.exe na vzdáleném serveru před ladění. Sledování vzdáleného ladění můžete nainstalovat z disku CD Visual Studio nebo můžete jednoduše spustit msvsmon.exe ze sdílené složky bez nutnosti instalace nic vůbec na webovém serveru.

Při spuštění msvsmon.exe, je pravděpodobné, že bude stěžovat porty blokuje vzdálené ladění. Naštěstí můžete snadno odblokovat porty přímo v dialogovém okně upozornění jak je znázorněno níže.


![Oznámení, že je brána Windows Firewall blokuje vzdálené ladění](improvements-in-visual-studio-2005/_static/image9.jpg)

**Obrázek 12**: je oznámení, aby brána Windows Firewall blokuje vzdálené ladění


Jakmile máte odblokováno porty potřebné pro ladění, zobrazí se sledování vzdáleného ladění, jak je znázorněno níže. Z tohoto rozhraní můžete monitorovat připojení a ladění oprávnění snadno změnit.


![Sledování vzdáleného ladění](improvements-in-visual-studio-2005/_static/image10.jpg)

**Obrázek 13**: sledování vzdáleného ladění


Je také možné vzdáleně ladit webové aplikace otevřít přes protokol FTP. Kroky jsou stejné jako ty, které dříve uvedené. Musíte ale zadat základní adresu URL pro procházení FTP projektu, jak je uvedeno výše v tomto modulu.

## <a name="lab-2"></a>Testovací prostředí 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Vzdálené ladění pomocí Visual Studio 2005

Toto testovací prostředí vás provede procesem vzdálené ladění pomocí Visual Studio 2005.

Video návod tohoto testovacího prostředí, klikněte sem.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Otevřít Video na celou obrazovku](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Toto testovací prostředí je potřeba mít dva počítače, jeden spuštěné sadě Visual Studio 2005 a ostatní spuštěné služby IIS 5 nebo novější.

1. Otevřít Visual Studio 2005 a vytvořit nový web na vzdáleném serveru.

> [!NOTE]
> Na webu můžete vytvořit ve vzdálené instanci služby IIS nebo přes protokol FTP.


1. Ze vzdáleného webového serveru vyhledejte msvsmon.exe na vývojovém počítači pomocí cesty UNC a spustit ho.  
 Výchozí umístění pro msvsmon.exe je //server/c$/Program soubory nebo Microsoft Visual Studio 8/Common7/IDE/vzdáleného ladicího programu/x86.
2. Pokud se zobrazí výzva k odblokování porty pro vzdálené ladění, udělejte to.
3. Z vývojového počítače otevřete kódu pro Default.aspx a nastavte zarážku v metodě stránky/_služba.
4. Spusťte ladění z vývojového počítače.

By měl dostanete k zarážce podle očekávání.

## <a name="aspnet-development-server"></a>Vývojový server ASP.NET

Jako weve již probírali Visual Studio 2005 se dodává s názvem serveru ASP.NET Development Server webového serveru. (Vývojový Server ASP.NET se někdy označuje jako Cassini.) Tento webový server je pohodlný způsob procházení a ladění webových aplikací spuštěných v systému souborů.

Serveru ASP.NET Development Server je webový server s omezeným přístupem. Nepovoluje vzdálená připojení, neumožňuje všechny žádosti od libovolného uživatele, než je uživatel, který spustil webový server. Také nemá schopnost obsluhovat stránky ASP. Pouze technologie ASP.NET a prostředků ve formátu HTML (včetně obrázků, souborů CSS, atd.) se obsluhují.

Serveru ASP.NET Development Server můžete spustit pomocí příkazového řádku spuštěním WebDev.WebServer.exe souboru v umístění c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/*. Zobrazí se následující dialogové okno parametry, které jsou k dispozici.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Obrázek 14**


> [!NOTE]
> Vývojový Server ASP.NET se nepodporuje při spuštění explicitně pomocí příkazového řádku.
