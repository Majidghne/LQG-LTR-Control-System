# 🔧 Commande robuste LQG/LTR pour la régulation thermique d’une salle blanche

Ce dépôt présente le développement complet d’une stratégie de **commande robuste** basée sur l’architecture **LQG/LTR** (Linear Quadratic Gaussian + Loop Transfer Recovery), appliquée à la régulation thermique d’une salle blanche.  
Le projet inclut la modélisation du système, la synthèse de l’observateur et du contrôleur, l’analyse de robustesse et la validation sous MATLAB R2023b.

---

## 📌 Objectif du projet

Les salles blanches nécessitent un contrôle thermique extrêmement précis.  
L’objectif est de concevoir une loi de commande :

- stable et performante  
- robuste aux incertitudes  
- capable d’annuler l’erreur statique  
- compatible avec une implémentation réelle  

Pour cela, une architecture **LQG** est d’abord développée, puis **robustifiée** via la procédure **LTR**, permettant de retrouver les propriétés du retour d’état idéal.

---

## 🧩 1. Modélisation du système

Le système thermique est identifié expérimentalement et modélisé sous forme :

- fonction de transfert  
- représentation d’état  
- analyse temporelle (réponse indicielle)  
- analyse fréquentielle (diagrammes de Bode)

Caractéristiques observées :

- dynamique lente  
- système stable et monotone  
- aucun dépassement  
- comportement typique d’un procédé thermique fortement amorti  

---

## 🛰️ 2. Observateur d’état optimal (Kalman)

Un observateur de Kalman est synthétisé pour estimer les états internes non mesurés.

Étapes principales :

- vérification de la stabilisabilité  
- vérification de la détectabilité  
- résolution de l’équation de Riccati  
- calcul du gain **L**

L’observateur reconstruit efficacement les états malgré un bruit de mesure.

---

## 🎛️ 3. Commande optimale LQ avec intégrateur

Pour éliminer l’erreur statique, un intégrateur est ajouté au système augmenté.

La commande LQ minimise :



\[
J = \int (x^T Q x + u^T R u)\, dt
\]



Résultats :

- stabilité asymptotique  
- dynamique rapide  
- erreur statique nulle  

---

## 🔄 4. Synthèse LQG (observateur + contrôleur)

L’observateur et le contrôleur sont assemblés pour former une architecture **LQG**.

Analyse :

- performances temporelles proches du LQ idéal  
- dégradation des marges de robustesse (gain, phase, module)  
- phénomène classique : *LQG n’est pas robuste par construction*  

---

## 🛡️ 5. Robustification LTR (Loop Transfer Recovery)

La procédure LTR permet de restaurer la robustesse du retour d’état idéal.

Principe :

- augmenter la covariance du bruit de processus  
- accélérer la dynamique de l’observateur  
- faire converger les transferts LQG → LQ

Paramètre optimal :
q = 10^9


Résultats :

- marge de gain > 35 dB  
- marge de phase > 60°  
- marge de module > 0.95  
- superposition quasi parfaite des Bode LQ et LQG-LTR  

---

## 📊 6. Résultats principaux

### ✔ Réponse indicielle
- dynamique rapide  
- aucune oscillation  
- suivi précis de la consigne  

### ✔ Diagrammes de Bode
- amélioration nette des marges après LTR  
- comportement robuste aux basses et moyennes fréquences  

### ✔ Fonction de sensibilité
- réduction du pic de sensibilité  
- meilleure tolérance aux perturbations  



