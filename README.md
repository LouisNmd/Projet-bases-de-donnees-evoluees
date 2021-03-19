# **Bases de données évoluées - Projet 2021** #
## Etude du bien-être dans le monde en 2015 et 2016 ##

#### *Schéma en étoile de l'entrepôt de données* ####

![Schéma en flocon](flocon.jpg)

Deux dimensions (PAYS et ECONOMIE) ainsi qu'une table des faits sont représentées dans ce schéma.  

- PAYS  
**nom_pays** (chaîne de caractère) : Nom du pays en toute lettre.  
**region_geographique** (chaîne de caractère) : Région géographique du pays concerné, en toute lettre.

- ECONOMIE  
**id_economie** (entier) : clé primaire necessaire pour différencier les différents tuples.  
**PIB_par_hab** (flottant) : le PIB/habitant.  
**IDH** (flottant) : valeur numérique comprise entre 0 et 1 qui représente l'IDH.  

- FAIT  
**classement_mondial_bonheur** (entier) : la position du pays de la population étudiée au classement mondial sur l'échelle du bonheur.  
**note_bonheur** (flottant) : valeur numérique comprise entre 0 et 10 qui représente le niveau de bonheur d'un pays selon ses habitants.  
**genre** (binaire) : Le genre de la population étudiée.    
**nombre_population** (entier) : taille de la population étudiée.  
**tranche_age** (chaîne de caractère) : tranche d'âge de la population étudiée.  
**nombre_suicide** (entier) : nombre de suicide dans l'année pour la population étudiée.  
**taux_suicide** (flottant) : nombre de suicide pour 100k habitants pour la population étudiée.  
**annee** (entier) : année concernée par les données.

#### *liste des requêtes* ####

- Group By Rollup  
```sql
SELECT ANNEE, NOM_PAYS, SUM(POPULATION) AS POPULATION_TOTALE, GROUPING(ANNEE) AS GRP
FROM FAITS
GROUP BY ROLLUP (ANNEE, NOM_PAYS)
ORDER BY NOM_PAYS;
```  
Calcule la population totale sur 3 niveaux d'aggrégats différent : 
  
-La population totale de chaque pays par année (2015 et 2016).  
*Utile à des fins d'analyse démographiques pour chaque pays*  
  
-La population totale répertoriée dans la base de donnée pour l'année 2015 et pour l'année 2016.  
*Utile pour connaitre le % de la population mondiale répertoriée dans la base de donnée et utile à des fins statistiques sur les données des suicides* 
  
-La somme de la population totale des années 2015 et 2016.  
*Utile à des fins statistiques sur les données des suicides*  
