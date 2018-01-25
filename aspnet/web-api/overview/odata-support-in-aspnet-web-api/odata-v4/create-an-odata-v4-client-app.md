---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: "Vytvoření aplikace OData v4 klienta (C#) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="create-an-odata-v4-client-app-c"></a>Vytvoření aplikace OData v4 klienta (C#)
====================
podle [Wasson Jan](https://github.com/MikeWasson)

V předchozích kurzu vytvoříte základní služba OData, která podporuje operace CRUD. Nyní vytvoříme klienta pro službu.

Spusťte novou instanci sady Visual Studio a vytvořte nový projekt konzolové aplikace. V **nový projekt** dialogovém okně, vyberte **nainstalovaná** &gt; **šablony** &gt; **Visual C#** &gt; **Windows Desktop**a vyberte **konzolové aplikace** šablony. Název projektu &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Konzolové aplikace můžete také přidat do stejného řešení sady Visual Studio, který obsahuje službu OData.


## <a name="install-the-odata-client-code-generator"></a>Nainstalujte klienta OData generátor kódu

Z **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**. Vyberte **Online** &gt; **Galerie sady Visual Studio**. Do vyhledávacího pole Hledat &quot;generátor kódu klienta OData&quot;. Klikněte na tlačítko **Stáhnout** k instalaci VSIX. Může se zobrazit výzva k restartování sady Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Spusťte službu OData místně

Spusťte projekt ProductService ze sady Visual Studio. Ve výchozím nastavení Visual Studio spustí prohlížeč na kořenový adresář aplikace. Poznámka: identifikátor URI; je nutné v dalším kroku. Nechte aplikaci spuštěnou.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Když vložíte obou projektů ve stejném řešení, nezapomeňte spustit projekt ProductService bez ladění. V dalším kroku musíte udržovat služby běží, když upravíte projekt konzolové aplikace.


## <a name="generate-the-service-proxy"></a>Generovat Proxy služby

Proxy server služby je třída rozhraní .NET, která definuje metody pro přístup ke službě OData. Proxy server přeloží volání metod na požadavky HTTP. Vytvoří třídu proxy spuštěním [šablony T4](https://msdn.microsoft.com/library/bb126445.aspx).

Klikněte pravým tlačítkem na projekt. Vyberte **přidat** &gt; **novou položku**.

![](create-an-odata-v4-client-app/_static/image5.png)

V **přidat novou položku** dialogovém okně, vyberte **Visual C# položky** &gt; **kód** &gt; **klient OData**. Název šablony &quot;ProductClient.tt&quot;. Klikněte na tlačítko **přidat** a klikněte na tlačítko prostřednictvím upozornění zabezpečení.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

V tomto okamžiku budete dojde k chybě, která můžete ignorovat. Visual Studio automaticky spustí šablony, ale šablona potřebuje některá nastavení konfigurace první.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Otevřete soubor ProductClient.odata.config. V `Parameter` elementu, vložte identifikátor URI z projektu ProductService (předchozí krok). Příklad:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Znovu spusťte šablonu. V Průzkumníku řešení klikněte pravým tlačítkem na soubor ProductClient.tt a vyberte **spustit nástroj pro vlastní**.

Šablona vytvoří kód soubor s názvem ProductClient.cs, která definuje proxy serveru. Když budete vyvíjet aplikace, pokud změníte koncový bod OData, spusťte šablonu znovu k aktualizaci proxy serveru.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Použít proxy server služby pro volání služby OData

Otevřete soubor Program.cs a nahradit následující často používaný kód.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Nahraďte hodnotu *serviceUri* s URI služby z dříve.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Při spuštění aplikace by měla výstup následující:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
