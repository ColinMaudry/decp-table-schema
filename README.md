# Schéma tabulaire pour les Données Essentielles de la Commande Publique (DECP)

Au format [Table Schema](https://specs.frictionlessdata.io/table-schema/).

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Schéma tabulaire pour les Données Essentielles de la Commande Publique (DECP)](#schma-tabulaire-pour-les-donnes-essentielles-de-la-commande-publique-decp)
	- [Description des fichiers](#description-des-fichiers)
	- [Documentation du schéma](#documentation-du-schma)
	- [Comment utiliser le schéma](#comment-utiliser-le-schma)
	- [Intégration continue](#intgration-continue)

<!-- /TOC -->

Ce schéma vise à remplacer le [schéma SCDL](https://scdl.opendatafrance.net/docs/schemas/marches-publics.html) développé par OpenDataFrance pour la publication des données de marchés publics en améliorant le support des modifications de marché. L'objectif de ce schéma est donc de publier, pour chaque marché, à la fois les données actuelles et les données historiques, en mettant en valeur les données actuelles pour faciliter l'exploitation des données.

Les données modifiées (`dureeMois`, `montant`, `titulaire.id`, `titulaire.typeIdentifiant`, `titulaire.denominationSociale`) sont renseignées dans le même champ que les données initiale du marché.

Par exemple, si un marché est initialement attribué pour un montant de 100 000 euros, puis son montant est modifié et est réévalué à 120 000 euros, la dernière version (à 120 000 euros) aura un `objetModification` non vide, `montant` = 120 000 et `donneesActuelles` = `1`.

Les [données exemples](https://github.com/ColinMaudry/decp-table-schema/tree/main/exemples) représentent un marché public attribué à deux attributaires. Le marché voit ensuite sa durée modifiée.

[Valider des données DECP Table Schema avec Validata](https://go.validata.fr/table-schema?schema_url=https%3A%2F%2Fgithub.com%2FColinMaudry%2Fdecp-table-schema%2Fraw%2Fmain%2Fschema.json)


## Description des fichiers

- [`CHANGELOG.md`](CHANGELOG.md) contient la liste des changements entre les différentes versions du schéma.
- [`exemples/`](tree/main/exemples) est un dossier qui contient des fichiers conformes au schéma décrit dans `schema.json`.
- [`LICENSE.md`](LICENSE.md) est le fichier de licence du dépôt.
- [`README.md`](README.md) est le fichier que vous lisez actuellement.
- [`requirements.txt`](requirements.txt) liste les dépendances Python nécessaires pour effectuer des tests en intégration continue du dépôt.
- [`schema.json`](schema.json) est le schéma au format Table Schema.

## Documentation du schéma

- Schéma créé le : 02/20/20
- Site web : https://github.com/ColinMaudry/decp-table-schema
- Version : 0.1.0

### Modèle de données

|Nom|Type|Description|Exemple|Propriétés|
|-|-|-|-|-|
|id (Numéro d'identification unique de marché public.)|chaîne de caractères|Les 4 premiers caractères sont l'année de notification, de 1 à 10 caractères l'identifiant internet, les deux derniers caractères sont le numéro de séquence (nombre de modifications : 00 pour les données initiales, 01 pour la première modification, etc).|2019056462312700|Valeur obligatoire, Taille minimale : 7, Taille maximale : 16|
|uid (Numéro d'identification unique de marché public (national))|chaîne de caractères|Identifiant du marché, préfixé avec le SIRET de l'acheteur pour être unique au niveau national.|233500016000402019056462312700|Valeur optionnelle, Taille minimale : 21, Taille maximale : 30|
|acheteur.id (Identification de l'acheteur (SIRET))|chaîne de caractères|En cas de groupement de commande, c'est le SIRET du mandataire.|23350001600040|Valeur obligatoire, Taille minimale : 14, Taille maximale : 14|
|acheteur.nom (Nom de l'acheteur)|chaîne de caractères|En cas de groupement de commande, c'est le nom du mandataire.|Conseil régional de Bretagne|Valeur obligatoire|
|nature (Nature du marché)|chaîne de caractères|Marché|Valeur obligatoire, Valeurs autorisées : Marché, Marché de partenariat, Accord-cadre, Marché subséquent|
|objet (Objet du marché ou du lot)|chaîne de caractères|Description synthétique de l'objet du marché ou du lot.|Impression d'affiches pour l'évènement Urban Trail|Valeur obligatoire, Taille maximale : 256|
|codeCPV (Code CPV principal)|chaîne de caractères|Nomenclature européenne permettant d'identifier les catégories de biens et de service faisant l'objet du marché (http://simap.ted.europa.eu/web/simap/cpv). Même si toléré, il préférable d'omettre le caractère de contrôle (-9))|79311000|Valeur obligatoire, Motif : `(^[0-9]{8,8}(\-[0-9])?)$`|
|procedure (Procédure de passation de marché)|chaîne de caractères|La procédure de passation de marché utilisée par l'acheteur.|Procédure adaptée|Valeur obligatoire, Valeurs autorisées : Procédure adaptée, Appel d'offres ouvert, Appel d'offres restreint, Procédure avec négociation, Marché passé sans publicité ni mise en concurrence préalable, Dialogue compétitif|
|lieuExecution.code (Code du lieu d'exécution)|chaîne de caractères|Code du lieu d'exécution (code postal, commune, canton, arrondissement, département, région, pays). Les codes INSEE sont à privilégier aux dépens du code postal.|35238|Valeur obligatoire|
|lieuExecution.typeCode (Type de code du lieu d'exécution)|chaîne de caractères|Le type de code utilisé pour désigner le lieu d'exécution. Hormis le Code postal, les codes sont des codes géographiques gérés par l'INSEE (http://www.insee.fr/fr/methodes/nomenclatures/cog/default.asp)|Code commune|Valeur obligatoire, Valeurs autorisées : Code postal, Code commune, Code arrondissement, Code canton, Code département, Code région, Code pays|
|lieuExecution.nom (Nom du lieu d'exécution)|chaîne de caractères|Rennes|Valeur obligatoire|
|dureeMois (Durée initiale du marché)|nombre entier|La durée du marché, en mois. La durée du marché comprend la durée des tranches et reconductions potentielles.|3|Valeur obligatoire|
|dateNotification (Date de notification)|date (format `%Y-%m-%d`)|Date à laquelle le marché ou la modification du marché a été notifié au(x) titulaire(s), au format AAAA-MM-JJ.|2019-04-27|Valeur obligatoire|
|datePublicationDonnees (Date de publication des données)|date (format `%Y-%m-%d`)|Date à laquelle les données du marché ou de la modification du marché ont été publiées, au format AAAA-MM-JJ.|2019-04-29|Valeur obligatoire|
|montant (Montant)|nombre réel|Montant forfaitaire ou estimé maximum HT du marché attribué.|21800.1|Valeur obligatoire|
|formePrix (Forme du prix)|chaîne de caractères|Ferme : le prix est fixé pour toute la durée marché. Ferme et actualisable : le prix peut évoluer périodiquement selon des conditions prévues dans le contrat initial (ex : variation d'indice). Révisable : l'acheteur et le(s) titulaire(s) peuvent s'entendre sur une modification du prix après la signature du marché.)|Ferme|Valeur obligatoire, Valeurs autorisées : Ferme, Ferme et actualisable, Révisable|
|titulaire.id (Identifiant du titulaire)|chaîne de caractères|Identifiant de l'opérateur économique auquel l'acheteur a attribué ce marché. En cas de marché multi-attributaires, il s'agit d'un des attributaires du marché.|81223113200034|Valeur obligatoire|
|titulaire.typeIdentifiant (Type d'identifiant (titulaire))|chaîne de caractères|Types d'identifiants possibles (favoriser le SIRET) : SIRET, TVA, TAHITI, RIDET, FRWF, IREP, UE, HORS-UE.|SIRET|Valeur obligatoire, Valeurs autorisées : SIRET, TVA, TAHITI, RIDET, FRWF, IREP, HORS-UE|
|titulaire.denominationSociale (Dénomination sociale)|chaîne de caractères|Nom de l'opérateur économique auquel l'acheteur a attribué ce marché. En cas de marché multi-attributaires, il s'agit d'un des attributaires du marché.|Colin Maudry|Valeur obligatoire|
|objetModification (Objet de la modification)|chaîne de caractères|Raisons de la modification du marché. Ce champ est vide pour une attribution et doit être rempli en cas de modification.|Augmentation de la durée du marché à 12 mois.|Valeur optionnelle|
|donneesActuelles (Données actuelles)|booléen|Valeurs considérées comme vraies : ['oui']Valeurs considérées comme fausses : ['non']Indique si cette ligne contient les données les plus récentes pour ce marché ("oui") ou bien des données qui ont été modifiées depuis ("non").|oui|Valeur optionnelle|
|anomalies (Anomalies)|chaîne de caractères|Les anomalies détéctées dans les données de ce marché, séparées par un point-virgule.|.id de l'acheteur manquant, trop court ou null;.titulaires manquant ou vide|Valeur optionnelle|


## Comment utiliser le schéma


## Intégration continue

Ce dépôt est configuré pour utiliser de l'intégration continue, si vous utilisez GitHub. À chaque commit, une suite de tests sera lancée via [GitHub Actions](https://github.com/features/actions) afin de vérifier :

- que votre schéma est valide à la spécification Table Schema ;
- que vos fichiers d'exemples sont conformes au schéma.

Si vous n'utilisez pas GitHub, vous pouvez lancer ces tests sur votre machine ou sur un autre service d'intégration continue comme Gitlab CI, Jenkins, Circle CI, Travis etc. Consultez la configuration utilisée dans [`.github/workflows/test.yml`](.github/workflows/test.yml).

Localement, voici la procédure à suivre pour installer l'environnement de test et lancer les tests :

```bash
echo "Création d'un environnement virtuel en Python 3.."
python3 -m venv venv
source venv/bin/activate

echo "Installation des dépendances..."
pip install -r requirements.txt

echo "Test de la validité du schéma..."
frictionless validate --type schema schema.json

echo "Test de la conformité des fichiers d'exemples.."
frictionless validate --schema schema.json exemple-valide.csv
```
