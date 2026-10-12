# Audit qualité (QuickOrder V3.1)

Audit QA de QuickOrder avant sa mise en production : stratégie, cas de test, campagne exécutée avec preuves, rapports de bug, analyse et plan d'amélioration.

## Verdict : **NO-GO**

| Cas exécutés | Pass | Fail | Blocked | Skipped | Anomalies |
|---|---|---|---|---|---|
| 24 | 15 | 9 | 0 | 0 | 9 (1 critique, 7 majeures, 1 moyenne) |

Les parcours nominaux fonctionnent (6 sur 6) et les contrôles d'erreur sont solides (5 sur 6). En revanche, les règles métier ne sont pas appliquées (5 cas sur 6 en échec) :

- un paiement PayPal crée une double commande ;
- un paiement en espèces insuffisant est accepté ;
- un restaurant fermé reçoit des commandes ;
- le stock peut devenir négatif ;
- une commande peut partir avec une adresse vide ;
- le minimum de commande n'est pas appliqué ;
- la promotion ATLAS15 n'est pas tenue.

Détail dans [docs/execution-report.md](docs/execution-report.md).

## Livrables

| Livrable | Fichier |
|---|---|
| Stratégie de test | [docs/test-strategy.md](docs/test-strategy.md) |
| Matrice des risques | [docs/risk-matrix.md](docs/risk-matrix.md) |
| Cahier de test (Excel, 24 cas) | [docs/test-cases.xlsx](docs/test-cases.xlsx) (mêmes données en CSV : [docs/test-cases-source.csv](docs/test-cases-source.csv)) |
| Cas de test (version lisible) | [docs/test-cases.md](docs/test-cases.md) |
| Rapport d'exécution et recommandation | [docs/execution-report.md](docs/execution-report.md) |
| Bug reports (9) | [docs/bug-reports/](docs/bug-reports/README.md) |
| Preuves brutes par cas | [docs/evidence/](docs/evidence/) |
| Tests utilisateurs (protocole et grilles) | [docs/user-feedback.md](docs/user-feedback.md) |
| Plan d'amélioration (8 actions) | [docs/improvement-plan.md](docs/improvement-plan.md) |

## Environnement de test

QuickOrder V3.1 lancé en local (`python seed.py` puis `python run.py`), sur une base réinitialisée avant la campagne. Les preuves de chaque cas sont dans `docs/evidence/` : captures d'écran de l'interface et, pour l'API, commandes curl avec leur réponse JSON.

Le code source de QuickOrder n'a pas été modifié pendant la campagne.
