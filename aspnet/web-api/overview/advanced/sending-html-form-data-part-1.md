---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Posílání dat formulářů HTML ve webovém rozhraní API technologie ASP.NET: Data formuláře kódovaná | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 617b16da20f448bf86e4b99841ad6eeaf8aafe4d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818064"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Posílání dat formulářů HTML ve webovém rozhraní API technologie ASP.NET: Data formuláře kódovaná
====================
podle [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>– Část 1: Data formuláře kódovaná

Tento článek ukazuje, jak publikovat data formuláře kódovaná do kontroleru webového rozhraní API.

- [Přehled formulářů HTML](#overview_of_html_forms)
- [Odesílání komplexních typů](#sending_complex_types)
- [Posílání dat formulářů prostřednictvím AJAX](#sending_form_data_via_ajax)
- [Odesílání jednoduché typy](#sending_simple_types)

> [!NOTE]
> [Stáhnout dokončený projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Přehled formulářů HTML

Používání formulářů HTML GET nebo POST k odesílání dat na server. **Metoda** atribut **formuláře** element poskytuje metoda HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Výchozí metodou je GET. Pokud formulář používá získáte, formuláře, který dat je zakódovaný v identifikátoru URI jako řetězec dotazu. Pokud formulář používá metodu POST, data formuláře je umístěn v textu požadavku. Pro zaúčtované data **enctype** atribut určuje formát těla požadavku:

| enctype | Popis |
| --- | --- |
| Application/x--www-form-urlencoded | Data formuláře kódovaná jako dvojice název/hodnota, podobně jako řetězce dotazu URI. Toto je výchozí formát pro metodu POST. |
| multipart/formuláře data | Data formuláře kódovaná jako vícedílné zprávě standardu MIME. Pokud nahráváte soubor na server, použijte tento formát. |

Část 1 tohoto článku bude vypadat ve formátu x--www-form-urlencoded. [Část 2](sending-html-form-data-part-2.md) popisuje vícedílné zprávy standardu MIME.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Odesílání komplexních typů

Obvykle se odesílají komplexní typ, skládající se z hodnot z několika ovládacích prvků formuláře. Vezměte v úvahu následující model, který představuje stav aktualizace:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Tady je kontroler Web API, která přijímá `Update` objekt přes POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Tento kontroler používá [směrování na základě akce](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), takže je šablona trasy &quot;rozhraní api / {controller} / {action} / {id}&quot;. Klient bude publikovat data, která mají &quot;/api/updates/complex&quot;.


Nyní napíšeme formuláře HTML pro uživatele k odeslání aktualizace stavu.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Všimněte si, **akce** atributu ve formuláři je identifikátor URI naší akce kontroleru. Tady je formulář s některé hodnoty zadané v:

![](sending-html-form-data-part-1/_static/image1.png)

Když uživatel klepne na tlačítko Odeslat, prohlížeč odešle požadavek HTTP podobný následujícímu:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Všimněte si, že text požadavku obsahuje data formuláře, formátovaný jako dvojice název/hodnota. Webové rozhraní API automaticky převede dvojice název/hodnota do instance `Update` třídy.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Posílání dat formulářů prostřednictvím AJAX

Když uživatel odešle formulář, prohlížeči přejde mimo aktuální stránku a vykreslí tělo zprávy s odpovědí. Který je v pořádku. Pokud je odpověď na stránku HTML. S webovým rozhraním API, ale text odpovědi je obvykle buď prázdná, nebo obsahuje strukturovaná data, jako je například JSON. V takovém případě je vhodnější k odesílání dat formuláře pomocí AJAX vyžádání, tak, aby stránky mohou zpracovat odpověď.

Následující kód ukazuje, jak publikovat data formuláře pomocí jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery **odeslat** funkce nahradí akce formuláře s novou funkci. Tím se přepíše výchozí chování tlačítka pro odeslání. **Serializovat** funkce serializuje data formuláře na páry název/hodnota. Chcete-li data formuláře odeslat k serveru, zavolejte `$.post()`.

Po dokončení žádosti `.success()` nebo `.error()` obslužné rutiny zobrazí odpovídající zprávu pro uživatele.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Odesílání jednoduché typy

V předchozích částech jsme vám poslali komplexní typ, který webového rozhraní API se deserializovala na instanci třídy modelu. Můžete také odeslat jednoduché typy, jako je například řetězec.

> [!NOTE]
> Před odesláním jednoduchého typu, zvažte možnost uzavřít hodnotu v komplexního typu. místo toho. To poskytuje výhody model ověření na straně serveru a usnadňuje rozšíření modelu, v případě potřeby.


Toto jsou základní kroky k odeslání jednoduchý typ stejný, ale existují dva drobné rozdíly. V kontroleru, musíte nejprve uspořádání název parametru se **FromBody** atribut.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Ve výchozím nastavení se webové rozhraní API pokusí získat jednoduché typy z identifikátoru URI požadavku. **FromBody** atribut oznamuje webového rozhraní API načíst hodnotu z textu požadavku.

> [!NOTE]
> Webové rozhraní API přečte text odpovědi nejvýše jednou, takže pouze jeden parametr akce můžou pocházet z textu požadavku. Pokud je potřeba získat několik hodnot z textu požadavku, definice komplexního typu.


Za druhé klient musí k odeslání hodnoty v následujícím formátu:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Konkrétně část název z dvojice název/hodnota musí být prázdná pro jednoduchý typ.. Některé prohlížeče nepodporují to pro formuláře HTML, ale následujícím způsobem vytvořte tento formát ve skriptu:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Tady je ukázkový formulář:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

A tady je skript, který chcete odeslat hodnota formuláře. Jediný rozdíl oproti předchozí skript je argument předaný do **příspěvku** funkce.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Stejný přístup můžete použít k odesílání pole jednoduchých typů:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Další prostředky

[Část 2: Nahrání souboru a vícedílné zprávy standardu MIME](sending-html-form-data-part-2.md)
