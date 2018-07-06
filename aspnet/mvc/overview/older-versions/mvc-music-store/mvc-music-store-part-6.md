---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: '6. část: Používání datových poznámek k ověření modelu | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 6. část se věnuje anotacemi dat pro Model V...
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 10884f569f0f5d95517b73daab31fbd269a4726a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825950"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>6. část: Používání datových poznámek k ověření modelu
====================
podle [Jon Galloway](https://github.com/jongalloway)

> MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.  
>   
> Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.  
>   
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 6. část se věnuje datových poznámek k ověření modelu.


Máme závažný problém s naší vytvořit a upravit formuláři: Nejedná se žádné ověření. Můžeme udělat kroky, jako je nechat prázdné povinná pole nebo typ písmena v poli pro cenu a první chyba, kterou uvidíme se z databáze.

Jsme můžete snadno přidat ověření pro naši aplikaci tak, že přidáte datových poznámek k naší tříd modelu. Datové poznámky umožňují nám pro popis pravidla, která chceme, aby u našich vlastnosti modelu a ASP.NET MVC se postará o vynucení a zobrazení příslušné zprávy pro naše uživatele.

## <a name="adding-validation-to-our-album-forms"></a>Přidání ověřování do našich alba formulářů

Použijeme následující atributy dat. Poznámka:

- **Požadované** – označuje, že vlastnost je povinné pole.
- **DisplayName** – definuje text jsme má být použit na pole formuláře a ověřovacích zpráv
- **StringLength** – definuje maximální délku pole řetězce
- **Rozsah** – poskytuje maximální a minimální hodnoty pro číselné pole
- **Vytvoření vazby** – obsahuje pole, které chcete vyloučit nebo zahrnout při vytváření vazby hodnoty parametru nebo formuláře na vlastnosti modelu
- **ScaffoldColumn** – umožňuje skrýt pole z editoru formuláře

*Poznámka: Další informace o ověření modelu pomocí atributů dat poznámky, najdete v dokumentaci MSDN na*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Otevřete třídu alba a přidejte následující *pomocí* příkazy.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Dále aktualizujte vlastnosti, které chcete přidat atributy zobrazení a ověření, jak je znázorněno níže.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Když jsme už máte, jste změnili jsme taky žánr a interpreta virtuálních vlastností. Díky rozhraní Entity Framework pro opožděné načtení je podle potřeby.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Po těchto atributů s přidá náš model alb, naše vytvořit a Upravit obrazovku okamžitě začne ověřování polí a pomocí zobrazované názvy jsme zvolili (například alba obrázky místo adresu Url AlbumArtUrl). Spusťte aplikaci a přejděte do /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

V dalším kroku budete věcí některé ověřovací pravidla. Zadejte cenu 0 a nechte prázdný název. Když klikneme na tlačítka pro vytvoření, uvidíme formuláře, zobrazí se chybové zprávy ověření na zobrazení, která pole nevyhovuje ověřovacích pravidel, že jsme definovali.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testování ověřování na straně klienta

Ověřování na straně serveru je velmi důležité z pohledu aplikace, protože uživatelé mohou obejít ověřování na straně klienta. Webové formuláře, implementující pouze ověření na straně serveru ale vykazuje tři významné problémy.

1. Uživatel má počkat pro formulář k publikování, ověřit na serveru a pro odpověď k odeslání do svého prohlížeče.
2. Uživatel nezíská okamžitou zpětnou vazbu, když neopraví pole tak, aby teď projde ověřovacím pravidlům.
3. Můžeme se plýtvání prostředky serveru k provedení ověření logic namísto využití webového prohlížeče.

Naštěstí šablony vygenerované uživatelské rozhraní ASP.NET MVC 3 mají na straně klienta ověření integrované vyžadující žádné další kroky, které jsou jakýmkoli způsobem.

Zadáte jedno písmeno v poli s názvem splňuje požadavky ověřování, tak ověření zprávy se okamžitě odebere.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-5.md)
> [další](mvc-music-store-part-7.md)
