# Changelog

Ce fichier répertorie les changements entre différentes versions du schéma.

## Version 2.0.0

### Version de Frictionless

Pour l'instant on reste sur Frictionless 4.

### Ajout des colonnes non-listes du [format DECP 2022](https://www.legifrance.gouv.fr/jorf/id/JORFARTI000048678668)

- attributionAvance
- tauxAvance
- ccag
- origineUE
- origineFrance
- marcheInnovant
- offresRecues
- sousTraitanceDeclaree
- typeGroupementOperateurs
- idAccordCadre

### Remplacement du "." de séparation par un "_"

- acheteur.id => acheteur_id
- acheteur.nom => acheteur_nom
- titulaire.id => titulaire_id
- titulaire.typeIdentifiant => titulaire_typeIdentifiant
- titulaire.nom => titulaire_nom
- lieuExecution.code => lieuExecution_code
- lieuExecution.typeCode => lieuExecution_typeCode

## Version 0.1.0 - 2018-06-29

Publication initiale, basée sur le format DECP 2019.
