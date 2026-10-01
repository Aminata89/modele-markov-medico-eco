# ==============================================================================

# PROJET PERSONNEL : ANALYSE COÛT-UTILITÉ (ACU) EN ONCOLOGIE

# Comparaison : Traitement Standard vs Innovation (ex: CAR-T Cells)

# ==============================================================================

library(heemod)
library(ggplot2)

# ==============================================================================

# 1. MATRICES DE TRANSITION (Probabilité de passer d'un état à l'autre chaque mois)

# ==============================================================================

# Un modèle de Markov en oncologie a généralement 3 états de santé :

# - PF (Progression-Free) : Le patient va bien, la maladie ne progresse pas.

# - PD (Progressed Disease) : La maladie a progressé, le patient rechute.

# - D (Death) : Décès.

# Matrice pour le traitement standard (haut risque de rechute)

# "C" est utilisé par heemod pour calculer automatiquement le Complément à 1

mat_standard <- define_transition(
state_names = c("PF", "PD", "D"),
C,    0.20, 0.10,  # Depuis PF : 20% rechute, 10% décès, C (70%) d'y rester
0.00, C,    0.40,  # Depuis PD : 0% guérison, 40% décès, C (60%) d'y rester
0.00, 0.00, 1.00   # Depuis D : État absorbant (100% de rester décédé)
)

# Matrice pour le nouveau traitement (baisse du risque de rechute et de décès)

mat_nouveau <- define_transition(
state_names = c("PF", "PD", "D"),
C,    0.10, 0.05,  # Depuis PF : 10% rechute, 5% décès, C (85%) d'y rester
0.00, C,    0.40,

0.00, 0.00, 1.00

)

# ==============================================================================

# 2. CRÉATION DES STRATÉGIES (Coûts et Utilités / Qualité de vie)

# ==============================================================================

# Stratégie 1 : Le traitement Standard

strat_standard <- define_strategy(
transition = mat_standard,
PF = define_state(cost = 2000, utility = 0.8), # Coût modéré, bonne qualité de vie
PD = define_state(cost = 5000, utility = 0.5), # Coût très élevé (hôpital), qualité dégradée
D  = define_state(cost = 0,    utility = 0)
)

# Stratégie 2 : Le Nouveau traitement (CAR-T)

strat_nouveau <- define_strategy(
transition = mat_nouveau,
PF = define_state(cost = 8000, utility = 0.85), # Coût initial lourd, mais meilleure qualité
PD = define_state(cost = 5000, utility = 0.5),
D  = define_state(cost = 0,    utility = 0)
)

# ==============================================================================

# 3. EXÉCUTION DU MODÈLE (Simulation sur 60 cycles/mois, cohorte de 1000 patients)

# ==============================================================================

modele_acu <- run_model(
Standard = strat_standard,
Nouveau  = strat_nouveau,
init     = c(1000, 0, 0), # 1000 patients commencent en état PF
cycles   = 60,            # Suivi sur 5 ans (60 mois)
cost     = cost,          # On veut minimiser les coûts
effect   = utility        # On veut maximiser la qualité de vie (utilité)
)

# ==============================================================================

# 4. ANALYSE ET RÉSULTATS (Le Ratio Différentiel Coût-Efficacité / ICER)

# ==============================================================================

# Afficher les résultats complets (Coûts totaux, QALYs totaux, ICER)

print(summary(modele_acu))

# Graphique 1 : Survie de la cohorte au fil du temps (Markov Trace)

# Montre combien de patients sont en PF, PD ou Décédés à chaque cycle

plot(modele_acu)

# Graphique 2 : Plan Coût-Efficacité (Cost-Effectiveness Plane)

# Montre visuellement le surcoût par rapport au gain de QALYs

plot(modele_acu, type = "ce")