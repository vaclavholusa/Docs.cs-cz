---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publikujte aplikaci do Azure Azure App Service | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867811"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Publikujte aplikaci do Azure Azure App Service
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://github.com/MikeWasson/BookService)

Jako poslední krok budete publikovat aplikaci do Azure. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **publikovat**.

![](part-10/_static/image1.png)

Kliknutím na tlačítko **publikovat** vyvolá **Publikovat Web** dialogové okno. Pokud je zaškrtnuto **hostitel v cloudu** při prvním vytvoření projektu a potom připojení a nastavení jsou již nakonfigurována. V takovém případě stačí kliknout na **nastavení** kartě a zkontrolujte &quot;spustit migrace Code First&quot;. (Pokud nebyl zkontrolujte **hostitel v cloudu** na začátku, postupujte podle pokynů v [další části](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Chcete-li nasadit aplikaci, klikněte na tlačítko **publikovat**. Publikování postup v lze zobrazit **aktivity publikování webového** okno. (Z **zobrazení** nabídce vyberte možnost **ostatní okna**, pak vyberte **aktivity publikování webového**.)

![](part-10/_static/image4.png)

Po dokončení nasazení aplikace Visual Studio automaticky otevře výchozí prohlížeč na adresu URL nasazené webové stránky a aplikace, kterou jste vytvořili je nyní spuštěna v cloudu. Adresu URL v adresním řádku prohlížeče ukazuje, že je právě načítán lokality z Internetu.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Nasazení na nový web

Pokud jste nezaškrtli políčko **hostitel v cloudu** při prvním vytvoření projektu, můžete nakonfigurovat nové webové aplikace teď. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **publikovat**. Vyberte **profil** a klikněte na **webové stránky společnosti Microsoft Azure**. Pokud nejste aktuálně přihlášení do Azure, zobrazí se výzva k přihlášení.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

V **existující weby** dialogové okno, klikněte na tlačítko **nový**.

![](part-10/_static/image9.png)

Zadejte název webového serveru. Vyberte předplatné Azure a oblast. V části **databázový server**, vyberte **vytvořit nový Server**, nebo vyberte existující server. Klikněte na tlačítko **vytvořit**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Klikněte **nastavení** kartě a zkontrolujte &quot;spustit migrace Code First&quot;. Pak klikněte na tlačítko **publikovat**.

> [!div class="step-by-step"]
> [Předchozí](part-9.md)
