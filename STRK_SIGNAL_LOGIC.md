# 📊 Logique des Signaux STRK Bot

Ce document explique en détail comment le bot STRK calcule les probabilités de hausse/baisse et génère ses signaux de trading.

---

## 🎯 Vue d'ensemble

Le bot analyse **6 composants** de données pour calculer une **probabilité haussière** (0-100%). Chaque composant a un **poids** différent dans le calcul final.

### Formule générale
```
Probabilité Haussière =
  (Score CEX × 30%) +
  (Score DEX × 25%) +
  (Score Inflows/Outflows × 20%) +
  (Score Baleines × 15%) +
  (Score Liquidité × 5%) +
  (Score Réseau × 5%)
```

### Seuils de décision
- **ACHAT** : Probabilité ≥ 65%
- **VENTE** : Probabilité ≤ 35%
- **NEUTRE** : Entre 35% et 65%

### Seuils d'alerte Telegram
- **Alerte ACHAT** : Probabilité ≥ 75%
- **Alerte VENTE** : Probabilité ≤ 25%

---

## 📈 Composant 1 : CEX (30% du poids)

**Source** : API CoinGecko
**Fichier** : `strk_bot_server.py:234-252`
**Cache** : 180 secondes (3 minutes)

### Données collectées
- Prix actuel en USD
- Variation de prix sur 1 heure
- Variation de prix sur 24 heures
- Volume de trading 24h
- Capitalisation de marché

### Normalisation (conversion en score 0.0-1.0)

#### Score variation 1h
```
Score = 0.5 + (variation_1h / 10.0)
```
- Variation limitée entre -5% et +5%
- Exemples :
  - +5% → score 1.0 (très haussier)
  - +2% → score 0.7
  - 0% → score 0.5 (neutre)
  - -3% → score 0.2
  - -5% → score 0.0 (très baissier)

#### Score variation 24h
```
Score = 0.5 + (variation_24h / 30.0)
```
- Variation limitée entre -15% et +15%
- Exemples :
  - +15% → score 1.0
  - +7.5% → score 0.75
  - 0% → score 0.5
  - -10% → score 0.17

#### Score volume
```
Si volume > $200M → score 0.7
Si volume > $50M  → score 0.6
Sinon            → score 0.5
```

### Score final CEX
```
Score CEX = (score_1h × 40%) + (score_24h × 40%) + (score_volume × 20%)
```

### Exemple concret
**Données** :
- Variation 1h : +3%
- Variation 24h : +10%
- Volume : $150M

**Calcul** :
```
score_1h = 0.5 + (3 / 10) = 0.8
score_24h = 0.5 + (10 / 30) = 0.83
score_volume = 0.6 (entre 50M et 200M)

Score CEX = (0.8 × 0.4) + (0.83 × 0.4) + (0.6 × 0.2)
          = 0.32 + 0.332 + 0.12
          = 0.772

Contribution au score final = 0.772 × 30% = 0.232 (23.2%)
```

---

## 🔄 Composant 2 : DEX (25% du poids)

**Source** : API Ekubo (DEX StarkNet)
**Fichier** : `strk_bot_server.py:254-255`
**Cache** : 300 secondes (5 minutes)

### Données collectées
- TVL (Total Value Locked) en USD
- Volume de trading 24h
- Ratio Volume/TVL

### Normalisation
```
ratio = volume_24h / (TVL + 1) × 10
ratio = min(1.0, max(0.0, ratio))

Score DEX = 0.5 + (ratio - 0.5) × 0.3
```

### Logique
- **Ratio élevé** : Beaucoup d'activité par rapport à la liquidité → Haussier
- **Ratio faible** : Peu d'activité → Baissier

### Exemple concret
**Données** :
- TVL : $8M
- Volume 24h : $2M

**Calcul** :
```
ratio = 2,000,000 / 8,000,000 × 10 = 2.5
ratio = min(1.0, 2.5) = 1.0

Score DEX = 0.5 + (1.0 - 0.5) × 0.3 = 0.65

Contribution au score final = 0.65 × 25% = 0.1625 (16.25%)
```

---

## 💰 Composant 3 : Inflows/Outflows (20% du poids)

**Fichier** : `strk_bot_server.py:221-222`
**Statut** : ⚠️ **STUB (non implémenté)**

### État actuel
```python
def get_inflows_outflows():
    return {"inflows_score": 0.0, "outflows_score": 0.0}
```

Retourne toujours un score de **0.5** (neutre), ce qui représente **20% du poids total inutilisé**.

### Normalisation
```
Score = 0.5 + (outflows_score - inflows_score) × 0.5
```

### Amélioration future
Pour implémenter réellement ce composant, il faudrait :
- Analyser les mouvements de tokens entre wallets
- Détecter les entrées/sorties des exchanges
- Utiliser une API blockchain (StarkScan, Voyager)

---

## 🐋 Composant 4 : Baleines (15% du poids)

**Fichier** : `strk_bot_server.py:224-225`
**Statut** : ⚠️ **STUB (non implémenté)**

### État actuel
```python
def get_whales_signal():
    return {"whales_accumulate": 0.5, "whales_distribute": 0.5}
```

Retourne toujours un score de **0.5** (neutre), ce qui représente **15% du poids total inutilisé**.

### Normalisation
```
Score = 0.5 + (accumulation - distribution) × 0.5
```

### Amélioration future
Pour implémenter réellement ce composant, il faudrait :
- Identifier les wallets "baleines" (gros holders)
- Tracker leurs transactions
- Détecter accumulation vs distribution

---

## 💧 Composant 5 : Liquidité (5% du poids)

**Fichier** : `strk_bot_server.py:227-231, 267-269`
**Cache** : Utilise les données DEX (300 secondes)

### Calcul
```python
change = (TVL - 5,000,000) / 10,000,000
change = max(-0.3, min(0.3, change))

Score = 0.5 + change / 0.4
```

### Logique
- **TVL > $5M** : Score augmente (plus de liquidité = plus stable)
- **TVL < $5M** : Score diminue (moins de liquidité = plus risqué)

### Exemple concret
**Données** :
- TVL actuel : $8M

**Calcul** :
```
change = (8,000,000 - 5,000,000) / 10,000,000 = 0.3
change = min(0.3, 0.3) = 0.3

Score Liquidité = 0.5 + (0.3 / 0.4) = 0.5 + 0.75 = 1.25 (limité à 1.0)

Contribution au score final = 1.0 × 5% = 0.05 (5%)
```

---

## 🌐 Composant 6 : Réseau (5% du poids)

**Source** : API StarkScan
**Fichier** : `strk_bot_server.py:199-218, 271-273`
**Cache** : 600 secondes (10 minutes)

### Données collectées
- Nombre de transactions 24h
- Nombre de transactions 24h précédentes
- Variation en pourcentage

### Normalisation
```
Score = 0.5 + (variation_tx_24h / 100.0)
```
- Variation limitée entre -50% et +50%

### Logique
- **Activité en hausse** : Plus d'utilisations du réseau → Haussier
- **Activité en baisse** : Moins d'utilisation → Baissier

### Exemple concret
**Données** :
- Transactions aujourd'hui : 130,000
- Transactions hier : 100,000

**Calcul** :
```
variation = ((130,000 - 100,000) / 100,000) × 100 = +30%

Score Réseau = 0.5 + (30 / 100) = 0.8

Contribution au score final = 0.8 × 5% = 0.04 (4%)
```

---

## 🔢 Calcul Final : Exemple Complet

### Scénario : Forte Hausse

**Données d'entrée** :
- CEX : +3% (1h), +10% (24h), $150M volume
- DEX : TVL $8M, Volume $2M
- Liquidité : TVL $8M
- Réseau : +30% transactions
- Inflows/Outflows : 0.5 (stub)
- Baleines : 0.5 (stub)

**Scores normalisés** :
```
Score CEX      = 0.772
Score DEX      = 0.65
Score I/O      = 0.5
Score Baleines = 0.5
Score Liquidité= 0.8
Score Réseau   = 0.8
```

**Calcul probabilité** :
```
Probabilité = (0.772 × 0.30) + (0.65 × 0.25) + (0.5 × 0.20) +
              (0.5 × 0.15) + (0.8 × 0.05) + (0.8 × 0.05)
            = 0.2316 + 0.1625 + 0.10 + 0.075 + 0.04 + 0.04
            = 0.6491
            = 64.91%
```

**Résultat** : Signal **NEUTRE** (proche du seuil ACHAT de 65%)

---

### Scénario : Forte Baisse

**Données d'entrée** :
- CEX : -4% (1h), -12% (24h), $30M volume
- DEX : TVL $3M, Volume $100K
- Liquidité : TVL $3M
- Réseau : -40% transactions

**Scores normalisés** :
```
Score CEX      = 0.1
Score DEX      = 0.35
Score I/O      = 0.5
Score Baleines = 0.5
Score Liquidité= 0.3
Score Réseau   = 0.1
```

**Calcul probabilité** :
```
Probabilité = (0.1 × 0.30) + (0.35 × 0.25) + (0.5 × 0.20) +
              (0.5 × 0.15) + (0.3 × 0.05) + (0.1 × 0.05)
            = 0.03 + 0.0875 + 0.10 + 0.075 + 0.015 + 0.005
            = 0.3125
            = 31.25%
```

**Résultat** : Signal **VENTE** (< 35%)

---

## ⚠️ Limitations Actuelles

### 1. Stubs non implémentés (35% du poids)
- **Inflows/Outflows (20%)** : Retourne toujours 0.5
- **Baleines (15%)** : Retourne toujours 0.5

Ces deux composants représentent **35% du scoring total** qui est actuellement neutralisé. Le bot s'appuie donc principalement sur :
- CEX (30%)
- DEX (25%)
- Réseau (5%)
- Liquidité (5%)

Total utilisé efficacement : **65%**

### 2. Normalisation simpliste
- Les seuils sont arbitraires ($200M pour volume CEX, $5M pour TVL)
- Pas de machine learning ou d'ajustement dynamique
- Pas de prise en compte du sentiment de marché général (Bitcoin, Ethereum)

### 3. Pas de contexte macro
- Ne tient pas compte du marché crypto global
- Pas d'analyse du Bitcoin ou Ethereum (leaders du marché)
- Pas de corrélation avec d'autres tokens Layer 2

### 4. Alertes simplistes
- Cooldown fixe de 30 minutes
- Pas de gestion de la volatilité (éviter spam en cas de forte fluctuation)
- Pas de confirmation de signal (ex: attendre 2 cycles consécutifs)

---

## 🚀 Améliorations Possibles

### Court terme
1. **Implémenter les stubs** :
   - Inflows/Outflows via StarkScan API
   - Détection baleines via analyse on-chain

2. **Ajouter des confirmations** :
   - Signal ACHAT si probabilité > 65% pendant 2 cycles (1 minute)
   - Évite les faux signaux sur pics temporaires

### Moyen terme
1. **Contexte macro** :
   - Ajouter score Bitcoin/Ethereum (10% du poids)
   - Ajuster seuils selon volatilité du marché

2. **Machine Learning** :
   - Entraîner un modèle sur historique (`strk_signals.csv`)
   - Optimiser les poids automatiquement

### Long terme
1. **Backtesting** :
   - Simuler stratégie sur données historiques
   - Calculer win rate et rendement

2. **Trading automatique** :
   - Intégration avec DEX (Ekubo, Avnu)
   - Exécution automatique des signaux ACHAT/VENTE

---

## 📊 Tableau Récapitulatif

| Composant | Poids | Source | Cache | Statut | Impact Réel |
|-----------|-------|--------|-------|--------|-------------|
| **CEX** | 30% | CoinGecko | 3 min | ✅ Actif | 30% |
| **DEX** | 25% | Ekubo | 5 min | ✅ Actif | 25% |
| **Inflows/Outflows** | 20% | - | - | ⚠️ Stub | 0% |
| **Baleines** | 15% | - | - | ⚠️ Stub | 0% |
| **Liquidité** | 5% | Ekubo | 5 min | ✅ Actif | 5% |
| **Réseau** | 5% | StarkScan | 10 min | ✅ Actif | 5% |
| **TOTAL** | 100% | - | - | - | **65%** |

---

## 🔍 Pour Aller Plus Loin

- **Code source** : `strk_bot_server.py` lignes 276-332 (fonction `compute_probabilities()`)
- **Normalisation** : lignes 234-273
- **Configuration seuils** : lignes 29-31
- **Alertes Telegram** : lignes 85-111

---

*Dernière mise à jour : 2025-11-26*
