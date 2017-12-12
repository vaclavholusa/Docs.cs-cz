---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: "Část 6: Pomocí datových poznámek pro ověření modelu | Microsoft Docs"
author: jongalloway
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 6 obsahuje pomocí datových poznámek pro Model V..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b2083d5604741a0142f504f92779c9f931d0d6d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Část 6: Pomocí datových poznámek pro ověření modelu
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.  
>   
> Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.  
>   
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 6 obsahuje pomocí datových poznámek pro ověření modelu.


Máme závažný problém s formuláři naše vytvořit a upravit: jejich nejsou probíhá žádné ověření. Provedeme akce, jako je ponechat typ písmena a povinná pole prázdné pole Cena a první chyba, kterou jsme zobrazí je z databáze.

Můžete snadno přidáme ověření k naší aplikaci přidáním datových poznámek k naší třídy modelu. Nám pro popis pravidla, která má být u naše vlastnosti modelu umožňují datových poznámek a ASP.NET MVC se postará o jejich vynucením a zobrazení příslušné zprávy pro naše uživatele.

## <a name="adding-validation-to-our-album-forms"></a>Přidání ověřování do našich Album formulářů

Použijeme následující atributy datové poznámky:

- **Požadované** – označuje, že vlastnost je povinné pole.
- **DisplayName** – definuje text chceme použité na pole formuláře a ověřovacích zpráv
- **StringLength** – definuje maximální délku pole řetězce
- **Rozsah** – poskytuje maximální a minimální hodnotu pro číselné pole
- **Vytvoření vazby** – obsahuje seznam polí pro vyloučení nebo zahrnutí při vytváření vazby parametru nebo formuláře hodnoty pro vlastnosti modelu
- **ScaffoldColumn** – umožňuje skrytí pole z editoru formulářů

*Poznámka: Další informace o ověření modelu pomocí datové poznámky atributů najdete v dokumentaci k webu MSDN na*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Otevřete Album třídu a přidejte následující *pomocí* příkazů do horní části.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Potom aktualizujte vlastnosti, které chcete přidat atributy zobrazení a ověření, jak je uvedeno níže.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Když jsme existuje, jsme vám také změnili Genre a umělcem virtuální vlastnosti. To umožňuje rozhraní Entity Framework opožděné zatížení je podle potřeby.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Po tyto atributy s přidá do našich modelu alb, naše vytvořit a upravit obrazovky okamžitě začne ověřování pole a pomocí zobrazované názvy jsme jste vybrali (například Album obrázky Url místo AlbumArtUrl). Spusťte aplikaci a přejděte do /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Dále budete jsme rozdělit některá pravidla ověřování. Zadejte cena 0 a ponechejte pole název prázdné. Když jsme klikněte na tlačítko pro vytvoření, vidíte formuláře, zobrazí se chybové zprávy ověření zobrazující pole, která nesplňuje pravidla ověření, že jsme definovali.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testování ověřování straně klienta

Ověřování na straně serveru je velmi důležité z perspektivy aplikaci, protože uživatelé mohou obejít ověřování na straně klienta. Webová stránka formulářů, implementující pouze ověření na straně serveru však vykazovat tři významné problémy.

1. Uživatel má počkat pro daný formulář, který se má odeslat, ověřit na serveru a pro odpověď k odeslání do svého prohlížeče.
2. Uživatel nemá získat okamžitou zpětnou vazbu, při jejich opravte pole tak, aby ho teď předá ověřovacích pravidel.
3. Nemůžeme se plýtvání prostředky serveru k provedení logiku ověření místo využívání prohlížeče uživatele.

Naštěstí šablony vygenerované uživatelské rozhraní ASP.NET MVC 3 mají ověřování na straně klienta integrovanou, vyžadování jakkoli další práci.

Zadáním jednoho písmeno v poli s názvem splňuje požadavky na ověření, zobrazí se zpráva ověření se okamžitě odebere.

![](mvc-music-store-part-6/_static/image3.png)


>[!div class="step-by-step"]
[Předchozí](mvc-music-store-part-5.md)
[další](mvc-music-store-part-7.md)
