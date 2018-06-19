---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Vytváření a používání pomocné rutiny v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs
author: tfitzmac
description: Tento článek popisuje postup vytvoření pomocné rutiny na webu technologie ASP.NET Web Pages (Razor). Pomocné rutiny je opakovaně použitelné komponenty, která obsahuje kód a značky k výkonu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573304"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Vytváření a používání pomocné rutiny v stránku ASP.NET Web Pages (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje postup vytvoření pomocné rutiny na webu technologie ASP.NET Web Pages (Razor). A *pomocná* je znovu použitelné komponentní, která obsahuje kód a značky provést úlohu, která může být zdlouhavé nebo komplexní.
> 
> **Získáte informace:** 
> 
> - Postup vytvoření a použití jednoduché pomocné rutiny.
> 
> Toto jsou nové v článku funkce ASP.NET:
> 
> - `@helper` Syntaxe.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.


## <a name="overview-of-helpers"></a>Přehled pomocné rutiny

Pokud potřebujete k provádění stejných úloh na různých stránkách ve vaší lokalitě, můžete použít pomocné rutiny. Rozhraní ASP.NET Web Pages zahrnuje několik pomocné rutiny, a neexistují další, které můžete stáhnout a nainstalovat. (Seznam předdefinovaných Pomocníci na webových stránkách ASP.NET je uvedena ve [Stručná referenční příručka rozhraní API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Pokud žádná z existující Pomocníci nevyhovuje vašim potřebám, můžete vytvořit vlastní pomocné rutiny.

Pomocné rutiny vám umožní používat běžné bloku kódu na více stránkách. Předpokládejme, že na stránce často chcete vytvořit položku Poznámka, který je nastavený kromě Normální odstavce. Možná je poznámka vytvořen jako `<div>` element který naformátovat jako pole s ohraničení. Místo přidat tento stejný kód na stránku pokaždé, když chcete zobrazit Poznámka, můžete balíček kód jako pomocné rutiny. Pak můžete vložit Poznámka s jedním řádkem kódu potřebujete kdekoli.

Pomocí pomocné rutiny takto mohou kód v jednotlivých stránek, jednoduchost a lepší přehlednost ke čtení. Je také usnadňuje udržovat lokalitu, protože pokud potřebujete změnit vzhled poznámky, můžete změnit kód na jednom místě.

## <a name="creating-a-helper"></a>Vytváření pomocné rutiny

Tento postup ukazuje, jak vytvořit pomocné rutiny, která vytvoří Všimněte si, jak je popsáno právě. Toto je jednoduchý příklad, ale vlastního pomocného objektu může zahrnovat všechny značek a kódu ASP.NET, které potřebujete.

1. V kořenové složce webové stránky, vytvořte složku s názvem *aplikace\_kód*. Toto je název vyhrazené složky v technologii ASP.NET, kde můžete ukládat kód pro komponenty, jako jsou pomocné rutiny.
2. V *aplikace\_kód* vytvořte novou složku *.cshtml* souboru a pojmenujte ji *MyHelpers.cshtml*.
3. Nahradí existující obsah s následujícími službami:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Kód používá `@helper` syntaxe deklarace nového Pomocníka s názvem `MakeNote`. Tato konkrétní Pomocník umožňuje předat parametr s názvem `content` obsahující kombinaci text a značku. Pomocné rutiny vloží text do pomocí textu Poznámka `@content` proměnné.

    Všimněte si, že je soubor s názvem *MyHelpers.cshtml*, ale pomocné rutiny jmenuje `MakeNote`. Více vlastní pomocné rutiny můžete umístit do jednoho souboru.
4. Soubor uložte a zavřete.

## <a name="using-the-helper-in-a-page"></a>Na stránce využitím pomocné rutiny

1. V kořenové složce vytvořte nový prázdný soubor s názvem *TestHelper.cshtml*.
2. Přidejte následující kód do souboru:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    K volání pomocné rutiny, které jste vytvořili, použijte `@` následuje název souboru, kde pomocné rutiny jsou, tečku a potom název pomocné rutiny. (Pokud jste měli více složek *aplikace\_kód* složky, můžete použít syntaxi `@FolderName.FileName.HelperName` volat pomáhající v rámci všechny vnořené složky úroveň). Text, který přidáte do uvozovek v závorkách je text, který pomocníka, který se zobrazí jako součást Poznámka na webové stránce.
3. Uložit stránky a spusťte ji v prohlížeči. Generuje položku Poznámky právo kde jste volali metodu pomocný pomocné rutiny: mezi dva odstavce.

    ![Snímek obrazovky se stránkou v prohlížeči a způsob, jakým pomocné rutiny vygeneruje kód, který převádí pole kolem zadaný text.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Další prostředky


[Vodorovné nabídky jako pomocné rutiny Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Tuto položku blogu podle Karel Pope ukazuje, jak vytvořit vodorovné nabídky jako pomocné rutiny pomocí značek, CSS a kódu.

[Využití HTML5 v rozhraní ASP.NET Web Pages pomocníci pro službu WebMatrix a ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Tuto položku blogu podle Sam Jirásek ukazuje pomocné rutiny, který vykreslí HTML5 `Canvas` elementu.

[Rozdíl mezi @Helpers a @Functions ve službě WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Popisuje tato položka blogu podle Karel Brind `@helper` syntaxe a `@function` syntaxe a kdy každý použít.
