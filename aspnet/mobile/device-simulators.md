---
uid: mobile/device-simulators
title: Simulace oblíbených mobilních zařízení testování | Dokumentace Microsoftu
author: rick-anderson
description: Emulátory pro oblíbených mobilních zařízení a prohlížečů si můžete stáhnout pomocí těchto odkazů
ms.author: aspnetcontent
ms.date: 01/28/2011
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 092186a48a304c1b9e3e140e74f65dbe5a035e66
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833682"
---
<a name="simulate-popular-mobile-devices-for-testing"></a>Simulace oblíbených mobilních zařízení pro účely testování
====================
> Emulátory pro oblíbených mobilních zařízení a prohlížečů si můžete stáhnout pomocí těchto odkazů


| Prohlížeč nebo zařízení | Emulátoru / simulátoru |
| --- | --- |
| Browserstackem hostované prohlížečem virtualizace ![Browserstackem hostované prohlížečem virtualizace](device-simulators/_static/image1.png) | [Virtualizace prohlížeče hostované Browserstackem](http://browserstack.com) testování místní nebo produkčního prostředí v jakémkoli prohlížeči na libovolné platformě. Můžete vytvořit tunelové propojení mezi počítači a sítí Browserstackem ve vlastní hostované virtuální počítač. Ujistěte se, že zobrazíte [rozšíření sady Visual Studio Browserstackem](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c) pro ještě větší bezproblémové prostředí. |
| Windows Phone | [Windows Phone SDK stáhne](https://dev.windowsphone.com/downloadsdk) The Windows Phone Software Development Kit (SDK) zahrnuje všechny nástroje, které potřebujete k vývoji aplikací a her pro Windows Phone |
| zařízení iPhone / iPod / iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) iPhone a iPad simulátory pro Windows, stejně jako Responzivní návrh nástroje. Můžete integrovat s možností "Procházet s.." sadu VS 2012. |
| Android | [Domovská stránka Android SDK](https://developer.android.com/sdk) |
| Prohlížeči Opera Mobile / Opera Mini | Nejnovější verze: [Opera vývojářské nástroje domácí](http://www.opera.com/developer/tools/) Opera Mini 4.2: [založené na jazyce Java Online simulátoru](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile 6.5.3 | [Sadu nástrojů pro vývojáře pro Windows Mobile 6.5.3](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) mějte na paměti, že pokud chcete dát přístup k síti telefon, musíte také síťový adaptér VPC součástí [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Připojení aplikace Internet Explorer v telefonu pro váš vývojový server sady Visual Studio, naleznete v tématu [Kiran Patil blogový příspěvek](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |
| Windows Mobile 6.1 | [– Image emulátorů pro Visual Studio 2005/2008](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

Všimněte si, že pokud chcete zobrazit aplikace na skutečném mobilního zařízení (což je jedinou možností pro plně testování iPhone nebo iPad, protože neexistuje žádný true emulátor pro Windows), musíte k hostování vaší aplikace v IIS nebo IIS Express. Nelze použít snadno integrovaný webový server sady Visual Studio pro tento, protože nebude odpovídat na požadavky z jiných počítačů.
