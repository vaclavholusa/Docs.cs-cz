---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: "Mapování uživatelů SignalR na připojení v systému SignalR 1.x | Microsoft Docs"
author: pfletcher
description: "Toto téma ukazuje, jak uchovávat informace týkající se uživatelů a jejich připojení."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 896bf4142ce090e39ed5697ff053cd56728318ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>Mapování uživatelů SignalR na připojení v systému SignalR 1.x
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)

> Toto téma ukazuje, jak uchovávat informace týkající se uživatelů a jejich připojení.


## <a name="introduction"></a>Úvod

Každý klient připojení k rozbočovači předá id jedinečný připojení. Můžete načíst tuto hodnotu v `Context.ConnectionId` vlastnost kontext rozbočovače. Pokud aplikace potřebuje zachovat mapování a mapování uživatele id připojení, můžete použít jednu z těchto možností:

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
| V paměti |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| Skupiny jednoho uživatele | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| Trvalé, externí | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Úložiště v paměti

Následující příklady ukazují, jak uchovávat informace o připojení a uživatele v slovník, který je uložen v paměti. Adresář používá `HashSet` pro ukládání id připojení. Kdykoli uživatel může mít víc než jedno připojení k aplikaci SignalR. Například uživatel, který je připojený prostřednictvím více zařízení nebo více než jedné karty prohlížeče by mít více než jeden id připojení.

Pokud aplikace ukončí, všechny informace dojde ke ztrátě, ale ho znovu vyplní podle uživatele znovu navázat připojení. Úložiště v paměti nefunguje, pokud vaše prostředí zahrnuje více než jednom webovém serveru, protože každý server by mít samostatné kolekce připojení.

První příklad ukazuje třídu, která spravuje mapování mezi uživateli k připojení. Klíč pro HashSet bude uživatelské jméno.

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Další příklad ukazuje způsob použití třídy mapování připojení od rozbočovače. Instance třídy je uložen v názvu proměnné `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Skupiny jednoho uživatele

Můžete vytvořit skupinu pro každého uživatele a pak odešle zprávu do této skupiny, pokud chcete dosáhnout pouze tento uživatel. Název každé skupiny je jméno uživatele. Pokud má uživatel více než jedno připojení, každý id připojení je přidán do skupiny uživatele.

Neměli ručně uživatele odeberete ze skupiny po odpojení. Tato akce se provádí automaticky rámcem SignalR.

Následující příklad ukazuje, jak implementovat skupiny jednoho uživatele.

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Trvalé, externí úložiště

Toto téma ukazuje, jak chcete použít pro ukládání informací o připojení databáze nebo úložiště tabulek Azure. Tento přístup funguje, když máte více webových serverů, protože každý webový server může komunikovat s úložišti dat stejné. Pokud vaše webové servery ukončete pracovní nebo se aplikace restartuje `OnDisconnected` metoda není volána. Proto je možné, že úložiště dat budou mít záznamy pro ID připojení, které již nejsou platné. Vyčistěte tyto osamocené záznamy, můžete zrušit platnost jakékoli připojení, která byla vytvořena mimo časový rámec, který je relevantní pro vaše aplikace. Příklady v této části obsahují hodnotu pro sledování v okamžiku vytvoření připojení, ale nezobrazovat postup vyčištění staré záznamy, protože můžete chtít udělat jako proces na pozadí.

### <a name="database"></a>Databáze

Následující příklady ukazují, jak uchovávat informace o připojení a uživatele v databázi. Můžete použít technologii přístup všech dat; Následující příklad ukazuje, jak definovat modely používající rozhraní Entity Framework. Tyto modely entity odpovídají databáze tabulky a pole. Datová struktura může značně lišit v závislosti na požadavcích vaší aplikace.

První příklad ukazuje, jak definovat entitu uživatele, který může být přidružen mnoho entit připojení.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Potom z centra, můžete sledovat stav každé připojení kódem vidíte níže.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Úložiště tabulek Azure

Následující příklad Azure table storage je podobné jako v příkladu databáze. Neobsahuje všechny informace, které by bylo nutné Začínáme s Azure Table Storage Service. Informace najdete v tématu [postup používání úložiště Table z .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Následující příklad ukazuje tabulka entity pro ukládání informací o připojení. Oddíly data podle uživatelského jména a identifikuje každé entity podle id připojení, takže uživatel může mít víc připojení kdykoli.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

V centru sledovat stav připojení jednotlivých uživatelů.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
