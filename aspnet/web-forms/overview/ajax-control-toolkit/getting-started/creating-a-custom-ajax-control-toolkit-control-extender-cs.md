---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Vytvoření vlastní AJAX – Extender ovládacího prvku Toolkit (C#) ovládacího prvku | Dokumentace Microsoftu
author: microsoft
description: Vlastní zařízení Extender umožňují přizpůsobit a rozšířit možnosti ovládacích prvků ASP.NET, bez nutnosti vytvářet nové třídy.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 0dd243ec1709324174ff48b52e7dfb5185d3aaa2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390950"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Vytvoření extenderu vlastní AJAX Control Toolkit ovládacího prvku (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> Vlastní zařízení Extender umožňují přizpůsobit a rozšířit možnosti ovládacích prvků ASP.NET, bez nutnosti vytvářet nové třídy.


V tomto kurzu se dozvíte, jak vytvořit vlastní – extender ovládacího prvku AJAX Control Toolkit. Vytvoříme jednoduchý, ale užitečné a nové rozšíření, která změní stav tlačítka ze zakázaného na povolený, když zadáte text do pole TextBox. Po přečtení tohoto kurzu, budete moct rozšířit ASP.NET AJAX Toolkit s vlastní ovládací prvek extenderů.

Můžete vytvořit vlastní ovládací prvek extenderů pomocí sady Visual Studio nebo Visual Web Developer (ujistěte se, že máte nejnovější verzi aplikace Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Přehled DisabledButton rozšiřujícího objektu

Naše nové zařízení extender ovládacího prvku má název rozšiřujícího objektu DisabledButton. Toto rozšíření bude mít tři vlastnosti:

- Vlastnost TargetControlID – textové pole, která rozšiřuje ovládací prvek.
- TargetButtonIID – tlačítko, které je zakázána nebo povolena.
- DisabledText – text, který je primárně zobrazen na tlačítku. Když začnete psát, na tomto tlačítku zobrazí hodnoty vlastnosti Text na tlačítku.

Můžete integrovat DisabledButton rozšiřujícího objektu do ovládacího prvku textového pole a tlačítko. Před zadáním textu je tlačítko neaktivní a do textového pole a tlačítko vypadat nějak takto:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))


Jakmile začnete zadávat text, tlačítko je povoleno a do textového pole a tlačítko vypadat nějak takto:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))


K vytvoření naší – extender ovládacího prvku, potřebujeme vytvořit následující tři soubory:

- DisabledButtonExtender.cs – tento soubor je třídě ovládacího prvku na straně serveru, který bude spravovat vytváření zařízení extender a umožní nastavit vlastnosti v době návrhu. Definuje také vlastnosti, které lze nastavit pro zařízení extender. Tyto vlastnosti jsou přístupné prostřednictvím kódu a v době návrhu a odpovídá vlastnosti definované v souboru DisableButtonBehavior.js.
- DisabledButtonBehavior.js--Tento soubor je, ve kterém přidáte všechny logiky skript klienta.
- DisabledButtonDesigner.cs – Tato třída umožňuje funkcích při návrhu. Pokud chcete, aby zařízení extender ovládacího prvku fungovat správně s Visual Studio nebo Visual Web Developer Designer potřebujete této třídy.

Extender ovládacího prvku tak se skládá z ovládacího prvku na straně serveru, chování na straně klienta a třídu návrháře na straně serveru. Se dozvíte, jak k vytvoření všech tří z těchto souborů v následujících částech.

## <a name="creating-the-custom-extender-website-and-project"></a>Vytvoření vlastního zařízení Extender webu a projektu

Prvním krokem je vytvoření projektu knihovny tříd a webu v aplikaci Visual Studio nebo Visual Web Developer. Jsme ll vytvoření vlastního zařízení extender v projektu knihovny tříd a otestování vlastního zařízení extender na webu.

Umožní začít s webem s. Postupujte podle těchto kroků můžete vytvořit na webu:

1. Vyberte možnost nabídky **soubor, nový web**.
2. Vyberte **Web ASP.NET s** šablony.
3. Pojmenujte nový web *web1*.
4. Klikněte na tlačítko **OK** tlačítko.

V dalším kroku potřeba vytvořit projekt knihovny tříd, který bude obsahovat kód – extender ovládacího prvku:

1. Vyberte možnost nabídky **soubor, přidat nový projekt**.
2. Vyberte **knihovny tříd** šablony.
3. Pojmenujte novou knihovnu tříd s názvem **CustomExtenders**.
4. Klikněte na tlačítko **OK** tlačítko.

Po dokončení těchto kroků okna Průzkumník řešení by měl vypadat jako obrázek 1.


[![Řešení pomocí projektu knihovny webu a třídy](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Obrázek 01**: řešení pomocí projektu knihovny webu a třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))


Dále je třeba přidat všechny potřebné reference sestavení do projektu knihovny tříd:

1. Klikněte pravým tlačítkem na projekt CustomExtenders a vyberte možnost nabídky **přidat odkaz**.
2. Vyberte kartu .NET.
3. Přidejte odkazy na následující sestavení:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Vyberte kartu Procházet.
5. Přidáte odkaz na sestavení AjaxControlToolkit.dll. Toto sestavení je umístěn ve složce, kam jste stáhli sadou nástrojů AJAX Control Toolkit.

Po dokončení těchto kroků, odkazy na složky projektu knihovny tříd by mělo vypadat jako na obrázku 2.


[![Složka s odkazy s požadované odkazy](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Obrázek 02**: složka s odkazy s odkazy na požadovaná ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Vytvoření extenderu vlastního ovládacího prvku

Teď, když jsme naše knihovny tříd, můžeme začít vytváření naší rozšiřující ovládací prvek. Umožní s začínat mnohastránkový třídy ovládacího prvku vlastního zařízení extender (viz seznam 1).

**Výpis 1 - MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Existuje několik věcí, které zjistíte informace o třídě zařízení extender ovládacího prvku v informacích 1. Napřed si všimněte, že třída dědí ze základní třídy ExtenderControlBase. Všechny rozšiřujících ovládacích prvků AJAX Control Toolkit odvozovat z této základní třídy. Základní třída patří třeba Id_cíle vlastnost, která se vyžaduje vlastnost každý – extender ovládacího prvku.

V dalším kroku Všimněte si, že třída zahrnuje následující dva atributy související s klientského skriptu:

- Zobrazení – způsobí, že soubor, který chcete zahrnout jako vložený prostředek v sestavení.
- ClientScriptResource - způsobí, že prostředek skriptu, který se má načíst ze sestavení.

Atribut webového prostředku se používá pro vložení souboru MyControlBehavior.js JavaScriptu do sestavení při kompilaci vlastního zařízení extender. Atribut ClientScriptResource slouží k načtení skriptu MyControlBehavior.js ze sestavení při použití vlastního zařízení extender na webové stránce.


Aby atributy webového prostředku a ClientScriptResource pracovat musíte zkompilovat soubor jazyka JavaScript jako vložený prostředek. Vyberte ho v okně Průzkumníka řešení, otevřete seznam vlastností a přiřaďte hodnotu *integrovaný prostředek* k **akce sestavení** vlastnost.


Všimněte si, že – extender ovládacího prvku také obsahuje atribut TargetControlType. Tento atribut slouží k určení typu ovládacího prvku, který je rozšířit pomocí – extender ovládacího prvku. V případě výpis 1 extender ovládacího prvku slouží k prodloužení textové pole.

Všimněte si, že vlastního zařízení extender obsahuje vlastnost s názvem MyProperty. Vlastnost je označena atributem ExtenderControlProperty. Metody GetPropertyValue() a SetPropertyValue() slouží k předání hodnotu vlastnosti ze zařízení extender ovládacího prvku na straně serveru k chování na straně klienta.

Umožní s pokračujte a implementujte kód pro naše DisabledButton zařízení extender. Kód pro toto rozšíření najdete v informacích 2.

**Výpis 2 - DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

Zařízení extender DisabledButton výpis 2 má dvě vlastnosti s názvem TargetButtonID a DisabledText. IDReferenceProperty použít pro vlastnost TargetButtonID brání přiřazení nic jiného než ID ovládacího prvku tlačítko slouží k této vlastnosti.

Atributy webového prostředku a ClientScriptResource přidružit nachází v souboru s názvem DisabledButtonBehavior.js se toto rozšíření chování na straně klienta. Podíváme na tento soubor jazyka JavaScript v další části.

## <a name="creating-the-custom-extender-behavior"></a>Vytvoření vlastního zařízení Extender chování

Komponentu na straně klienta – extender ovládacího prvku se nazývá chování. Skutečné logiku pro zakázání a povolení tlačítka je součástí DisabledButton chování. Kód jazyka JavaScript pro chování je součástí výpis 3.

**Výpis 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Soubor jazyka JavaScript v 3 výpis obsahuje třídu na straně klienta s názvem DisabledButtonBehavior. Tato třída, stejně jako jeho dvojčete na straně serveru zahrnuje dvě vlastnosti s názvem TargetButtonID a získat DisabledText, kterým můžete přistupovat pomocí\_TargetButtonID/set\_TargetButtonID a získejte\_DisabledText/set\_ DisabledText.

Metodu initialize() přidruží obslužnou rutinu události keyup cílového elementu chování. Spustí obslužnou rutinu keyup pokaždé, když zadáte nějaké písmeno, do textového pole přidružené k tomuto chování. Keyup obslužná rutina povolí nebo zakáže tlačítko v závislosti na tom, zda textové pole spojených s chováním obsahuje jakýkoli text.

Mějte na paměti, že je nutné kompilovat soubor jazyka JavaScript v informacích 3 jako vložený prostředek. Vyberte ho v okně Průzkumníka řešení, otevřete seznam vlastností a přiřaďte hodnotu *integrovaný prostředek* k **akce sestavení** vlastnosti (viz obrázek 3). Tato možnost je k dispozici v sadě Visual Studio a Visual Web Developer.


[![Přidání souboru jazyka JavaScript jako vložený prostředek](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Obrázek 03**: přidání souboru jazyka JavaScript jako vložený prostředek ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Vytvoření vlastního zařízení Extender návrháře

Existuje jeden poslední třídu, která potřebujeme vytvořit k dokončení našich zařízení extender. Musíme vytvořit třídu návrháře v informacích 4. Tato třída se požadovat, aby zařízení extender s Visual Studio nebo Visual Web Developer Designer se chovají správně.

**Část 4 – DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Návrhář v informacích 4 přidružíte DisabledButton rozšíření s atributem návrháře. Je třeba použít atribut návrháře pro třídu DisabledButtonExtender takto:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Pomocí vlastních rozšíření

Teď, když jsme dokončili, vytvoření extenderu ovládacího prvku DisabledButton, je čas ho použít v naší webové stránky ASP.NET. Nejdřív potřebujeme přidat vlastní rozšíření do panelu nástrojů. Postupujte podle těchto kroků:

1. Dvojitým kliknutím na stránce v okně Průzkumník řešení otevřete stránku ASP.NET.
2. Klikněte pravým tlačítkem na panelu nástrojů a vyberte možnost nabídky **zvolit položky**.
3. V dialogovém okně Zvolit položky panelu nástrojů přejděte na CustomExtenders.dll sestavení.
4. Klikněte na tlačítko **OK** tlačítka zavřete dialogové okno.

Po dokončení těchto kroků – extender ovládacího prvku DisabledButton objevit na panelu nástrojů (viz obrázek 4).


[![DisabledButton na panelu nástrojů](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Obrázek 04**: DisabledButton na panelu nástrojů ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))


Dále musíme vytvořit novou stránku ASP.NET. Postupujte podle těchto kroků:

1. Vytvořte novou stránku ASP.NET s názvem ShowDisabledButton.aspx.
2. Přetáhněte ovládací prvek ScriptManager na stránce.
3. Přetáhněte ovládací prvek textového pole na stránce.
4. Přetáhněte ovládací prvek tlačítko na stránku.
5. V okně Vlastnosti změňte vlastnost ID tlačítek na hodnotu <em>btnSave</em> a vlastnost textu na hodnotu *Uložit\**.
  

Na stránce jsme vytvořili pomocí standardního ovládacího prvku ASP.NET textového pole a tlačítko.

Dále musíme rozšířit ovládací prvek TextBox s DisabledButton rozšiřující objekt:

1. Vyberte **přidat zařízení Extender** úloh možnost otevření dialogového okna rozšíření průvodce (viz obrázek 5). Všimněte si, že dialogového okna obsahuje naše vlastního zařízení extender DisabledButton.
2. Vyberte zařízení extender DisabledButton a klikněte na tlačítko **OK** tlačítko.


[![Dialogové okno Průvodce rozšiřujícího objektu](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Obrázek 05**: dialogové okno Průvodce zařízení Extender ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))


Nakonec jsme můžete nastavit vlastnosti DisabledButton rozšiřujícího objektu. Vlastnosti DisabledButton rozšiřujícího objektu, který můžete upravit tak, že upravíte vlastnosti ovládacího prvku textového pole:

1. V Návrháři vyberte textového pole.
2. V okně Vlastnosti rozbalte uzel zařízení Extender (viz obrázek 6).
3. Přiřaďte hodnotu *Uložit* DisabledText vlastnosti a hodnotu *btnSave* TargetButtonID vlastnosti.


[![Nastavení vlastností rozšíření](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Obrázek 06**: nastavení vlastností zařízení extender ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))


Při spuštění stránky (stisknutím klávesy F5) ovládacího prvku tlačítko je zpočátku zakázáno. Jakmile začnete zadávat text do textového pole, tlačítko ovládací prvek je povoleno (viz obrázek 7).


[![Zařízení extender DisabledButton v akci](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Obrázek 07**: The DisabledButton rozšiřujícího objektu v akci ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))


## <a name="summary"></a>Souhrn

Cílem tohoto kurzu bylo vysvětlují, jak můžete rozšířit pomocí rozšíření vlastních ovládacích prvků AJAX Control Toolkit. V tomto kurzu jsme vytvořili jednoduchý – extender ovládacího prvku DisabledButton. Implementovali jsme toto rozšíření vytvořením třídy DisabledButtonExtender, chování DisabledButtonBehavior JavaScript a DisabledButtonDesigner třídy. Můžete sledovat podobnou sadu kroků při každém vytvoření rozšíření vlastního ovládacího prvku.

> [!div class="step-by-step"]
> [Předchozí](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [další](get-started-with-the-ajax-control-toolkit-vb.md)
