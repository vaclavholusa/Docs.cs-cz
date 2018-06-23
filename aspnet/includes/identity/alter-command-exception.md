> Některé příkazy nejsou podporovány, pokud aplikace používá jako své Identity datové úložiště SQLite. Z důvodu omezení v databázovém stroji `Alter` příkazy throw následující výjimky:
>
> "System.NotSupportedException: SQLite této migrace operaci nepodporuje." 
>
> Jako řešení spusťte migrace Code First na databázi, které chcete změnit tabulky.
