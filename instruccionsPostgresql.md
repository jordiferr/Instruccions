# PostgreSQL

## Acces com a ***postgres***

$ sudo su - postgres<br />
$ psql -U postgres

## Copies de seguretat

$ sudo su - postgres<br />
$ pg_dumpall > <fitxer>

## Actualització Database

(Primer realitzar [còpies de seguretat](#Copies-de-seguretat))<br />
$ sudo pg_ctlcluster <versioAntiga> main stop<br />
$ sudo pg_dropcluster <versioNova> main --stop<br />
$ sudo pg_upgradecluster -v <versioNova> <versioAntiga> main<br />
$ sudo pg_ctlcluster <versioNova> main start<br />


## Canviar editor de text

Per canviar l'editor de text<br />
```sql
\setenv PSQL_EDITOR "/usr/bin/vim"
```

## Primers pasos (entrar com **postgres**)

```sql
CREATE ROLE < nom_del_usuari /> WITH login;
CREATE DATABASE < nom_base_de_dades /> WITH OWNER < nom_del_usuari />;
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
