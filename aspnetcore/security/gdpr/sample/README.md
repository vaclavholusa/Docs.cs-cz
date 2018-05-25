# <a name="gdpr-sample"></a>Ukázka GDPR

* V *appSettings.JSON určený*, nastavte `CheckNotConsentNeeded` k `false` vyžadovat souhlas; jinak hodnota nastavena na hodnotu true nebo vynechejte. Testování aplikace s `CheckNotConsentNeeded` nastavena na `false` a nastaven na hodnotu "true".
* Vytvořte základní a nepotřebných souborů cookie s každou varianta `CheckConsentNeeded` a udělen souhlas.
* Registrace uživatele.
* Soubory cookie odstraňte.
