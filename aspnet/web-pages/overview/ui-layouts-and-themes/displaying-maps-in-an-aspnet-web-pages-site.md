---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: "Zobrazení mapy v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tento článek vysvětluje, jak zobrazit interaktivní mapy na stránkách na webu technologie ASP.NET Web Pages (Razor) podle mapování služeb poskytovaných Bing, Google, Ma..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 6f3e6a0cfb8c08cd971e88986d0f059dd8237aab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Zobrazení mapy v Web Pages (Razor) technologie ASP.NET
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak zobrazit interaktivní mapy na stránkách na webu technologie ASP.NET Web Pages (Razor) podle mapování služeb poskytovaných Bing, Google, MapQuest a Yahoo.
> 
> Získáte informace:
> 
> - Popisuje, jak Generovat mapu založené na adresu.
> - Popisuje, jak Generovat mapu podle zeměpisné šířky a délky.
> - Jak zaregistrovat účet služby Bing Maps Developer a získat klíč pro použití s mapy Bing.
> 
> Toto je funkce technologie ASP.NET byla zavedená v článku:
> 
> - `Maps` Pomocné rutiny.
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


Ve webových stránkách, můžete zobrazení mapy na stránce pomocí `Maps` pomocné rutiny. Můžete vygenerovat založené na adresu nebo na sadu zeměpisné šířky a délky souřadnice mapy. `Maps` Třída umožňuje volání do oblíbených mapy moduly včetně Bing, Google, MapQuest a Yahoo.

Postup přidání mapování na stránku jsou stejné bez ohledu na to, které z modulů mapy volání. Stačí přidat odkaz na soubor JavaScript, který umožňuje dostupné metody pro zobrazení mapy, a pak volání metody `Maps` pomocné rutiny.

Vybrat službu mapy závislosti na němž `Maps` Pomocná metoda, která používáte. Můžete použít některou z těchto:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Instalace součásti, které potřebujete

Chcete-li zobrazit mapy, je třeba tyto údaje:

- `Maps` Pomocné rutiny. Verze 2 knihovnu ASP.NET Web Helpers se tohoto pomocníka. Pokud jste ještě nepřidali knihovny, můžete ho nainstalovat ve vaší lokalitě jako balíčku NuGet. Podrobnosti najdete v tématu [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372). (V galerii, vyhledejte `microsoft-web-helpers` balíčku.)
- Knihovna jQuery. Několik šablon webu služby WebMatrix již zahrnují knihovny jQuery v jejich *skriptu* složek. Pokud jste tyto knihovny, si můžete stáhnout nejnovější knihovny jQuery přímo z [jQuery.org](http://jQuery.org) lokality. Nebo můžete vytvořit nové lokality pomocí šablony (například **Starter Site** šablony) a poté zkopírujte soubory jQuery z této lokality k aktuální lokalitě.

Nakonec pokud chcete použít mapy Bing, musíte nejprve vytvořit účet (zdarma) a získat klíč. Získat klíč, postupujte takto:

1. Vytvoření účtu na [Bing Maps vývojářský účet](https://www.microsoft.com/maps/developers/web.aspx). Musíte mít účet Microsoft (Windows Live ID) také.

    Můžete zadat, že chcete použít klíč pro **vyhodnocení a testovací**. Pokud testujete mapování funkce ve vašem počítači pomocí nástroje WebMatrix a služby IIS Express, přejděte **lokality** prostoru a poznamenejte si adresu URL vašeho webu (například `http://localhost:50408`, i když vaše číslo portu bude pravděpodobně lišit). Můžete to použít *localhost* adresu jako lokalita při registraci.
2. Po registraci pro účet, přejděte na účet Center pro mapy Bing a klikněte na tlačítko **vytvořit nebo zobrazení klíče**:

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Záznam klíč, který vytvoří Bing.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Vytváření mapy založené na adresu (pomocí Google)

Následující příklad ukazuje, jak vytvořit stránku, která vykreslí mapu založené na adresu. Tento příklad ukazuje, jak používat službu mapy Google.

1. Vytvořte soubor s názvem *MapAddress.cshtml* v kořenovém adresáři serveru. Tato stránka bude Generovat mapu založené na adresu, která můžete předat.
2. Zkopírujte následující kód do souboru, přepsání existujícího obsahu.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Všimněte si následujících funkcí stránky:

    - `<script>` Element v `<head>` elementu. V příkladu `<script>` element odkazy *jquery 1.6.4.min.js* souboru, který je minifikovaný (komprimované) verze knihovny jQuery, verze 1.6.4. Všimněte si, že odkaz na předpokládá, že *.js* soubor *skripty* složku vašeho webu. 

        > [!NOTE]
        > Pokud používáte jinou verzi knihovny jQuery, ujistěte se, že nastavíte ukazatel na příslušnou verzi správně.
    - Volání `@Maps.GetGoogleHtml` v těle stránky. Chcete-li mapování adresy, musíte zadat řetězec adresy. Metody pro jiné moduly mapy fungovat podobným způsobem (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
- Spuštění stránky a zadejte adresu. Na stránce zobrazuje mapě, podle Google Maps, který ukazuje umístění, které jste zadali.

    ![mapping-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Vytváření mapy podle zeměpisnou šířku a délku koordinuje (pomocí Bing)

Tento příklad ukazuje postup vytvoření mapy podle souřadnice. Tento příklad ukazuje, jak používat mapy Bing a jak se zahrnuje klíč Bing. (Můžete vytvořit mapu podle souřadnice také pomocí jiné mapy motory bez použití klíče služby Bing.)

1. Vytvořte soubor s názvem *MapCoordinates.cshtml* v kořenu lokality a nahradit existující obsah s následující kód a značky:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Nahraďte `your-key-here` klíčem mapy Bing, který jste vygenerovali dříve.
3. Spustit *MapCoordinates.cshtml* zadejte zeměpisné šířky a délky a pak klikněte na tlačítko **mapy ji!** tlačítko. (Pokud neznáte žádné souřadnice, zkuste následující postup. Toto je umístění v Redmondu Microsoft kanceláře.)

    - Zeměpisná šířka: 47.6781005859375
    - Zeměpisná délka:-122.158317565918

    Stránka se zobrazí pomocí souřadnice, které jste zadali.

    ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


[Referenční dokumentace rozhraní API Microsoft.Maps](https://msdn.microsoft.com/library/gg427611.aspx)
