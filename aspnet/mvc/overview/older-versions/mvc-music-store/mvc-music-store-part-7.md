---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: "Část 7: Členství a autorizace | Microsoft Docs"
author: jongalloway
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 7 popisuje členství a autorizaci."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: db459de687db862be00a9b59ff5b1b238fa75061
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
<a name="part-7-membership-and-authorization"></a>Část 7: Členství a autorizace
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.  
>   
> Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.  
>   
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 7 popisuje členství a autorizaci.


Správce obchodu kontroleru je nyní k dispozici všem uživatelům, kteří navštěvují náš web. Umožňuje změnit to omezit oprávnění na správce webu.

## <a name="adding-the-accountcontroller-and-views"></a>Přidání zobrazení a AccountController

Jeden rozdíl mezi kompletní šablonou webové aplikace ASP.NET MVC 3 a šablony ASP.NET MVC 3 prázdný webové aplikace je, že prázdné šablonu neobsahuje řadič účet. Vytvoření účtu řadiče přidáme zkopírováním několik souborů z nové aplikace ASP.NET MVC z úplné šablony webové aplikace ASP.NET MVC 3.

Vytvoření nové aplikace ASP.NET MVC pomocí úplné šablony webové aplikace ASP.NET MVC 3 a zkopírujte následující soubory do stejných adresářích v našem projektu:

1. Zkopírujte AccountController.cs v adresáři řadiče
2. Zkopírujte AccountModels v adresáři modely
3. Vytvořte adresář účet uvnitř zobrazení adresáře a zkopírujte všechny čtyři zobrazení

Změňte obor názvů pro třídy Controller a modelu, tak budou začínat MvcMusicStore. Třída AccountController by měl použít obor názvů MvcMusicStore.Controllers a třída AccountModels by měl použít obor názvů MvcMusicStore.Models.

*Poznámka: Tyto soubory jsou také k dispozici při stahování MvcMusicStore Assets.zip, ze kterého jsme zkopírovat soubory návrhu naše lokality na začátku tohoto kurzu. Členství soubory jsou umístěny v adresáři kódu.*

Aktualizované řešení by měl vypadat asi takto:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Přidání správce s lokalitou konfigurace ASP.NET

Než budeme autorizace vyžadovat v našem webu, budeme potřebovat pro vytvoření uživatele s přístupem. Nejjednodušší způsob, jak vytvořit uživateli se má používat integrované konfigurace ASP.NET Web.

Konfigurace technologie ASP.NET Web spusťte kliknutím na následující ikona v Průzkumníku řešení.

![](mvc-music-store-part-7/_static/image2.png)

Spustí se konfigurace webu. Klikněte na kartě zabezpečení na domovské obrazovce a potom klikněte na odkaz "Povolit role" v centru obrazovky.

![](mvc-music-store-part-7/_static/image3.png)

Klikněte na odkaz "Vytvořit nebo spravovat role".

![](mvc-music-store-part-7/_static/image4.png)

Zadejte název role "Správce" a klikněte na tlačítko Přidat roli.

![](mvc-music-store-part-7/_static/image5.png)

Klikněte na tlačítko Zpět a potom klikněte na odkaz vytvořit uživatele na levé straně.

![](mvc-music-store-part-7/_static/image6.png)

Vyplňte pole informace uživatele na levé straně pomocí následujících informací:

| **Pole** | **Hodnota** |
| --- | --- |
| **Uživatelské jméno** | Správce |
| **Heslo** | password123! |
| **Potvrzení hesla** | password123! |
| **E-mail** | (všechny e-mailová adresa bude fungovat.) |
| **Bezpečnostní otázku** | (ať je třeba) |
| **Zabezpečení odpovědí** | (ať je třeba) |

*Poznámka: Samozřejmě můžete žádné heslo, které si přejete. Výchozí nastavení zabezpečení heslo vyžadovat zadání hesla, která má délku 7 znaků dlouhé a obsahuje jeden jiný než alfanumerický znak.*

Vyberte roli správce pro tohoto uživatele a klikněte na tlačítko Vytvořit uživatele.

![](mvc-music-store-part-7/_static/image7.png)

V tomto okamžiku zobrazí zprávu s upozorněním, že byl uživatel vytvořen úspěšně.

![](mvc-music-store-part-7/_static/image8.png)

Teď můžete zavřít okno prohlížeče.

## <a name="role-based-authorization"></a>Ověřování na základě rolí

Nyní jsme můžete omezit přístup k určení, že uživatel musí být v roli správce pro přístup k žádné akci kontroleru ve třídě StoreManagerController pomocí atributu [Authorize].

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Poznámka: Atribut [autorizovat] můžete umístit na konkrétní akce metody i na úrovni třídy Kontroleru.*

Nyní procházení k /StoreManager zobrazí dialogové okno přihlášení:

![](mvc-music-store-part-7/_static/image9.png)

Po přihlášení s naše nový účet správce se nám moci přejít na obrazovce upravit Album jako před.

>[!div class="step-by-step"]
[Předchozí](mvc-music-store-part-6.md)
[další](mvc-music-store-part-8.md)
