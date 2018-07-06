---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Upgrade aplikace ASP.NET MVC 1,0 do architektury ASP.NET MVC 2 | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument popisuje, jak tom, jak upgradovat ručně a pomocí Průvodce aplikace ASP.NET MVC 1,0 ASP.NET MVC 2. Tento dokument je také k dispozici pro d...
ms.author: aspnetcontent
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: fe1696cd8f98f2ff253d385b62a6bcd74b536d33
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823868"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Upgrade aplikace ASP.NET MVC 1,0 do architektury ASP.NET MVC 2
====================
> Tento dokument popisuje, jak tom, jak upgradovat ručně a pomocí Průvodce aplikace ASP.NET MVC 1,0 ASP.NET MVC 2. Tento dokument je také k dispozici pro [stáhnout](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Úvod

ASP.NET MVC 2 lze nainstalovat souběžně s ASP.NET MVC 1,0 na stejném serveru. Díky tomu aplikace vývojáři flexibilitu při výběru, kdy se má upgradovat aplikaci ASP.NET MVC 1,0 do ASP.NET MVC 2.

Visual Studio 2010 obsahuje průvodce tento upgrade existující projekty ASP.NET MVC 1,0 vytvořených pomocí sady Visual Studio 2008 na ASP.NET MVC 2. Průvodce upgradem je zahájeno otevřením projektu aplikace ASP.NET MVC 1,0 v sadě Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Průvodce pro technologii ASP.NET MVC 1,0 na Visual Studio 2008 SP1 upgradu

Pokud chcete upgradovat aplikaci ASP.NET MVC 1,0 do ASP.NET MVC 2 v aplikaci Visual Studio 2008 SP1, pomocí (nepodporované) MvcAppConverter aplikace. Tuto aplikaci si můžete stáhnout z následující adresy URL:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Ruční upgrade projektu aplikace ASP.NET MVC 1,0

Ruční upgrade stávající aplikace ASP.NET MVC 1,0 do verze 2, postupujte podle těchto kroků:

1. Vytvořte záložní kopii existujícího projektu.
2. V textovém editoru otevřete soubor projektu (soubor s příponou souboru .csproj nebo .vbproj) a najděte ProjectTypeGuid element. Jako hodnotu tohoto prvku nahradí GUID {603c0e0b-db56-11dc-be95-000d561079b0} s {F85E285D-A4E0-4152-9332-AB1D724D3325}. Jakmile budete hotovi, hodnota tohoto prvku by měla být následujícím způsobem: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. V kořenové složce webové aplikace upravte soubor Web.config. Hledat System.Web.Mvc, verze = 1.0.0.0 a nahradí všechny instance System.Web.Mvc, verze = 2.0.0.0.
4. Opakujte předchozí krok pro soubor Web.config umístěný ve složce zobrazení.
5. Otevřete projekt pomocí sady Visual Studio a v **Průzkumníka řešení**, rozbalte **odkazy** uzlu. Odstraňte odkazy na System.Web.Mvc (která odkazuje na sestavení verze 1.0). Přidejte odkaz na System.Web.Mvc (v2.0.0.0).
6. Přidejte následující prvek bindingRedirect v souboru Web.config v kořenovém adresáři aplikace v sekci configuraton:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Vytvořte novou prázdnou aplikaci ASP.NET MVC 2. Zkopírujte soubory ze složky skripty nové aplikace do složky Scripts existující aplikace.
8. Aktualizovat existující € applicationâ™ soubor CSS s pomocí definice stylů CSS v souboru Site.css.
9. Zkompilujte aplikaci a spustíme ji. Pokud dojde k nějaké chybě, podívejte se na část Rozbíjející změny [co je nového v ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) stránky.
