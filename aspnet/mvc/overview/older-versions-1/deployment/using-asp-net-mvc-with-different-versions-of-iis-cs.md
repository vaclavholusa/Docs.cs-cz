---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: "Pomocí technologie ASP.NET MVC s různými verzemi služby IIS (C#) | Microsoft Docs"
author: microsoft
description: "V tomto kurzu zjistěte, jak používat rozhraní ASP.NET MVC a směrování adres URL, s různými verzemi služby IIS. Dozvíte různé strategie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: 8f2b98d5e5ae677fdac32336d542202a40290e21
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Pomocí technologie ASP.NET MVC s různými verzemi služby IIS (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> V tomto kurzu zjistěte, jak používat rozhraní ASP.NET MVC a směrování adres URL, s různými verzemi služby IIS. Zjistíte, různé strategie pro rozhraní ASP.NET MVC pomocí služby IIS 7.0 (classic režim), služby IIS 6.0 a starší verze služby IIS.


Rozhraní ASP.NET MVC, závisí na směrování ASP.NET na požadavky prohlížeče trasy, aby akce kontroleru. Aby bylo možné využít výhod směrování ASP.NET, bude pravděpodobně nutné provést další kroky konfigurace na webovém serveru. Všechny závisí na verzi Internetové informační služby (IIS) a režim pro vaši aplikaci zpracování požadavku.

Zde je souhrn různé verze služby IIS:

- IIS 7.0 (integrovaný režim) - žádná speciální konfigurace, které jsou nutné k použití směrování ASP.NET.
- Služba IIS 7.0 (klasickém režimu) -, budete muset provést speciální konfigurace, pokud chcete používat směrování ASP.NET.
- Služby IIS 6.0 nebo níže – je potřeba provést zvláštní konfiguraci chcete používat směrování ASP.NET.

Nejnovější verze služby IIS je verze 7.5 (na Win7). Služby IIS 7 IIS je součástí s Windows Server 2008 a VISTA/SP1 a vyšší. Také můžete nainstalovat službu IIS 7.0 na žádné verze operačního systému Vista kromě Home Basic (viz [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

IIS 7.0 podporuje dva režimy pro zpracování požadavků. Můžete použít integrovaného režimu nebo v klasickém režimu. Nemusíte provádět žádné kroky speciální konfigurace, při použití služby IIS 7.0 v integrovaném režimu. Však nutné provést další konfigurace, při použití služby IIS 7.0 v klasickém režimu.

Microsoft Windows Server 2003 zahrnuje služby IIS 6.0. Při použití operačního systému Windows Server 2003, nelze upgradovat služby IIS 6.0 do služby IIS 7.0. Při použití služby IIS 6.0, je nutné provést další kroky konfigurace.

Microsoft Windows XP Professional zahrnuje služba IIS 5.1. Při použití služby IIS 5.1, je nutné provést další kroky konfigurace.

Nakonec Microsoft Windows 2000 a Microsoft Windows 2000 Professional zahrnuje služby IIS 5.0. Při použití služby IIS 5.0, je nutné provést další kroky konfigurace.

## <a name="integrated-versus-classic-mode"></a>Integrované versus klasickém režimu

IIS 7.0 zpracovávat požadavky pomocí dvou režimů zpracování různé žádosti: integrované a classic. Integrovaný režim poskytuje lepší výkon a další funkce. Klasickém režimu je zahrnuté pro zpětnou kompatibilitu s předchozími verzemi služby IIS.

Režim zpracování požadavku je určen podle fondu aplikací. Můžete určit, který způsob zpracování je stále používán konkrétní webovou aplikaci tak, že určíte fond aplikací, které jsou přidružené k aplikaci. Postupujte podle těchto kroků:

1. Spusťte Správce Internetové informační služby
2. V okně připojení vybrat aplikaci
3. V okně Akce klikněte na **základní nastavení** odkaz otevřete dialogové okno Upravit aplikaci (viz obrázek 1)
4. Poznamenejte si vybraný fond aplikací.

Ve výchozím nastavení, služba IIS byla nakonfigurovaná pro podporu dvou fondů aplikací: **DefaultAppPool** a **Classic .NET AppPool**. Pokud je vybrána DefaultAppPool, vaše aplikace běží v režimu zpracování integrované žádosti. Pokud je vybrána Classic .NET AppPool, vaše aplikace běží v režimu zpracování classic žádosti.

[![Dialogové okno Nový projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Obrázek 1**: zjišťování režim zpracování požadavku ([Kliknutím zobrazit obrázek v plné velikosti](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Všimněte si, že můžete upravit režim zpracování požadavků v dialogovém okně Upravit aplikaci. Kliknutím na tlačítko Vybrat a změnit fond aplikací, které jsou přidružené k aplikaci. Uvědomte si, že existují problémy s kompatibilitou při změně aplikace ASP.NET z klasického do integrovaného režimu. Další informace najdete v následujících článcích:

- Upgrade ASP.NET 1.1 pro službu IIS 7.0 v systému Windows Vista a Windows Server 2008 – [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- Integrace technologie ASP.NET se službou IIS 7.0 - [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Pokud aplikace ASP.NET používá DefaultAppPool, pak nemusíte provádět žádné další kroky k získání směrování ASP.NET (a proto ASP.NET MVC) fungovat. Pokud aplikace ASP.NET je nakonfigurovaná pro použití Classic .NET AppPool pak zachovat čtení, ale máte další práci udělat.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>ASP.NET MVC pomocí starší verze služby IIS

Pokud budete muset použít rozhraní ASP.NET MVC pomocí starší verze služby IIS, než IIS 7.0, nebo budete muset použít v klasickém režimu služby IIS 7.0, máte dvě možnosti. První můžete upravit směrovací tabulka používat přípony souborů. Například místo vyžadování adresu URL podobnou /Store/Details, by vyžadovat adresu URL podobnou /Store.aspx/Details.

Druhou možností je vytvořit takzvaný *mapu skriptů se zástupnými znaky*. Mapu skriptů se zástupnými znaky umožňuje mapování každou žádost na rozhraní ASP.NET.

Pokud nemáte přístup k vašemu webovému serveru (například vaše ASP.NET MVC aplikace je hostitelem byl poskytovatele služeb Internetu) pak budete muset použít první možnost. Pokud nechcete změnit vzhled adresám URL, a máte přístup k vašemu webovému serveru, můžete použít druhou možnost.

Jsme prozkoumejte jednotlivé možnosti podrobně v následujících částech.

## <a name="adding-extensions-to-the-route-table"></a>Přidání rozšíření do tabulky směrování

Nejjednodušší způsob, jak získat směrování ASP.NET pro starší verze služby IIS je upravit tabulky tras v souboru Global.asax. Výchozí hodnota a beze změny souboru Global.asax v výpis 1 nakonfiguruje jeden postup s názvem výchozí trasu.

**Listing 1 - Global.asax (unmodified)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

Výchozí trasu nakonfigurované v výpis 1 umožňuje adresy URL trasy, které vypadají takto:

/ Home nebo Index

/ Produktu/podrobnosti/3

/ Produktu

Starší verze služby IIS bohužel nebude tyto požadavky předat rozhraní ASP.NET. Tyto požadavky proto nebude získat směrovány do řadiče. Například pokud musíte provést žádost prohlížeče pro adresy URL/Home nebo Index pak získáte chybové stránky na obrázku 2.

[![Dialogové okno Nový projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Obrázek 2**: přijímání chybu 404 nebyl nalezen ([Kliknutím zobrazit obrázek v plné velikosti](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Starší verze služby IIS mapovat jenom určité požadavky na technologii ASP.NET. Žádost musí být pro adresu URL s příponou souboru správné. Žádost o /SomePage.aspx získá například namapované na technologii ASP.NET. Však žádost o /SomePage.htm neexistuje.

Proto směrování ASP.NET pro práci získáte jsme musíte změnit výchozí trasu tak, že obsahují příponu souboru, který je namapovaný na technologii ASP.NET.

To se provádí pomocí skriptu s názvem `registermvc.wsf`. Byl zahrnut ve verzi ASP.NET MVC 1 v `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, ale od 2 technologie ASP.NET tento skript se přesunula do Futures ASP.NET k dispozici na [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).

Provádění tohoto skriptu zaregistruje nové rozšíření .mvc službou IIS. Po dokončení registrace .mvc rozšíření, můžete upravit trasy v souboru Global.asax tak, aby trasy používat .mvc rozšíření.

Změněný soubor Global.asax v výpis 2 pracuje s starší verze služby IIS.

**Výpis 2 - Global.asax (upraveno s příponami)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Důležité**: Mějte na paměti k vytvoření aplikace ASP.NET MVC znovu po změně souboru Global.asax.

Existují dvě důležité změny v souboru Global.asax v výpis 2. Existují dvě trasy definované v soubor Global.asax. Vzor adresy URL pro výchozí trasu, první trasy, teď vypadá takto:

{controller}.mvc/{action}/{id}

Přidání rozšíření .mvc změní typ souborů, které zabrání modul Směrování ASP.NET. Díky této změně aplikace ASP.NET MVC směruje požadavky takto:

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

Druhý postup, kořenové trasy, je nová. Tento vzor adresy URL pro trasu kořenovou je řetězec prázdný. Tato trasa je nezbytné pro párování požadavky na kořenovém adresáři aplikace. Například kořenový trasy, která bude shodovat s požadavek, který vypadá takto:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Po provedení těchto změn do směrovací tabulky, budete potřebovat, abyste měli jistotu, že jsou kompatibilní s tyto nové adresy URL vzory všechny odkazy v aplikaci. Jinými slovy Ujistěte se, že všechny vaše odkazy obsahují .mvc rozšíření. Pokud používáte metodu helper Html.ActionLink() ke generování odkazů, by neměl musíte žádné změny.

Místo použití skriptu registermvc.wcf, můžete přidat nové rozšíření pro službu IIS, který je namapovaný na technologii ASP.NET ručně. Při přidávání nové rozšíření, ujistěte se, že políčko s názvem bez přípony **ověřte, že soubor existuje** není zaškrtnuto.

## <a name="hosted-server"></a>Hostovaný Server

Vždy nemáte přístup k vašemu webovému serveru. Například pokud vaše aplikace ASP.NET MVC pomocí poskytovatele hostování Internetu jsou hostiteli, pak nebudete mít nutně přístup do služby IIS.

V takovém případě by měl použít jednu z existujících přípon souborů, které jsou namapované na technologii ASP.NET. Přípony souborů, které jsou mapovány na technologii ASP.NET příkladem .aspx, AXD a ASHX rozšíření.

Změněný soubor Global.asax ve výpisu 3 například místo rozšíření .mvc používá příponou .aspx.

**Výpis 3 - Global.asax (upraveno s příponou .aspx)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Soubor Global.asax výpis 3 je přesně stejný jako předchozí souboru Global.asax s tou výjimkou, použije místo rozšíření .mvc příponou .aspx. Není nutné provádět jakékoliv nastavení na vzdáleném webovém serveru používat příponou .aspx.

## <a name="creating-a-wildcard-script-map"></a>Vytváření mapy skriptů se zástupnými znaky

Pokud nechcete, aby k úpravě adresy URL pro aplikaci ASP.NET MVC a mít přístup k vašemu webovému serveru, máte další možnost. Můžete vytvořit mapu skriptů zástupný znak, který mapuje všechny požadavky na webový server pro technologii ASP.NET. Tímto způsobem můžete vytvořit výchozí směrovací tabulka ASP.NET MVC pomocí služby IIS 7.0 (v klasickém režimu) nebo služby IIS 6.0.

Upozorňujeme, že tato možnost způsobí, že IIS zachytí každou žádost odeslanou na webový server. To zahrnuje požadavky pro obrázky, classic ASP stránky a stránky HTML. Proto povolení zástupný znak mapu skriptů ASP.NET mít vliv na výkon.

Zde je, jak povolit mapu skriptů se zástupnými znaky pro službu IIS 7.0:

1. Vyberte svou aplikaci v okně připojení
2. Ujistěte se, že **funkce** je vybráno zobrazení
3. Dvakrát klikněte **mapování obslužných rutin** tlačítko
4. Klikněte **přidat mapu skriptů se zástupnými znaky** odkaz (viz obrázek 3)
5. Zadejte cestu k aspnet\_isapi.dll souboru (tuto cestu můžete zkopírovat z mapu skriptů PageHandlerFactory)
6. Zadejte název MVC
7. Klikněte **OK** tlačítko

[![Dialogové okno Nový projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Obrázek 3**: vytvoření mapy skriptů se zástupnými znaky se službou IIS 7.0 ([Kliknutím zobrazit obrázek v plné velikosti](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Postupujte podle těchto kroků můžete vytvořit mapu skriptů se zástupnými znaky se službou IIS 6.0:

1. Klikněte pravým tlačítkem na web a vyberte vlastnosti
2. Vyberte **domovský adresář** karta
3. Klikněte **konfigurace** tlačítko
4. Vyberte **mapování** karta
5. Klikněte **vložit** tlačítko (viz obrázek 4)
6. Vložte cestu ke aspnet\_isapi.dll do pole spustitelný soubor (tuto cestu můžete zkopírovat z mapu skriptů pro soubory .aspx)
7. Zrušte zaškrtnutí políčka s názvem bez přípony **ověřte, že soubor existuje**
8. Klikněte **OK** tlačítko

[![Dialogové okno Nový projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Obrázek 4**: vytvoření mapy skriptů se zástupnými znaky pomocí služby IIS 6.0 ([Kliknutím zobrazit obrázek v plné velikosti](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Jakmile povolíte mapy skriptů se zástupnými znaky, musíte upravit směrovací tabulka v souboru Global.asax tak, že obsahují kořenové trasy. Jinak získáte chybové stránky na obrázku 5 při podání žádosti o pro stránku kořenové vaší aplikace. Můžete použít upravené soubor Global.asax výpis 4.

[![Dialogové okno Nový projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Obrázek 5**: kořenové chybí trasu chyby ([Kliknutím zobrazit obrázek v plné velikosti](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Výpis 4 - Global.asax (upraveno s trasou Root)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Jakmile povolíte mapu skriptů se zástupnými znaky pro služby IIS 7.0 nebo služby IIS 6.0, můžete provést požadavků, které pracují se výchozí směrovací tabulka, které vypadat takto:

/

/ Home nebo Index

/ Produktu/podrobnosti/3

/ Produktu

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo vysvětlují, jak můžete použít ASP.NET MVC při použití starší verze služby IIS (nebo služby IIS 7.0 v klasickém režimu). Jsme probrali dvě metody získání směrování ASP.NET pro starší verze služby IIS: Upravit výchozí směrovací tabulka, nebo vytvořit mapu skriptů se zástupnými znaky.

První možnost vyžaduje, abyste upravili adres URL používaných ve vaší aplikaci ASP.NET MVC. Jeden velmi významné výhody této první možnosti je, že nepotřebujete přístup k webovému serveru k úpravě směrovací tabulka. To znamená, že můžete použít tuto možnost první i v případě hostování vaší aplikace ASP.NET MVC pomocí Internetu hostování společnosti.

Druhou možností je vytvořit mapu skriptů se zástupnými znaky. Výhodou této druhé možnosti je, že není potřeba změnit vaší adresy URL. Nevýhodou této druhé možnosti je, že může mít dopad na výkon vaší aplikace ASP.NET MVC.

>[!div class="step-by-step"]
[Next](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
