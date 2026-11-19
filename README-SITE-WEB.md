# Personal Wealth Manager — Édition personnelle (site web autonome)

**Fichier : `personal-wealth-manager.html`** — un seul fichier HTML (179 Ko) qui s'ouvre dans n'importe quel navigateur, sur ordinateur comme sur téléphone. Aucun serveur, aucune installation, aucune dépendance : double-cliquez sur le fichier et l'application démarre.

## Ce que c'est

Votre gestionnaire patrimonial simplifié pour une **utilisation strictement personnelle** :

- **Un seul espace** : plus de compartiments « Personnel / Trading / Consolidé » ni de sélecteur de vue — tout votre patrimoine est réuni dans une vue unique, sans double comptage (l'equity d'un compte de trading remplace son solde de caisse, et les actifs rattachés à ce compte ne sont jamais additionnés deux fois).
- **Aucun compte utilisateur, aucun mot de passe, aucune donnée envoyée à un serveur** : la base vit dans votre navigateur (localStorage), et vous en gardez la maîtrise totale via les sauvegardes locales et GitHub.

## Modules conservés

| Module | Contenu |
|---|---|
| **Tableau de bord** | 12-14 indicateurs (patrimoine net, actifs, passifs, liquidités, revenus/dépenses du mois, cash-flow, revenus passifs, taux d'épargne, endettement, portefeuille, performance, XIRR/TWR), filtres de période (3/6/12 mois, début), 5 graphiques |
| **Comptes** | 8 types (banque, Mobile Money, espèces, épargne, courtage, trading…), soldes calculés, transferts neutres, rapprochement de solde documenté, archivage |
| **Transactions** | Revenus / dépenses / transferts / ajustements, catégories, statuts, fréquences, actif associé, filtres et recherche |
| **Actifs** | 10 catégories, historique de valorisation **jamais écrasé**, plus/moins-value latente, rendements brut/net, coût de détention, classifications économiques indépendantes |
| **Passifs** | Capital restant dû, paiements **capital / intérêts / frais séparés** (le capital remboursé n'est pas une dépense de consommation), échéancier théorique |
| **Trading** | Page optionnelle pour comptes de type trading / courtage / portefeuille d'investissement : equity avec instantanés historiques, P&L latent et réalisé, frais, **XIRR** (Money-Weighted) et **TWR** (Time-Weighted), placements financiers long terme |
| **Objectifs** | 5 types, progression calculée en direct depuis vos données réelles |
| **Rapports** | Mensuel et annuel, export **JSON**, **CSV** (Excel), **impression PDF** |
| **Multi-devises** | 9 devises initiales + ajout libre, historique des taux (jamais écrasé), devise de consolidation configurable, taux jamais inventés (avertissements explicites) |
| **Sauvegardes** | Locale automatique + instantanés + **GitHub via token** (voir ci-dessous) |

## Sauvegardes — trois niveaux

### 1. Locale automatique (défaut)
Chaque modification est écrite immédiatement dans le navigateur (indicateur « Sauvegardé HH:MM » dans l'en-tête). Vos données survivent aux rechargements et redémarrages.

### 2. Historique local d'instantanés
Les **5 derniers instantanés complets** sont conservés dans le navigateur (`Paramètres → Historique local d'instantanés`) : un automatique chaque jour à la première saisie, un avant chaque restauration / démonstration / remise à zéro, plus les instantanés manuels (« + Instantané maintenant »). Chacun est restaurable en un clic.

### 3. Sauvegarde GitHub via token personnel
`Paramètres → Sauvegarde GitHub` : votre sauvegarde JSON complète est envoyée dans **un fichier de votre dépôt GitHub** via l'API officielle.

**Créer le token (une fois, ~2 minutes) :**
1. GitHub → *Settings* → *Developer settings* → *Personal access tokens* → **Fine-grained tokens** → *Generate new token*.
2. Nommez-le (ex. `sauvegarde-finances`), fixez une expiration (90 jours conseillé, à renouveler).
3. *Repository access* → **Only select repositories** → choisissez **un dépôt privé dédié** (ex. `mes-finances` — créez-le d'abord sur GitHub).
4. *Permissions* → *Repository permissions* → **Contents : Read and write**.
5. *Generate token* → copiez le token (il commence par `github_pat_…`).

**Dans l'application :** collez le token, renseignez propriétaire / dépôt / branche (main par défaut) / chemin du fichier (`pwm-backup.json` par défaut), puis :
- **Tester la connexion** → vérifie le token et le dépôt (affiche les permissions) ;
- **⬆ Envoyer vers GitHub** → crée ou met à jour le fichier (un commit par sauvegarde) ;
- **⬇ Restaurer depuis GitHub** → récupère la dernière sauvegarde distante et remplace les données locales (instantané local créé avant) ;
- **Sauvegarde automatique** (case à cocher) → push ≈ 30 s après chaque modification, au maximum une fois toutes les 5 minutes.

**Sécurité :**
- Le token reste dans **ce navigateur uniquement** (localStorage) ; il n'apparaît jamais en clair (champ de type mot de passe) et n'est transmis qu'à `api.github.com`.
- Utilisez un token **limité à un seul dépôt privé** : en cas de fuite, l'exposition reste confinée à ce dépôt. Révoquez-le depuis GitHub si vous changez d'appareil ou en cas de doute.
- Le dépôt peut être privé : vos montants ne sont visibles que de vous.
- Les erreurs sont explicites (401 token invalide, 403 permission manquante, 404 dépôt/fichier introuvable, hors ligne…) et le statut de la dernière sauvegarde est affiché avec un lien vers le commit.

## Démarrage

1. Ouvrez `personal-wealth-manager.html` (double-clic).
2. « **+ Créer mon premier compte** » pour vos vraies données, ou « **Charger les données de démonstration** » pour explorer (12 mois d'historique fictif marqué `[Démo]`).
3. Avant de saisir des montants dans une devise étrangère, ajoutez son taux dans `Paramètres → Taux de change` (en devise de base par 1 unité).
4. Configurez ensuite votre sauvegarde GitHub (voir ci-dessus) — c'est la garantie de ne jamais perdre vos données avec ce navigateur.

## Limites (choix de simplification)

- La « base de données » est le localStorage de **ce navigateur sur cet appareil** ; d'où l'importance des sauvegardes GitHub/JSON pour retrouver vos données ailleurs.
- Pas d'import CSV de relevés, pas de budgets mensuels, pas de journal d'audit : disponibles dans la version serveur complète conservée dans le projet (`README-WEALTH-MANAGER.md`).
- Projections d'objectifs simplifiées (progression réelle, sans scénarios d'apport mensuel).
- Montants convertis au taux connu à la date de chaque opération, repli documenté si le taux manquait ; les montants d'origine ne sont jamais convertis définitivement.
