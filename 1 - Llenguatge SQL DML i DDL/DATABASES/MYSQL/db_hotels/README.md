# Hotels Sample Database

Aquest document descriu la base de dades Hotels

hotels �s una base de dades creada per [Robert Ventura Vall-llovera](https://www.linkedin.com/in/robertvallllovera/ "Perfil LinkedIn Robert Ventura Vall-llovera"), professor de Cicles Formatius d'Inform�tica per la Generalitat de Catalunya .
Aquesta base de dades va ser creada amb dades aleat�ries i destinada a usos did�ctics en assignatures com gesti� de base de dades i administraci� de base de dades.

Aquesta base de dades vol simular una BD de reserves d'hotels de 4/5 estrelles situats a Catalunya. Les seves taules principals s�n:

* Paisos (~19 registres)
* Poblacions (~100 registres)
* Hotels (~340 registres)
* Habitacions (~32.700 registres)
* Clients (~17.900 registres)
* Reserves (~182.000 registres)

> Aquesta base de dades no cont� cap �ndex que no sigui procedent d'una clau prim�ria o forana.
> 
> Tampoc cont� cap control d'entrada de dades excepte les contraints d'integritat referencial proporcionades per les claus foranes i les contraints d'unicitats proporcionades per les claus prim�ries de cada taula.

## Taules

### Paisos

La taula de paisos cont� el codi i el nom de paisos. Aquesta taula s'utilitza per emmagatzemar el nom dels diferents paisos dels clients.

### Poblacions

La taula de poblacions cont� les poblacions d'on estan situtats els hotels. Tal i com hem dit anteriorment s�n poblacions dins de Catalunya.

### Hotels

La taula hotels cont� un subconjut de tots els hotels de 4 i 5 estrelles de Catalunya extrets d'un llistat a l'any 2017. Cosa que aquests poden estar desactualitzats a data d'avui.

### Habitacions

La taula d'habitacions cont� la informaci� de cadascuna de les habitacions dels diferents hotels

### Clients

La taula clients cont� la informaci� de cadascun dels clients que realitza reserves en els hotels.
Degut a que les dades s'han generat de forma aleat�ria pot ser que un mateix client tingui dues reserves en habitacions diferents dins del mateix per�ode.

### Reserves

La taula de reserves cont� reserves dels anys compresos entre 2014 i 2016 

Si es vol controlar l'entrada de reserves per tal de que aquestes siguin consistents es pot fer �s del seg�ent disparador/trigger a la taula reserves
```sql
-- -----------------------------------
-- Trigger que controla el solapament 
-- de les reserves en la mateixa habitaci� 
-- alhora de fer l'INSERT
-- -----------------------------------
CREATE TRIGGER trg_ReservesInsert BEFORE INSERT ON reserves
FOR EACH ROW
BEGIN

   SELECT COUNT(*) INTO @vCount
      FROM reserves r
   WHERE r.hab_id = NEW.hab_id 
    AND NEW.data_inici <= r.data_fi AND NEW.data_fi >= r.data_inici;
    
   IF @vCount > 0 THEN
      SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = "Error: Hi ha reserves solapades";
   END IF;

END
```


