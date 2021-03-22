# **Bases de données évoluées - Projet 2021** #
## Etude du bien-être dans le monde en 2015 et 2016 ##

#### *Schéma en étoile de l'entrepôt de données* ####

![Schéma en flocon](/flocon.jpg)

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

#### *Mise en oeuvre de l'entrepôt* ####

L'entrepôt de données a été implémenté en utilisant le logiciel SQLDeveloper. Seule les données concernant les années 2015 et 2016 nous intéressent.   
L'entrepôt repose sur le schéma en étoile présenté ci-dessus : il est nécessaire de respecter les différentes organisations des tables ainsi que les différents types primitifs utilisés dans le schéma. 

- <u>Sources</u>  
    - [1] https://www.kaggle.com/russellyates88/suicide-rates-overview-1985-to-2016  
    Ensemble de données qui regroupe des statistiques sur les suicides par pays, ainsi que les données économiques (PIB par habitant et IDH) de ces pays.
      
    - [2] https://www.kaggle.com/unsdsn/world-happiness  
    Ensemble de données qui regroupent des statistiques sur le bonheur des pays.  
      
- <u>Intégration & nettoyage des données</u>  
    Le processus d'intégration est différent pour les 4 tables de l'entrepôt de données.
    - La table **PAYS**  
    La table **PAYS** est relativement aisée à remplir : il suffit d'importer les données des colonnes *Region* (renommée en **Region**) et *Country* (renommée en **NOM_PAYS**) de l'ensemble [2].
    - La table **ECONOMIE**  
    La table **ECONOMIE** est tout aussi facile à remplir : il suffit d'importer les données des colonnes *GDP_PER_CAPITA* (renommée en **PIB_PAR_HABITANT**), *HDI for year* (renommée en **IDH**), *country* (renommée en **NOM_PAYS**) et *year* (renommée en **ANNEE**) de l'ensemble [1].
    - La table **BONHEUR**  
    La table **BONHEUR**, bien que facile à remplir, nécessite une légère manipulation : il suffit d'importer les colonnes *Country* (renommée en **NOM_PAYS**), *Happiness Rank* (renommée en **CLASSEMENT_MONDIAL_BONHEUR**) et *Happiness Score* (renommée en **NOTE_BONHEUR**) de l'ensemble [2]. 
      La colonne **ANNEE** est remplie en fonction du fichier source : il existe plusieurs fichiers .csv dans l'ensemble de données sources, dont 2015.csv et 2016.csv qui nous intéressent. Il suffit ensuite de remplir tout les ```INSERT``` générés avec la bonne année (à l'aide d'un CTRL+F couplé d'un 'Test to Search For' et 'Replace With'), puis les exécuter.
    - La table **FAITS** est la dernière table de l'entrepôt de données. Une petite manipulation est nécessaire pour remplir la table : il suffit d'importer les colonnes *country* (renommée en **NOM_PAYS**), *age* (renommée en **TRANCHE_AGE**), *year* (renommée en **ANNEE**), *sex* (renommée en **GENRE**), *suicides_no* (renommée en **NOMBRE_SUICIDE**), *suicides/100k pop* (renommée en **TAUX_SUICIDE**) et *population* (renommée en **POPULATION**).
    Il est nécessaire de procéder à un *bitmapping* pour la colonne **GENRE** à l'aide du même procédé utilisé pour modifier les requêtes ````INSERT```` : il faut remplacer toutes les occurrences de "female" par 0 et toutes les occurrences de "male" par 1. 

Les multiples instructions énoncées ci-dessus permettent de reproduire l'entrepôt de données du projet. 

#### *liste des requêtes* ####

- Group By  
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
