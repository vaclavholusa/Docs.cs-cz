# <a name="cookie-sharing-sample-app"></a>Ukázková aplikace pro sdílení souborů cookie

Ukázka ukazuje soubor cookie sdílení napříč tří aplikací, které používala ověřování souborů cookie:

| Projekt                             | Popis |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | Aplikace ASP.NET Core stránky Razor 2.0 bez použití ASP.NET Core Identity |
| CookieAuthWithIdentity.Core         | Jádro ASP.NET 2.0 aplikace MVC se službou ASP.NET Core Identity |
| CookieAuthWithIdentity.NETFramework | Aplikace MVC rozhraní ASP.NET 4.6.1 s ASP.NET Identity |

Pokyny:

1. Spusťte aplikaci CookieAuth.Core. Registrace uživatele. Aplikace ověřuje uživatele při registraci uživatele. Odhlášení uživatele.
1. Ve stejné relaci prohlížeče spusťte aplikaci CookieAuthWithIdentity.Core. Zaregistrujte stejného uživatele jako použité v aplikaci jádra. Aplikace ověřuje uživatele při registraci uživatele. Odhlášení uživatele.
1. Ve stejné relaci prohlížeče spusťte aplikaci CookieAuthWithIdentity.NETFramework. Zaregistrujte se stejným uživatelem jako použít s jinými aplikacemi. Aplikace ověřuje uživatele při registraci uživatele. Odhlášení uživatele.
1. Přihlášení uživatele k některé z tři aplikací. Ověřovacího souboru cookie je sdílena mezi aplikacemi. Všimněte si, že je uživatel přihlášený automaticky do dalších dvou aplikací.
1. Odhlaste uživatele ze všech aplikací. Všimněte si, že je uživatel přihlášený automaticky mimo dalších dvou aplikací.

Tento příklad znázorňuje funkce popsané v [sdílet soubory cookie mezi aplikací](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) tématu.
