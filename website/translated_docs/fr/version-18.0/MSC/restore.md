---
id: version-18.0-restore
title: Page restitution
sidebar_label: Page restitution
original_id: restore
---

## Restauration d'une sauvegarde

You can manually restore an archive of the current database using the **Restore** page. Cette page propose plusieurs options permettant de contrôler la restitution des bases :

![](assets/en/MSC/MSC_restore.png)

> Les systèmes de restitution automatique de 4D restituent les bases et incluent le fichier d'historique si nécessaire.

La liste située dans la partie gauche de la fenêtre affiche les sauvegardes existantes de la base. Vous pouvez également cliquer sur le bouton Parcourir... situé sous la zone afin d’ouvrir tout autre fichier d’archive provenant d’un autre emplacement. Il est alors ajouté à la liste des archives.

Lorsque vous sélectionnez une sauvegarde dans cette liste, la partie droite de la fenêtre affiche les informations relatives à cette sauvegarde :

- **Path**: Complete pathname of the selected backup file. Le bouton Montrer ouvre le dossier de sauvegarde dans une fenêtre système.
- **Date and Time**: Date and time of backup.
- **Content**: Contents of the backup file. Chaque élément de la liste est associé à une case à cocher, permettant de spécifier si vous souhaitez ou non le restituer. You can also use the **Check All** or **Uncheck All** buttons to set the list of items to be restored.
- **Destination folder of the restored files**: Folder where the restored files will be placed. Par défaut, 4D restitue les fichiers dans un dossier nommé “Nomarchive” (sans extension) placé à côté du fichier de structure de la base. To change this location, click on **[...]** and specify the folder where you want the restored files to be placed.

The **Restore** button launches the manual restoration of the selected element(s).

## Intégration successive de plusieurs fichiers d’historiques

The **Integrate one or more log file(s) after restore** option allows you to integrate several data log files successively into a database. Si, par exemple, vous disposez de 4 sauvegardes d’historique (.4BL) consécutives (correspondant à 4 sauvegardes de la base), vous pouvez restituer la première sauvegarde de la base puis intégrer une à une les sauvegardes d’historique. Ce principe permet par exemple de récupérer un fichier de données alors que les derniers fichiers de sauvegarde de la base sont manquants.

Lorsque cette option est cochée, 4D affiche une boîte de dialogue standard d’ouverture de fichiers à l’issue de la restitution, vous permettant de sélectionner la sauvegarde d’historique à intégrer. La boîte de dialogue d’ouverture est affichée de nouveau après chaque intégration, jusqu’à ce qu’elle soit annulée.

## Restituer une base chiffrée

Souvenez-vous que la clé de chiffrement des données (phrase secrète) a peut-être été changée au fil des versions des fichiers de sauvegarde (.4BK), des fichiers .journal (.4BL) et de la base courante. Il est important que les clés de chiffrement soient toujours mises en correspondance.

Au moment de restituer une sauvegarde et d'intégrer le fichier d'historique courant dans une base chiffrée :

- Si vous restituez une sauvegarde à l'aide d'une ancienne phrase secrète, cette dernière sera demandée au prochain démarrage de la base.
- Après un chiffrement, à l'ouverture du fichier de données chiffrées, une sauvegarde est exécutée et un nouveau fichier journal est créé. Ainsi, il n'est pas possible de restituer un fichier chiffré .4BK avec une clé et d'intégrer les fichiers chiffrés .4BL avec une autre clé.

La séquence suivante illustre les principes d'une opération de clé multiple de sauvegarde/restitution :


| Opération                                | Fichiers générés                                   | Commentaire                                                                                                                                                                                                                                                                                           |
| ---------------------------------------- | -------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Créer une nouvelle base de données       |                                                    |                                                                                                                                                                                                                                                                                                       |
| Ajouter des données (enregistrement # 1) |                                                    |                                                                                                                                                                                                                                                                                                       |
| Sauvegarder la base de données           | 0000.4BL et 0001.4BK                               |                                                                                                                                                                                                                                                                                                       |
| Ajouter des données (enregistrement # 2) |                                                    |                                                                                                                                                                                                                                                                                                       |
| Sauvegarder la base de données           | 0001.4BL et 0002.4BK                               |                                                                                                                                                                                                                                                                                                       |
| Ajouter des données (enregistrement # 3) |                                                    |                                                                                                                                                                                                                                                                                                       |
| Chiffrer un fichier de données avec clé1 | Fichier 0003.4BK (chiffré avec clé1)               | Le chiffrement sauvegarde les fichiers originaux (y compris le fichier journal) dans le dossier "Replaced files (Encrypting) YYY-DD-MM HH-MM-SS". À l'ouverture du fichier de données chiffrées, un nouveau fichier journal est créé et une sauvegarde est effectuée pour activer ce fichier journal. |
| Ajouter des données (enregistrement # 4) |                                                    |                                                                                                                                                                                                                                                                                                       |
| Sauvegarder la base de données           | Fichiers 0003.4BL et 0004.4BK (chiffrés avec clé1) | Il est possible de restituer 0003.4BK et d'intégrer 0003.4BL                                                                                                                                                                                                                                          |
| Ajouter des données (enregistrement # 5) |                                                    |                                                                                                                                                                                                                                                                                                       |
| Sauvegarder la base de données           | Fichiers 0004.4BL et 0005.4BK (chiffrés avec clé1) | Il est possible de restituer 0003.4BK et d'intégrer 0003.4BL + 0004.4BL. Il est possible de restituer 0004.4BK et d'intégrer 0004.4BL                                                                                                                                                                 |
| Ajouter des données (enregistrement # 6) |                                                    |                                                                                                                                                                                                                                                                                                       |
| Chiffrer un fichier de données avec clé2 | Fichier 0006.4BK (chiffré avec clé2)               | Le chiffrement sauvegarde les fichiers originaux (y compris le fichier journal) dans le dossier "Replaced files (Encrypting) YYY-DD-MM HH-MM-SS". À l'ouverture du fichier de données chiffrées, un nouveau fichier journal est créé et une sauvegarde est effectuée pour activer ce fichier journal. |
| Ajouter des données (enregistrement # 7) |                                                    |                                                                                                                                                                                                                                                                                                       |
| Sauvegarder la base de données           | Fichiers 0006.4BL et 0007.4BK (chiffrés avec clé2) | Il est possible de restituer 0006.4BK et d'intégrer 0006.4BL                                                                                                                                                                                                                                          |
| Ajouter des données (enregistrement # 8) |                                                    |                                                                                                                                                                                                                                                                                                       |
| Sauvegarder la base de données           | Fichiers 0007.4BL et 0008.4BK (chiffrés avec clé2) | Il est possible de restituer 0006.4BK et d'intégrer 0006.4BL + 0007.4BL. Il est possible de restituer 0007.4BK et d'intégrer 0007.4BL                                                                                                                                                                 |
> Au moment de restituer une sauvegarde et d'intégrer un ou plusieurs fichiers .4BL, les fichiers restitués .4BK et .4BL doivent avoir la même clé de chiffrement. Durant le processus d'intégration, si aucune clé de chiffrement valide n'est trouvée dans le trousseau de 4D lors de l'intégration du fichier .4BL, une erreur est générée.
> 
> Si vous avez stocké plusieurs clés de données sur le même appareil externe, la restitution d'une sauvegarde et l'intégration de fichiers d'historique permettront de trouver automatiquement la clé correspondant à l'appareil connecté.