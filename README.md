# **Bases de données évoluées - Projet 2021** #
## Etude du bien-être dans le monde en 2015 et 2016 ##
  
#### *Mise en oeuvre de l'entrepôt* ####

L'entrepôt de données a été implémenté en utilisant le logiciel SQLDeveloper. Seule les données concernant les années 2015 et 2016 nous intéressent.   
L'entrepôt repose sur le schéma en étoile présenté ci-dessus : il est nécessaire de respecter les différentes organisations des tables ainsi que les différents types primitifs utilisés dans le schéma.

- <u>Sources</u>
  - [1] https://www.kaggle.com/russellyates88/suicide-rates-overview-1985-to-2016  
    Ensemble de données qui regroupe des statistiques sur les suicides par pays, ainsi que les données économiques (PIB par habitant et IDH) de ces pays.  
    *master.csv*

  - [2] https://www.kaggle.com/unsdsn/world-happiness  
    Ensemble de données qui regroupent des statistiques sur le bonheur des pays.  
    *2015.csv et 2016.csv*

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
