# Partogramme — suivi du travail en salle de naissance

Suivi d'une patiente pendant l'accouchement : dilatation, examens, équipe, naissance.
Données 100 % fictives (TP pédagogique). Maquette de référence : `maquette.png`.

## Fichiers

- `config.js` : adresse et clé du projet Supabase. **Il ne doit contenir que la clé
  publishable** (celle qui commence par `sb_publishable_`). Si la requête Supabase renvoie
  401, vérifier cette ligne en premier : un jour, plusieurs clés s'y sont retrouvées collées
  bout à bout (dont des clés secrètes) et tout était bloqué.
- `app.js` : navigation entre les onglets + page Patientes. Un fichier par page :
  `grossesses.js`, `examens.js`, `enfants.js`, `praticiens.js`, `export.js` (Excel).
- Bibliothèques depuis un CDN : supabase-js v2, chart.js v4, exceljs 4.4.0.

## Base Supabase (projet `gqawcxmvizundgcjkgkm`)

6 tables : `patiente`, `grossesse`, `enfant`, `examen`, `orientation`, `praticien`.

- `grossesse` relie une `patiente` aux 4 praticiens (sage-femme, obstétricien,
  anesthésiste, pédiatre) ; `examen` et `enfant` dépendent d'une `grossesse`.
- `orientation` porte un pictogramme utilisé par les examens.
- `sexe` (patiente, enfant) et `specialite` (praticien) sont des énumérations Postgres.
- Les règles RLS sont déjà ouvertes en lecture, ajout, modification et suppression
  pour `anon` (données fictives) : ne pas les refermer.

## Tester en local (pièges du PC)

- Pas de Python sur ce PC ; `npx` est bloqué par la stratégie d'exécution PowerShell.
  Lancer le serveur ainsi : `cmd /c "npx -y http-server partogramme -p 8765 -c-1"`.
- Le `-c-1` est important : sans lui, le serveur garde les fichiers 1 h en cache et le
  navigateur affiche l'ancienne version des modifications.
- Le navigateur de test (MCP Playwright) refuse d'ouvrir les fichiers en `file://`.

## Mise en ligne

- Dépôt GitHub public du même nom ; chaque `git push` publie via `.github/workflows/deploy.yml`
  (ne pas le modifier).
- Adresse du site : https://sps-g38-parto.professeurpetitchat.com/
