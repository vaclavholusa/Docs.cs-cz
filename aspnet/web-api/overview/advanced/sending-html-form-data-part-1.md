---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "Odeslání dat formuláře HTML v rozhraní ASP.NET Web API: Data formuláře urlencoded | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Odeslání dat formuláře HTML v rozhraní ASP.NET Web API: Data formuláře urlencoded
====================
podle [Wasson Jan](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Část 1: Data formuláře urlencoded

Tento článek ukazuje, jak k odesílání dat formuláře urlencoded na kontroleru webového rozhraní API.

- [Přehled formuláře HTML](#overview_of_html_forms)
- [Odesílání komplexních typů](#sending_complex_types)
- [Odesílání dat formuláře pomocí rozhraní AJAX](#sending_form_data_via_ajax)
- [Odesílání jednoduché typy](#sending_simple_types)

> [!NOTE]
> [Stáhněte si dokončený projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Přehled formuláře HTML

Použití formuláře HTML GET nebo POST k odesílání dat na server. **Metoda** atribut **formuláře** element poskytuje metodu protokolu HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Výchozí metoda je GET. Pokud formulář používá získáte, formulář, který je zakódován data v identifikátoru URI jako řetězec dotazu. Pokud formulář používá POST, data formuláře je umístěn v textu požadavku. Pro data účtováno **enctype** Určuje atribut formátu textu na žádost:

| enctype | Popis |
| --- | --- |
| Application/x--www-form-urlencoded | Data formuláře je kódovaná jako dvojice název/hodnota, podobně jako řetězec dotazu identifikátoru URI. Toto je výchozí formát pro metodu POST. |
| multipart/formulář data | Data formuláře je kódovaná jako vícedílné zprávě standardu MIME. Pokud odesíláte soubor na server, použijte tento formát. |

Část 1 / Tento článek vypadá na formát x--www-form-urlencoded. [Část 2](sending-html-form-data-part-2.md) popisuje vícedílné zprávy standardu MIME.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Odesílání komplexních typů

Obvykle se odesílají komplexního typu, skládá z hodnot, které jsou převzaty z několika ovládacích prvků formuláře. Vezměte v úvahu následující model, který představuje stav aktualizace:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Tady je kontroleru webového rozhraní API, který přijímá `Update` objektu přes POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Tento řadič používá [směrování podle akce](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), takže je šablona trasy &quot;rozhraní api nebo {controller} / {action} / {id}&quot;. Klient bude odeslána data, která mají &quot;/api/updates/complex&quot;.


Teď můžete napsat formuláře HTML pro uživatele k odeslání aktualizace stavu.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Všimněte si, že **akce** atribut na formuláři je identifikátor URI naše akce kontroleru. Tady je formulář s některé hodnoty zadané v:

![](sending-html-form-data-part-1/_static/image1.png)

Po kliknutí na odeslání, prohlížeč odešle požadavek HTTP podobný následujícímu:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Všimněte si, že text žádosti obsahuje data formuláře, naformátovaná jako dvojice název/hodnota. Webové rozhraní API automaticky převede dvojice název/hodnota do instance `Update` třídy.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Odesílání dat formuláře pomocí rozhraní AJAX

Když uživatel odešle formulář, prohlížeč přejde mimo aktuální stránku a vykreslí tělo zprávy s odpovědí. Který je v pořádku. Pokud je odpověď na stránce HTML. Pomocí webového rozhraní API, ale text odpovědi je obvykle buď prázdná, nebo obsahuje strukturovaných dat, jako je například JSON. V takovém případě je vhodnější pro odeslání žádosti o data formuláře pomocí AJAX, tak, aby stránky může zpracovat odpověď.

Následující kód ukazuje, jak vystavit data formuláře pomocí jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery **odeslání** funkce nahrazuje akce formuláře s novou funkci. Přepíše výchozí chování tlačítka pro odeslání. **Serializovat** funkce serializuje data formuláře do dvojice název/hodnota. Chcete-li odeslat data formuláře do serveru, volejte `$.post()`.

Po dokončení žádosti `.success()` nebo `.error()` obslužná rutina zobrazí odpovídající zprávu pro uživatele.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Odesílání jednoduché typy

V předchozích částech jsme vám poslali komplexní typ, který webového rozhraní API deserializovat do instance třídy modelu. Můžete také odeslat jednoduché typy, jako je například řetězec.

> [!NOTE]
> Před odesláním jednoduchého typu, zvažte, místo toho zabalení hodnota v komplexního typu. To vám dává výhod modelu ověření na straně serveru a usnadňuje rozšířit modelu, v případě potřeby.


Základní postup odesílání jednoduchý typ. jsou stejné, ale existují dva drobné rozdíly. V kontroleru, musíte nejdřív uspořádání název parametru se **FromBody** atribut.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Ve výchozím nastavení se webové rozhraní API pokusí získat jednoduché typy z identifikátoru URI požadavku. **FromBody** atribut informuje webového rozhraní API načíst hodnotu z textu požadavku.

> [!NOTE]
> Webové rozhraní API přečte text odpovědi alespoň jednou, takže jenom jeden parametr akce mohou pocházet z textu požadavku. Pokud potřebujete získat více hodnot z textu žádosti, definujte komplexního typu.


Druhý klient musí odeslat hodnotu v následujícím formátu:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Část názvu dvojici název – hodnota musí být konkrétně pro jednoduchý typ. prázdné. Některé prohlížeče podporují formuláře HTML, ale můžete vytvořit tento formát ve skriptu takto:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Tady je příklad formuláře:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

A tady je skript, který chcete odeslat hodnot formulářů. Jediným rozdílem z předchozí skript je argument předaný do **post** funkce.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Můžete použít ve stejný přístup k odeslání pole jednoduchých typů:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Další prostředky

[Část 2: Odeslání souboru a vícedílné zprávy standardu MIME](sending-html-form-data-part-2.md)
