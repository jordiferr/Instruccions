# PostgreSQL

## Acces com a ***postgres***

```bash
$ sudo su - postgres
$ psql -U postgres
postgres=# 
```

i entrar seguint els [primers pasos](#Primers-pasos-(entrar-com-**postgres**))

## Copies de seguretat

```bash
$ sudo su - postgres
$ pg_dumpall > <fitxer>
```

## Actualització Database (només útil a Ubuntu i derivats)

(Primer realitzar [còpies de seguretat](#Copies-de-seguretat))
```bash
$ sudo pg_ctlcluster <versioAntiga> main stop
$ sudo pg_dropcluster <versioNova> main --stop
$ sudo pg_upgradecluster -v <versioNova> <versioAntiga> main
$ sudo pg_ctlcluster <versioNova> main start
```

## Errors coneguts i solucions

Si apareix aquest error:
```sql
ERROR:  could not access file "$libdir/btree_gist":
```

La solució consisteix a instal·lar el paquet <code>postgresql<VERSIO\>-contrib</code>

## Canviar editor de text

Per canviar l'editor de text<br />
```sql
\setenv PSQL_EDITOR "/usr/bin/vim"
```

## Primers pasos (entrar com **postgres**)

```sql
CREATE ROLE < nom_del_usuari /> LOGIN PASSWORD '< contrassenya />';
CREATE DATABASE < nom_base_de_dades /> WITH OWNER < nom_del_usuari />;
```

## Copies de seguretat del directori

```bash
pg_basebackup -P -h 127.0.0.1 -U <usuari> -p 5432 -D <directori complet> -Ft -z -Xs
```

## Selecció dades en funció de la data

1. Entrar com a superusuari
2. \c {BASE de DADES}
3. CREATE EXTENSION btree_gist(**{COLUMNA on aplicar el filtre}**);
4. CREATE INDEX ON <taula> USING gist(**{COLUMNA on aplica el filtre}**);
5. SELECT <bla, bla> FROM <taula> ORDER BY (SELECT NOW()) <-> **{COLUMNA on aplica el filtre}**;

## Crear extensió

CREATE EXTENSION <nom_extensio\>;

### Extensió per a poder utilitzar **sort**

CREATE EXTENSION intarray;<br />
Un cop creada la extensió dins la base de dades podrem utilitzar la comanda **sort()**

## Eliminar i resetejar id

```sql
DELETE FROM **<taula>** WHERE id = **<num>**;
ALTER SEQUENCE **<taula>\_id\_seq** RESTART WITH **<num_desitjat>**;
```

## Eliminar funcions i triggers associats

```sql
DROP TRIGGER <nom_trigger> ON <taula_on_esta_el_trigger> ;
DROP FUNCTION IF EXISTS <nom_de_la_funció_que_cridava_el_trigger>;
```

## PROCEDURE vs FUNCTION

Una FUNCTION pot o no retornar alguna cosa.<br />
Un PROCEDURE pot cridar a varies funcions. Vindria a ser com una mega funció.

```sql
CREATE FUNCTION hola() RETURNS setof void AS $$
BEGIN
RAISE NOTICE 'hola mon';
END;
$$ LANGUAGE plpgsql;

CREATE FUNCTION adeu() RETURNS setof void AS $$
BEGIN
RAISE NOTICE 'adeu mon';
END;
$$ LANGUAGE plpgsql;

CREATE OR REPLACE PROCEDURE transfer()
AS $$
BEGIN
PERFORM hola();
PERFORM adeu();
END;
$$ LANGUAGE plpgsql;

CALL transfer();
```

Dins d'un **procedure** podem realitzar diverses operacions:

- INSERT
- UPDATE
- SELECT (si ha de retornar algun valor)
- PERFORM (un SELECT que no retorna cap valor)

Finalment per a cridar un **procedure** el que farem és:

```sql
CALL nomProcedure();
```

## Funció per a veure el que consumeix cada taula

### Forma 1

```sql
SELECT
  nspname || '.' || relname as "relation",
  pg_size_pretty(pg_relation_size(C.oid)) AS "size"
FROM
  pg_class C
LEFT JOIN
  pg_namespace N
ON
  (N.oid = C.relnamespace)
WHERE
  nspname
NOT IN ('pg_catalog', 'information_schema', 'pg_toast')
ORDER BY
  pg_relation_size(C.oid) DESC;
```

### Forma 2

```sql
SELECT pg_size_pretty(pg_database_size('Database Name')) as tamany;
```

## Funció només útil per a cercar una compra dins la taula *cistell_compra*

```sql
DO $$
DECLARE
    id_quantitats integer[];
    id_cistell_compra integer[];
    cistell_compra_list text;
BEGIN
    EXECUTE 'SELECT ARRAY(SELECT id FROM quantitat WHERE fkey_historicpreus IN (SELECT id FROM historicpreus WHERE fkey_producte = (SELECT id FROM producte WHERE descripcio ~* ''alguna_cosa_a_cercar'')))' INTO id_quantitats;

    EXECUTE 'SELECT array_agg(cc.id) FROM cistell_compra cc, jsonb_each_text(to_jsonb(cc)) kv WHERE kv.key LIKE ''fkey_quantitat_%'' AND kv.value::text = ANY ($1::text[])'
    INTO id_cistell_compra
    USING id_quantitats;

    cistell_compra_list := array_to_string(id_cistell_compra, ', ');
    RAISE NOTICE 'IDs de cistell de compra: %', cistell_compra_list;
END;
$$;
```

# Dates

## Data actual

```sql
SELECT current_date;
```

## Data i temps actual

```sql
SELECT NOW();
```

## Trobar una data (útil per a restar setmanes, dies...)

```sql
SELECT NOW() - INTERVAL '99 weeks';
```
<br />
```sql
SELECT NOW() - INTERVAL '1 DAY';
```
