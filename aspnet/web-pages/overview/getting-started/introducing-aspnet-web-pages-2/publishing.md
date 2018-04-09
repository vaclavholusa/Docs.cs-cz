---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Představení technologie ASP.NET Web Pages – publikování webu pomocí služby WebMatrix | Microsoft Docs
author: tfitzmac
description: V tomto kurzu je poslední část v sadě kurz, který představuje rozhraní ASP.NET Web Pages a Microsoft WebMatrix. Popisuje, jak publikování webu t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 7b9bffac5cc72e1bea3f1b211cc03be2ccb8e499
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Představení technologie ASP.NET Web Pages – publikování webu pomocí služby WebMatrix
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> V tomto kurzu je poslední část v sadě kurz, který představuje rozhraní ASP.NET Web Pages a Microsoft WebMatrix. Popisuje, jak publikovat vaší lokality k Internetu, aby ostatní s ním pracovat. Předpokládá, že jste dokončili řady prostřednictvím [vytváření konzistentní vypadat pro weby technologie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Naučíte, jak publikovat používá web:
> 
> - Microsoft Azure
> - Web hostingové společnosti


## <a name="about-publishing-your-site"></a>O publikování webu

Až nyní kroky dokončíte všechnu práci na místním počítači, včetně testování vaší stránky. Ke spuštění vaší<em>.cshtml</em> stránky, jste použili webový server, který je integrovaný do služby WebMatrix, a to služba IIS Express. Můžete ale samozřejmě nikdo můžete zobrazit na server, který jste vytvořili s výjimkou je. Umožníte dalším uživatelům práci s webem musíte publikovat na Internetu.

Pokud již máte přístup k veřejné webovému serveru, publikování znamená, že budete mít tak, aby měl účet s *Cloudová platforma* nebo *poskytovatele hostitelských služeb*. Cloudové platformy, jako je Microsoft Azure poskytuje infrastrukturu na vyžádání pro vaše aplikace. Poskytovatel hostingu je společnosti, který je vlastníkem veřejně přístupná webové servery a který bude pronajímat, můžete místo pro svůj web. Hostování plány spustit z několika dolarů za měsíc (nebo i volné) pro malé weby na mnoho stovky dolarů za měsíc pro vysoký počet komerčních webů.

> [!NOTE]
> Máte přístup k veřejné webovému serveru prostřednictvím poskytovatele služeb Internetu (ISP), které umožňují získat doma internetové služby. Poskytovatel hostitelských však musí podporovat rozhraní ASP.NET Web Pages. Nemáte mnoho poskytovatelů internetových služeb, ale je vždy vhodné kontrola.


V tomto kurzu jsme vám poskytují přehled o tom, jak publikovat. Není praktické zajistit najdete přesné informace pro všechno, protože proces se trochu liší pro všechny poskytovatele hostitelských služeb. Ale získáte představu o tom, jak probíhá proces.

Tento kurz obsahuje čtyři části:

1. [Nastavení výchozí stránka](#defaultpage)
2. Publikování (zvolte jednu z následujících)  
 a. [Publikování webu do služby Microsoft Azure](#azure)  
 b. [Publikování webu Webhosting společnosti](#host)
3. [Aktualizace živý web: opětovné publikování](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Nastavení výchozí stránka

Když uživatel přejde na základní adresa pro svůj web, se uživateli zobrazí výchozí stránku pro váš web. Například pokud Default.htm je nastaven jako výchozí stránky pro lokalitu na www.contoso.com, přejděte do <strong>www.contoso.com</strong> je stejný jako přejdete na <strong>www.contoso.com/Default.htm</strong>.

V současné době váš web používá **Default.cshtml** jako výchozí stránky. Tato stránka je vhodná pro výchozí stránku, ale v tomto kurzu jste nepřidali žádný obsah na této stránce, by se zobrazily prázdné stránky. Otevřete stránku Default.cshtml a nahradí obsah následujícím kódem.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Nyní je připraven pro publikaci vaší lokality. První zobrazí se postup nasazení webu na Azure a jak ji nasadit do webhosting společnosti. Buď možnost funguje pro váš web z umístění a je potřeba jenom použijte jednu z možností nasazení.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Publikování webu do služby Microsoft Azure

V tomto kurzu se nejprve zobrazí nasazení webu na Microsoft Azure. Po přihlášení pomocí účtu Microsoft, můžete vytvořit až 10 bezplatných webů v Azure. Tyto volné lokality poskytují pohodlný způsob pro testování vaší lokality. Vždy můžete odstranit tento server příklad později a nepoužívejte všechny volné stránek. Během několika minut můžete vytvořit Bezplatný zkušební účet. Podrobnosti najdete v tématu [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Na pásu karet WebMatrix, klikněte **publikovat** tlačítko.

!["Publikování" tlačítko WebMatrix pásu karet](publishing/_static/image1.png)

**Publikovat vaše lokality** se zobrazí dialogové okno. Pokud nejste přihlášeni ke svému účtu Microsoft, bude obsahovat dialogové okno **Začínáme s Azure** odkaz. Kliknutím na tento odkaz.

![Publikování webu](publishing/_static/image2.png)

Pokud nejste přihlášeni k účtu Microsoft, můžete je znovu příležitost k přihlášení. Musíte se přihlásit k účtu Microsoft k publikování webu v Azure.

![Přihlásit se](publishing/_static/image3.png)

Po přihlášení k účtu Microsoft, dialogové okno obsahuje odkazy na vytvoření nového webu v Azure nebo připojit k existující weby v Azure.

![Vytvoří nový web](publishing/_static/image4.png)

Vyberte **vytvoří nový web**.

Pokud název projektu **WebPagesMovies**, bude výchozí název pro svůj web **webpagesmovies.azurewebsites.net**. Tento výchozí název je pravděpodobně není k dispozici, jak je indikován červený vykřičník.

![název výchozí webové stránky](publishing/_static/image5.png)

Změňte název webového serveru na jinou hodnotu, která je k dispozici a vyberte umístění, které je blízko vaši polohu.

![název změněné lokality](publishing/_static/image6.png)

Click **OK**.

Služba WebMatrix performss test ke zjištění, zda je server kompatibilní s vaší lokality.

![testování kompatibility](publishing/_static/image7.png)

Vyberte **pokračovat**.

Výsledky testování kompatibility se zobrazí.

![výsledek kompatibility](publishing/_static/image8.png)

Vyberte **pokračovat**.

Služba WebMatrix zobrazí soubory a databáze, které bude publikována do lokality. Vzhledem k tomu, že je to první, kdy jsou publikování webu, jsou uvedeny všechny soubory. Zrušte zaškrtnutí políčka soubor, který není připravena k publikování. V dalších publikacích se zobrazí pouze soubory, které se změnily. V tématu [aktualizace živý web: opětovné publikování](#update).

![publikování náhledu](publishing/_static/image9.png)

Vyberte **pokračovat**.

Po nasazení webu do Azure, se zobrazí zpráva, která určuje, že je dokončená nasazení.

![publikování dokončení](publishing/_static/image10.png)

Lokality a databáze byly publikovány do služby Azure a jsou nyní k dispozici na veřejnost. Kliknutím na odkaz ve zprávě o dokončení publikování a zobrazí vaše nasazené lokality. Můžete nebo každý, kdo má přístup k Internetu, můžete přidat nebo upravit záznamy v databázi.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Publikování webu Webhosting společnosti

Pokud se rozhodnete nebude publikován do Azure, můžete místo toho publikování webu na web hostingové společnosti.

Klikněte **najít hostování webů** odkaz.

![Tlačítko "najít hostování webů, v dialogovém okně Nastavení publikování](publishing/_static/image12.png)

Přejděte na stránku na webu Microsoft, který uvádí poskytovatelé hostitelských služeb, které podporují ASP.NET.

![Stránky na webu společnosti Microsoft, které uvádí poskytovatelé hostitelských služeb](publishing/_static/image13.png)

Samozřejmě může být obtížné zjistit, teď právě jaký hostování funkcí může vyžadovat na dlouhou dobu. Tady je několik věcí, které je třeba zvážit:

- Pro účely WebPagesMovies lokality nemusíte mít samostatné rozšíření pro SQL Server, který se velmi často stojí. Ve vaší lokalitě používáte SQL Server Compact Edition, což je úplný a samostatný. Přístup k serveru SQL je však může být zapotřebí pro budoucí webu práce, kterou, které můžete provést. Pokud se domníváte, že pravděpodobně, zajistěte, aby funkce SQL serveru můžete později přidat.
- Zkontrolujte, zda poskytovatel hostingu podporuje protokol publikování nasazení webu. Můžete publikovat pomocí protokolu FTP, ale je pohodlnější službu Web Deploy používat.

Některé servery nabízejí bezplatné zkušební období. Bezplatná zkušební verze je dobrý způsob, jak zkuste publikování a hostování při jste stále experimentování se službou WebMatrix a webové stránky ASP.NET.

Vyberte ten, který chcete. V tomto kurzu jsme vybrat DiscountASP.NET, protože při jsme vytvořili kurz, že společnosti měli povýšení, které uživatelé mohou hostovat lokalitu zdarma pro několik měsíců.

> [!NOTE]
> Námi zvolený poskytovatele hostitelských služeb v tomto kurzu se nemohou interpretovat jako souhlas této společnosti nad všemi ostatními. Ale jsme měli vybrat jednu pro obrázek a DiscountASP.NET je jedním z řada společností, které podporuje technologie ASP.NET Web Pages a protokol nasazení webu pro publikování.


Obvykle se poté, co jste se přihlásili s poskytovateli hostitelských služeb, společnost vám pošle e-mailu, který obsahuje uživatelské jméno a heslo, adresu URL webového serveru a tak dále. Pokud hostingové společnosti podporuje protokol nasazení webu, se může odeslat můžete soubor, který obsahuje nastavení publikování, nebo můžete stáhnout z Internetu. Soubor nastavení publikování zjednodušuje proces pro vás.

Pokud jste se přihlásili a jste připraveni publikovat, klikněte na tlačítko **publikovat** tlačítko pásu karet služby WebMatrix. **Nastavení publikování** se zobrazí dialogové okno.

Pokud poskytovatel hostingu odeslat soubor nastavení publikování, klikněte na tlačítko **importu nastavení publikování** propojení a import souboru. Pokud nemáte soubor nastavení publikování, vyplňte pole pomocí hodnoty, které jste hostingové společnosti odeslat v e-mailu. Tady je co **nastavení publikování** dialogové okno může vypadat například po dokončení:

![Nastavení publikování vyplněno v dialogovém okně, nastavení publikování.](publishing/_static/image14.png)

Klikněte na tlačítko **ověření připojení**. Pokud se vše, co je to v pořádku, dialogové okno sestavy **připojení úspěšně**, což znamená, že může komunikovat s server poskytovatele hostitelských služeb.

![Úspěch zprávy Pokud publikování jsou nastavení správná](publishing/_static/image15.png)

Pokud dojde k potížím, služba WebMatrix nemá je nejlepší říct, co problém je:

![Chybová zpráva, pokud došlo k potížím s nastavením publikování](publishing/_static/image16.png)

Klikněte na tlačítko **Uložit** uložte nastavení. Služba WebMatrix nabízí chcete provést test a ujistěte se, že správně může komunikovat s hostování lokality:

![Zprávy, které chcete provést test procesu publikování nabídky](publishing/_static/image17.png)

Klikněte na tlačítko **Ano**. Služba WebMatrix ukládání některých ukázkových souborů do poskytovatele hostitelských služeb. Pokud se provádí testování kompatibility, služba WebMatrix hlásí výsledky:

![Výsledky testu publikování](publishing/_static/image18.png)

Pokud jste připravení přejít, pokračujte a klikněte na tlačítko **pokračovat** pro skutečných zahájíte proces publikování. Služba WebMatrix obrázky co soubory jsou ve vaší lokalitě a jsou již na hostitelském serveru (ihned, none) a poskytuje náhled proces publikování:

![Verze Preview jaké soubory, které odešlete proces publikování](publishing/_static/image19.png)

Seznam souborů k publikování, které zahrnuje webové stránky, které jste vytvořili jako *Movies.cshtml*. Seznam také obsahuje soubory pro pomocné rutiny, které jste nainstalovali, soubory a spustit SQL Server Compact Edition pro vaši databázi a tak dále. V důsledku toho počáteční publikování proces může být výrazně.

Klikněte na tlačítko **pokračovat**. Služba WebMatrix zkopíruje soubory na server poskytovatele hostitelských služeb. Až skončíte, výsledky jsou uvedeny ve stavovém řádku:

![Stavová zpráva panelu při procesu publikování byl úspěšně dokončen.](publishing/_static/image20.png)

Pokud chcete zobrazit živý web, klikněte na odkaz ve stavovém řádku. Přidat *filmy* na adresu URL, a zobrazí se *Movies.cshtml* soubor, který jste vytvořili:

![Živý web stránkou filmy](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Aktualizace živý web: opětovné publikování

Po publikování webu (do Azure nebo webhosting společnosti) jsou dvě kopie &mdash; verze na váš počítač a verze na poskytovatele služeb. Pravděpodobně budete chtít pokračovat ve vývoji lokality (Pokud nic jiného, jako součást sady další kurz). Když to uděláte, budete muset web znovu publikovat, aby bylo možné kopírovat změny z vašeho počítače a poskytovatelem služeb. Proces publikování ve službě WebMatrix můžete určit, jaké soubory se změnili na svém webu a publikovat pouze tyto soubory.

Chcete-li zjistit, jak funguje opětovné publikování, otevřete *Movies.cshtml* lokality, provedení některých malých změn a pak soubor uložte. Můžete například změnit název na `Movies - Updated`.

Klikněte **publikovat** tlačítka na pásu karet. Služba WebMatrix Určuje, co se změnilo a zobrazí náhled souborů, které budete publikovat.

![Dialogové okno "Publikovat" zobrazující změněné soubory připravené pro opětovné publikování](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Ve výchozím nastavení, služba WebMatrix publikuje vaše databáze (*SDF* souboru) pouze při prvním publikování webu. Jakmile vaše lokalita je publikována a osoby jsou interakci s webem, databázi na živý web má obvykle reálná data lokality. Budete muset buďte velmi opatrní nechcete přepsat databázi za provozu s *SDF* soubor, který je v počítači, který obvykle obsahuje pouze testovacích datech. Proto se zobrazí upozornění **publikování přepíše všechny vzdálené databáze**, a proč je zaškrtnutí políčka pro *WebPagesMovies.sdf* ve výchozím nastavení zaškrtnuto.


Klikněte na tlačítko **pokračovat**. Služba WebMatrix zobrazuje zpráva o úspěšném provedení jakou měla poprvé, kterou jste publikovali a publikuje změněné soubory.

Přejděte na web za provozu (můžete kliknout na odkaz ve zprávě úspěch Pokud se stále zobrazuje) a ověřit, že byl publikovaný změny.

> [!TIP] 
> 
> **Vzdálené úpravy souborů**
> 
> Jako alternativu k změna vaší lokality a poté opětovné publikování můžete upravit vzdálené soubory přímo ve službě WebMatrix. V tomto scénáři otevřete soubor, který je na poskytovatele služeb a služba WebMatrix stáhne kopii ho můžete upravit. Pokaždé, když jste uložili soubor, služba WebMatrix odešle změny do lokality.
> 
> Vzdálené úpravy je snadný způsob, jak změnit živý web. Změny, které uděláte takto však nejsou synchronizovány se soubory ve vaší místní lokalitě. Chcete-li synchronizovat místní soubory s vzdálené lokality, můžete stáhnout vzdálených souborů. Tento proces funguje podobně jako publikování, s výjimkou pozpátku.
> 
> Jsme nebude popisují více o vzdálené úpravy a stahování vzdálené zařízení WebMatrix sem. Jsou velmi užitečné, pokud máte více uživatelů pro práci na stejném webu na různých počítačích. Další informace najdete v tématu [publikování a upravit vzdálené lokality pomocí služby WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Další prostředky

- [Fórum služby WebMatrix rozhraní ASP.NET Web Pages ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), skvělým místem pro zveřejňování otázky a odpovědi.

> [!div class="step-by-step"]
> [Předchozí](layouts.md)
