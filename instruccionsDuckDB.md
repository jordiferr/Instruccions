# DuckDB 

## Què és?

Una eina que permet treballar amb fitxers ".csv" com si es tractessin d'una base de dades, amb les mateixes comandes de SQL.

## Instruccions generals

Exemple de codi per actualitzar un fitxer

```bash
CREATE TABLE mytable AS SELECT * FROM read_csv_auto('Drive/xfer3/FINAL_xfer3_temp1.csv', delim=';', header=false, columns={'nif':TEXT,'nom':TEXT,'mobil':TEXT,'mail':TEXT});
UPDATE mytable SET mail = REPLACE(mail, ' ', '');
COPY mytable TO 'FINAL_xfer3_temp1_updated.csv' (DELIMITER ';', HEADER false);
```
