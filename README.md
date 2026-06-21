# Étude de cas SQL — E-commerce Olist

Analyse business du [dataset e-commerce brésilien Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (~100k commandes réelles) en **SQL avancé** sur **DuckDB**.

Ce projet répond à 10 questions business concrètes — chacune avec la requête, le résultat et un insight exploitable. L'objectif n'est pas seulement d'écrire du SQL, mais de transformer la donnée en décisions.

---

## Stack technique

`SQL` · `DuckDB` · `Python` · `pandas` · `Jupyter`

## Dataset

9 tables relationnelles : commandes, articles, paiements, avis, produits, vendeurs, clients, géolocalisation, traduction des catégories.
*Données non incluses dans le repo (Kaggle, ~120MB) — voir le lien ci-dessus.*

---

## Techniques SQL démontrées

- `JOIN` multi-tables sur le schéma relationnel
- Agrégation et filtrage avec `GROUP BY` + `HAVING`
- Logique conditionnelle et bucketing avec `CASE WHEN`
- **CTEs** (`WITH`) pour une logique lisible et structurée en étapes
- **Window functions** : `LAG()`, `SUM() OVER ()`, `RANK()`

---

## Questions business & insights

### Q1 — Délai moyen de livraison par état
Quels états brésiliens ont les pires délais de livraison ?
**Insight :** Les états du nord (RR, AP, AM) attendent en moyenne ~29 jours contre 8.7 jours pour São Paulo — soit 3x plus longtemps. Ces états ont un très faible volume de commandes, ce qui pointe une faible couverture logistique. Des stocks décentralisés réduiraient ce gap.

### Q2 — Top 10 vendeurs par chiffre d'affaires
Quels vendeurs génèrent le plus de revenus ?
**Insight :** 9 des 10 meilleurs vendeurs sont basés à São Paulo. Deux stratégies distinctes émergent : volume élevé / prix bas (SP) vs volume faible / prix premium (le vendeur de Bahia, panier moyen 543€). Encourager les vendeurs premium hors SP diversifierait géographiquement le CA.

### Q3 — Taux de livraisons en retard
Quelle proportion des commandes est livrée après la date estimée ?
**Insight :** Seulement 8.1% des commandes sont en retard. En moyenne, les livraisons arrivent ~12 jours *avant* la date estimée — Olist sur-estime volontairement les délais pour gérer les attentes clients. Le vrai problème se concentre sur les 8% en retard, probablement corrélés aux états du nord (Q1).

### Q4 — Catégories avec les meilleurs scores d'avis
Quelles catégories génèrent la meilleure satisfaction client ?
**Insight :** Les livres et accessoires de voyage dominent (4.45 et 4.32 / 5). Ce sont des produits légers, faciles à expédier et peu sujets aux dommages — la qualité logistique explique probablement ces scores autant que le produit lui-même.

### Q5 — Croissance mensuelle du CA *(window function)*
Quelle est la tendance du CA mois par mois ?
**Insight :** Le CA a été multiplié par ~21 en 18 mois. Pic en novembre 2017 (+52%, Black Friday). La croissance se stabilise début 2018 autour de 900k–1M€/mois — signe de maturité de la plateforme. Utilise `LAG()` pour calculer la croissance MoM sans auto-jointure.

### Q6 — Panier moyen par méthode de paiement
Les clients dépensent-ils plus selon leur méthode de paiement ?
**Insight :** La carte de crédit domine — 75% des commandes et le panier moyen le plus élevé (163€). Les vouchers représentent 3.8% des commandes avec un panier 2x plus faible (65€), suggérant un usage pour de petits achats ou en complément.

### Q7 — Clients fidèles vs clients uniques *(CTE)*
Les clients récurrents représentent-ils une part significative du CA ?
**Insight :** 97% des clients ne commandent qu'une seule fois — un problème de rétention majeur. Pourtant les clients fidèles dépensent 3x plus en moyenne (421€ pour 3+ commandes vs 138€ pour une seule). Un programme de fidélisation ciblant même 5% des clients uniques pourrait générer des millions sans acquisition.

### Q8 — Vendeurs avec un fort taux d'avis négatifs *(CTE)*
Quels vendeurs concentrent les pires expériences client ?
**Insight :** Le pire vendeur a un taux d'avis négatifs de 65% sur 136 avis — un candidat à la suspension. Un système d'alerte automatique à 20% de taux négatif permettrait d'intervenir avant que la note globale de la plateforme se dégrade.

### Q9 — Prix du produit vs score d'avis
Les produits plus chers reçoivent-ils de meilleurs avis ?
**Insight :** Le prix n'a quasiment aucun impact sur la satisfaction — les scores restent entre 4.00 et 4.06 quelle que soit la tranche de prix. La satisfaction est pilotée par la logistique et le service, pas par le prix du produit.

### Q10 — Top 5 catégories par CA & part du total *(deux window functions)*
Quelles catégories génèrent le plus de revenus et quelle part du total ?
**Insight :** Les 5 premières catégories représentent ~40% du CA total. Health & Beauty domine (9.4%) avec un panier moyen élevé malgré moins de commandes. Utilise `SUM() OVER ()` pour la part du total et `RANK()` pour le classement.

---

## Lancer le projet

```bash
pip install duckdb pandas jupyter
# Télécharger le dataset Olist depuis Kaggle dans le dossier du projet
jupyter notebook analysis.ipynb
```

---

## Auteur

Said Choukri — [LinkedIn](https://www.linkedin.com/in/choukrisaid/) · [GitHub](https://github.com/ChoukriSaid)
