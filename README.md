# Monevia Data

Base de données publique des placements financiers utilisée par l'application [Monevia](https://apps.apple.com/), simulateur d'épargne et d'investissement disponible sur iOS.

## À propos

Ce dépôt contient les données de référence consommées par l'application Monevia pour proposer des suggestions de placements lors de la création de simulations d'épargne. Les données sont régulièrement mises à jour : taux des livrets réglementés, rendements des ETF, performances des SCPI, et rendements des fonds euros.

## Contenu

### `placements.json`

Fichier principal contenant deux sections :

- **`taux_livrets`** — Livrets réglementés français (Livret A, LDDS, LEP, Livret Jeune, CEL, PEL) avec leurs taux en vigueur, plafonds et règles fiscales.
- **`placements_suggeres`** — Catalogue de placements suggérés (ETF, SCPI, fonds euros) avec rendements historiques, frais, niveau de risque et compatibilité avec les enveloppes fiscales (PEA, CTO, AV, PER).

## Structure des données

### Champs communs à tous les placements

| Champ | Type | Description |
|---|---|---|
| `id` | string | Identifiant unique du placement |
| `type` | string | Catégorie : `etf`, `scpi`, `fonds_euros` |
| `actif` | boolean | `true` si le placement est encore proposé |
| `nom_court` | string | Nom affiché dans l'application |
| `nom_complet` | string | Nom officiel complet |
| `description` | string | Description courte (1-2 phrases) |
| `risque` | integer | Indicateur de risque de 1 (faible) à 7 (élevé), aligné sur l'échelle SRI européenne |
| `enveloppes_compatibles` | array | Liste des enveloppes compatibles : `pea`, `pea_pme`, `cto`, `av`, `per` |

### Champs spécifiques par type

**ETF**
- `ticker`, `isin`, `emetteur`, `zone_geo`, `devise`
- `frais_annuels_pct` (TER en %)
- `rendement_5ans`, `rendement_10ans`, `rendement_20ans` (annualisés)
- `url_source` (lien justETF ou émetteur)

**SCPI**
- `societe_gestion`, `ticket_entree`
- `frais_entree_pct`, `frais_gestion_pct`
- Rendements historiques (peuvent être `null` pour les SCPI récentes)

**Fonds euros**
- `assureur`
- `rendement_2024_pct`, `rendement_2025_pct`, `rendement_3ans_moyenne`
- `garantie_capital`

### Livrets réglementés

Structure dédiée avec `taux_actuel`, `plafond`, `garanti_etat`, `fiscalite`.

## Utilisation

L'application Monevia récupère le fichier `placements.json` au démarrage pour proposer des suggestions de placements aux utilisateurs. Les utilisateurs gardent la possibilité de saisir manuellement n'importe quel placement personnalisé.

URL d'accès direct au fichier :
```
https://raw.githubusercontent.com/nebuca/monevia-data/main/placements.json
```

## Mises à jour

Le fichier est mis à jour selon le calendrier suivant :

- **Livrets réglementés** : à chaque révision officielle (1er février et 1er août)
- **Fonds euros** : annuellement, en février-mars (publication des taux de l'année précédente)
- **SCPI** : annuellement, au premier trimestre
- **ETF** : annuellement, ou lors d'ajouts/retraits significatifs

Le champ `derniere_maj` indique la date de la dernière modification, et `version` suit un format année-mois.

## Sources

Les données sont compilées à partir de sources publiques officielles :

- Taux des livrets : Banque de France, Ministère de l'Économie
- ETF : [justETF](https://www.justetf.com), sites des émetteurs (Amundi, BlackRock, BNP Paribas)
- SCPI : sociétés de gestion, ASPIM
- Fonds euros : ACPR, France Assureurs, communications des assureurs

## Avertissement

Les rendements historiques affichés sont **indicatifs** et ne préjugent pas des performances futures. Les données fournies dans ce dépôt ne constituent en aucun cas un conseil en investissement. Les utilisateurs sont invités à vérifier les informations auprès des émetteurs avant toute décision d'investissement réel.

## À propos de Monevia

Monevia est une application iOS de simulation d'épargne et d'investissement, indépendante et sans publicité. Aucune donnée utilisateur n'est transmise à un serveur externe : toutes les simulations restent stockées localement sur l'appareil.

- Site officiel : [à venir]
- Application : disponible sur l'App Store

## Licence

Les données contenues dans ce dépôt sont mises à disposition à des fins informatives. Elles peuvent être consultées et étudiées librement. Toute réutilisation commerciale ou redistribution doit faire l'objet d'une autorisation préalable.

---

© Nebuca — Microentreprise éditrice de Monevia
