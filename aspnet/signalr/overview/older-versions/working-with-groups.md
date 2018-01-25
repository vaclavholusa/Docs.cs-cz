---
uid: signalr/overview/older-versions/working-with-groups
title: "Práce se skupinami v systému SignalR 1.x | Microsoft Docs"
author: pfletcher
description: "Toto téma popisuje, jak uchovávat informace o členství ve skupině s rozhraním API rozbočovače."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 7bc0ff73ade72729cc5e1217b3fe704ac0d8cab8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="working-with-groups-in-signalr-1x"></a>Práce se skupinami v systému SignalR 1.x
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)

> Toto téma popisuje postup přidání uživatelů do skupiny a zachovat informace o členství ve skupině.


## <a name="overview"></a>Přehled

Skupiny v systému SignalR poskytují metodu pro zprávy všesměrové vysílání pro zadaný podmnožiny připojených klientů. Skupina může mít libovolný počet klientů a klienta může být členem skupiny libovolný počet skupin. Nemáte nemusel vytvářet skupiny. Ve skutečnosti skupina se automaticky vytvoří při prvním zadejte její název ve volání Groups.Add a je odstraněn, když odeberete poslední připojení z členství v ní. Úvod do používání skupin, najdete v části [Správa členství ve skupině z třídy rozbočovače](index.md) v rozhraní API centra – Příručka pro Server.

Neexistuje žádné rozhraní API pro získání seznamu členství ve skupinách nebo seznam skupin. SignalR odešle zprávy do klientů a skupin založené na modelu pub nebo sub a serveru neumožňuje spravovat seznam skupin nebo členství ve skupinách. To pomáhá maximalizovat škálovatelnost, protože při každém přidání uzlu do webové farmy, nějaký stav, který udržuje SignalR musí být rozšířena do nového uzlu.

Při přidávání uživatele do skupiny pomocí `Groups.Add` metoda, uživatel obdrží zprávy směrované do této skupiny pro dobu trvání aktuálního připojení, ale není trvalý členství uživatele v dané skupině mimo aktuální připojení. Pokud chcete trvale uchovávat informace o skupinách a členství ve skupině, je třeba uložit data v úložišti, jako je databáze nebo úložiště tabulek Azure. Potom pokaždé, když uživatel připojí k vaší aplikaci, můžete načíst z úložiště, které skupiny, které uživatel patří do a ručně přidejte tohoto uživatele do těchto skupin.

Když znovu obnovovat po dočasné přerušení, uživatel automaticky znovu připojí dřív přiřazené skupiny. Automaticky opětovné připojování ke skupině platí pouze při opětovné připojení, není při vytvoření nového připojení. Token digitálně podepsané je předán z klienta, který obsahuje seznam dřív přiřazené skupin. Pokud chcete ověřit, zda uživatel patří do požadované skupiny, můžete přepsat výchozí chování.

Toto téma obsahuje následující části:

- [Přidávání a odebírání uživatelů](#add)
- [Volání metody členové skupiny](#call)
- [Ukládání do databáze členství ve skupině](#storedatabase)
- [Ukládání členství ve skupině ve službě Azure table storage](#storeazuretable)
- [Ověření členství ve skupině při opětovném připojení](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Přidávání a odebírání uživatelů

Chcete-li přidat nebo odebrat uživatele ze skupiny, zavolejte [přidat](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) nebo [odebrat](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody a předejte jí id uživatele připojení a názvu skupiny jako parametry. Nemusíte ručně odeberte uživatele ze skupiny po ukončení připojení.

Následující příklad ukazuje `Groups.Add` a `Groups.Remove` metody používané v metodách rozbočovače.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

`Groups.Add` a `Groups.Remove` metody provedení asynchronně.

Pokud chcete okamžitě odeslání zprávy do klienta pomocí skupiny a přidání klienta do skupiny, budete muset zajistěte, aby nejprve dokončí metodu Groups.Add. Následující příklady kódu ukazují, jak to udělat, jeden pomocí kódu, který funguje v rozhraní .NET 4.5 a jeden pomocí kódu, který funguje v rozhraní .NET 4.

#### <a name="asynchronous-net-45-example"></a>Asynchronní .NET 4.5 Příklad

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Asynchronní .NET 4 příklad

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

Obecně platí, by neměla zahrnovat `await` při volání metody `Groups.Remove` metoda protože id připojení, který se pokoušíte odebrat může nadále již nebudou dostupné. V takovém případě `TaskCanceledException` je vyvolána po vypršením časového limitu požadavku. Pokud vaše aplikace musíte zajistit, uživatel byl odebrán ze skupiny před odesláním zprávy do skupiny, můžete přidat `await` před Groups.Remove a pak catch `TaskCanceledException` výjimka, která mohou být vyvolány.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Volání metody členové skupiny

Mohou zasílat zprávy do všech členů skupiny nebo pouze členové zadané skupiny, jak je znázorněno v následujících příkladech.

- **Všechny** připojených klientů v zadané skupině. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Všichni připojení klienti v zadané skupině **s výjimkou zadaných klientů**, podle ID připojení. 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Všichni připojení klienti v zadané skupině **s výjimkou volajícího klienta**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Ukládání do databáze členství ve skupině

Následující příklady ukazují, jak uchovávat informace skupiny a uživatele v databázi. Můžete použít technologii přístup všech dat; Následující příklad ukazuje, jak definovat modely používající rozhraní Entity Framework. Tyto modely entity odpovídají databáze tabulky a pole. Datová struktura může značně lišit v závislosti na požadavcích vaší aplikace. Tento příklad obsahuje třídu s názvem `ConversationRoom` který bude pro aplikaci, která umožňuje uživatelům připojit konverzace o různých tématům, jako je například sportu nebo zahrady jedinečné. Tento příklad také obsahuje třídu pro připojení. Třída připojení není nezbytně nutné pro sledování členství ve skupině, ale často je součástí robustní řešení ke sledování uživatelů.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

Potom v rozbočovači, můžete načíst informace o skupiny a uživatele z databáze a ručně přidejte uživatele do příslušných skupin. V příkladu nezahrnuje kód pro sledování připojení uživatelů. V tomto příkladu `await` – klíčové slovo není použita před `Groups.Add` vzhledem k tomu, že členové skupiny není okamžitě odeslána zpráva. Pokud chcete odeslat zprávu na všechny členy skupiny okamžitě po přidání nového člena, chcete použít `await` – klíčové slovo a ujistěte se, asynchronní operace byla dokončena.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Ukládání členství ve skupině ve službě Azure table storage

Použití úložiště tabulek Azure k ukládání informací skupiny a uživatele je podobný používání databáze. Následující příklad ukazuje entitu tabulky, která ukládá uživatelské jméno a název skupiny.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

V centru je načíst přiřazené skupiny, když se uživatel připojí.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Ověření členství ve skupině při opětovném připojení

Ve výchozím nastavení SignalR automaticky znovu přiřadí uživatele do příslušných skupin po opětovném připojení z dočasné přerušení, jako je například když je připojení vyřazen a znovu vytvořit předtím, než vyprší časový limit připojení. Informace o skupině uživatele je předán do tokenu při opětovné připojení, a tento token se ověřuje na serveru. Informace o procesu ověření pro opětovné připojování ke skupinám uživatelů najdete v tématu [při opětovné připojení, opětovné připojování ke skupinám](index.md).

Obecně byste měli používat výchozí chování automaticky opětovné připojování ke, že skupin v připojení. SignalR skupiny nejsou určeny jako bezpečnostní mechanismus pro omezení přístupu k citlivým datům. Pokud vaše aplikace musí Zkontrolujte členství ve skupině uživatele při opětovné připojení, ale můžete přepsat výchozí chování. Změna výchozí chování můžete přidat zatížení k vaší databázi protože členství ve skupině uživatele musí být načtena pro každý opětovné připojení, ne jenom když se uživatel připojí.

Pokud členství ve skupině musí ověřit na znovu připojit, vytvořte nový modul kanálu rozbočovače, který vrátí seznam hodnot přiřazených skupin, jak vidíte níže.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

Tento modul potom přidáte do kanálu rozbočovače, jak je znázorněno dole.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
