---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: "Mapování uživatelů SignalR na připojení | Microsoft Docs"
author: tfitzmac
description: "Toto téma ukazuje, jak uchovávat informace týkající se uživatelů a jejich připojení. Patrik Fletcher pomohl zápisu v tomto tématu. Použité v tomto tématu verze softwaru..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections"></a>Mapování uživatelů SignalR na připojení
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Toto téma ukazuje, jak uchovávat informace týkající se uživatelů a jejich připojení.
> 
> Patrik Fletcher pomohl zápisu v tomto tématu.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Verze softwaru použitým v tomto tématu
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR verze 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Předchozí verze tohoto tématu
> 
> Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


## <a name="introduction"></a>Úvod

Každý klient připojení k rozbočovači předá id jedinečný připojení. Můžete načíst tuto hodnotu v `Context.ConnectionId` vlastnost kontext rozbočovače. Pokud aplikace potřebuje zachovat mapování a mapování uživatele id připojení, můžete použít jednu z těchto možností:

- [Uživatelské ID zprostředkovatele (SignalR 2)](#IUserIdProvider)
- [Úložiště v paměti](#inmemory), jako je například slovník
- [Skupiny SignalR pro každého uživatele](#groups)
- [Trvalé, externí úložiště](#database), jako jsou tabulky databáze nebo úložiště Azure table

Těchto jednotlivých instancí se zobrazuje v tomto tématu. Můžete použít `OnConnected`, `OnDisconnected`, a `OnReconnected` metody `Hub` třídy ke sledování stavu připojení uživatele.

Nejlepší metodou pro vaši aplikaci, závisí na:

- Počet webových serverů, který je hostitelem vaší aplikace.
- Jestli chcete získat seznam aktuálně připojeného uživatele.
- Jestli je potřeba uchovávat informace, skupiny a uživatele po restartování aplikace nebo serveru.
- Jestli latence volání externí server je problém.

Následující tabulka ukazuje, jaký přístup se dá použít pro tyto aspekty.

|  | Více než jeden server | Získat seznam aktuálně připojených uživatelů | Zachovat informace po restartování | Optimální výkon |
| --- | --- | --- | --- | --- |
| ID uživatele zprostředkovatele | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| V paměti |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Skupiny jednoho uživatele | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Trvalé, externí | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>IUserID zprostředkovatele

Tato funkce umožňuje uživatelům určit, co je ID uživatele podle IRequest prostřednictvím nové rozhraní IUserIdProvider.

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Ve výchozím nastavení, bude implementace, která používá uživatele `IPrincipal.Identity.Name` jako uživatelské jméno. Chcete-li toto nastavení změnit, zaregistrujte vaši implementaci `IUserIdProvider` s globální hostitelem při spuštění aplikace:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Z v rozbočovači, budete moct odesílat zprávy pro tyto uživatele prostřednictvím rozhraní API následující:

**Odesílání zprávy pro konkrétního uživatele**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Úložiště v paměti

Následující příklady ukazují, jak uchovávat informace o připojení a uživatele v slovník, který je uložen v paměti. Adresář používá `HashSet` pro ukládání id připojení. Kdykoli uživatel může mít víc než jedno připojení k aplikaci SignalR. Například uživatel, který je připojený prostřednictvím více zařízení nebo více než jedné karty prohlížeče by mít více než jeden id připojení.

Pokud aplikace ukončí, všechny informace dojde ke ztrátě, ale ho znovu vyplní podle uživatele znovu navázat připojení. Úložiště v paměti nefunguje, pokud vaše prostředí zahrnuje více než jednom webovém serveru, protože každý server by mít samostatné kolekce připojení.

První příklad ukazuje třídu, která spravuje mapování mezi uživateli k připojení. Klíč pro HashSet bude uživatelské jméno.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Další příklad ukazuje způsob použití třídy mapování připojení od rozbočovače. Instance třídy je uložen v názvu proměnné `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Skupiny jednoho uživatele

Můžete vytvořit skupinu pro každého uživatele a pak odešle zprávu do této skupiny, pokud chcete dosáhnout pouze tento uživatel. Název každé skupiny je jméno uživatele. Pokud má uživatel více než jedno připojení, každý id připojení je přidán do skupiny uživatele.

Neměli ručně uživatele odeberete ze skupiny po odpojení. Tato akce se provádí automaticky rámcem SignalR.

Následující příklad ukazuje, jak implementovat skupiny jednoho uživatele.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Trvalé, externí úložiště

Toto téma ukazuje, jak chcete použít pro ukládání informací o připojení databáze nebo úložiště tabulek Azure. Tento přístup funguje, když máte více webových serverů, protože každý webový server může komunikovat s úložišti dat stejné. Pokud vaše webové servery ukončete pracovní nebo se aplikace restartuje `OnDisconnected` metoda není volána. Proto je možné, že úložiště dat budou mít záznamy pro ID připojení, které již nejsou platné. Vyčistěte tyto osamocené záznamy, můžete zrušit platnost jakékoli připojení, která byla vytvořena mimo časový rámec, který je relevantní pro vaše aplikace. Příklady v této části obsahují hodnotu pro sledování v okamžiku vytvoření připojení, ale nezobrazovat postup vyčištění staré záznamy, protože můžete chtít udělat jako proces na pozadí.

### <a name="database"></a>Databáze

Následující příklady ukazují, jak uchovávat informace o připojení a uživatele v databázi. Můžete použít technologii přístup všech dat; Následující příklad ukazuje, jak definovat modely používající rozhraní Entity Framework. Tyto modely entity odpovídají databáze tabulky a pole. Datová struktura může značně lišit v závislosti na požadavcích vaší aplikace.

První příklad ukazuje, jak definovat entitu uživatele, který může být přidružen mnoho entit připojení.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Potom z centra, můžete sledovat stav každé připojení kódem vidíte níže.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Úložiště tabulek Azure

Následující příklad Azure table storage je podobné jako v příkladu databáze. Neobsahuje všechny informace, které by bylo nutné Začínáme s Azure Table Storage Service. Informace najdete v tématu [postup používání úložiště Table z .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Následující příklad ukazuje tabulka entity pro ukládání informací o připojení. Oddíly data podle uživatelského jména a identifikuje každé entity podle id připojení, takže uživatel může mít víc připojení kdykoli.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

V centru sledovat stav připojení jednotlivých uživatelů.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
