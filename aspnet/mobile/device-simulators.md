---
uid: mobile/device-simulators
title: Simulace oblíbených mobilních zařízení testování | Dokumentace Microsoftu
author: rick-anderson
description: Emulátory pro oblíbených mobilních zařízení a prohlížečů si můžete stáhnout pomocí těchto odkazů
ms.author: riande
ms.date: 10/11/2018
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 8299295154d23f8fc430676b2c8ad8efc98ad185
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348374"
---
# <a name="simulate-popular-mobile-devices-for-testing"></a>Simulace oblíbených mobilních zařízení pro účely testování

> Emulátory pro oblíbených mobilních zařízení a prohlížečů si můžete stáhnout pomocí těchto odkazů.

| Prohlížeč nebo zařízení | Emulátoru / simulátoru |
| --- | --- |
| Browserstackem hostované prohlížečem virtualizace ![Browserstackem hostované prohlížečem virtualizace](device-simulators/_static/image1.png) | [Virtualizace prohlížeče hostované Browserstackem](http://browserstack.com) testování místní nebo produkčního prostředí v jakémkoli prohlížeči na libovolné platformě. Můžete vytvořit tunelové propojení mezi počítači a sítí Browserstackem ve vlastní hostované virtuální počítač. Ujistěte se, že zobrazíte [rozšíření sady Visual Studio Browserstackem](https://marketplace.visualstudio.com/items?itemName=browserstackcom.BrowserStack) pro ještě větší bezproblémové prostředí. |
| zařízení iPhone / iPod / iPad | [Electric Mobile Studio](http://www.electricplum.com/studio.aspx) iPhone a iPad simulátory pro Windows, stejně jako Responzivní návrh nástroje. |
| Android | [Android Studio](https://developer.android.com/studio/) nebo [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) |
| Prohlížeči Opera Mobile | [Klasické emulátor Opera Mobile](https://www.opera.com/developer/mobile-emulator) |
| Windows Mobile 6.5.3 | [Sadu nástrojů pro vývojáře pro Windows Mobile 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) mějte na paměti, že pokud chcete dát přístup k síti telefon, musíte také síťový adaptér VPC součástí [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Připojení aplikace Internet Explorer v telefonu pro váš vývojový server sady Visual Studio, naleznete v tématu [Kiran Patil blogový příspěvek](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |

> [!NOTE]
> Pokud chcete zobrazit aplikace na skutečném mobilního zařízení (což je jedinou možností pro plně testování iPhone nebo iPad, protože neexistuje žádný true emulátor pro Windows) budete potřebovat k hostování vaší aplikace v IIS nebo IIS Express. Nelze použít snadno integrovaný webový server sady Visual Studio pro tento, protože nebude odpovídat na požadavky z jiných počítačů.