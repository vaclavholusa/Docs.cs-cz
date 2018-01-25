---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: "Nasazení webu pomocí klienta FTP (C#) | Microsoft Docs"
author: rick-anderson
description: "Nejjednodušší způsob, jak nasadit aplikaci ASP.NET je ručně zkopírovat potřebné soubory z vývojového prostředí do produkčního prostředí. Thi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: 1edd53b1005449c060ff92fc7ebd02dbe7fa6ac2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="deploying-your-site-using-an-ftp-client-c"></a>Nasazení webu pomocí klienta FTP (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> Nejjednodušší způsob, jak nasadit aplikaci ASP.NET je ručně zkopírovat potřebné soubory z vývojového prostředí do produkčního prostředí. Tento kurz ukazuje, jak použít klienta k získání souborů z plochy ke zprostředkovateli webového hostitele.


## <a name="introduction"></a>Úvod

Předchozí kurz uvádí jednoduchou kniha zkontrolujte ASP.NET webovou aplikaci, která se skládá z několik stránek ASP.NET, hlavní stránky, na vlastní základní `Page` třídy, počet bitových kopií, a šablony stylů CSS tři. Nyní jsme připraveni k nasazení této aplikace do zprostředkovatele webového hostitele, na bod, který bude aplikace přístupné všem uživatelům s připojením k Internetu!


Z našich diskusí v [ *určení co soubory musí být nasazeny* ](determining-what-files-need-to-be-deployed-cs.md) kurz, abychom věděli, co soubory je nutné zkopírovat ke zprostředkovateli webového hostitele. (Pro vyvolání, které soubory se zkopírují závisí na tom, jestli vaše aplikace explicitně nebo automaticky kompiluje.) Ale jak jsme získat soubory z vývojové prostředí (naše desktop) až do produkčního prostředí (spravovaných zprostředkovatelem služby webového hostitele serveru webové)? [ **F** fil **T** transferu **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) je běžně používané protokol pro kopírování souborů z jednoho počítače do druhého přes síť. Další možností je FrontPage Server Extensions (FPSE). Tento kurz se zaměřuje na použití samostatného FTP klientský software pro nasazení potřebné soubory z vývojového prostředí do produkčního prostředí.

> [!NOTE]
> Visual Studio obsahuje nástroje pro publikování webů pomocí protokolu FTP; Tyto nástroje, podívejte se na nástroje, které používají FPSE, jsou popsané v dalším kurzu.


Kopírování souborů přes FTP, potřebujeme *klient FTP* na vývojovém prostředí. Klient FTP je aplikace, která slouží ke kopírování souborů z počítače, je nainstalovaný na počítači, který běží *FTP server*. (Pokud váš poskytovatel hostitele webu podporuje přenos souborů prostřednictvím protokolu FTP, stejně jako většinu, pak se k serveru FTP systémem své webové servery.) Nejsou k dispozici několik FTP klientské aplikace. Webový prohlížeč můžete i double jako klient FTP. Moje Oblíbené klient FTP a jedno I bude používat pro tento kurz je [FileZilla](http://filezilla-project.org/), klient FTP volné, open source, který je k dispozici pro Windows, Linux a počítače Mac. Jakéhokoliv FTP klienta bude fungovat, ale tak chování můžete používat jakoukoli klienta se nejvíce vyhovuje.

Použijete-li společně můžete třeba vytvořit účet s poskytovatelem webového hostitele předtím, než můžete dokončí v tomto kurzu nebo následné snímků. Jak jsme uvedli v předchozím kurzu, existují gaggle webového hostitele zprostředkovatele společností s široké spektrum ceny, funkce a kvalitu služeb. Pro tento kurz řady I budou používat [slevách ASP.NET](http://discountasp.net) jako můj webového hostitele poskytovatele, ale můžete postupovat podle společně s kteréhokoli zprostředkovatele webového hostitele také podporují ASP.NET verze vaší lokality je vyvinuta v. (Tyto kurzy byly vytvořené pomocí technologie ASP.NET 3.5.) Navíc vzhledem k tomu, že jsme kopírování souborů na zprostředkovateli webového hostitele pomocí protokolu FTP v tomto kurzu a v budoucích ty, které je nutné, zprostředkovateli webového hostitele podporuje přístup FTP na své webové servery. Téměř všechny webové hostitele zprostředkovatele nabízí tuto funkci, ale měli byste zkontrolovat před registrací.

## <a name="deploying-the-book-review-web-application-project"></a>Nasazení projektu kniha zkontrolujte webové aplikace

Odvolat, že existují dvě verze webové aplikace kontrolní seznam: jeden implementovaná pomocí modelu projektu webové aplikace (BookReviewsWAP) a dalších pomocí modelu webový projekt (BookReviewsWSP). Typ projektu vliv zda webu kompiluje automaticky nebo explicitně, a že kompilace modelu určuje soubory nutné k nasazení. V důsledku toho vyzkoušíme nasazení projekty BookReviewsWAP a BookReviewsWSP samostatně, počínaje BookReviewsWAP. Pokud jste tak již neučinili, stáhněte si tyto dvě aplikace ASP.NET chvíli trvat.

Spusťte projekt BookReviewsWAP přechodem na `BookReviewsWAP` složku a dvakrát klikněte na `BookReviewsWAP.sln` souboru. Před nasazením projektu je důležité k sestavení, aby obsahovaly všechny změny do zdrojového kódu v kompilovaném sestavení. Projekt sestavíte přejděte do nabídky sestavení a vyberte možnost nabídky BookReviewsWAP sestavení. To kompilovaný zdrojového kódu v projektu do jednoho sestavení `BookReviewsWAP.dll`, která je umístěna v `Bin` složky.

Nyní jsme připravení nasadit potřebné soubory! Spusťte vašeho klienta FTP a připojit k webovému serveru u svého poskytovatele webového hostitele. (Při registraci se na web hostingové společnosti se bude e-mailu informace o tom, jak se připojit k serveru FTP, jedná se o adresu serveru FTP a také uživatelské jméno a heslo.)

Zkopírujte následující soubory z plochy do kořenové složky webu u svého poskytovatele webového hostitele. Pokud je na webovém serveru FTP na webu hostitelem poskytovatele budete chtít nejspíš do kořenového adresáře webu. Ale některé webové hostitele zprostředkovatele se pojmenovaná podsložka `www` nebo `wwwroot` sloužícím jako kořenová složka pro soubory vašeho webu. Nakonec, když FTPing soubory může musíte vytvořit odpovídající struktura složek v provozním prostředí – `Bin` složku, `Fiction` složku, `Images` složky a tak dále.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Úplný obsahu `Styles` složky
- Úplný obsah `Images` složky (a její podsložce `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Obrázek 1 zobrazuje FileZilla po potřebné soubory byly zkopírovány. FileZilla zobrazuje na levé straně a soubory na vzdáleném počítači na pravé straně soubory v místním počítači. Obrázek 1 ukazuje, soubory zdrojového kódu ASP.NET, jako například `About.aspx.cs`, jsou v místním počítači (vývojového prostředí), ale nebyly zkopírovat do poskytovatel hostitele webu (produkčním prostředí), protože není nutné nasadit při použití souborů kódu explicitní kompilace.

> [!NOTE]
> Neexistuje žádné škodu v nutnosti soubory zdrojového kódu v provozním serveru, jako jsou ignorovány. ASP.NET zakazuje požadavky HTTP na soubory zdrojového kódu ve výchozím nastavení tak, aby i v případě, že soubory zdrojového kódu se nacházejí na provozním serveru jsou dostupná pro návštěvníci vašeho webu. (To znamená, pokud se uživatel pokusí o navštivte `http://www.yoursite.com/Default.aspx.cs` získá chybovou stránku, která vysvětluje, který tyto typy souborů – `.cs` soubory – jsou zakázané.)


[![Pomocí klienta zkopírujte potřebné soubory z plochy na webový server na zprostředkovateli webového hostitele](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**Obrázek 1**: pomocí klient FTP zkopírujte potřebné soubory z plochu na webový server na zprostředkovateli webového hostitele ([Kliknutím zobrazit obrázek v plné velikosti](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))


Po nasazení vaší lokality pozorně testování webu. Pokud jste zakoupili název domény a konfiguraci nastavení DNS správně, navštíví váš web tak, že zadáte název domény. Alternativně zprostředkovateli webového hostitele by měl zadali jste adresou URL vašeho webu, který bude vypadat podobně jako *accountname*. *webhostprovider*.com nebo *webhostprovider*.com nebo*accountname*. Například adresa URL pro účet na slevách ASP.NET je: `http://httpruntime.web703.discountasp.net`.

Obrázek 2 ukazuje bude web knihy recenze. Všimněte si, že mi se zobrazují na slevách ASP. NET na serverech, na `http://httpruntime.web703.discountasp.net`. V tuto chvíli každý, kdo má připojení k Internetu může zobrazit Moje Web! Jak byste očekávali jsme, lokality vzhled a chování stejně, jako při testování ve vývojovém prostředí.

> [!NOTE]
> Pokud dojde k chybě při zobrazení aplikace pozorně Ujistěte se, že jste nasadili správnou sadu souborů. Dále zkontrolujte chybové zprávy zobrazíte, pokud odhalí jakékoli různá vodítka, problém. Následující, můžete zapnout na technickou podporu společnosti webového hostitele nebo vystavte tady svůj dotaz na odpovídající fórum na [ASP.NET fóra](https://forums.asp.net/).


[![Lokalita recenze adresáře je nyní přístupné všem uživatelům s připojením k Internetu](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**Obrázek 2**: lokality recenze adresáře je nyní přístupné všem uživatelům s připojením k Internetu ([Kliknutím zobrazit obrázek v plné velikosti](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Nasazení projektu webu kontrolní seznam

Při nasazování aplikace ASP.NET, která používá automatickou kompilaci, jako je například BookReviewsWSP webový projekt, neexistuje žádné kompilované sestavení v `Bin` složky. Soubory zdrojového kódu webové aplikace se v důsledku toho musí nasadit do produkčního prostředí. Podívejme se tento proces.

Stejně jako u projekt webové aplikace je vhodné první sestavení aplikace ještě před nasazením. Při vytváření webového projektu nevytváří sestavení, vyhledejte všechny chyby při kompilaci na stránce. Lepší hledání těchto chyb nyní místo nutnosti návštěvníka na váš web zjišťování pro vás!

Jakmile úspěšně jste vytvořili projekt, zkopírujte následující soubory do kořenové složky webu u svého poskytovatele webového hostitele pomocí vaší FTP klienta. Musíte vytvořit odpovídající struktura složek na produkční prostředí.

> [!NOTE]
> Pokud už nasazená BookReviewsWAP projektu se ale stále chcete vyzkoušet nasazení projektu BookReviewsWSP, nejprve odstranit všechny soubory na webovém serveru, které byly odeslány při nasazování BookReviewsWAP a pak nasadit soubory pro BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- Úplný obsahu `Styles` složky
- Úplný obsah `Images` složky (a její podsložce `BookCovers`)
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

Obrázek 3 ukazuje FileZilla po kopírování si potřebné soubory. Jak můžete vidět, ASP.NET, jako zdrojové soubory kódu `About.aspx.cs`, se nacházejí na místním počítači (vývojového prostředí) a poskytovatel hostitele webu (produkčním prostředí), protože soubory kódu je potřeba nasadit při použití automatické kompilace.


[![Pomocí klienta zkopírujte potřebné soubory z plochy na webový server na zprostředkovateli webového hostitele](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**Obrázek 3**: pomocí klient FTP zkopírujte potřebné soubory z plochu na webový server na zprostředkovateli webového hostitele ([Kliknutím zobrazit obrázek v plné velikosti](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))


Činnost koncového uživatele není ovlivněná ve model kompilace aplikace. Jsou dostupné na stejné stránkách ASP.NET a jejich vzhled a chování stejné, zda web byla vytvořena pomocí modelu projektu webové aplikace nebo model webový projekt.

### <a name="updating-a-web-application-on-production"></a>Aktualizace webové aplikace na produkční

Vývoj webových aplikací a nasazení nejsou jednorázový proces. Například při vytváření webu recenze knihy I postavené na různých stránkách a doprovodné kód napsali v mém počítači osobní (vývojového prostředí). Po dosažení určité stabilního stavu, nasadil jsem Moje aplikace tak, aby ostatní může přejděte na web a Moje recenze. Ale nasazení neoznačí konec Moje vývoj na tomto webu. I může přidat další adresáře recenze nebo implementovat nové funkce, například umožníte Moje návštěvníky míra knihách nebo psát vlastní komentáře. Těchto vylepšení by být vyvinutá na vývojového prostředí a po dokončení bude potřeba nasadit. Vývoj a nasazení, proto jsou cyklické. Vývoj aplikace a poté ji nasadit. Při provozu webu a v produkčním prostředí, se přidají nové funkce a chyby budou opraveny v čase, který vyžaduje opětovného nasazení aplikace. A podobně a tak dále.

Podle očekávání, když znovu nasazení webové aplikace je potřeba jenom kopírovat nové a změněné soubory. Není nutné znovu nasadit beze změny stránky nebo straně serveru nebo klienta podpůrné soubory (i když není škodu přitom).

> [!NOTE]
> Jednou z věcí třeba vzít v úvahu při použití explicitní kompilace je můžete kdykoli přidat novou stránku ASP.NET do projektu nebo provést změny související se kód, budete muset znovu sestavte projekt, který aktualizuje sestavení v `Bin` složky. V důsledku toho budete muset zkopírovat tato aktualizovaném sestavení do produkčního prostředí při aktualizaci webové aplikace na produkční (spolu s další nové a aktualizované obsah).


Také chápou, že žádné změny `Web.config` či soubory v `Bin` directory zastaví a restartuje fond aplikací webu. Pokud vaše stav relace se ukládá pomocí `InProc` režimu (výchozí), pak návštěvníci vašeho webu dojde ke ztrátě stavu relace vždy, když jsou tyto soubory klíčů upraveny. Abyste se vyhnuli tomuto nebezpečí, zvažte uložení, relace pomocí `StateServer` nebo `SQLServer` režimy. Další informace v tomto tématu najdete v tématu [režim stavu relace](https://msdn.microsoft.com/library/ms178586.aspx).

Nakonec mějte na paměti, že znovu nasazení aplikace může trvat od několik sekund po několik minut v závislosti na počtu a velikosti souborů, které je nutné zkopírovat do produkčního prostředí. Během této doby uživatelům, kteří navštíví váš web může zaznamenat chyby nebo podivného chování. Můžete "vypnout" celá aplikace přidáním stránku s názvem `App_Offline.htm` kořenový adresář vaší aplikace, který vysvětluje uživatelům že lokalita je vypnutý pro údržby (nebo jiná) a bude se zálohování za chvíli. Když `App_Offline.htm` je soubor k dispozici, modulem runtime ASP.NET přesměruje všechny příchozí požadavky na této stránce.

## <a name="summary"></a>Souhrn

Nasazení webové aplikace zahrnuje kopírování potřebné soubory z vývojového prostředí do produkčního prostředí. Nejběžnější způsob přenosu souborů přes síť se protokolu FTP (File Transfer) a většina webové hostitele zprostředkovatele podpory přístup FTP na své webové servery. V tomto kurzu jsme viděli, jak nasadit potřebné soubory na webový server pomocí klienta FTP. Po nasazení webu můžete použít každý s připojením k Internetu!

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Aplikace\_Offline.htm a obejít funkci "IE popisný chyby"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Režim stavu relace](https://msdn.microsoft.com/library/ms178586.aspx)

>[!div class="step-by-step"]
[Předchozí](determining-what-files-need-to-be-deployed-cs.md)
[další](deploying-your-site-using-visual-studio-cs.md)
