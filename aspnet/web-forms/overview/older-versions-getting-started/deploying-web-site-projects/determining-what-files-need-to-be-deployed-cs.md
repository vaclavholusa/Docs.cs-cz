---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Zjištění, jaké soubory musí mít nasadit (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Soubory musí být nasazeny z vývojového prostředí do produkčního prostředí zčásti závisí na aplikaci ASP.NET určuje, zda byla vytvořena nám...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: 750e2e19fdaaf2b11304b2227e7c582668d1a567
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363070"
---
<a name="determining-what-files-need-to-be-deployed-c"></a>Zjištění, jaké soubory musí mít nasadit (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Soubory musí být nasazeny z vývojového prostředí do produkčního prostředí závisí částečně na tom, jestli aplikace ASP.NET bylo vytvořeno prostřednictvím webu Model nebo Model webových aplikací. Další informace o těchto dvou projektu modelů a vliv nasazení modelu projektu.


## <a name="introduction"></a>Úvod

Nasazení webové aplikace ASP.NET zahrnuje kopírování souborů souvisejících s ASP.NET z vývojového prostředí do produkčního prostředí. Soubory související s ASP.NET obsahují kódu webové stránky ASP.NET a kódu a na straně klienta a serveru podpůrných souborů. Podpora na straně klienta jsou soubory odkazuje webové stránky a odeslat přímo do prohlížeče – obrázky, šablony stylů CSS soubory a soubory jazyka JavaScript, třeba. Soubory podpory na straně serveru zahrnují ty, které se používají ke zpracování požadavku na straně serveru. K souborům SQL, mimo jiné to zahrnuje konfiguračních souborů, webové služby, soubory tříd, typované datové sady a LINQ.

Obecně platí všechny na straně klienta podpůrné soubory mají být zkopírovány z vývojového prostředí do produkčního prostředí, ale se zkopíruje soubory na straně serveru podporu závisí na tom, jestli jsou explicitně kompilování kódu na straně serveru do sestavení ( `.dll` soubor) nebo pokud máte tato sestavení automaticky generovány. Tento kurz ukazuje, co soubory musí být nasazen při explicitně kompilování kódu do sestavení a s této kompilační krok automaticky provedou.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Explicitní kompilace a automatickou kompilaci

Rozhraní ASP.NET web pages jsou rozdělené do deklarativní kód a zdrojový kód. Část deklarativní zahrnuje HTML, webové ovládací prvky a datové vazby syntaxe; část kódu obsahuje obslužné rutiny událostí, které jsou napsané v kódu jazyka Visual Basic nebo C#. Části značek a kódu jsou obvykle rozdělena do různých souborů: `WebPage.aspx` obsahuje deklarativní při `WebPage.aspx.cs` jsou uloženy kódu.

Vezměte v úvahu stránku ASP.NET s názvem Clock.aspx, který obsahuje ovládací prvek popisku, jejichž Text je nastavena na aktuální datum a čas načtení stránky. Deklarativní část (v `Clock.aspx`) bude obsahovat značky pro ovládací prvek popisek webové –`<asp:Label runat="server" id="TimeLabel" />` – při část kódu (v `Clock.aspx.cs`) by měla `Page_Load` obslužnou rutinu události s následujícím kódem:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Aby modul ASP.NET ke zpracování žádosti pro tuto stránku, na stránce část kódu ( `WebPage.aspx.cs` souboru) musí být nejprve zkompilovány. Tato kompilace může dojít, explicitně nebo automaticky.

Pokud explicitně se stane kompilace, pak bude celá aplikace zdrojový kód je zkompilován do jednoho nebo více sestavení (`.dll` soubory) umístěný ve vaší aplikace `Bin` adresáře. Pokud kompilace probíhá automaticky a výsledné automaticky generované sestavení je ve výchozím nastavení, které jsou umístěny do `Temporary ASP.NET` soubory složky, kterou lze nalézt v `%WINDOWS%\Microsoft.NET\Framework\`  *&lt;verze&gt;*, i když toto umístění se dají konfigurovat přes [ `<compilation>` element](https://msdn.microsoft.com/library/s10awwz0.aspx) v `Web.config`. S explicitní kompilace je nutné provést některé akce pro kompilaci kódu vaší aplikace ASP.NET do sestavení a k tomuto kroku dojde před jejich nasazením. S automatickou kompilaci vyvolá proces kompilace na webovém serveru se při prvním přístupu k prostředku.

Bez ohledu na to, jaký model kompilace použijete, značky části všech stránek ASP.NET ( `WebPage.aspx` souborů) je nutné zkopírovat do produkčního prostředí. S explicitní kompilace, je nutné zkopírovat do sestavení v `Bin` složka, ale není potřeba zkopírovat do části kódu na stránkách ASP.NET ( `WebPage.aspx.cs` soubory). S automatickou kompilaci musíte zkopírovat soubory část kódu tak, že kód je k dispozici a můžete zkompilovat automaticky, když uživatel na stránce. Obsahuje části kódu každé webové stránky ASP.NET `@Page` s atributy, které označují, jestli už explicitně cfw přidružený kód na stránce nebo zda potřebuje automaticky zkompilovat. V důsledku toho produkčním prostředí můžete bez problému fungují s modelem buď kompilace a není potřeba použít nějaká speciální nastavení k označení, že se používá explicitní nebo automatickou kompilaci.

1 tabulka shrnuje různé soubory k nasazení při použití explicitní kompilace a automatickou kompilaci. Poznámka: bez ohledu na to kompilace model používá jste měli vždycky nasazovat v sestavení `Bin` složky, pokud tato složka existuje. `Bin` Složka obsahuje sestavení, specifické pro webovou aplikaci, mezi které patří kompilované zdrojového kódu při použití modelu explicitní kompilace. `Bin` Adresář obsahuje také sestavení z jiných projektů a open source nebo třetích stran sestavení, která se může používat, a tyto názvy musí být na provozním serveru. Kopírovat jako obecné pravidlo, proto `Bin` složky až do produkčního prostředí až po nasazení. (Pokud používáte automatickou kompilaci modelu a nepoužívají žádné externí sestavení, pak nebudete mít `Bin` adresář –, který je v pořádku!)

| **Model kompilace** | **Nasadit soubor část značky?** | **Nasadit soubor zdrojového kódu?** | **Nasazení sestavení v `Bin` adresáře?** |
| --- | --- | --- | --- |
| Explicitní kompilace | Ano | Ne | Ano |
| Automatickou kompilaci | Ano | Ano | Ano (pokud existuje) |

**Tabulka 1:** soubory nasazení závisí na modelu kompilace používá.

## <a name="taking-a-trip-down-memory-lane"></a>Pořízení výlet dráhou paměti

Jaké kompilace přístup se používá závisí, v části Jak se spravuje aplikace ASP.NET v sadě Visual Studio. Od verze. NET od vzniku v roce 2000 byly čtyři různé verze sady Visual Studio – Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 a Visual Studio 2008. Visual Studio .NET 2002 a 2003 spravované aplikace ASP.NET využívající *modelu projektu webové aplikace*. Jsou klíčové funkce modelu projektu webové aplikace:

- Soubory, strukturu projektu jsou definovány v souboru jednoho projektu. Všechny soubory není definován v souboru projektu nejsou považovány za součást webové aplikace pomocí sady Visual Studio.
- Používá explicitní kompilace. Sestavení projektu zkompiluje soubory kódu v rámci projektu do jednoho sestavení, který je umístěn ve `Bin` složky.

Společnost Microsoft vydala Visual Studio 2005 vyřadit podpora modelu projektu webové aplikace a nahradí modelem projektu webové stránky. Model projektu webu samotné rozlišené z modelu projektu webové aplikace následujícími způsoby:

- Místo jediného souboru projektu, který obsahuje soubory projektu, se místo toho používá systém souborů. Stručně řečeno všechny soubory ve složce webové aplikace (nebo podsložky) jsou považovány za součást projektu.
- Sestavení projektu v sadě Visual Studio neslouží k vytvoření sestavení `Bin` adresáře. Místo toho vytváření webového projektu hlásí chyby kompilace.
- Podpora pro automatickou kompilaci. Webové projekty jsou obvykle implementovány zkopírováním kód a zdrojový kód do produkčního prostředí, i když kód může být předkompilované (explicitní kompilace).

Microsoft obnovená modelu projektu webové aplikace po vydání Visual Studio 2005 Service Pack 1. Visual Web Developer však nadále podporují pouze model projektu webové stránky. Dobrou zprávou je, že bylo toto omezení vyřadit Visual Web Developer 2008 Service Pack 1. Dnes můžete vytvořit aplikace ASP.NET v sadě Visual Studio (a Visual Web Developer) pomocí modelu projektu webové aplikace nebo model projektu webové stránky. Oba modely mají své výhody a nevýhody. Odkazovat na [Úvod do projektů webových aplikací: porovnání webové projekty a projekty webových aplikací](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) porovnání obou modelů a vám pomůže rozhodnout, jaký model projektu je nejvhodnější pro vaše konkrétní potřeby.

## <a name="exploring-the-sample-web-application"></a>Zkoumání ukázkové webové aplikace

Soubor ke stažení pro účely tohoto kurzu obsahuje aplikaci ASP.NET recenzí. Na webu napodobuje koníčku webu někdo může vytvořit pro sdílení jejich knihy revizí pomocí online komunita. Tato webová aplikace ASP.NET je velmi jednoduché a se skládá z následujících prostředků:

- `Web.config`, konfigurační soubor aplikace.
- Stránky předlohy (`Site.master`).
- Sedm různých stránkách ASP.NET: 

    - ~`/Default.aspx`-domovské stránky tohoto webu.
    - ~`/About.aspx` – na stránce "O lokality".
    - ~`/Fiction/Default.aspx` -stránku se seznamem fiction knih, které byly zkontrolovány. 

        - ~`/Fiction/Blaze.aspx` -Kontrola nové Richard Bachman *Blaze*.
    - ~/`Tech/Default.aspx` -stránku se seznamem knih technologie, které byly zkontrolovány. 

        - ~/`Tech/CYOW.aspx`-kontrolu *vytvořit svůj vlastní web*.
        - ~/`Tech/TYASP35.aspx` -kontrolu *naučit sami technologie ASP.NET 3.5 za 24 hodin*.
- Tři různé šablony stylů CSS soubory ve složce styly.
- Čtyři soubory obrázků - používá technologii ASP.NET logo a obrázky na pozadí tři knihy přezkoumání – všechny umístěny v `Images` složky.
- A `Web.sitemap` soubor, který definuje mapu webu a slouží k zobrazení v nabídkách `Default.aspx` stránky v kořenovém adresáři a `Fiction` a `Tech` složek.
- Soubor třídy s názvem `BasePage.cs` definující základní `Page` třídy. Tato třída rozšiřuje funkce `Page` třídy automaticky nastavením `Title` vlastnost založeny na umístění na stránce v mapy webu. Řečeno v kostce, jsou všechny třídy modelu code-behind technologie ASP.NET, která rozšiřuje `BasePage` (místo `System.Web.UI.Page`) mají záhlaví nastavit na hodnotu v závislosti na jeho pozice v mapě webu. Například při prohlížení ~ /`Tech/CYOW.aspx` název stránky, je nastavena na "Domů: technologie: vytvoření vlastního webu".

Obrázek 1 ukazuje snímek obrazovky webu recenzí při prohlížení prostřednictvím prohlížeče. Tady se zobrazí stránka ~ /`Tech/TYASP35.aspx`, který zkontroluje knihu *naučit sami technologie ASP.NET 3.5 za 24 hodin*. Tento navigační prvek určuje, která zahrnuje horní části stránky a v nabídce v levém sloupci jsou založeny na strukturu mapy webu definované v `Web.sitemap`. Obrázek v pravém horním rohu je jedním z knihy titulní Image nachází v `Images` složky. Na webu vzhled a chování jsou definovány pomocí kaskádových pravidla stylů států šablon stylů CSS soubory ve složce styly, zatímco zastřešujícího rozložení stránky je definován na hlavní stránce `Site.master`.


[![Na webu knihy kontroly nabízí recenzí na celé řady různých doprovodných produktů](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Obrázek 1:** web knihy kontroly nabízí recenzí na celé řady různých doprovodných názvy ([kliknutím ji zobrazíte obrázek v plné velikosti](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


Tuto aplikaci nebude používat databázi. každou recenzi je implementovaný jako samostatné webové stránky v aplikaci. V tomto kurzu (a další kurzy několik) provede procesem nasazení webové aplikace, která nemá žádné databáze. Ale v budoucích kurzech jsme se zlepšila tuto aplikaci k uložení recenze, čtečky komentáře a další informace v databázi a bude prozkoumejte, jaké kroky je potřeba se provádí, aby správně nasazení s daty webové aplikace.

> [!NOTE]
> Tyto kurzy zaměřit na hostování aplikací ASP.NET u poskytovatele webového hostitele a není prozkoumat pomocné zajímavá témata zahrnují třeba ASP. NET pro systém lokality mapy nebo pomocí základní `Page` třídy. Další informace o těchto technologií a další informace o dalších tématech zahrnuté v rámci tohoto kurzu najdete na konci každého kurzu v části Další čtení.


V tomto kurzu ke stažení obsahuje dvě kopie webovou aplikaci, každý implementovaná jako jiný typ projektu sady Visual Studio: BookReviewsWAP, projekt webové aplikace a BookReviewsWSP webového projektu. Oba projekty vytvořené pomocí aplikace Visual Web Developer 2008 SP1 a pomocí technologie ASP.NET 3.5 SP1. Pro práci s projekty začněte rozzipovávání obsah na plochu. Otevřete projekt webové aplikace (BookReviewsWAP), přejděte do složky BookReviewsWAP a poklikejte na soubor řešení `BookReviewsWAP.sln`. K otevření projektu webové stránky (BookReviewsWSP), spusťte sadu Visual Studio a potom z nabídky soubor, zvolte možnost Otevřít web, vyhledejte `BookReviewsWSP` složky v počítači a klikněte na tlačítko OK.


Zbývající dva oddíly v tomto kurzu pohled na soubory budete muset zkopírovat do produkčního prostředí při nasazování aplikace. Následující dva kurzy - *[nasazení vašeho webu pomocí protokolu FTP](deploying-your-site-using-an-ftp-client-cs.md)* a *[nasazení webu pomocí sady Visual Studio](deploying-your-site-using-visual-studio-cs.md)* -ukazují různé způsoby, jak Zkopírujte tyto soubory zprostředkovatele webového hostitele.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Určení souborů k nasazení pro projekt webové aplikace

Model projektu webové aplikace používá explicitní kompilace – projektu zdrojový kód je zkompilován do jednoho sestavení při každém sestavení aplikace. Tato kompilace obsahuje soubory kódu na pozadí na stránkách ASP.NET (~ /`Default.aspx.cs`, ~ /`About.aspx.cs`, a tak dále), stejně jako `BasePage.cs` třídy. Výsledné sestavení jmenuje BookReviewsWAP.dll a nachází se ve vaší aplikaci `Bin` adresáře.

Obrázek 2 ukazuje soubory, které tvoří knihy revize webové aplikace.


[![V Průzkumníku řešení zobrazí soubory, které tvoří projektu webové aplikace](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Obrázek 2**: Průzkumníku řešení zobrazí soubory, které tvoří projektu webové aplikace


K nasazení aplikace ASP.NET vytvořené pomocí modelu počáteční projekt webové aplikace vytvořením aplikace tak, aby explicitně nejnovější zdrojový kód zkompilovat do sestavení. V dalším kroku zkopírujte následující soubory do produkčního prostředí:

- Soubory, které obsahují deklarativní pro každý ASP.NET stránky, například ~ /`Default.aspx`, ~ /`About.aspx`, a tak dále. Zkopírujte také nahoru deklarativní pro všechny hlavní stránky a uživatelské ovládací prvky.
- Sestavení (`.dll` souborů) v `Bin` složky. Není potřeba zkopírovat soubory databáze programu (`.pdb`) ani žádné soubory XML, najdete v `Bin` adresáře.

Kopírování souborů se zdrojovým kódem na stránkách ASP.NET k produkčnímu prostředí nepotřebujete, ani je potřeba zkopírovat `BasePage.cs` souboru třídy.

> [!NOTE]
> Obrázek 2 ukazuje, `BasePage` třídy je implementovaný jako soubor třídy v projektu umístěn ve složce s názvem `HelperClasses`. Pokud je projekt kompilován kód v `BasePage.cs` soubor je zkompilován spolu s stránek technologie ASP.NET použití modelu code-behind třídy do jednoho sestavení `BookReviewsWAP.dll.` ASP.NET má zvláštní složku s názvem `App_Code` , který slouží k uložení souborů třídy pro Web Projekty webů. Kód v `App_Code` složku automaticky kompilaci a proto by neměl být použit s projekty webových aplikací. Místo toho byste měli umístit soubory třídy vaší aplikace do normální složku s názvem `HelperClasses`, nebo `Classes`, nebo něco podobného. Alternativně můžete umístit soubory tříd v samostatném projektu knihovny tříd.


Kromě zkopírování soubory související s ASP.NET revize a sestavení v `Bin` složky, musíte také kopírovat soubory podpory na straně klienta – na obrázky a soubory šablon stylů CSS – stejně jako ostatní soubory podpory na straně serveru, `Web.config` a `Web.sitemap`. Tyto klientské - a -podporu na straně serveru soubory je třeba zkopírovat do produkčního prostředí bez ohledu na to, jestli použít explicitní nebo automatickou kompilaci.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Určení souborů k nasazení pro souborů webových projektů

Model projektu webu podporuje automatické kompilaci, funkce není k dispozici při použití modelu projektu webové aplikace. Explicitní kompilace musí váš projekt zdrojový kód zkompilovat do sestavení a zkopírujte sestavení do produkčního prostředí. Na druhé straně s automatickou kompilaci můžete jednoduše zkopírovat zdrojový kód do produkčního prostředí a je zkompilován s modul runtime na požádání, podle potřeby.

Možnost nabídky sestavení v sadě Visual Studio je k dispozici v projektech webových aplikací a webových projektů. Vytváření projektů webových aplikací do jednoho sestavení v zkompiluje zdrojový kód projektu `Bin` directory; vytváření webového projektu zkontroluje chyby kompilace, ale nevytvoří žádná sestavení. Chcete-li nasadit aplikaci ASP.NET s vyvinutý pomocí všech webový projekt modelu je potřeba je kopie příslušné soubory do produkčního prostředí, ale by neváhejte na prvním sestavení projektu zajistit, že zde nejsou žádné chyby kompilace.

Obrázek 3 ukazuje soubory, které tvoří knihy revize webový projekt.


 [![V Průzkumníku řešení zobrazí soubory, které tvoří webového projektu](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Obrázek 3**: Průzkumníku řešení zobrazí soubory, které tvoří webového projektu


Nasazení webového projektu zahrnuje kopírování všech souborů souvisejících s ASP.NET do produkčního prostředí –, který obsahuje značky stránky pro stránky ASP.NET, hlavní stránky a uživatelské ovládací prvky, spolu s jejich soubory kódu. Také musíte zkopírovat všechny třídy soubory, jako je například BasePage.cs. Všimněte si, že `BasePage.cs` soubor se nachází v `App_Code` složce, která je speciální složky ASP.NET používá v projektech webu pro soubory tříd. Speciální složky je potřeba vytvořit v produkčním prostředí, stejně, jako soubory tříd v `App_Code` složky ve vývojovém prostředí musí být zkopírován do `App_Code` složky v produkčním prostředí.

Kromě zkopírování souborů ASP.NET značkami a zdrojový kód, musíte také kopírovat soubory podpory na straně klienta – na obrázky a soubory šablon stylů CSS – stejně jako ostatní soubory podpory na straně serveru, `Web.config` a `Web.sitemap`.

> [!NOTE]
> Webové projekty lze také použít explicitní kompilace. Budoucí kurz se zaměřuje jak explicitně zkompilovat projekt webu.


## <a name="summary"></a>Souhrn

Nasazení aplikace ASP.NET zahrnuje kopírování potřebné soubory z vývojového prostředí do produkčního prostředí. Přesné sadu souborů, které je třeba synchronizovat, závisí na tom, jestli aplikace ASP.NET explicitně nebo automaticky kompilace kódu. Strategie kompilaci použijí ovlivňuje, jestli je nakonfigurovaná sady Visual Studio ke správě aplikace ASP.NET pomocí modelu projektu webové aplikace nebo modelu projektu webové stránky.

Model projektu webové aplikace používá explicitní kompilace a kompiluje do jednoho sestavení v projektu kódu `Bin` složky. Při nasazování aplikace, část značek stránek ASP.NET a obsah `Bin` složka musí doručit do produkčního prostředí; zdrojový kód v aplikaci – soubory kódu a použití modelu code-behind třídy, například – nepotřebují. které se mají zkopírovat do produkčního prostředí.

Webový projekt modelu používá automatické kompilaci ve výchozím nastavení, i když je možné explicitní kompilace webového projektu, jak vidíte v budoucích kurzech. Nasazení aplikace ASP.NET, který používá automatické kompilaci, která vyžaduje část značky *a* zdrojového kódu musí být zkopírovány do produkčního prostředí. Kód je zkompilován automaticky v produkčním prostředí, pokud se požaduje poprvé.

Teď, když budeme zkoumat, co soubory musí být synchronizovány mezi vývojovou a provozní prostředí jsme připraveni k nasazení aplikace recenzí na zprostředkovateli webového hostitele.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Kompilace ASP.NET: Přehled](https://msdn.microsoft.com/library/ms178466.aspx)
- [Uživatelské ovládací prvky ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Zkoumání ASP. Navigace na webu společnosti NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Úvod do projektů webových aplikací](https://msdn.microsoft.com/library/aa730880.aspx)
- [Hlavní stránka kurzy](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Sdílení kódu mezi stránkami](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Použití vlastní základní třída pro třídy modelu Code-Behind stránek ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Systém projektu webu Visual Studio 2005: co je a proč jsme to udělali?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Postupy: Převod webového projektu do projektu webové aplikace v sadě Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Předchozí](asp-net-hosting-options-cs.md)
> [další](deploying-your-site-using-an-ftp-client-cs.md)
