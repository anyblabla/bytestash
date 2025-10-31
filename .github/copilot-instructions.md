## Contexte rapide

Ce dépôt est un miroir public du contenu d'une instance ByteStash (collection de fragments de code/snippets). Il contient essentiellement des fichiers Markdown et des extraits destinés au partage; il ne contient pas le code source de l'application ByteStash elle-même.

Principaux points à connaître :
- Le répertoire racine contient des nombreux fichiers Markdown (snippets) organisés par sujet/langage.
- Le fichier `README.md` décrit le but du dépôt et contient des liens vers l'instance publique (par exemple : https://bytestash.blablalinux.be/public/snippets).

## Objectif pour un agent AI

Fournir des modifications non invasives, organiser ou corriger la documentation, normaliser les métadonnées des snippets, et aider à regénérer/structurer le contenu Markdown. Eviter de modifier l'historique ou la logique d'une application inexistante dans ce dépôt.

## Règles de contribution automatiques (pour l'IA)

1. Prioriser la clarté et la cohérence des fichiers Markdown (titres, en-têtes, métadonnées front-matter si présentes).
2. Ne pas créer ou modifier du code d'exécution (scripts binaires) qui n'existent pas ici — ce dépôt archive du contenu, pas l'app.
3. Quand vous reformatez ou renommez un snippet, conservez le contenu original dans un commit séparé avec un message explicite (par ex. "docs: normalize title for <nom_de_fichier>").

## Architecture et patterns observables

- Pas d'app serveur dans le dépôt. Contenu = collection de fichiers Markdown.
- Les snippets sont souvent des playbooks/autres scripts stockés directement en Markdown (exemples : `Docker_script_install.md`, `Nginx_Proxy_Manager_compose.md`). Utiliser ces fichiers comme source d'exemples pour normalisation.
- Les messages de commit peuvent contenir listes massives de suppressions (voir `.git/COMMIT_EDITMSG` lors d'une validation locale) — l'agent doit éviter les changements de masse non demandés.

## Workflows développeur utiles (découverts)

- Parcourir le dépôt localement et utiliser la recherche texte pour trouver des snippets liés (grep / IDE search).
- Aucune commande build/test spécifique au dépôt (pas de package.json, pyproject.toml, etc.). Ne pas tenter d'exécuter des builds.

## Exemples concrets à mentionner

- Pour normaliser un snippet de déploiement Docker compose, ouvrir `Nginx_Proxy_Manager_compose____partir_de_la_v1_12_6_.md` et vérifier la section `docker-compose.yml` — conserver la structure originale des blocs de code.
- Si un fichier contient des instructions système (ex: `Mise___jour_automatique_des_serveurs_Ubuntu_par_Cron.md`), vérifier que les commandes sont présentées en blocs de code et qu'elles utilisent des chemins absolus ou variables d'environnement clairs.

## Suggestions d'édition pratiques

- Favoriser des commits atomiques et bien nommés.
- Préserver les extraits originaux en ajoutant une section "Original" si vous refactorez un snippet long.
- Si vous détectez des liens externes morts, proposer une mise à jour ou laisser un commentaire dans le commit (ne pas remplacer automatiquement sans vérification).

## Fichiers de référence

- `README.md` — description du projet et lien vers l'instance ByteStash.
- `.git/COMMIT_EDITMSG` (local) — utile pour comprendre récents changements lors d'une validation locale, mais pas commité dans le dépôt.

## Quand demander de l'aide

- Si une modification nécessite d'exécuter des scripts ou de valider des commandes en environnement (par ex. tester une install), demander une VM/CI ou des instructions supplémentaires.

---

Si vous voulez que j'ajoute des règles supplémentaires (p. ex. front-matter YAML standard à appliquer, conventions de nommage de fichiers ou une checklist de revue), dites lesquelles et je les intégrerai.
