---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Upgrade aplikace ASP.NET MVC 1,0 do architektury ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Tento dokument popisuje postup upgradu ručně a pomocí Průvodce aplikace ASP.NET MVC 1,0 ASP.NET MVC 2. Tento dokument je také k dispozici pro d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573322"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Upgrade aplikace ASP.NET MVC 1,0 do architektury ASP.NET MVC 2
====================
> Tento dokument popisuje postup upgradu ručně a pomocí Průvodce aplikace ASP.NET MVC 1,0 ASP.NET MVC 2. Tento dokument je také k dispozici pro [stáhnout](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Úvod

ASP.NET MVC 2 lze nainstalovat node souběžně s ASP.NET MVC 1,0 na stejném serveru. To dává flexibilní vývojáři aplikace při volbě, když chcete upgradovat aplikaci ASP.NET MVC 1,0 ASP.NET MVC 2.

Visual Studio 2010 obsahuje Průvodce pro tento upgrade existující projekty ASP.NET MVC 1,0 vytvořené s nástroji Visual Studio 2008 na ASP.NET MVC 2. Průvodce upgradem inicializuje otevření projektu ASP.NET MVC 1,0 v sadě Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Průvodce pro technologii ASP.NET MVC 1,0 na Visual Studio 2008 SP1 upgradu

Pokud chcete upgradovat aplikaci ASP.NET MVC 1,0 ASP.NET MVC 2 v aktualizaci Visual Studio 2008 SP1, použijte (nepodporované) MvcAppConverter aplikace. Tuto aplikaci si můžete stáhnout z následující adresy URL:

[https://go.microsoft.com/fwlink/?LinkId=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Ruční upgrade projektu ASP.NET MVC 1,0

Pokud chcete ručně upgradovat stávající aplikace ASP.NET MVC 1,0 do verze 2, postupujte takto:

1. Vytvořte záložní kopii existující projekt.
2. V textovém editoru otevřete soubor projektu (soubor s příponou souboru .csproj nebo .vbproj) a najděte ProjectTypeGuid element. Jako hodnotu tohoto prvku nahraďte identifikátor GUID {603c0e0b-db56-11dc-be95-000d561079b0} s {F85E285D-A4E0-4152-9332-AB1D724D3325}. Až skončíte, hodnota tohoto elementu by měl vypadat takto: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. V kořenové složce webové aplikace upravte soubor Web.config. Vyhledejte System.Web.Mvc, verze = 1.0.0.0 a nahraďte všechny instance System.Web.Mvc, verze = 2.0.0.0.
4. Opakujte předchozí krok pro soubor Web.config umístěný ve složce zobrazení.
5. Otevřete projekt pomocí sady Visual Studio a v **Průzkumníku řešení**, rozbalte **odkazy** uzlu. Odstraňte odkaz na System.Web.Mvc (který odkazuje na sestavení verze 1.0). Přidáte odkaz na System.Web.Mvc (v2.0.0.0).
6. Přidejte následující bindingRedirect – element v souboru Web.config v kořenovém adresáři aplikace v sekci se zobrazí:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Vytvořte novou prázdnou aplikaci ASP.NET MVC 2. Zkopírujte soubory ze složky skriptů nové aplikace do složky skriptů existující aplikace.
8. Aktualizovat stávající applicationâ€™ soubor CSS s definicemi stylu CSS v souboru Site.css.
9. Kompilace aplikace a spusťte ji. Pokud dojde k chybám, podívejte se na části nejnovějších změn [co je nového v technologii ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) stránky.
