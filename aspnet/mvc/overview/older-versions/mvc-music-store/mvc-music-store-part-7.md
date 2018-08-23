---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: '7. část: Členství a ověřování | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 7. část se věnuje členství a autorizace.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 44205f0ef59e00ad9fb1c45fdc0ba8934b5804cc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757104"
---
<a name="part-7-membership-and-authorization"></a>7. část: Členství a ověřování
====================
podle [Jon Galloway](https://github.com/jongalloway)

> MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.  
>   
> Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.  
>   
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 7. část se věnuje členství a autorizace.


Správce Store kontroleru je nyní k dispozici všem uživatelům na našem webu. Změňme ji omezit oprávnění na správce webu.

## <a name="adding-the-accountcontroller-and-views"></a>Přidání zobrazení a AccountController

Jedním z rozdílů mezi kompletní šablonu webové aplikace ASP.NET MVC 3 a šablony ASP.NET MVC 3 prázdná webová aplikace je, že prázdnou šablonu neobsahuje řadič účet. Přidáme řadič účet tak, že zkopírujete několik souborů z nové aplikace ASP.NET MVC vytvořené z úplné šablony webové aplikace ASP.NET MVC 3.

Vytvoření nové aplikace ASP.NET MVC pomocí úplné šablony webové aplikace ASP.NET MVC 3 a zkopírujte následující soubory do stejného adresáře v našem projektu:

1. Zkopírujte AccountController.cs v adresáři řadiče
2. Zkopírujte AccountModels v adresáři modelů
3. Vytvořte účet adresář do adresáře zobrazení a zkopírujte všechny čtyři zobrazení v

Změna oboru názvů pro třídy Kontroleru a Model, začnou se MvcMusicStore. Třída AccountController by měl používat obor názvů MvcMusicStore.Controllers a třída AccountModels by měl používat obor názvů MvcMusicStore.Models.

*Poznámka: Tyto soubory jsou také k dispozici MvcMusicStore Assets.zip soubor ke stažení, ze kterého jsme naše lokality návrhu soubory zkopírovány na začátku tohoto kurzu. Členství soubory jsou umístěny v adresáři kódu.*

Aktualizované řešení by měl vypadat nějak takto:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Přidání správce s webem konfigurace ASP.NET

Předtím, než jsme v našem webu vyžadují autorizace, potřebujeme vytvořit uživatele s přístupem. Nejjednodušší způsob, jak vytvořit uživatele je použití webu integrované konfigurace technologie ASP.NET.

Kliknutím na ikonu v okně Průzkumník řešení po spuštění webu konfigurace technologie ASP.NET.

![](mvc-music-store-part-7/_static/image2.png)

Tím se spustí konfigurace webu. Klikněte na kartu zabezpečení na domovské obrazovce a pak klikněte na odkaz "Povolit role" na střed obrazovky.

![](mvc-music-store-part-7/_static/image3.png)

Klikněte na odkaz "Vytvořit nebo spravovat role".

![](mvc-music-store-part-7/_static/image4.png)

Zadejte "Administrator" jako název role a klikněte na tlačítko Přidat roli.

![](mvc-music-store-part-7/_static/image5.png)

Klikněte na tlačítko Zpět a potom klikněte na odkaz vytvořit uživatele na levé straně.

![](mvc-music-store-part-7/_static/image6.png)

Vyplňte pole informace uživatele na levé straně pomocí následujících informací:

| **Pole** | **Hodnota** |
| --- | --- |
| **Uživatelské jméno** | Správce |
| **Heslo** | / password123! |
| **Potvrzení hesla** | / password123! |
| **E-mailu** | (žádné e-mailová adresa bude fungovat) |
| **Bezpečnostní otázku** | (cokoli, co chcete) |
| **Zabezpečovací odpověď** | (cokoli, co chcete) |

*Poznámka: Můžete samozřejmě používat jakékoli heslo, které chcete. Výchozí nastavení zabezpečení hesel vyžadují heslo, které má 7 znaků a obsahuje jeden jiný než alfanumerický znak.*

Vyberte roli správce u tohoto uživatele a klikněte na tlačítko Vytvořit uživatele.

![](mvc-music-store-part-7/_static/image7.png)

V tomto okamžiku byste měli vidět zprávu s oznámením, že uživatel byl úspěšně vytvořen.

![](mvc-music-store-part-7/_static/image8.png)

Teď můžete zavřít okno prohlížeče.

## <a name="role-based-authorization"></a>Autorizace na základě rolí

Nyní jsme můžete omezit přístup k určení, že uživatel musí být v roli správce pro přístup k žádné akci kontroleru ve třídě StoreManagerController pomocí atributu [Authorize].

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Poznámka: Atribut [Authorize] lze umístit na konkrétní akci metody i na úrovni třídy Kontroleru.*

Teď přejdete do /StoreManager otevře dialogové okno přihlášení:

![](mvc-music-store-part-7/_static/image9.png)

Po přihlášení se náš nový účet správce, jsme byli schopni přejděte na obrazovku alba upravit jako před.

> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-6.md)
> [další](mvc-music-store-part-8.md)
