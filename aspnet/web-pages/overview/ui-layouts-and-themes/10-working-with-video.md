---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Zobrazení Video v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs
author: tfitzmac
description: Tato kapitola vysvětluje postup zobrazení videa na webových stránkách ASP.NET stránky syntaxe Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153677"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Zobrazení Video v Web Pages (Razor) technologie ASP.NET
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak pokud chcete, aby uživatelům prohlížet videa, které jsou uloženy v lokalitě použijte přehrávač video (médium) na webu technologie ASP.NET Web Pages (Razor). Technologie ASP.NET Web Pages se syntaxí Razor umožňuje hrát Flash (*SWF*), přehrávač médií (*.wmv*) a Silverlight (*.xap*) videa.
> 
> Získáte informace:
> 
> - Jak vybrat přehrávání videa.
> - Jak přidat video na webovou stránku.
> - Jak nastavit atributy přehrávání videa.
> 
> Jedná se o syntaxi ASP.NET Razor stránky v článku nové funkce:
> 
> - `Video` Pomocné rutiny.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 2
> - Služba WebMatrix 2
>   
> 
> V tomto kurzu taky spolupracuje se službou WebMatrix 3.


## <a name="introduction"></a>Úvod

Můžete chtít zobrazit video na váš web. Jedním ze způsobů, které má odkaz k lokalitě, která již na video, jako je YouTube. Pokud chcete vložit video z těchto webů přímo do vlastní stránky, můžete obvykle získat kód HTML z lokality a zkopírujte jej do stránku. Například následující příklad ukazuje, jak pro vložení YouTube video:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Pokud chcete přehrát video, které je na vlastní web (nikoli na veřejný web sdílení video), nelze přímo propojit se pomocí vložených značek takto. Ale přehrávání videa z webu pomocí `Video` pomocné rutiny, která vykreslí přehrávač médií přímo v stránky.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Výběr přehrávání videa

Mnoho z formátů pro videosoubory a každý formát obvykle vyžaduje jiný přehrávač a jiný způsob, jak nakonfigurovat přehrávač. Na stránkách ASP.NET Razor můžete přehrát video webovou stránku pomocí `Video` pomocné rutiny. `Video` Pomocná zjednodušuje proces vložení videa na webové stránce, protože automaticky vygeneruje `object` a `embed` elementy HTML, které se obvykle používají k přidat video na stránku.

`Video` Pomocná podporuje následující přehrávače médií:

- Adobe Flash
- Windows Media Player
- Microsoft Silverlight

### <a name="the-flash-player"></a>Flash Player

`Flash` Přehrávač dle `Video` pomocné rutiny umožňují přehrávání videa Flash (*SWF* soubory) na webové stránce. Minimálně je třeba zadat cestu k souboru videa. Pokud zadáte nic ale cestu, přehrávač používá výchozí hodnoty, které jsou nastavené jako aktuální verze Flash. Typický výchozí nastavení je:

- Přehrávání videa se zobrazí pomocí jeho výchozí šířky a výšky a bez barvu pozadí.
- Video hraje automaticky při načtení stránky.
- Video opakovací nepřetržitě explicitně je zastavena.
- Video je škálovat zobrazíte všechna videa, nikoli oříznutí video podle konkrétní velikost.
- Přehrávání videa v okně.

### <a name="the-mediaplayer-player"></a>Přehrávač Media Player

`MediaPlayer` Přehrávač dle `Video` Pomocník umožňuje přehrávání videa Windows Media (*.wmv* soubory), Windows Media zvuk (*WMA* soubory) a MP3 (*MP3* soubory) na webové stránce. Musí zahrnovat cestu k souboru média k přehrání; všechny ostatní parametry jsou volitelné. Pokud zadáte pouze cestu, přehrávač používá výchozí nastavení, nastavte pomocí aktuální verze Media Player, jako je třeba:

- Zobrazí se na video, pomocí jeho výchozí šířka a výška.
- Video hraje automaticky při načtení stránky.
- Video přehraje jednou (ho nebude cykly).
- Přehrávač zobrazí úplnou sadu ovládacích prvků v uživatelském rozhraní.
- Přehrávání videa v okně.

### <a name="the-silverlight-player"></a>Přehrávač Silverlight

`Silverlight` Přehrávač dle `Video` Pomocník umožňuje přehrát Video média systému Windows (*.wmv* soubory), Windows Media Audio (*WMA* soubory) a MP3 (*MP3* soubory). Je nutné nastavit parametr path tak, aby odkazoval na balíček aplikace založená na technologii Silverlight (*.xap* souboru). Je třeba také nastavit parametry šířky a výšky. Všechny ostatní parametry jsou volitelné. Při použití Silverlight player pro video, pokud jste nastavili pouze požadované parametry, zobrazí přehrávač Silverlight video bez barvu pozadí.

> [!NOTE]
> V případě, že již neznáte Silverlight: *.xap* soubor je komprimovaný soubor, který obsahuje rozložení pokyny v *XAML* souboru spravovaným kódem v sestavení a volitelné prostředky. Můžete vytvořit *.xap* souborů v sadě Visual Studio jako projekt aplikace Silverlight.


`Silverlight` Přehrávání videa používá jak nastavení, které zadáte pro přehrávač a nastavení, které jsou součástí *.xap* souboru.

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Typy MIME
> 
> Pokud prohlížeč stáhne soubor, v prohlížeči zajišťuje, že typ souboru odpovídá typ MIME, který je zadán pro dokument, který je vykreslován. Typ MIME je typ obsahu typu nebo médium souboru. `Video` Pomocné rutiny používá následující typy MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Přehrávání videa Flash (SWF)

Tento postup ukazuje, jak k přehrávání videa Flash s názvem *sample.swf*. Postupu se předpokládá, že máte k dispozici složku s názvem *média* na váš web a který *SWF* soubor je v této složce.

1. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste to ještě už přidali.
2. Na webu, přidat stránku a pojmenujte ji *FlashVideo.cshtml*.
3. Přidejte následující kód na stránku: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Spusťte stránku v prohlížeči. (Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.) Stránka se zobrazí a video hraje automaticky. 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

Můžete nastavit `quality` parametr Flash Video k `low`, `autolow`, `autohigh`, `medium`, `high`, a `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Můžete změnit na Flash video přehrávání na konkrétní velikost pomocí `scale` parametr, který můžete nastavit na následující:

- `showall`. To zviditelní celé video původního poměru stran obrázku je zachován. Však může skončili s ohraničení na každé straně.
- `noorder`. Toto video škáluje původního poměru stran obrázku je zachován, ale může být oříznuty.
- `exactfit`. Díky tomu celé video viditelné bez zachování původní poměr stran, ale může dojít k narušení.

Pokud nezadáte `scale` parametr celé video budou viditelné, bez jakékoli oříznutí se zachová původní poměr stran. Následující příklad ukazuje způsob použití `scale` parametr:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash player podporuje video režim nastavení s názvem `windowMode`. Můžete nastavit na `window`, `opaque`, a `transparent`. Ve výchozím nastavení `windowMode` je nastaven na `window`, který zobrazuje videa v samostatném okně na webové stránce. `opaque` Nastavení skryje vše za video na webové stránce. `transparent` Nastavení umožňuje na pozadí webové stránky, zobrazit prostřednictvím video, za předpokladu, že je transparentní, jakékoliv části videa.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Přehrávání Media Player (*.wmv*) videa

Následující postup ukazuje, jak přehrát video média okno s názvem *sample.wmv* který se v *média* složky.

1. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.
2. Vytvořit novou stránku s názvem *MediaPlayerVideo.cshtml*.
3. Přidejte následující kód na stránku: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Spusťte stránku v prohlížeči. Video načte a hraje automaticky. 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

Můžete nastavit `playCount` na celé číslo, která určuje, jak často má přehrát video, automaticky:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode` Parametr slouží k určení ovládacích prvků, které zobrazí v uživatelském rozhraní. Můžete nastavit `uiMode` k `invisible`, `none`, `mini`, nebo `full`. Pokud nezadáte `uiMode` parametr, bude se na video zobrazí se stavové okno, Hledat panelu, řídit tlačítek a ovládacích prvků svazku kromě okně videa. Tyto ovládací prvky se zobrazí také v případě použití přehrávače pro přehrání zvukového souboru. Tady je příklad použití `uiMode` parametr:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Ve výchozím nastavení je zvuk na při přehrávání videa. Zvuk můžete ztlumení nastavením `mute` parametr na hodnotu true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Můžete řídit úroveň audio video Media Player nastavením `volume` parametr na hodnotu mezi 0 a 100. Výchozí hodnota je 50. Tady je příklad:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Přehrávání videa technologie Silverlight

Tento postup ukazuje, jak přehrát video, které jsou součástí Silverlight *.xap* stránky, která je ve složce s názvem *média*.

1. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.
2. Vytvořit novou stránku s názvem *SilverlightVideo.cshtml*.
3. Přidejte následující kód na stránku: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Spusťte stránku v prohlížeči. 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


[Přehled technologie Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Flash OBJEKT a vložení atributů značky](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM značky](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
