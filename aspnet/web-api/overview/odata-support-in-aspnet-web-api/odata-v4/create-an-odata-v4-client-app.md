---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Vytvoření klientské aplikace OData v4 (C#) | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 4d62a64006e2a500e0379419dbebe7ddff16fba5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753263"
---
<a name="create-an-odata-v4-client-app-c"></a>Vytvoření klientské aplikace OData v4 (C#)
====================
podle [Mike Wasson](https://github.com/MikeWasson)

V předchozím kurzu jste vytvořili základní služby OData, který podporuje operace CRUD. Nyní Pojďme vytvořit klienta pro službu.

Spustit novou instanci sady Visual Studio a vytvořte nový projekt konzolové aplikace. V **nový projekt** dialogového okna, vyberte **nainstalováno** &gt; **šablony** &gt; **Visual C#** &gt; **Windows Desktop**a vyberte **konzolovou aplikaci** šablony. Pojmenujte projekt &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Aplikace konzoly můžete také přidat do stejného řešení sady Visual Studio, která obsahuje službu OData.


## <a name="install-the-odata-client-code-generator"></a>Instalace generátoru kódu klienta OData

Z **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**. Vyberte **Online** &gt; **Galerie sady Visual Studio**. Do vyhledávacího pole vyhledejte &quot;generátor kódu klienta OData&quot;. Klikněte na tlačítko **Stáhnout** nainstalovat VSIX. Vám může zobrazit výzva k restartování sady Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Místní spuštění služby OData

Spusťte projekt ProductService ze sady Visual Studio. Ve výchozím nastavení spustí aplikace Visual Studio prohlížeč na kořenový adresář aplikace. Poznámka: identifikátor URI; to budete potřebovat v dalším kroku. Nechte aplikaci spuštěnou.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Když vložíte oba projekty ve stejném řešení, ujistěte se, že ke spuštění ProductService projektu bez ladění. V dalším kroku je potřeba udržet službu při úpravě projektu konzolové aplikace.


## <a name="generate-the-service-proxy"></a>Generování Proxy služby

Proxy služby je třída rozhraní .NET, která definuje metody pro přístup ke službě OData. Proxy server překládá volání metod na požadavky HTTP. Vytvoří třídu proxy spuštěním [šablona T4](https://msdn.microsoft.com/library/bb126445.aspx).

Klikněte pravým tlačítkem na projekt. Vyberte **přidat** &gt; **nová položka**.

![](create-an-odata-v4-client-app/_static/image5.png)

V **přidat novou položku** dialogového okna, vyberte **položky Visual C#** &gt; **kód** &gt; **klient OData**. Název šablony &quot;ProductClient.tt&quot;. Klikněte na tlačítko **přidat** proklikat upozornění zabezpečení.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

V tomto okamžiku budete dojde k chybě, které můžete ignorovat. Visual Studio automaticky spustí šablony, ale šablona potřebuje některá nastavení konfigurace první.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Otevřete soubor ProductClient.odata.config. V `Parameter` elementu, vložením identifikátoru URI z projektu ProductService (předchozího kroku). Příklad:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Spusťte šablonu znovu. V Průzkumníku řešení klikněte pravým tlačítkem myši klikněte na soubor ProductClient.tt a vyberte **spustit vlastní nástroj**.

Šablona vytvoří soubor kódu s názvem ProductClient.cs, který definuje proxy serveru. Při vývoji vaší aplikace, pokud změníte koncový bod OData, spusťte šablonu znovu a aktualizujte server proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Použít proxy server služby k volání služby OData

Otevřete soubor Program.cs a nahraďte často používaný kód následujícím kódem.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Nahraďte hodnotu *serviceuri:* s URI služby ze dříve.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Při spuštění aplikace by měl výstup následující:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
