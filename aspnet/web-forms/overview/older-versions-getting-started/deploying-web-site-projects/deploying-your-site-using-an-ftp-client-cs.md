---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: Nasazení webu pomocí klienta FTP (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Nejjednodušší způsob, jak nasadit aplikaci ASP.NET je ručně zkopírovat potřebné soubory z vývojového prostředí do produkčního prostředí. Tent...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: 1fa0c72beb18ceabefeae41bec64dda036372d79
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385324"
---
<a name="deploying-your-site-using-an-ftp-client-c"></a>Nasazení webu pomocí klienta FTP (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> Nejjednodušší způsob, jak nasadit aplikaci ASP.NET je ručně zkopírovat potřebné soubory z vývojového prostředí do produkčního prostředí. Tento kurz ukazuje, jak použít klienta k získání souborů z plochy ke zprostředkovateli webového hostitele.


## <a name="introduction"></a>Úvod

Předchozí kurz o službě zavedená jednoduchou knihy revize webovou aplikaci ASP.NET, která se skládá z několika stránek ASP.NET, hlavní stránky, vlastní základní `Page` třídy, počet imagí, a tři šablony stylů CSS stylů. Nyní jsme připraveni nasadit tuto aplikaci na web hostitele zprostředkovatele, v tomto okamžiku bude aplikace přístupné všem uživatelům s připojením k Internetu!


Z našich diskuzích v [ *určující, co soubory musí být nasazeny* ](determining-what-files-need-to-be-deployed-cs.md) výukový program, budeme vědět, co soubory musí být zkopírován do hostitele poskytovatele webových. (Si možná Vzpomínáte, jaké soubory se zkopírují závisí na, jestli vaše aplikace je explicitně nebo automaticky kompilován.) Ale jak jsme získat soubory z vývojového prostředí (naše desktopové verze) až do produkčního prostředí (webový server spravované poskytovatelem webového hostitele)? [ **F** ile **T** transferu **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) je běžně používaný protokol pro kopírování souborů z jednoho počítače do jiného přes síť. Další možností je rozšíření serveru FrontPage (FPSE). Tento kurz se zaměřuje na pomocí samostatné FTP klientský software nasadit do produkčního prostředí potřebné soubory z vývojového prostředí.

> [!NOTE]
> Visual Studio obsahuje nástroje pro publikování webů přes protokol FTP; Tyto nástroje také podívat na nástroje, které používají FPSE, jsou popsané v dalším kurzu.


Kopírování souborů přes FTP potřebujeme *klienta FTP* ve vývojovém prostředí. Klient FTP je aplikace, která slouží ke kopírování souborů z počítače, je nainstalovaný na počítači, na kterém běží *FTP server*. (Pokud poskytovatel webového hostitele podporuje přenosy souborů přes FTP, stejně jako většinu, pak se server FTP, který běží na své webové servery.) Nejsou k dispozici několik FTP klientské aplikace. Ve webovém prohlížeči můžete dokonce double klienta FTP. Moje Oblíbené klienta FTP a tak můžu používat pro účely tohoto kurzu je [Filezilly](http://filezilla-project.org/), zdarma, open source klienta FTP, který je k dispozici pro Windows, Linux a počítače Mac. Jakéhokoliv FTP klienta bude fungovat, ale teď tedy můžete používat jakýkoli klient se vyhovuje nejvíce.

Pokud postupujete podél budete potřebovat k vytvoření účtu pomocí zprostředkovatele webového hostitele před můžete dokončí v tomto kurzu nebo další balíčky. Jak je uvedeno v předchozím kurzu, existují gaggle webového hostitele zprostředkovatele společností s široké spektrum ceny, funkcí a kvality služeb. Pro tuto řadu kurzů můžu používat [slevy ASP.NET](http://discountasp.net) jako Moje webového hostitele zprostředkovatel, ale můžete postupovat podle jakýkoli poskytovatel webového hostitele tak dlouho, dokud podporují verzi technologie ASP.NET je napsán v jazyce vašeho webu. (Tyto kurzy byly vytvořeny ASP.NET 3.5). Navíc protože jsme kopírování souborů na zprostředkovateli webového hostitele pomocí FTP v tomto kurzu a v budoucnu těch, které jsou je nutné, váš poskytovatel webového hostitele podporuje přístup pomocí protokolu FTP na své webové servery. Tato funkce přináší prakticky všechny webové hostitele zprostředkovatele, ale měli byste zkontrolovat, ještě než si zaregistrujete.

## <a name="deploying-the-book-review-web-application-project"></a>Nasazení projektu knihy revize webové aplikace

Připomínáme, že existují dvě verze recenze knihy webové aplikace: jednu implementované pomocí modelu projektu webové aplikace (BookReviewsWAP) a druhá pomocí modelu projektu webové stránky (BookReviewsWSP). Typ projektu ovlivňuje, jestli je webu je zkompilován automaticky nebo explicitně a kompilace modelu určuje soubory nutné k nasazení. V důsledku toho prozkoumáme nasazení projektů BookReviewsWAP a BookReviewsWSP samostatně, počínaje BookReviewsWAP. Pokud jste tak již neučinili, stáhněte si tyto dvě aplikace ASP.NET chvíli trvat.

Spustit projekt BookReviewsWAP tak, že přejdete na `BookReviewsWAP` složky a dvojitým kliknutím `BookReviewsWAP.sln` souboru. Před nasazením projektu je důležité k zajištění, že všechny změny zdrojového kódu jsou zahrnuty ve zkompilovaném sestavení sestavení. K sestavení projektu přejděte do nabídky sestavení a vyberte možnost nabídky BookReviewsWAP sestavení. Tento zdrojový kód v projektu kompiluje do jednoho sestavení, `BookReviewsWAP.dll`, která je umístěna v `Bin` složky.

Nyní jsme připraveni k nasazení potřebné soubory! Spusťte svého klienta FTP a připojit k webovému serveru na zprostředkovateli webového hostitele. (Při registraci s webhosting společnosti se bude e-mailem informace o tom, jak se připojit k serveru FTP, jedná se o adresu serveru FTP a uživatelské jméno a heslo)

Zkopírujte následující soubory z plochy do kořenové složky webu v zprostředkovateli webového hostitele. Pokud je FTP do webového serveru na webu hostitelem poskytovatele budete pravděpodobně v kořenovém adresáři webu. Nicméně, někteří poskytovatelé webového hostitele se pojmenovaná podsložka `www` nebo `wwwroot` , který slouží jako kořenová složka pro soubory vašeho webu. A konečně, když FTPing soubory budete muset vytvořit odpovídající strukturu složek v produkčním prostředí – `Bin` složku, `Fiction` složky, `Images` složky a tak dále.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Úplný obsah `Styles` složky
- Úplný obsah `Images` složku (a jejích podsložkách `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Obrázek 1 ukazuje Filezilly po potřebné soubory zkopírovaly. Filezilly zobrazí soubory v místním počítači na levé straně a soubory na vzdáleném počítači, na pravé straně. Obrázek 1 ukazuje, soubory zdrojového kódu ASP.NET, jako například `About.aspx.cs`, jsou na místním počítači (vývojové prostředí), ale nebyly zkopírovány do webového hostitele zprostředkovatele (produkční prostředí), protože soubory kódu nemusí být nasazen při použití explicitní kompilace.

> [!NOTE]
> Není nezpůsobily žádné potíže tím, že soubory zdrojového kódu na provozním serveru, jako jsou ignorovány. ASP.NET zakazuje požadavky HTTP na souborech zdrojového kódu ve výchozím nastavení tak, že i v případě, že soubory zdrojového kódu jsou k dispozici na provozním serveru jsou přístupné pro návštěvníky na váš web. (Pokud se uživatel pokusí o navštivte `http://www.yoursite.com/Default.aspx.cs` chybovou stránku, která vysvětluje, použije se tyto typy souborů – `.cs` soubory – jsou zakázané.)


[![Pomocí klienta FTP zkopírujte potřebné soubory z plochy na webový server na zprostředkovateli webového hostitele](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**Obrázek 1**: pomocí klienta FTP na webový server na hostiteli poskytovatele webových zkopírujte potřebné soubory z plochu ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))


Po nasazení webu využijte k otestování webu. Pokud jste zakoupili název domény a konfiguraci nastavení DNS správně, můžete navštívit web tak, že zadáte název vaší domény. Alternativně zprostředkovateli webového hostitele by měl zadali jste adresou URL vašeho webu, který bude vypadat podobně jako *accountname*. *webhostprovider*.com nebo *webhostprovider*.com /*accountname*. Například je adresa URL pro svůj účet na slevy ASP.NET: `http://httpruntime.web703.discountasp.net`.

Obrázek 2 ukazuje nasazené lokality recenzí. Všimněte si, že mám teď zobrazení na slevy ASP. NET pro servery, na `http://httpruntime.web703.discountasp.net`. V tomto okamžiku všem uživatelům s připojením k Internetu může zobrazit Můj web! Jak byste očekávali jsme, web vypadat a jak se bude chovat stejně jako při testování ve vývojovém prostředí.

> [!NOTE]
> Pokud dojde k chybě při zobrazení aplikace využít k Ujistěte se, že jste nasadili správnou sadu souborů. V dalším kroku najdete v chybové zprávě, pokud chcete zobrazit, pokud zjistí jakékoli příčiny, problém. Pod můžete zapnout na technickou podporu vaší společnosti webového hostitele nebo zveřejněte svůj dotaz ve fóru odpovídající [fóra ASP.NET](https://forums.asp.net/).


[![Server revize adresáře je nyní dostupný všem uživatelům s připojením k Internetu](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**Obrázek 2**: kontrol lokality adresáře je nyní dostupný všem uživatelům s připojením k Internetu ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Nasazení projektu webu revize knihy

Při nasazení aplikace ASP.NET, který používá automatické kompilaci, jako je například BookReviewsWSP webový projekt, neexistuje žádné kompilované sestavení v `Bin` složky. V důsledku toho soubory zdrojového kódu webové aplikace se musí nasadit do produkčního prostředí. Projděme si tento proces.

Stejně jako u projektu webové aplikace je vhodné sestavení první aplikace ještě před nasazením. Při vytváření projektu webu neslouží k vytvoření sestavení, zkontrolujte chyby kompilace na stránce. Najít tyto chyby je lepší místo nutnosti návštěvníkům webu zjišťování pro vás!

Jakmile úspěšně sestavíte projekt, zkopírujte následující soubory do kořenové složky webu v zprostředkovateli webového hostitele pomocí svého klienta FTP. Budete muset vytvořit odpovídající strukturu složek v produkčním prostředí.

> [!NOTE]
> Pokud jste už nasadili BookReviewsWAP projekt, ale přesto chcete zkuste nasazení BookReviewsWSP projektu, nejprve odstranit všechny soubory na webovém serveru, které byly odeslány při nasazování BookReviewsWAP a pak nasadit soubory pro BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- Úplný obsah `Styles` složky
- Úplný obsah `Images` složku (a jejích podsložkách `BookCovers`)
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

Obrázek 3 ukazuje Filezilly po zkopírování si potřebné soubory. Jak je vidět, ASP.NET souborů zdrojového kódu, jako například `About.aspx.cs`, jsou k dispozici na místním počítači (vývojové prostředí) a webového hostitele zprostředkovatele (produkční prostředí), protože soubory kódu je nutné nasadit při použití automatického kompilace.


[![Pomocí klienta FTP zkopírujte potřebné soubory z plochy na webový server na zprostředkovateli webového hostitele](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**Obrázek 3**: pomocí klienta FTP na webový server na hostiteli poskytovatele webových zkopírujte potřebné soubory z plochu ([kliknutím ji zobrazíte obrázek v plné velikosti](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))


Činnost koncového uživatele není ovlivněn model kompilace aplikace. Stejné stránky technologie ASP.NET jsou dostupné a jejich vzhled a chování stejné, zda web se vytvořil pomocí modelu projektu webové aplikace nebo modelu projektu webové stránky.

### <a name="updating-a-web-application-on-production"></a>Aktualizace webové aplikace v produkčním prostředí

Vývoj webových aplikací a nasazení nejsou jednorázového procesu. Například při vytváření na webu knihy revize založená na různých stránkách i související kód napsali osobní počítače (vývojové prostředí). Po dosažení určitých stabilní, můžu nasadit svoji aplikaci tak, aby ostatní mohli najdete na webu a čtení Moje recenze. Ale nasazení neoznačí end mé vývoje na tomto webu. Můžu přidat další kontroly knihy nebo implementují nové funkce, například můžete umožnit Moje návštěvníkům míra knihy nebo nechte své vlastní komentáře. Tato vylepšení by být vytvořeny ve vývojovém prostředí a po dokončení bude nutné k nasazení. K vývoji a nasazení, proto se cyklické. Vývoj aplikace a pak ho nasadíme. Při živého webu a v produkčním prostředí, se přidají nové funkce a opravených v čase, což vyžaduje opětovné nasazení aplikace. A podobně a tak dále.

Jak byste asi očekávali, při opětovné nasazení webové aplikace je potřeba jenom kopírovat nové a změněné soubory. Není nutné znovu nasadit beze změny stránky nebo serveru nebo klienta podpůrných souborů (ačkoli neexistuje nezpůsobily žádné potíže přitom).

> [!NOTE]
> Jedna věc, kterou je potřeba mít na paměti, při použití explicitní kompilace je kdykoli do projektu přidejte novou stránku ASP.NET nebo provést změny související s kódem, budete muset znovu sestavit projekt, který aktualizuje sestavení v `Bin` složky. V důsledku toho budete muset zkopírovat tento aktualizovaný sestavení do produkčního prostředí při aktualizaci webové aplikace v produkčním prostředí (společně s další nové a aktualizované obsah).


Také pochopit, že jakékoli změny `Web.config` či soubory v `Bin` adresáře se zastaví a restartuje fond aplikací na webu. Pokud váš stav relace se ukládá pomocí `InProc` režimu (výchozí) pak návštěvníci vašeho webu dojde ke ztrátě jejich stav relace vždy, když se mění tyto soubory klíčů. Abyste zabránili tomuto nebezpečí, zvažte uložení, pomocí relace `StateServer` nebo `SQLServer` režimy. Další informace o tomto tématu najdete [režim stavu relace](https://msdn.microsoft.com/library/ms178586.aspx).

A konečně mějte na paměti, že opětovné nasazení aplikace může trvat několik sekund až několik minut v závislosti na počtu a velikosti souborů, které je nutné zkopírovat do produkčního prostředí. Během této doby může dojít uživatelům, kteří navštíví váš web chyby nebo podivného chování. Můžete "vypnout" celé aplikace tak, že přidáte na stránku s názvem `App_Offline.htm` do kořenového adresáře aplikace, který vysvětluje vašim uživatelům, že lokalita je mimo provoz kvůli údržbě (nebo cokoli, co) a bude zanedlouho zálohování. Když `App_Offline.htm` soubor je k dispozici, modul runtime ASP.NET přesměruje všechny příchozí žádosti na této stránce.

## <a name="summary"></a>Souhrn

Nasazení webové aplikace zahrnuje kopírování potřebné soubory z vývojového prostředí do produkčního prostředí. Nejběžnějších způsobů, pomocí kterého se soubory se přenášejí přes síť se protokol FTP (File Transfer) a většina poskytovatelů webového hostitele podporují přístup FTP na své webové servery. V tomto kurzu jsme viděli, jak pomocí klienta FTP nasaďte potřebné soubory do webového serveru. Po nasazení na webu může použít každý, s připojením k Internetu!

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Aplikace\_Offline.htm a obejít funkce "IE popisný chyby"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Režim stavu relace](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Předchozí](determining-what-files-need-to-be-deployed-cs.md)
> [další](deploying-your-site-using-visual-studio-cs.md)
