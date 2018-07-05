---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: Použití ASP.NET MVC s různými verzemi služby IIS (C#) | Dokumentace Microsoftu
author: microsoft
description: V tomto kurzu se dozvíte, jak používat rozhraní ASP.NET MVC a směrování adres URL s různými verzemi služby IIS. Zjistíte, různé strategie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: ddf7bc4548099ba5d95b4c0bb6f94a31ab38beb6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371723"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Použití ASP.NET MVC s různými verzemi služby IIS (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> V tomto kurzu se dozvíte, jak používat rozhraní ASP.NET MVC a směrování adres URL s různými verzemi služby IIS. Zjistíte, různé strategie pro ASP.NET MVC pomocí služby IIS 7.0 (v klasickém režimu), služby IIS 6.0 a starších verzích služby IIS.


Architektura ASP.NET MVC, závisí na směrování ASP.NET pro směrování požadavků prohlížeče na akce kontroleru. Pokud chcete využít výhod směrování ASP.NET, budete muset provést další kroky konfigurace na webovém serveru. Všechno závisí na verzi Internetové informační služby (IIS) a režim pro vaši aplikaci zpracování požadavku.

Zde je uveden seznam různými verzemi služby IIS:

- Služba IIS 7.0 (integrovaný režim) – žádná zvláštní konfigurace, které jsou nutné k použití směrování ASP.NET.
- Služba IIS 7.0 (klasický režim) – je třeba provést zvláštní konfigurace, aby používala směrování ASP.NET.
- Služba IIS 6.0 nebo níže – je třeba provést zvláštní konfigurace, aby používala směrování ASP.NET.

Nejnovější verze služby IIS je verze 7.5 (na Win7). Služby IIS 7 IIS je součástí s Windows Server 2008 a VISTA/SP1 a vyšší. Také můžete nainstalovat službu IIS 7.0 na žádné verze operačního systému Vista kromě Home Basic (viz [ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

Služba IIS 7.0 podporuje dva režimy pro zpracování požadavků. Můžete použít integrované režimu nebo v klasickém režimu. Není nutné provádět žádné zvláštní konfigurační kroky, při použití služby IIS 7.0 v integrovaném režimu. Ale je nutné provést další konfigurace, při použití služby IIS 7.0 v klasickém režimu.

Microsoft Windows Server 2003 obsahuje služby IIS 6.0. Při použití operačního systému Windows Server 2003, nelze upgradovat služby IIS 6.0 pro službu IIS 7.0. Při použití služby IIS 6.0, je nutné provést další kroky konfigurace.

Microsoft Windows XP Professional zahrnuje služba IIS 5.1. Při použití IIS 5.1, je nutné provést další kroky konfigurace.

A konečně Microsoft Windows 2000 a Microsoft Windows 2000 Professional zahrnuje služby IIS 5.0. Při použití služby IIS 5.0, je nutné provést další kroky konfigurace.

## <a name="integrated-versus-classic-mode"></a>Integrované a klasický režim

IIS 7.0 může zpracovat pomocí dvou režimů jinou žádost o zpracování žádosti o: integrované a classic. Integrovaný režim poskytuje lepší výkon a další funkce. Klasický režim je součástí pro zpětnou kompatibilitu s předchozími verzemi služby IIS.

Fond aplikací je určeno režim zpracování požadavků. Můžete určit, který způsob zpracování je používán konkrétní webovou aplikaci tak, že určíte fond aplikací přidružených k aplikaci. Postupujte podle těchto kroků:

1. Spusťte Správce Internetové informační služby
2. V okně připojení vybrat aplikaci
3. V okně Akce klikněte **základní nastavení** odkaz k otevření dialogového okna Upravit aplikaci (viz obrázek 1)
4. Poznamenejte si vybraný fond aplikací.

Ve výchozím nastavení, služba IIS konfigurována pro podporu dvou fondů aplikací: **DefaultAppPool** a **Classic .NET AppPool**. Pokud je vybrána DefaultAppPool, vaše aplikace běží v režimu zpracování integrované žádosti. Pokud je vybrána Classic .NET AppPool, aplikace běží v režimu zpracování classic žádosti.

[![Dialogové okno Nový projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Obrázek 1**: zjišťování režim zpracování požadavků ([kliknutím ji zobrazíte obrázek v plné velikosti](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Všimněte si, že můžete změnit režim zpracování požadavků v dialogovém okně Upravit aplikaci. Klikněte na tlačítko pro výběr a změnit fond aplikací přidružených k aplikaci. Uvědomte si, že existují problémy s kompatibilitou při změně aplikace ASP.NET z modelu nasazení classic do integrovaného režimu. Další informace najdete v následujících článcích:

- Upgrade na službě IIS 7.0 ve Windows Vista a Windows Server 2008 – ASP.NET 1.1 [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- Integrace ASP.NET se službou IIS 7.0: [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Pokud aplikace ASP.NET používá DefaultAppPool, pak není nutné provádět žádné další kroky k získání směrování ASP.NET (a tedy ASP.NET MVC) pro práci. Pokud aplikace ASP.NET je nakonfigurováno pomocí Classic .NET AppPool pak pokračujte ve čtení, ale máte další práci.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Pomocí technologie ASP.NET MVC se staršími verzemi služby IIS

Pokud je třeba použít technologie ASP.NET MVC pomocí starší verze služby IIS, než IIS 7.0 nebo budete muset použít službu IIS 7.0 v režimu classic, máte dvě možnosti. Nejprve můžete upravit směrovací tabulky pro použití souboru rozšíření. Namísto žádosti adresy URL /Store/Details, například bude adresa URL jako /Store.aspx/Details požadavku.

Druhou možností je vytvořit s názvem *mapu skriptů se zástupnými znaky*. Mapy skriptů se zástupnými znaky vám umožní mapovat každou žádost do architektury ASP.NET.

Pokud nemáte přístup na webový server (například vaší ASP.NET MVC se aplikace hostuje poskytovatele internetových služeb) pak budete muset použít první možnost. Pokud nechcete změnit vzhled vaší adresy URL a mít přístup k webovému serveru, můžete použít druhou možnost.

Podíváme na jednotlivé možnosti podrobně v následující části.

## <a name="adding-extensions-to-the-route-table"></a>Přidávání rozšíření do směrovací tabulky

Nejjednodušší způsob, jak získat směrování ASP.NET pro práci se staršími verzemi služby IIS je upravit vaše směrovací tabulku v souboru Global.asax. Výchozí nastavení a verzí bez úprav souboru Global.asax ve výpisu 1 nakonfiguruje jednu trasu s názvem výchozí trasu.

**Listing 1 - Global.asax (unmodified)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

Výchozí trasy nakonfigurované v informacích 1 umožňuje směrování adres URL, které vypadat nějak takto:

/ Home/Index

/ Produkt/podrobnosti/3

/ Produktu

Starší verze služby IIS bohužel nebudou předávat tyto žádosti na rozhraní ASP.NET. Proto nebude získat tyto požadavky směrovány do kontroleru. Například pokud vytvoříte žádost prohlížeče na adresu URL/Home/Index pak získáte chybovou stránku na obrázku 2.

[![Dialogové okno Nový projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Obrázek 2**: Chyba 404 nebyl nalezen ([kliknutím ji zobrazíte obrázek v plné velikosti](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Starší verze služby IIS mapovat pouze určité požadavky na rozhraní ASP.NET. Žádost musí být pro adresu URL s příponou souboru správné. Žádost o /SomePage.aspx získá například mapovat na technologii ASP.NET. Ale žádost o /SomePage.htm nepodporuje.

Proto zobrazíte směrování ASP.NET pro práci jsme musíte upravit výchozí trasu tak, že obsahují příponu souboru, který je namapovaný na technologii ASP.NET.

To se provádí pomocí skriptu s názvem `registermvc.wsf`. Je součástí verze technologie ASP.NET MVC 1 v `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, ale od 2 technologie ASP.NET tento skript byl přesunut do Futures ASP.NET k dispozici na [ http://aspnet.codeplex.com/releases/view/39978 ](http://aspnet.codeplex.com/releases/view/39978).

Spuštěním tohoto skriptu zaregistruje nové rozšíření .mvc službou IIS. Až dokončíte registraci .mvc rozšíření, tak, aby trasy používat rozšíření .mvc můžete upravit trasy v souboru Global.asax.

Upravený soubor Global.asax výpis 2 funguje ve starších verzích služby IIS.

**Výpis 2 - Global.asax (upravit pomocí rozšíření)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Důležité**: Mějte na paměti k vytvoření aplikace ASP.NET MVC po změně souboru Global.asax.

Existují dvě důležité změny v souboru Global.asax ve výpisu 2. Nyní existují dvě trasy definované v souboru Global.asax. Vzor adresy URL pro výchozí trasa první trasa teď vypadá jako:

{controller}.mvc/{action}/{id}

Přidání rozšíření .mvc změní typ souborů, které zachycuje modulu Směrování ASP.NET. Díky této změně teď aplikace ASP.NET MVC směruje požadavky vypadat asi takto:

/Home.MVC/index/

/Product.mvc/Details/3

/Product.mvc/

Druhá trasa trasy kořenové novinky. Tento vzor adresy URL pro trasu Root je prázdný řetězec. Tato trasa je nezbytné pro odpovídající požadavků na přístup kořenového adresáře aplikace. Trasy kořenové bude například odpovídat požadavek, který vypadá takto:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Po provedení těchto změn do směrovací tabulky, budete muset ověřit, že všechny odkazy v aplikaci jsou kompatibilní s tyto nové vzory adres URL. Jinými slovy Ujistěte se, že všechny odkazy zahrnují .mvc rozšíření. Pokud používáte Html.ActionLink() Pomocná metoda ke generování odkazů, pak neměli byste potřebovat provádět žádné změny.

Namísto použití skriptu registermvc.wcf, můžete přidat nové rozšíření do služby IIS, který je namapovaný na rozhraní ASP.NET ručně. Při přidávání nové rozšíření, ujistěte se, že označení zaškrtávacího políčka **věřit existenci souboru** políčko není zaškrtnuté.

## <a name="hosted-server"></a>Hostovaný Server

Vždy nemáte přístup k webovému serveru. Například pokud vaše aplikace ASP.NET MVC pomocí poskytovatele hostingu Internet, pak nebudete mít nutně přístup do služby IIS.

V takovém případě by měl používat jednu z existujících přípony souborů, které jsou mapovány na rozhraní ASP.NET. Příklady přípon souborů, které jsou mapovány na technologii ASP.NET, které zahrnují .aspx, AXD a ASHX rozšíření.

Upravený soubor Global.asax výpis 3 například používá rozšíření .aspx místo .mvc rozšíření.

**Výpis 3 - Global.asax (upraveno s příponou .aspx)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Soubor Global.asax výpis 3 je přesně stejný jako předchozí soubor Global.asax kromě toho používá rozšíření .aspx místo .mvc rozšíření. Není nutné provádět žádné nastavení na vzdáleném webovém serveru na použití rozšíření .aspx.

## <a name="creating-a-wildcard-script-map"></a>Vytvoření mapy skriptů se zástupnými znaky

Pokud nechcete změnit adresy URL pro aplikaci ASP.NET MVC a mít přístup k webovému serveru, máte možnost Další. Můžete vytvořit mapu skriptů se zástupnými znaky, který mapuje všechny požadavky na webový server pro technologii ASP.NET. Tímto způsobem můžete výchozí aplikaci ASP.NET MVC směrovací tabulku s IIS 7.0 (v klasickém režimu) nebo služby IIS 6.0.

Mějte na paměti, že tato možnost způsobí, že služby IIS, aby se zachytily každý požadavek směřovaný webový server. Patří sem požadavky pro obrázky, klasické ASP stránky a stránky HTML. Proto povolení zástupný znak mapování skriptů ASP.NET nemá vliv na výkon.

Zde je, jak povolit mapu skriptů se zástupnými znaky pro službu IIS 7.0:

1. Vyberte svou aplikaci v okně připojení
2. Ujistěte se, že **funkce** je vybráno zobrazení
3. Dvakrát klikněte **mapování obslužných rutin** tlačítko
4. Klikněte na tlačítko **přidat mapu skriptů se zástupnými znaky** propojení (viz obrázek 3)
5. Zadejte cestu k aspnet\_isapi.dll souboru (tuto cestu můžete zkopírovat z mapování skriptů PageHandlerFactory)
6. Zadejte název MVC
7. Klikněte na tlačítko **OK** tlačítko

[![Dialogové okno Nový projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Obrázek 3**: vytvoření mapy skriptů se zástupnými znaky se službou IIS 7.0 ([kliknutím ji zobrazíte obrázek v plné velikosti](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Postupujte podle těchto kroků můžete vytvořit mapu skriptů se zástupnými znaky se službou IIS 6.0:

1. Klikněte pravým tlačítkem na web a vyberte vlastnosti
2. Vyberte **domovský adresář** kartu
3. Klikněte na tlačítko **konfigurace** tlačítko
4. Vyberte **mapování** kartu
5. Klikněte na tlačítko **vložit** tlačítko (viz obrázek 4)
6. Vložte cestu k aspnet\_isapi.dll do pole spustitelný soubor (tuto cestu můžete zkopírovat z mapy skriptů pro soubory .aspx)
7. Zrušte zaškrtnutí políčka popisek **věřit existenci souboru**
8. Klikněte na tlačítko **OK** tlačítko

[![Dialogové okno Nový projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Obrázek 4**: vytvoření mapy skriptů se zástupnými znaky se službou IIS 6.0 ([kliknutím ji zobrazíte obrázek v plné velikosti](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Po povolení mapy skriptů se zástupnými znaky, budete muset upravit směrovací tabulky v souboru Global.asax tak, že obsahují kořenový trasy. Vytvořit žádost o kořenové stránky aplikace zobrazí, jinak se chybové stránky na obrázku 5. Můžete použít upravený soubor Global.asax výpis 4.

[![Dialogové okno Nový projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Obrázek 5**: Chyba chybí kořenový trasy ([kliknutím ji zobrazíte obrázek v plné velikosti](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Část 4 – Global.asax (upraveno s trasou kořenový)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Po povolení mapy skriptů se zástupnými znaky služby IIS 7.0 a IIS 6.0, můžete provést požadavků, které pracují s výchozí směrovací tabulka, které vypadat nějak takto:

/

/ Home/Index

/ Produkt/podrobnosti/3

/ Produktu

## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo vysvětlují, jak můžete ASP.NET MVC, jestli používáte starší verzi služby IIS (nebo službě IIS 7.0 v klasickém režimu). Jsme probírali dva způsoby získávání směrování ASP.NET pro práci se staršími verzemi služby IIS: Úprava výchozí směrovací tabulka nebo vytvoření mapy skriptů se zástupnými znaky.

První možnost vyžaduje úpravu adresy URL použité v aplikaci ASP.NET MVC. Velmi důležité výhod první možnost je nepotřebují přístup k webovému serveru pro úpravu směrovací tabulky. To znamená, že i v případě hostování vaší aplikace ASP.NET MVC pomocí Internetu můžete použít první možnost hostování společnosti.

Druhou možností je vytvořit mapu skriptů se zástupnými znaky. Výhodou této druhé možnosti je, že není potřeba měnit vaší adresy URL. Nevýhodou této druhou možností je, že může mít vliv na výkon vaší aplikace ASP.NET MVC.

> [!div class="step-by-step"]
> [Next](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
