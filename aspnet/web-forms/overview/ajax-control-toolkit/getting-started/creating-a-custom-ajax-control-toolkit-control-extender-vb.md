---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: "Vytvoření vlastní AJAX ovládacího prvku rozšiřujícího objektu řízení Toolkit (VB) | Microsoft Docs"
author: microsoft
description: "Vlastní Extender umožňují přizpůsobit a rozšířit možnosti ovládacích prvků technologie ASP.NET, aniž by bylo nutné vytvořit nové třídy."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3e8fceb3c7570aa1bf085c8e1037736254e74ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Vytváření vlastních AJAX řízení Toolkit řízení Extender (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> Vlastní Extender umožňují přizpůsobit a rozšířit možnosti ovládacích prvků technologie ASP.NET, aniž by bylo nutné vytvořit nové třídy.


V tomto kurzu zjistěte, jak vytvořit vlastní extender řízení sadu ovládacích prvků AJAX. Vytvoříme jednoduchou, ale užitečný, nové rozšiřujícího objektu, který změní stav tlačítko ze zakázaného na povolený, když zadáte text do textové pole. Po přečtení tohoto kurzu, bude moct rozšířit s Extender vlastního ovládacího prvku ASP.NET AJAX Toolkit.

Můžete vytvořit vlastní ovládací prvek Extender pomocí sady Visual Studio nebo Visual Web Developer (ujistěte se, že máte nejnovější verzi aplikace Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Přehled DisabledButton rozšiřujícího objektu

Naše nové rozšiřujícího objektu řízení jmenuje DisabledButton rozšiřujícího objektu. Tato rozšiřujícího objektu bude mít tři vlastnosti:

- TargetControlID – textové pole, které rozšiřuje ovládacího prvku.
- TargetButtonIID – tlačítko, které je zakázat nebo povolit.
- DisabledText – text, který je zobrazena v tlačítko. Když začnete psát, tlačítko zobrazí hodnotu vlastnosti Text tlačítka.

Můžete napojit rozšiřujícího objektu DisabledButton do ovládacího prvku textového pole a tlačítko. Před zadáním textu je zakázané tlačítko a textové pole a tlačítko vypadat například takto:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))


Až začnete psát text, tlačítko je k dispozici a textové pole a tlačítko vypadat například takto:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))


K vytvoření naše rozšiřujícího objektu ovládací prvek, je potřeba vytvořit následující tři soubory:

- DisabledButtonExtender.vb – tento soubor je třída prvek na straně serveru, která bude spravovat vaše rozšiřujícího objektu pro vytváření a vám umožní nastavit vlastnosti v době návrhu. Definuje také vlastnosti, které lze nastavit u vašeho rozšiřujícího objektu. Tyto vlastnosti jsou přístupné prostřednictvím kódu a v době návrhu a odpovídat vlastnosti definované v souboru DisableButtonBehavior.js.
- DisabledButtonBehavior.js – Tento soubor je, kde bude přidejte všechny logika klientský skript.
- DisabledButtonDesigner.vb - Tato třída umožňuje návrh – funkce. Tato třída je nutné, pokud chcete, aby rozšiřujícího objektu ovládacího prvku pomocí Visual Studio nebo Visual Web Developer návrháře fungovala správně.

Proto rozšiřujícího objektu ovládacího prvku se skládá z ovládacího prvku na straně serveru, chování klienta a třídu návrháře straně serveru. Zjistíte, jak vytvořit všechny tři těchto souborů v následujících částech.

## <a name="creating-the-custom-extender-website-and-project"></a>Vytvoření vlastního webu rozšiřujícího objektu a projektu

Prvním krokem je vytvoření projektu knihovny tříd a webu v sadě Visual Studio nebo Visual Web Developer. Jsme budete vytvářet vlastní rozšiřujícího objektu v projektu knihovny tříd a testovat vlastní rozšiřujícího objektu na webu.

Umožní s začínat na webu. Postupujte podle těchto kroků můžete vytvořit na webu:

1. Vyberte možnost nabídky **soubor, nový web**.
2. Vyberte **webové stránky ASP.NET** šablony.
3. Název nového webu *web1*.
4. Klikněte **OK** tlačítko.

Dále je potřeba vytvořit projektu knihovny tříd, která bude obsahovat kód pro ovládací prvek rozšiřujícího objektu:

1. Vyberte možnost nabídky **souboru, přidat nový projekt**.
2. Vyberte **knihovny tříd** šablony.
3. Název nové třídy knihovny s názvem **CustomExtenders**.
4. Klikněte **OK** tlačítko.

Po dokončení těchto kroků vaše okna Průzkumníka řešení by mělo vypadat jako obrázek 1.


[![Řešení s projektem knihovny webu a – třída](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Obrázek 01**: řešení s projektem knihovny webu a třídy ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))


Dále je nutné přidat všechny potřebné sestavení odkazy na projekt knihovny tříd:

1. Klikněte pravým tlačítkem na projekt CustomExtenders a vyberte možnost nabídky **přidat odkaz na**.
2. Vyberte kartu .NET.
3. Přidejte odkazy na následující sestavení:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Vyberte kartu Procházet.
5. Přidáte odkaz na sestavení AjaxControlToolkit.dll. Toto sestavení se nachází ve složce, kam jste stáhli Toolkitu AJAX.

Můžete ověřit, že jste všechny odkazy na pravém přidali pravým tlačítkem myši na projekt, vyberte vlastnosti a kliknutím na kartě odkazy (viz obrázek 2).


[![Odkazy na složku se vyžaduje odkazy](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Obrázek 02**: odkazy na složku se vyžaduje odkazy ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Vytvoření vlastního ovládacího prvku rozšiřujícího objektu

Teď, když máme naše knihovny tříd, můžeme začít vytváření naše rozšiřující ovládací prvek. Umožní s začínat mnohastránkový vlastní rozšiřujícího objektu třídy ovládacího prvku (viz seznam 1).

**Výpis 1 - MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Existuje několik věcí, které jste si všimli o třídě ovládacího prvku rozšiřujícího objektu v výpis 1. První, Všimněte si, že třídy dědí vlastnosti ze základní třídy ExtenderControlBase. Všechny prvky rozšiřujícího objektu AJAX Control Toolkit odvozovat z této základní třídy. Například základní třída obsahuje %{targetid/ vlastnost, která se vyžaduje vlastnost každých rozšiřujícího objektu ovládacího prvku.

V dalším kroku Všimněte si, že třída zahrnuje následující dva atributy související s klientského skriptu:

- Webového prostředku - způsobí, že soubor, který chcete-li být zahrnuta jako vložený prostředek v sestavení.
- ClientScriptResource - způsobí, že prostředek skript má být načtena ze sestavení.

Atribut webového prostředku slouží k vložení soubor MyControlBehavior.js JavaScript do sestavení při kompilaci vlastní rozšiřujícího objektu. Atribut ClientScriptResource slouží k načtení skriptu MyControlBehavior.js od sestavení, pokud se používá vlastní rozšiřujícího objektu na webové stránce.


Aby atributy webového prostředku a ClientScriptResource postup musíte zkompilovat soubor JavaScript jako vložený prostředek. Vyberte soubor v okně Průzkumníka řešení, otevřete seznam vlastností a přiřadí hodnotu *vložený prostředek* k **akce sestavení** vlastnost.


Všimněte si, že rozšiřujícího objektu ovládací prvek také obsahuje atribut TargetControlType. Tento atribut slouží k určení typu ovládacího prvku rozšířený o rozšiřujícího objektu ovládacího prvku. V případě výpis 1 rozšiřujícího objektu ovládacího prvku slouží k prodloužení textové pole.

Všimněte si, že vlastní rozšiřujícího objektu obsahuje vlastnost s názvem MyProperty. Je vlastnost označena atributem ExtenderControlProperty. Metody GetPropertyValue() a SetPropertyValue() se používají k předat hodnotu vlastnosti z rozšiřujícího objektu serverové řízení chování klienta.

Umožní s pokračujte a implementovat kód pro naše DisabledButton rozšiřujícího objektu. Kód pro tento rozšiřujícího objektu naleznete v výpis 2.

**Výpis 2 - DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

Rozšiřujícího objektu DisabledButton v výpis 2 má dvě vlastnosti s názvem TargetButtonID a DisabledText. IDReferenceProperty použité na vlastnost TargetButtonID zabrání jakoukoli jinou hodnotu než ID ovládacího prvku tlačítko přiřazení k této vlastnosti.

Atributy webového prostředku a ClientScriptResource přidružit chování klienta nachází v souboru s názvem DisabledButtonBehavior.js se tento rozšiřujícího objektu. Tento soubor JavaScript v další části probereme.

## <a name="creating-the-custom-extender-behavior"></a>Vytváření vlastních rozšiřujícího objektu chování

Komponenta klienta rozšiřujícího objektu ovládacího prvku se nazývá chování. Skutečné logiku pro zakázání a povolení tlačítko je obsažený v DisabledButton chování. Kód jazyka JavaScript pro chování je součástí výpis 3.

**Výpis 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

Soubor JavaScript v výpis 3 obsahuje třídy klienta s názvem DisabledButtonBehavior. Tato třída jako jeho twin serverové zahrnuje dvě vlastnosti s názvem TargetButtonID a získat DisabledText, kterým můžete přistupovat pomocí\_TargetButtonID nebo nastavení\_TargetButtonID a získat\_DisabledText nebo nastavení\_ DisabledText.

Metoda inicializaci() přidruží obslužnou rutinu události keyup cílový element chování. Pokaždé, když zadáte písmeno do textového pole přidružené k tomuto chování, zpracuje obslužná rutina keyup. Obslužná rutina KeyUp – povolí nebo zakáže tlačítko v závislosti na tom, jestli do textového pole přidružené chování obsahuje jakýkoli text.

Mějte na paměti, že je nutné zkompilovat soubor JavaScript v výpis 3 jako vložený prostředek. Vyberte soubor v okně Průzkumníka řešení, otevřete seznam vlastností a přiřadí hodnotu *vložený prostředek* k **akce sestavení** vlastnost (viz obrázek 3). Tato možnost je k dispozici v sadě Visual Studio a aplikace Visual Web Developer.


[![Přidání souboru jazyka JavaScript jako vloženého prostředku](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Obrázek 03**: přidání souboru jazyka JavaScript jako vložený prostředek ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Vytváření vlastních rozšiřujícího objektu návrháře

Neexistuje jeden poslední třída, která potřebujeme k vytvoření k dokončení naše rozšiřujícího objektu. Musíme vytvořit třídu návrháře výpis 4. Tato třída je nutná ke správnému rozšiřujícího objektu chovat správně pomocí Visual Studio nebo Visual Web Developer návrháře.

**Výpis 4 - DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Návrhář v výpis 4 přidružíte rozšiřujícího objektu DisabledButton s atributu Designer. Je třeba použít atributu Designer k třídě DisabledButtonExtender takto:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Pomocí vlastních rozšiřujícího objektu

Teď, když jsme dokončili vytváření rozšiřujícího objektu ovládacího prvku DisabledButton, je čas pro použití v našem webu ASP.NET. Nejdřív je potřeba přidat vlastní rozšiřujícího objektu do sady nástrojů. Postupujte podle těchto kroků:

1. Otevřete stránku ASP.NET poklepáním na stránku okna Průzkumníka řešení.
2. Klikněte pravým tlačítkem na panelu nástrojů a vyberte možnost nabídky **zvolit položky**.
3. V dialogovém okně Zvolit položky nástrojů vyhledejte CustomExtenders.dll sestavení.
4. Klikněte **OK** tlačítko zavřete dialogové okno.

Po dokončení těchto kroků, by se rozšiřujícího objektu ovládacího prvku DisabledButton v panelu nástrojů (viz obrázek 4).


[![DisabledButton v panelu nástrojů](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Obrázek 04**: DisabledButton v panelu nástrojů ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))


Dále je potřeba vytvořit novou stránku ASP.NET. Postupujte podle těchto kroků:

1. Vytvořte novou stránku ASP.NET s názvem ShowDisabledButton.aspx.
2. Přetáhněte ovládací prvek ScriptManager na stránku.
3. Přetáhněte ovládací prvek textové pole na stránce.
4. Přetáhněte ovládací prvek tlačítko na stránce.
5. V okně vlastností změňte na hodnotu vlastnosti ID tlačítko *btnSave* a vlastnost Text na hodnotu *Uložit\**.
  

Na stránce jsme vytvořili pomocí standardního ovládacího prvku ASP.NET TextBox a tlačítko.

Dále je potřeba rozšířit ovládacího prvku textového pole s DisabledButton rozšiřujícího objektu:

1. Vyberte **přidat rozšiřujícího objektu** úloh možnost otevřete dialogové okno Průvodce rozšiřujícího objektu (viz obrázek 5). Všimněte si, že dialogové okno obsahuje naše vlastní DisabledButton rozšiřujícího objektu.
2. Vyberte rozšiřujícího objektu DisabledButton a klikněte na **OK** tlačítko.


[![Dialogové okno Průvodce rozšiřujícího objektu](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Obrázek 05**: dialogové okno Průvodce rozšiřujícího objektu ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))


Nakonec jsme můžete nastavit vlastnosti DisabledButton rozšiřujícího objektu. Vlastnosti rozšiřujícího objektu DisabledButton můžete upravit změnou vlastností ovládacího prvku TextBox:

1. Vyberte textové pole v návrháři.
2. V okně vlastností rozbalte uzel Extender (viz obrázek 6).
3. Přiřadí hodnotu *Uložit* vlastnost DisabledText a hodnota *btnSave* TargetButtonID vlastnosti.


[![Nastavení vlastností rozšiřujícího objektu](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Obrázek 06**: nastavení vlastností rozšiřujícího objektu ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))


Při spuštění stránky (s stále mačkat F5), ovládacího prvku tlačítko původně vypnutá. Jakmile začnete zadají text do textového pole, je ovládací prvek tlačítko povoleno (viz obrázek 7).


[![Rozšiřujícího objektu DisabledButton v akci](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Obrázek 07**: rozšiřujícího objektu DisabledButton v akci ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))


## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo vysvětlují, jak můžete rozšířit Toolkitu AJAX s rozšiřující vlastní ovládací prvky. V tomto kurzu jsme vytvořili jednoduché rozšiřujícího objektu prostředí DisabledButton ovládacího prvku. Implementovali jsme tento rozšiřujícího objektu vytvořením třídy DisabledButtonExtender, chování DisabledButtonBehavior JavaScript a třídu DisabledButtonDesigner. Pokaždé, když vytvoříte vlastní ovládací prvek extender provedením podobnou sadu kroků.

>[!div class="step-by-step"]
[Předchozí](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
