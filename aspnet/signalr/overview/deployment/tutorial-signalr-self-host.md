---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Kurz: SignalR hostování na vlastním serveru | Microsoft Docs'
author: pfletcher
description: 'Tento kurz ukazuje, jak vytvořit vlastním hostováním SignalR 2: server a jak se připojit k němu pomocí jazyka JavaScript klienta. Použité v tomto kurzu V softwaru verze...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: d38e6fbc3407e4beca6942bbdefcaa8258ebc5ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036775"
---
<a name="tutorial-signalr-self-host"></a>Kurz: SignalR hostování na vlastním serveru
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Tento kurz ukazuje, jak vytvořit vlastním hostováním SignalR 2: server a jak se připojit k němu pomocí jazyka JavaScript klienta.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR verze 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Tento kurz pomocí sady Visual Studio 2012
> 
> 
> Pokud chcete používat Visual Studio 2012 v tomto kurzu, postupujte takto:
> 
> - Aktualizace vašeho [Správce balíčků](http://docs.nuget.org/docs/start-here/installing-nuget) na nejnovější verzi.
> - Nainstalujte [webové platformy](https://www.microsoft.com/web/downloads/platform.aspx).
> - Ve webové platformy, vyhledání a instalace **ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012**. Šablony sady Visual Studio pro třídy SignalR dojde k instalaci, jako **rozbočovače**.
> - Některé šablony (jako například **třídy pro spuštění OWIN**) nebudou k dispozici; pro tyto třídy soubor použít místo.
> 
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Přehled

SignalR server je obvykle hostované v aplikaci ASP.NET ve službě IIS, ale může být také vlastním hostováním (například v konzolové aplikace nebo služba systému Windows) pomocí knihovny hostování na vlastním serveru. Tuto knihovnu, jako jsou všechny funkce SignalR 2, je založená na OWIN ([Open Web Interface pro .NET](http://owin.org)). OWIN definuje abstrakci mezi .NET webové servery a webové aplikace. OWIN oddělí webové aplikace ze serveru, takže je ideální pro samoobslužné webové aplikace v vlastní proces mimo službu IIS hostování OWIN.

Pro hostování není ve službě IIS z těchto důvodů:

- Prostředí, kde služby IIS není k dispozici nebo žádoucí, jako je například existující serverové farmy bez služby IIS.
- Nároky na výkon služby IIS, je potřeba se vyhnout.
- Funkce SignalR je přidat do aplikace exising, která běží v služby systému Windows, role pracovního procesu Azure nebo jiný proces.

Pokud řešení je vyvíjen jako hostování na vlastním z důvodů výkonu, se doporučuje také testovací aplikace hostované ve službě IIS k určení výhody výkonu.

Tento kurz obsahuje následující části:

- [Vytvoření serveru](#server)
- [Přístup k serveru pomocí klienta JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Vytvoření serveru

V tomto kurzu vytvoříte serveru, který je hostován v konzolové aplikaci, ale server je možné hostovat v žádné řazení procesu, jako je služba systému Windows nebo role pracovního procesu Azure. Ukázkový kód pro hostování serveru SignalR ve službě Windows, najdete v části [Self-Hosting SignalR ve službě Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Otevřete Visual Studio 2013 s oprávněními správce. Vyberte **soubor**, **nový projekt**. Vyberte **Windows** pod **Visual C#** uzel v **šablony** panelu a vyberte **konzolové aplikace** šablony. Název nového projektu "SignalRSelfHost" a klikněte na tlačítko **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Otevření konzoly Správce balíčků knihoven výběrem **nástroje**, **Správce balíčků knihoven**, **Konzola správce balíčků**.
3. V konzole správce balíčku zadejte následující příkaz:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Tento příkaz přidá do projektu knihovny SignalR 2 Self-Host.
4. V konzole správce balíčku zadejte následující příkaz:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Tento příkaz přidá do projektu knihovny Microsoft.Owin.Cors. V této knihovně se použije pro podporu jiné domény, který je nutný pro aplikace, které jsou hostiteli SignalR a klient webové stránky v různých doménách. Vzhledem k tomu, že je budete hostování serveru SignalR a webového klienta na jiné porty, to znamená, že mezi doménami musí být povolen pro komunikaci mezi těmito součástmi.
5. Nahraďte obsah souboru Program.cs následujícím kódem.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Ve výše uvedeném kódu obsahuje tři třídy:

    - **Program**, včetně **hlavní** metoda definování primární cestu provádění. V této metodě, webovou aplikaci typu **spuštění** je spuštěna na zadané adrese URL (`http://localhost:8080`). Pro potřeby zabezpečení na koncový bod se dá implementovat protokol SSL. V tématu [postup: Nakonfigurujte certifikát protokolu SSL Port](https://msdn.microsoft.com/library/ms733791.aspx) Další informace.
    - **Spuštění**, třída obsahující konfiguraci serveru SignalR (pouze konfigurace tento kurz používá je volání `UseCors`) a volání k `MapSignalR`, která vytvoří trasy pro všechny objekty rozbočovače v projektu.
    - **MyHub**, rozbočovače SignalR třídu, která aplikace bude poskytovat klientům. Tato třída obsahuje jedinou metodu, **odeslat**, že klienti zavolá k vysílání zprávy do všech ostatních připojených klientů.
6. Zkompilování a spuštění aplikace. Zadaná adresa na serveru běží by měl zobrazit v okně konzoly.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Pokud se nezdaří spuštění s výjimkou `System.Reflection.TargetInvocationException was unhandled`, budete muset restartovat Visual Studio s oprávněními správce.
8. Zastavte aplikaci, než budete pokračovat k dalšímu oddílu.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Přístup k serveru pomocí klienta JavaScript

V této části budete používat stejného klienta JavaScript z [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md). Vytočit pouze jeden úpravy klientovi, který je explicitně zadat adresu URL rozbočovače. S vlastním hostováním aplikací server nemusí být nutně na stejnou adresu jako adresu URL připojení (z důvodu reverzní proxy a nástroje pro vyrovnávání zatížení), takže adresa URL musí být explicitně definován.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení a vyberte **přidat**, **nový projekt**. Vyberte **webové** uzel a vyberte možnost **webové aplikace ASP.NET** šablony. Název projektu "JavascriptClient" a klikněte na tlačítko **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Vyberte **prázdný** šablony a ponechejte zbývající možnosti zrušit. Vyberte **vytvoření projektu**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. V konzole správce balíčku vyberte projekt "JavascriptClient" v **výchozí projekt** rozevíracího seznamu a spusťte následující příkaz:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Tento příkaz nainstaluje SignalR a JQuery knihoven, které budete potřebovat v klientovi.
4. Klikněte pravým tlačítkem na projekt a vyberte **přidat**, **novou položku**. Vyberte **webové** uzel a vyberte stránku HTML. Název stránky **Default.html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Nahraďte obsah novou stránku HTML následující kód. Ověřte, že zde reference skriptu odpovídají skripty ve složce skripty projektu.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Následující kód (zvýrazněných v výše ukázka kódu) je přidání, které jste udělali u klienta použít v kurzu Začínáme Stared (kromě kód upgradu na verzi beta verze 2 SignalR). Tento řádek kódu explicitně nastaví adresu URL základní připojení pro SignalR na serveru.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění...** . Vyberte **více projektů po spuštění** přepínač a nastavte oba projekty **akce** k **spustit**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Klikněte pravým tlačítkem na "Default.html" a vyberte **nastavit jako úvodní stránku**.
8. Spusťte aplikaci. Spustí se server a stránky. Budete muset znovu načíst webovou stránku (nebo vyberte **pokračovat** v ladicím programu) Pokud se stránka načte, než je server spuštěn.
9. V prohlížeči zadejte uživatelské jméno při zobrazení výzvy. Zkopírujte adresu URL stránky do jiné okně nebo kartě prohlížeče a zadejte jiné uživatelské jméno. Bude moct odesílat zprávy v podokně Prohlížeč jeden na druhý jako v kurzu Začínáme.
