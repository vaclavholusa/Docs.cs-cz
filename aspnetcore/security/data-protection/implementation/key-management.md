---
title: "Správa klíčů"
author: rick-anderson
description: "Tento dokument popisuje podrobnosti implementace ASP.NET Core data protection klíče rozhraní API pro správu."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 00f48718ad8b9cc9070b7adc54b26ecd89eb320f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="key-management"></a>Správa klíčů

<a name="data-protection-implementation-key-management"></a>

Systém ochrany dat automaticky spravuje životnost hlavního klíče, které slouží k ochraně a zrušení datové části. Každý klíč může existovat v jednom ze čtyř fází:

* Vytvořit - klíč existuje v řetězci klíč, ale nebyla ještě aktivována. Klíč nesmí použít pro nových operací chránit dokud uplynutí dostatečné doby, klíč dostal příležitost rozšíří do všech počítačů, které spotřebovávají prstenec tento klíč.

* Aktivní - klíč existuje v řetězci klíč a slouží pro všechny nové operace chránit.

* Platnost - klíč byl spuštěn přirozené celé jeho životnosti a již nelze použít pro nové operace chránit.

* Odvolat - klíč dojde k ohrožení a nesmí se používat pro nových operací chránit.

Všechny vytvořené, aktivní a vypršela platnost klíče může používat se odemknout příchozí datové části. Odvolané klíče ve výchozím nastavení se nedá použít k odemknutí datových částí, ale může vývojář aplikace [toto chování potlačit](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) v případě potřeby.

>[!WARNING]
> Vývojář může být tendenci odstranění klíče z okruhu klíč (například odstraněním odpovídající soubor ze systému souborů). V tomto okamžiku je trvale undecipherable všechna data chráněná pomocí klíče a není žádné nouzový potlačení stejně, jako je odvolané klíče. Odstranění klíče je opravdu škodlivý chování a v důsledku toho systému ochrany dat zveřejňuje žádné první třídy rozhraní API k provedení této operace.

## <a name="default-key-selection"></a>Výchozí možností výběru klíče

Když systému ochrany dat čte prstenec klíč z úložiště zálohování, pokusí se najít "Výchozí" klíč z okruhu klíč. Výchozí klíč se používá pro nových operací chránit.

Obecné heuristiky je, že systém ochrany dat zvolí klíč s nejnovější datum aktivace jako výchozí klíč. (Není malé fudge faktorem, který umožňuje serveru na server hodiny zkosení.) Pokud je klíč platnost nebo odebrán, a pokud aplikace nepovolil automatické generování klíče, pak se vygeneruje nový klíč s okamžitou aktivaci za [klíčů vypršení platnosti a vrácení](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) zásad níže.

Z důvodu ochrany dat systému vygeneruje nový klíč okamžitě spíše než návratem zpět k jiný klíč je, že nové generování klíče má být použit jako implicitní uplynutí všechny klíče, které byly aktivované před nový klíč. Obecnou představu je, že nových klíčů může být nakonfigurován se různé algoritmy nebo šifrování na rest mechanismy než původního klíče a systému by měla přednost aktuální konfiguraci přes návratem zpět.

Dojde k výjimce. Pokud má vývojář aplikace [zakázáno automatické generování klíče](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), pak systému ochrany dat, musíte zvolit něco jako výchozí klíč. V tomto scénáři záložní systém vybere-odvolat klíč s nejnovější datum aktivace přednost se přitom ke klíčům, které předtím čas potřebný k šíření na jiné počítače v clusteru. Záložní systém může stát v důsledku výběru vypršela platnost výchozí klíče. Záložní systém nikdy vybere odvolané klíč jako výchozí klíč, a pokud prstenec klíč je prázdný nebo byl odvolán každým klíčem pak systém způsobí k chybě při inicializaci.

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>Vypršení platnosti klíče a vrácení

Když klíč je vytvořen, se automaticky přiděluje datum aktivace {nyní + 2 dny} a datum vypršení platnosti {nyní + 90 dnů}. 2 dny zpoždění před aktivace poskytuje klíče čas potřebný k šíření prostřednictvím systému. To znamená umožňuje sledovat klíč v jejich další období automatického obnovení, proto maximalizace pravděpodobnost, že pokud klíč prstenec nemá stane aktivní, který má rozšířen na všechny aplikace, které mohou vyžadovat ho použít jiné aplikace jednotka ukazovat na úložiště zálohování.

Pokud výchozí klíč vyprší v rámci dvou dnů a prstenec klíč ještě nemá klíč, který bude aktivní po vypršení platnosti klíče výchozí, pak v systému ochrany dat automaticky zachovat nový klíč na klíč prstenec. Tento nový klíč má datum aktivace {datum vypršení platnosti výchozí klíč} a datum vypršení platnosti {nyní + 90 dnů}. To umožňuje systému automaticky vrátit klíče v pravidelných intervalech bez přerušení služby.

Je možné okolností kde klíč bude vytvořen s okamžitou aktivaci. Příkladem může být, kdy aplikace nebyla spuštěna na dobu a všechny klíče v řetězci klíč platnosti. V takovém případě je klíč zadané datum aktivace z {nyní} bez prodlevy normální aktivace 2 dny.

Výchozí doba života klíče je 90 dnů, i když jde konfigurovat jako v následujícím příkladu.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

Správce můžete také změnit výchozí systémové, i když explicitní volání `SetDefaultKeyLifetime` přepíší všechny systémové zásady. Výchozí doba života klíče nesmí být kratší než 7 dní.

## <a name="automatic-key-ring-refresh"></a>Aktualizace automatické prstenec klíč

Když inicializuje systému ochrany dat, čte prstenec klíč z základní úložiště a ukládá do mezipaměti v paměti. Tato mezipaměť umožňuje chránit a Unprotect operace pokračovat bez stiskne záložnímu úložišti. Systém automaticky zkontroluje úložiště zálohování pro změny přibližně každých 24 hodin nebo když vyprší platnost aktuálního klíče výchozí, nastane dříve.

>[!WARNING]
> Vývojáři měli velmi zřídka (Pokud někdy) měli používat klíče rozhraní API pro správu přímo. Systém ochrany dat provede automatické správy klíčů, jak je popsáno výše.

Zpřístupňuje rozhraní systému ochrany dat `IKeyManager` který lze použít ke kontrole a provést změny prstenec klíč. DI systém, který poskytuje instanci `IDataProtectionProvider` můžete také zadat instanci `IKeyManager` pro vaší spotřeby. Alternativně můžete vyžádat `IKeyManager` přímo z `IServiceProvider` jako v příkladu níže.

Všechny operace, která upraví klíč prstenec (vytvořit nový klíč explicitně nebo provádění odvolání) zruší platnost mezipaměti v paměti. Další volání `Protect` nebo `Unprotect` způsobí, že systém ochrany dat na znovu načíst prstenec klíče a znovu vytvořte mezipaměti.

Následující ukázka ukazuje, jak pomocí `IKeyManager` rozhraní ke kontrole a manipulaci s prstenec klíč, včetně odvolání existující klíče a nový klíč generování ručně.

[!code-csharp[Main](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Úložiště klíčů

Systém ochrany dat má Heuristika které pokusí se automaticky odvodit umístění příslušné úložiště klíčů a šifrování na mechanismus rest. Toto je konfigurovatelná vývojáře aplikace. Tyto dokumenty popisují implementace v poli z těchto mechanismů:

* [Pole poskytovatelů úložiště klíčů](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [Pole klíče šifrování v rest zprostředkovatelé](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)
