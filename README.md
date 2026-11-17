# Row Health — Analyse de la performance des campagnes marketing

**Projet portfolio | Data Analytics · Business Analysis · Marketing Analytics**  
**Statut :** en cours — audit des données, Executive Overview et analyse des canaux terminés ; analyse approfondie des campagnes et restitution finale à venir.

## 1. Contexte et problématique

Row Health est le cadre d'une étude de cas consacrée à la performance de campagnes marketing dans le secteur de la santé. L'enjeu est d'évaluer comment les investissements publicitaires se traduisent en **inscriptions attribuées**, puis de formuler des recommandations d'optimisation budgétaire fondées sur les données.

> **Problématique :** Quels canaux et quelles caractéristiques de campagnes sont associés aux meilleures performances d'acquisition, et comment identifier des opportunités d'optimisation sans confondre attribution, engagement et rentabilité ?

L'analyse privilégie une démarche décisionnelle : **question business → préparation des données → indicateurs → interprétation → limites → action à tester**.

## 2. Objectifs

- Mesurer l'efficacité d'acquisition attribuée par plateforme, catégorie et type de campagne.
- Comparer visibilité publicitaire (impressions, CPM), clics (CTR, CPC) et acquisition (inscriptions, CPS).
- Mesurer la concentration des inscriptions au sein des portefeuilles de campagnes.
- Quantifier les dépenses associées aux campagnes sans inscription attribuée.
- Prioriser les audits et les opportunités d'expérimentation budgétaire, sans extrapoler les rendements futurs.

## 3. Données

| Table | Volume | Granularité | Contenu principal |
|---|---:|---|---|
| `campaigns.csv` | 57 lignes | Une ligne par campagne | Plateforme, catégorie, type, dépenses, impressions, clics, durée |
| `customers.csv` | 16 338 lignes | Une ligne par client | Identifiant client, campagne attribuée, premier contact, canal d'inscription, offre, date |
| `claims.csv` | 49 998 lignes | Une ligne par demande | Identifiant client, produit, date, montant demandé et montant couvert |

**Relations analytiques :** `campaigns.campaign_id` → `customers.campaign_id` (1:N) ; `customers.customer_id` → `claims.customer_id` (1:N).

**Périmètre actuellement analysé :** campagnes et inscriptions attribuées. L'exploitation métier des demandes (`claims`) et la segmentation client feront l'objet de phases ultérieures.

## 4. Stack technique

- **Python / Pandas / NumPy :** audit qualité, nettoyage, jointures contrôlées et agrégations.
- **Google Colab :** notebooks reproductibles et documentation des analyses.
- **Visualisation :** graphiques Python pour l'analyse exploratoire ; tableau de bord Power BI prévu pour la restitution.
- **GitHub :** documentation, code, résultats et livrables du projet.

## 5. Méthodologie

1. **Audit et préparation :** contrôler les identifiants, les valeurs manquantes, les dates, les doublons et la cohérence des champs.
2. **Modélisation :** relier les clients aux campagnes en conservant la granularité des coûts pour éviter leur duplication.
3. **Executive Overview :** calculer les KPI globaux et repérer les principaux facteurs de concentration.
4. **Marketing Channel Performance :** comparer les canaux, les catégories et les types ; mesurer la sensibilité aux campagnes dominantes ; étudier les coûts publicitaires et les dépenses sans inscription attribuée.
5. **Campaign Deep Dive (à venir) :** examiner les campagnes atypiques et les mécanismes d'attribution.
6. **Recommandations et restitution (à venir) :** formuler, tester et documenter des scénarios d'optimisation ; construire le tableau de bord final.

### Définitions des KPI

| Indicateur | Calcul | Lecture |
|---|---|---|
| **CPS** — Coût par inscription attribuée | Dépenses / inscriptions attribuées | Coût moyen historique par inscription attribuée |
| **CTR** — Taux de clics | Clics / impressions × 100 | Part des impressions associées à un clic |
| **CPC** — Coût par clic | Dépenses / clics | Coût publicitaire moyen par clic |
| **CPM** — Coût pour mille impressions | Dépenses / impressions × 1 000 | Coût d'exposition |
| **Signup Share** | Inscriptions du segment / inscriptions totales × 100 | Contribution à l'acquisition attribuée |
| **Budget Share** | Dépenses du segment / dépenses totales × 100 | Poids budgétaire |
| **Allocation Gap** | Signup Share − Budget Share | Écart descriptif, exprimé en points de pourcentage |
| **Top 2 Share** | Inscriptions des deux premières campagnes / inscriptions du canal × 100 | Concentration de l'acquisition attribuée |

**Précaution :** pour le CTR et le CPC, les calculs excluent les campagnes dont les clics sont manquants et utilisent les impressions et les dépenses correspondant à ce même périmètre. Les zéros valides restent distincts des données manquantes. Les mesures de *Signup Rate* (inscriptions attribuées / impressions) sont des rapports agrégés, **pas des taux de conversion individuels prouvés**.

## 6. Résultats provisoires

### Vue d'ensemble

| Indicateur | Résultat |
|---|---:|
| Campagnes | **57** |
| Clients inscrits dans le fichier | **16 338** |
| Inscriptions attribuées à une campagne | **16 289** |
| Budget marketing total | **60 190,25 USD** |
| CPS global attribué | **3,70 USD** |
| Part des inscriptions issue des 5 premières campagnes | **75,97 %** |
| Part du budget de ces 5 campagnes | **9,91 %** |
| Campagnes sans inscription attribuée | **24 / 57** |
| Dépenses associées à ces campagnes | **24 709,05 USD — 41,05 % du budget** |

### Performance par plateforme

| Plateforme | Campagnes | Inscriptions attribuées | Budget (USD) | CPS (USD) | CTR numérique* |
|---|---:|---:|---:|---:|---:|
| Social Media | 16 | 7 610 | 17 089,57 | **2,25** | 9,19 % |
| Email | 15 | 4 130 | 16 678,97 | 4,04 | **16,71 %** |
| SEO | 19 | 4 055 | 21 246,99 | 5,24 | 8,52 % |
| TV | 7 | 494 | 5 174,71 | 10,48 | Non applicable |

\* CTR établi sur les seules campagnes numériques disposant de clics renseignés ; les autres KPI n'utilisent pas nécessairement ce même sous-ensemble.

### Principaux enseignements

**1 — Des performances très concentrées.** Les cinq premières campagnes produisent 75,97 % des inscriptions attribuées avec 9,91 % du budget. Cette concentration traverse plusieurs canaux : Social Media, Email et SEO.

**2 — Social Media domine l'acquisition attribuée, mais dépend de ses campagnes phares.** Le canal affiche le CPS le plus faible (2,25 USD), tandis que ses deux premières campagnes génèrent 82,48 % des inscriptions du canal.

**3 — Email génère les clics les plus efficacement, sans dominer les inscriptions.** Son CTR est de 16,71 % et son CPC d'environ 0,049 USD, mais son CPS est supérieur à celui de Social Media. Un bon engagement publicitaire ne garantit pas une acquisition attribuée peu coûteuse.

**4 — Les moyennes par canal masquent une forte hétérogénéité.** Les performances changent selon la combinaison plateforme × catégorie × type de campagne, et peuvent être portées par une seule campagne exceptionnelle.

**5 — Une part importante des dépenses ne présente aucune inscription attribuée.** Les 24 campagnes concernées mobilisent 41,05 % du budget total. SEO (8 469,90 USD) et Email (7 870,11 USD) sont les deux plus gros postes à auditer en valeur absolue.

### Concentration et budget sans inscription par canal

| Plateforme | Part des inscriptions du Top 2 | Budget sans inscription attribuée (USD) | Part du budget du canal |
|---|---:|---:|---:|
| Email | 93,58 % | 7 870,11 | 47,19 % |
| Social Media | 82,48 % | 4 841,84 | 28,33 % |
| SEO | 69,52 % | 8 469,90 | 39,86 % |
| TV | 100,00 % | 3 527,20 | 68,16 % |

## 7. Recommandations préliminaires

| Priorité | Action | Motif | Vérification avant décision |
|---|---|---|---|
| **Audit** | Examiner les dépenses sans inscription sur SEO et Email | **16 340 USD** concernés à eux deux | Objectif de campagne, instrumentation et attribution |
| **Validation** | Étudier les campagnes dominantes (notamment CAM031, CAM030 et CAM018) | Forte concentration des résultats | Cohérence temporelle, attribution, reproductibilité |
| **Expérimentation** | Tester prudemment des investissements ciblés sur des campagnes à faible CPS historique | Opportunités potentielles d'acquisition | Coût marginal, saturation, qualité des inscriptions |
| **Mesure** | Adapter l'évaluation de TV à son objectif réel | Acquisition directe potentiellement mal captée | Mesures de portée, notoriété et effets incrémentaux |

**Ces recommandations sont des pistes d'investigation et de test, pas des réallocations budgétaires déjà justifiées.**

## 8. Limites de l'analyse

- **Attribution ≠ causalité :** une inscription rattachée à une campagne ne prouve pas que la campagne l'a provoquée.
- **Zéro inscription attribuée ≠ absence de valeur :** certaines campagnes peuvent poursuivre des objectifs de notoriété ou d'information.
- **Rentabilité non mesurable à ce stade :** les dépenses et inscriptions ne suffisent pas à calculer le ROI, le revenu par client ou la valeur vie client.
- **Mesure des clics incomplète :** certaines campagnes possèdent des clics manquants ; les KPI de clics utilisent un périmètre restreint et documenté.
- **Performances historiques non extrapolables automatiquement :** une campagne à faible CPS n'offre aucune garantie de coût marginal identique si son budget augmente.
- **Cas d'étude :** la provenance et le caractère éventuellement synthétique des données doivent être explicités avant toute généralisation au secteur de la santé.

## 9. Avancement et prochaines étapes

- [x] Audit initial et préparation des données
- [x] Executive Overview et synthèse des KPI
- [x] Analyse des canaux : performance, catégories, types et concentration
- [x] Comparaison CTR, CPC, CPM et CPS sur des périmètres adaptés
- [x] Analyse des dépenses sans inscription attribuée
- [x] Matrice de priorisation stratégique préliminaire
- [ ] Analyse approfondie des campagnes et vérification de l'attribution
- [ ] Analyse des profils clients et exploitation des données de demandes (`claims`)
- [ ] Scénarios d'optimisation et protocole de test
- [ ] Dashboard Power BI, storytelling final et conclusions

## 10. Reproductibilité et organisation du dépôt

L'analyse est développée dans **Google Colab**. Les notebooks, sources de données autorisées à être partagées et visualisations seront progressivement organisés dans le dépôt GitHub. Les chemins des notebooks et des livrables seront ajoutés ici lors de leur publication.

### Dépendances Python principales

```python
import numpy as np
import pandas as pd
```

**Note de lecture :** les résultats de ce README sont provisoires et pourront évoluer après l'audit approfondi de l'attribution. Toute comparaison doit conserver un dénominateur cohérent et préciser si elle concerne le portefeuille global, une plateforme ou les seules campagnes numériques.
