---
title: Obrázek značky Pomocník ASP.NET Core
author: pkellner
description: Ukazuje, jak pracovat s pomocná značka obrázku
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 7ed160354b25aa0183ac49db93307b1f1b4d0666
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276640"
---
# <a name="image-tag-helper-in-aspnet-core"></a>Obrázek značky Pomocník ASP.NET Core

Podle [Petr Kellner](http://peterkellner.net) 

Pomocná značka obrázku rozšiřuje `img` (`<img>`) značky. Vyžaduje `src` značky a taky `boolean` atribut `asp-append-version`.

Pokud zdroj bitové kopie (`src`) je statický soubor na webovém serveru hostitele se připojí jedinečný mezipaměti nejnovějších řetězec jako parametr dotazu na zdroj bitové kopie. Tím se zajistí, že pokud se změní soubor na webovém serveru hostitele, adresu URL jedinečný požadavku je vygenerováno, která zahrnuje parametr aktualizované žádosti. Mezipaměť nejnovějších řetězec je jedinečnou hodnotu představující hodnotu hash souboru statické bitové kopie.

Pokud zdroj bitové kopie (`src`) není statický soubor (například soubor nebo vzdálenou adresou URL neexistuje na serveru), `<img>` tagu `src` atribut je generována žádné mezipaměti nejnovějších parametr řetězce dotazu.

## <a name="image-tag-helper-attributes"></a>Atributy pomocné rutiny značka obrázku


### <a name="asp-append-version"></a>ASP připojit verze

-Li zadána spolu s `src` atribut pomocná značka obrázku je volána.

Příklad platné `img` Pomocník značky:

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Pokud se statický soubor existuje v adresáři *... Wwwroot/Images/asplogo.PNG* generovaný kód jazyka html je podobný následujícímu (hodnota hash bude jiný):

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

Hodnota přiřazená k parametr `v` je hodnota hash souboru na disku. Pokud webový server nemůže získat přístup pro čtení ke statickému souboru odkazováno, ne `v` parametry se přidá do `src` atribut.

- - -

### <a name="src"></a>src

Pokud chcete aktivovat pomocná značka obrázku, src atribut je požadován na `<img>` elementu. 

> [!NOTE]
> Pomocník značka obrázku používá `Cache` zprostředkovatele na serveru místní web ukládat počítané `Sha512` daného souboru. Pokud je soubor požádat znovu `Sha512` nemusí být přepočítána. Je mezipaměť zneplatněna pomocí souboru sledovacího procesu, který je připojen k souboru po souboru `Sha512` se počítá.

## <a name="additional-resources"></a>Další zdroje

* <xref:performance/caching/memory>
