Analyse Coût-Utilité (ACU) en Oncologie : Thérapie CAR-T vs Traitement Standard

Présentation du projet

Ce projet est une modélisation médico-économique réalisée sous R. Il vise à comparer le ratio coût-efficacité d'une thérapie innovante (type CAR-T cells) par rapport à un traitement standard en oncologie.

L'objectif de cette analyse est de calculer le Ratio Différentiel Coût-Efficacité (RDCE / ICER) et les QALYs (Quality-Adjusted Life Years) gagnés pour aider à la prise de décision publique concernant le remboursement de cette innovation.

Méthodologie

L'analyse repose sur un Modèle de Markov à 3 états de santé simulé sur une cohorte de 1000 patients :

PF (Progression-Free) : Maladie non progressive, bonne qualité de vie.

PD (Progressed Disease) : Rechute, qualité de vie dégradée et coûts hospitaliers élevés.

D (Death) : Décès (état absorbant).

Le modèle a été exécuté sur 60 cycles (5 ans) à l'aide du package R heemod.

Résultats Clés

Différence d'efficacité : + 2,97 QALYs par patient avec le nouveau traitement.

Différence de coût : + 43 663 € de surcoût par rapport au traitement standard.

ICER (RDCE) : 14 678,52 € / QALY.

Conclusion : Le coût pour obtenir une année de vie supplémentaire en parfaite santé s'élève à environ 14 678 €. Ce montant étant largement inférieur aux seuils d'acceptabilité européens habituels (30 000 € - 50 000 € / QALY), cette thérapie innovante est considérée comme hautement coût-efficace.

Technologies et Outils

Langage : R

Packages : heemod (modélisation de Markov), ggplot2 (visualisation du plan coût-efficacité)

Auteure

Aminata DIAW
Économètre et Data Analyst - Évaluation des politiques publiques et Économie de la Santé
