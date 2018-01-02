---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: "Pomocí ovládacích prvků Toolkit řízení AJAX a řízení Extender (VB) | Microsoft Docs"
author: microsoft
description: "Informace o postupu přidání ovládacích prvků sadu ovládacích prvků AJAX a rozšíření na stránky ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b248855a1b82f3e8f172b439ee36502f95a39ca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Pomocí ovládacích prvků Toolkit řízení AJAX a řízení Extender (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> Informace o postupu přidání ovládacích prvků sadu ovládacích prvků AJAX a rozšíření na stránky ASP.NET.


Toolkitu AJAX obsahuje sadu ovládacích prvků a řízení rozšíření. V tomto kurzu stručný zjistěte, jak přidat ovládací prvky a Extender ovládacího prvku do stránky ASP.NET.

> [!NOTE] 
> 
> Pokyny k instalaci Toolkitu AJAX a přidání Toolkitu AJAX do sady nástrojů Visual Studio nebo Visual Web Developer naleznete v kurzu [začít pracovat s Toolkitu AJAX](get-started-with-the-ajax-control-toolkit-vb.md).


## <a name="using-ajax-control-toolkit-controls"></a>Použití ovládacích prvků Toolkit AJAX ovládací prvek

Ovládací prvek sadu ovládacích prvků AJAX funguje stejně jako normální ovládací prvek ASP.NET. Můžete přetáhnout ovládací prvek z panelu nástrojů na stránky ASP.NET. Ovládací prvek můžete přidat na stránku v návrhovém zobrazení nebo zobrazení zdroje.

Při použití ovládacích prvků z Toolkitu AJAX je jeden speciální požadavek. Stránka musí obsahovat ovládací prvek ScriptManager. Ovládací prvek ScriptManager zodpovídá za včetně všechny nezbytné JavaScript vyžaduje sadu ovládacích prvků AJAX ovládací prvky.

Například na kartě AJAX Control Toolkit obsahuje ovládací prvek s názvem ovládacího prvku Editor. Tento ovládací prvek zobrazí bohatě vybavený editor HTML. Použijte následující postup přidání ovládacího prvku Editor na stránku:

1. Vytvořit novou stránku ASP.NET s názvem ShowEditor.aspx
2. V panelu nástrojů vyberte ovládací prvek ScriptManager from beneath karty rozšíření AJAX a přetáhněte ovládacího prvku na stránku.
3. Vyberte ovládací prvek Editor from beneath kartě sadu ovládacích prvků AJAX v panelu nástrojů a přetáhněte ovládací prvek na stránce (viz obrázek 1). Návrhář by měl vypadat jako na obrázku 2.
4. Spusťte web tak, že vyberete možnost nabídky **ladit, spustit ladění** nebo stiskne klávesu F5.
5. Měli byste vidět stránky na obrázku 3.


[![Výběr ovládacího prvku HTML Editor](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Obrázek 01**: výběr ovládacího prvku HTML Editor ([Kliknutím zobrazit obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))


[![Návrháři sady Visual Studio s ovládací prvek ScriptManager a úpravy](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Obrázek 02**: návrháři sady Visual Studio s ovládací prvek ScriptManager a úpravy ([Kliknutím zobrazit obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))


[![Stránka DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Obrázek 03**: stránka DisplayEditor.aspx ([Kliknutím zobrazit obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>Pomocí AJAX řízení Toolkit řízení Extender

Toolkitu AJAX také obsahuje Extender ovládacího prvku. Jak naznačuje název rozšiřujícího objektu ovládacího prvku rozšiřuje funkce existujícího ovládacího prvku. Například rozšiřujícího objektu ovládacího prvku ConfirmButton rozšiřuje standardního ovládacího prvku tlačítko ASP.NET. Zařízení extender změní chování s ovládací prvek tlačítko tak, aby na tlačítko se zobrazí dialogové okno potvrzení po kliknutí na tlačítko.

Ovládací prvek rozšiřujícího objektu, stejně jako ovládací prvek sadu ovládacích prvků AJAX vyžaduje ovládací prvek ScriptManager. Ovládací prvek ScriptManager musí přidat na stránku, než začnete používat ovládací prvek Extender na stránce.

Postupujte podle těchto kroků pro použití rozšíření ConfirmButton ovládacího prvku:

1. Vytvořit novou stránku ASP.NET s názvem ShowConfirmButton.aspx
2. Přidáte ovládací prvek ScriptManager na stránku přetažením ovládacího prvku na stránce from beneath karty rozšíření AJAX.
3. Přidáte standardního ovládacího prvku tlačítko na stránku přetažením tlačítko from beneath standardní karta v panelu nástrojů na plochu návrháře.
4. Klikněte **přidat rozšiřujícího objektu** úloh možnost (viz obrázek 4).
5. V dialogovém okně Zvolit rozšíření, vyberte ConfirmButtonExtender (viz obrázek 5) a klikněte na tlačítko OK.
6. Vyberte ovládací prvek tlačítko v návrháři a rozbalte Extender, Button1\_ConfirmButtonExtender uzlu v okně vlastností (viz obrázek 6). Přiřadí hodnotu *skutečně?* ConfirmText vlastnosti.
7. Spuštění stránky tak, že vyberete možnost nabídky **ladit, spustit ladění** nebo stiskněte klávesu F5.


[![Možnost Přidat rozšiřujícího objektu úlohy](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Obrázek 04**: rozšiřujícího objektu přidat úloha možnost ([Kliknutím zobrazit obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))


[![Výběr rozšiřujícího objektu ovládacího prvku ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Obrázek 05**: výběr rozšiřujícího objektu ovládacího prvku ConfirmButton ([Kliknutím zobrazit obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))


[![Nastavení vlastnosti ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Obrázek 06**: nastavení vlastnosti ConfirmButton ([Kliknutím zobrazit obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))


Po otevření stránky, měli byste vidět tlačítko. Když kliknete na tlačítko, zobrazí dialogové okno pro potvrzení na obrázku 7.


[![Zobrazení dialogového okna potvrzení](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Obrázek 07**: zobrazení dialogového okna potvrzení ([Kliknutím zobrazit obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))


Všimněte si, že obvykle není přetažení rozšiřujícího objektu ovládacího prvku na stránku. Místo toho použít **přidat rozšiřujícího objektu** úloh možnost Přidat extender do ovládacího prvku, který jste už přidali na stránku. Všimněte si kromě toho nastavit řízení rozšiřujícího objektu vlastnosti otevřením seznamu vlastností pro ovládací prvek rozšiřovanou.

Jeden ovládací prvek ASP.NET můžete rozšířit pomocí více Extender ovládacího prvku. Seznam vlastností ovládacího prvku rozšiřovanou zobrazí seznam všech Extender ovládací prvek přidružený k ovládacímu prvku.

>[!div class="step-by-step"]
[Předchozí](get-started-with-the-ajax-control-toolkit-vb.md)
[další](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
