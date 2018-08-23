# Prérequis

Python 3, pip.

# Installation

```sh
pip install -r requirements.txt
```

# Utilisation

- Récupérer les [fichiers des barèmes IPP au format XSLX](https://framagit.org/french-tax-and-benefit-tables/ipp-tax-and-benefit-tables-xlsx)

- Facultatif: Préprocésser le fichier pour tenter d'ajouter les headers manquants:
```sh
./preprocess.py baremes-ipp-prelevements-sociaux-social-security-contributions.xlsx preprocessed_baremes.xlsx
```

> Cette étape peut ajouter les headers standards si les étiquettes situées en deuxième ligne sont standards (par exemple, `"Date d'effet"` pour `date`). Il ne permet pas d'ajouter un header pour les colonnes de paramètres.

- Parser le fichier:

```sh
./parse_xlsx.py path/to/xslx/file/or/directory path/to/output/
```

# Format normalisé d'une feuille de paramètres

- La première ligne (cachée à la visialisation) doit être un header qui contient:
  - Soit un des mots clés suivant: `date`, `reference`, `notes`, `date_parution_jo`
  - Soit le nom du paramètre de la colonne.

- Il doit exister une colonne `date`, dont les valeurs sont de type date.
  - À la première valeur nulle ou d'au autre type dans la colonne date, le parseur ignore la ligne et toutes celles qui suivent.
  - S'il n'y a pas de colonne nommée `date`, la feuille sera ignorée, avec warning.

- Une colonne de paramètre qui n'a pas de header sera ignorée, sans warning.

- Le nom d'un paramètre peut être un chemin. Par exemple, si une feuille contient deux colonnes dont les titres sont `modulation/taux_1` et  `modulation/taux_2`, ces deux colonnes seront interprétés comme deux paramètres `taux_1` et `taux_2` d'un noeud `modulation`.
