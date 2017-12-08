---
uid: mobile/device-simulators
title: "Simulovat oblíbených mobilních zařízení testování | Microsoft Docs"
author: rick-anderson
description: "Podle těchto odkazů si můžete stáhnout emulátorů oblíbených mobilních zařízení a prohlížeče"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2011
ms.topic: article
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 48145b15b4983d6a143a8c53c9e6e8b4639da91e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="simulate-popular-mobile-devices-for-testing"></a>Simulovat oblíbených mobilních zařízení pro testování
====================
> Podle těchto odkazů si můžete stáhnout emulátorů oblíbených mobilních zařízení a prohlížeče


| Zařízení nebo prohlížeče | Emulátoru / simulátoru |
| --- | --- |
| BrowserStack hostované prohlížeče virtualizace ![BrowserStack hostované prohlížeče virtualizace](device-simulators/_static/image1.png) | [Virtualizace prohlížeče hostované BrowserStack](http://browserstack.com) testování místní nebo produkční prostředí v libovolného prohlížeče na jakékoli platformě. Tunelové propojení mezi počítači a BrowserStack sítí můžete vytvořit vlastní hostovaný virtuální počítač. Ujistěte se, získat [BrowserStack rozšíření sady Visual Studio](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c) i více bezproblémové prostředí. |
| Windows Phone | [Windows Phone SDK stáhne](https://dev.windowsphone.com/en-us/downloadsdk) Windows Phone Software Development Kit (SDK) obsahuje všechny funkce nástroje, které potřebujete k vývoji aplikace a hry pro Windows Phone |
| zařízení iPhone nebo iPod nebo iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) iPhone a iPad simulátorů pro systém Windows a také Responsive návrh nástroj. Můžete integrovat s možností "Vyhledat s.." VS 2012. |
| Android | [Android SDK domovské stránky](https://developer.android.com/sdk) |
| Prohlížeči Opera Mobile / Opera malé | Nejnovější verze: [Opera Developer Tools domácí](http://www.opera.com/developer/tools/) Opera malé 4.2: [simulátoru založené na jazyce Java Online](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile 6.5.3 | [Sady nástrojů pro vývojáře systému Windows Mobile 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) Všimněte si, že udělit přístup k síti phone, musíte taky VPC síťového adaptéru, která je součástí [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Připojení aplikace Internet Explorer na telefonu váš vývojový server sady Visual Studio, najdete v části [Kiran Patil příspěvku na blogu](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |
| Windows Mobile 6.1 | [Emulátor bitové kopie pro Visual Studio 2005/2008](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

Poznámka: Pokud chcete zobrazit aplikace na skutečném mobilních zařízení (což je jedinou možností pro plně testování iPhone nebo iPad, protože neexistuje žádná hodnota true, emulátor pro Windows) budete muset hostování vaší aplikace v IIS nebo IIS Express. Nemůžete použít snadno Visual Studio integrovaný webový server pro to, protože nebude odpovídat na požadavky z jiných počítačů.
