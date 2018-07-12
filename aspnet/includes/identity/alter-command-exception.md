> Některé příkazy nejsou podporovány, pokud aplikace používá jako své úložiště dat Identity SQLite. Vzhledem k omezením v databázovém stroji `Alter` příkazy vyvolání následující výjimky:
>
> "System.NotSupportedException: SQLite této migrace operaci nepodporuje." 
>
> Jako alternativní řešení spusťte migrace Code First v databázi, a změnit v tabulkách.
