# 📖 Glossaire Crypto pour Débutants

Guide complet des termes utilisés dans le STRK Bot et le trading de cryptomonnaies.

---

## 🔤 Termes Généraux

### Blockchain
Technologie de stockage et de transmission d'informations sous forme de blocs liés entre eux. Chaque bloc contient des transactions validées par le réseau. C'est la base technique des cryptomonnaies.

**Exemple** : StarkNet est une blockchain, Bitcoin est une blockchain.

---

### Token / Coin
Unité de valeur numérique sur une blockchain.

**Différence** :
- **Coin** : Cryptomonnaie native d'une blockchain (ex: Bitcoin sur Bitcoin, ETH sur Ethereum)
- **Token** : Actif créé sur une blockchain existante (ex: STRK sur StarkNet)

**Dans ce projet** : STRK est le token natif de StarkNet.

---

### Wallet (Portefeuille)
Compte numérique permettant de stocker, envoyer et recevoir des cryptomonnaies. Il contient une clé privée (mot de passe secret) et une adresse publique (numéro de compte).

**Exemple** : MetaMask, Ledger, Argent (pour StarkNet)

---

## 📊 Trading

### Prix / Price
Valeur d'un token exprimée en monnaie fiduciaire (USD, EUR) ou en autre crypto.

**Exemple** : "STRK est à $0.45" signifie 1 token STRK = 0.45 dollar américain.

---

### Volume
Montant total échangé sur une période donnée (généralement 24h).

**Exemple** : Volume 24h = $150M signifie que pour 150 millions de dollars de STRK ont été échangés aujourd'hui.

**Interprétation** :
- Volume élevé = Beaucoup d'intérêt, liquidité forte
- Volume faible = Peu d'intérêt, risque de manipulation

---

### Variation / Change
Pourcentage de hausse ou baisse du prix sur une période.

**Formules** :
```
Variation 1h = ((Prix actuel - Prix il y a 1h) / Prix il y a 1h) × 100
Variation 24h = ((Prix actuel - Prix il y a 24h) / Prix il y a 24h) × 100
```

**Exemples** :
- Prix hier : $0.40 → Prix aujourd'hui : $0.44 → Variation : +10%
- Prix il y a 1h : $0.45 → Prix maintenant : $0.43 → Variation : -4.4%

---

### Market Cap (Capitalisation Boursière)
Valeur totale d'un token en circulation.

**Formule** :
```
Market Cap = Prix × Nombre de tokens en circulation
```

**Exemple** : Si STRK coûte $0.45 et qu'il y a 10 milliards de tokens :
```
Market Cap = 0.45 × 10,000,000,000 = $4.5 milliards
```

**Interprétation** :
- Bitcoin : ~$800 milliards (leader)
- STRK : ~$3-4 milliards (mid-cap)
- Projet obscur : < $1 million (micro-cap, très risqué)

---

### Signal
Recommandation d'achat ou de vente basée sur l'analyse de données.

**Types** :
- **ACHAT / BUY** : Le bot pense que le prix va monter
- **VENTE / SELL** : Le bot pense que le prix va baisser
- **NEUTRE / NEUTRAL** : Situation incertaine, attendre

---

### Probabilité Haussière / Bullish Probability
Pourcentage de chances que le prix monte selon l'algorithme du bot.

**Interprétation** :
- 90% haussier = Forte confiance dans une hausse
- 50% haussier = Incertain (neutre)
- 10% haussier = Forte confiance dans une baisse (90% baissier)

---

### Haussier / Bullish
Tendance à la hausse. Un marché haussier signifie que les prix montent.

**Origine** : Le taureau (bull) attaque de bas en haut avec ses cornes.

---

### Baissier / Bearish
Tendance à la baisse. Un marché baissier signifie que les prix descendent.

**Origine** : L'ours (bear) attaque de haut en bas avec ses pattes.

---

## 🏦 Exchanges (Plateformes d'échange)

### CEX (Centralized Exchange)
Plateforme centralisée d'échange de cryptomonnaies. Une entreprise gère la plateforme.

**Exemples** : Binance, Coinbase, Kraken

**Avantages** :
- Facile à utiliser
- Service client
- Haute liquidité

**Inconvénients** :
- Contrôle vos fonds (risque de piratage ou faillite)
- KYC (vérification d'identité) obligatoire

---

### DEX (Decentralized Exchange)
Plateforme décentralisée d'échange. Pas d'intermédiaire, les transactions se font directement entre utilisateurs via smart contracts.

**Exemples** : Uniswap (Ethereum), Ekubo (StarkNet), PancakeSwap (BSC)

**Avantages** :
- Vous gardez le contrôle de vos fonds
- Pas de KYC
- Transparent (code open-source)

**Inconvénients** :
- Interface plus complexe
- Frais de gas
- Pas de service client

**Dans ce projet** : Ekubo est le principal DEX de StarkNet pour échanger du STRK.

---

### Liquidité / Liquidity
Facilité à acheter ou vendre un actif sans impacter fortement son prix.

**Exemples** :
- **Haute liquidité** : Bitcoin (facile à vendre, prix stable)
- **Basse liquidité** : Token obscur (difficile à vendre, prix volatil)

**Impact sur le trading** :
- Haute liquidité = Moins de slippage (différence entre prix attendu et prix réel)
- Basse liquidité = Risque de ne pas pouvoir vendre au bon moment

---

### TVL (Total Value Locked)
Montant total de cryptomonnaies bloquées dans un protocole DeFi ou un pool de liquidité.

**Formule** :
```
TVL = Somme de tous les actifs déposés
```

**Exemple** : Pool STRK/ETH sur Ekubo a $8M de TVL :
- $4M de STRK
- $4M d'ETH

**Interprétation** :
- TVL élevé = Protocole populaire et sécurisé
- TVL faible = Risque plus élevé

---

### Ratio Volume/TVL
Indicateur d'activité d'un DEX ou pool de liquidité.

**Formule** :
```
Ratio = Volume 24h / TVL
```

**Interprétation** :
- Ratio > 1 : Beaucoup d'activité (le volume dépasse le TVL)
- Ratio < 0.1 : Peu d'activité (pool "dormant")

**Exemple** :
```
TVL = $8M
Volume 24h = $2M
Ratio = 2M / 8M = 0.25 (25% du TVL échangé en 24h)
```

**Dans le bot** : Ratio élevé = Score DEX augmente (activité forte = haussier)

---

## 🐋 Termes Avancés

### Baleine / Whale
Détenteur d'une très grande quantité d'un token, capable d'influencer le marché par ses achats/ventes.

**Exemple** : Un wallet possédant 10% de l'offre totale de STRK.

**Impact** :
- Achat baleine = Prix monte (accumulation)
- Vente baleine = Prix baisse (distribution)

**Détection** : Analyse des grandes transactions on-chain.

---

### Inflows / Outflows
Flux entrants et sortants de tokens d'un exchange.

**Définitions** :
- **Inflow** : Tokens envoyés vers un exchange (potentiel vendeur = baissier)
- **Outflow** : Tokens retirés d'un exchange (potentiel holder = haussier)

**Interprétation** :
- Inflows > Outflows = Pression vendeuse (baissier)
- Outflows > Inflows = Accumulation (haussier)

---

### On-chain
Données enregistrées directement sur la blockchain (transactions, smart contracts).

**Exemples** :
- Nombre de transactions
- Transferts entre wallets
- Activité des smart contracts

**Dans le bot** : StarkScan fournit les données on-chain de StarkNet.

---

### API (Application Programming Interface)
Interface permettant à un programme d'accéder aux données d'un service externe.

**Dans ce projet** :
- CoinGecko API : Prix, volume, market cap
- Ekubo API : TVL, volume DEX
- StarkScan API : Activité réseau

---

## 🔧 Termes Techniques du Bot

### Score
Valeur normalisée entre 0.0 et 1.0 représentant une tendance.

**Interprétation** :
- 1.0 = Maximum haussier
- 0.5 = Neutre
- 0.0 = Maximum baissier

**Exemple** : Score CEX de 0.8 signifie que les données CoinGecko sont très haussières.

---

### Normalisation
Processus de conversion de données brutes en score 0.0-1.0.

**Pourquoi** : Permettre de comparer et combiner des données de sources différentes.

**Exemple** :
```
Prix variation brute : +10%
Score normalisé : 0.5 + (10 / 30) = 0.83
```

---

### Pondération / Weight
Importance relative d'un composant dans le calcul final.

**Dans le bot** :
- CEX : 30% (plus important)
- DEX : 25%
- Inflows/Outflows : 20%
- Baleines : 15%
- Liquidité : 5% (moins important)
- Réseau : 5%

**Total** : 100%

---

### Cache
Stockage temporaire de données pour éviter de surcharger les APIs.

**Exemple** :
```
1ère requête : Appel API CoinGecko (lent)
2ème requête (< 3 min) : Lecture cache (rapide)
Après 3 min : Cache expiré, nouvel appel API
```

**Durées dans le bot** :
- CEX : 3 minutes
- DEX : 5 minutes
- Réseau : 10 minutes

---

### Seuil / Threshold
Valeur limite pour déclencher une action.

**Dans le bot** :
- Seuil ACHAT : 65% (si probabilité ≥ 65%)
- Seuil VENTE : 35% (si probabilité ≤ 35%)
- Seuil alerte ACHAT : 75%
- Seuil alerte VENTE : 25%

---

### Cooldown
Période d'attente avant de pouvoir refaire une action.

**Dans le bot** : Cooldown de 30 minutes entre 2 alertes Telegram identiques.

**Pourquoi** : Éviter le spam si le prix oscille autour d'un seuil.

---

### Stub
Code temporaire non fonctionnel, servant de placeholder.

**Dans le bot** :
```python
def get_whales_signal():
    return {"whales_accumulate": 0.5, "whales_distribute": 0.5}
```

Ce code retourne toujours 0.5 (neutre) car la vraie détection de baleines n'est pas encore implémentée.

---

## 🌐 Blockchain StarkNet

### StarkNet
Blockchain Layer 2 pour Ethereum, utilisant la technologie ZK-Rollup pour réduire les frais et augmenter la vitesse.

**Token natif** : STRK

---

### Layer 2 (L2)
Blockchain secondaire construite au-dessus d'une blockchain principale (Layer 1 comme Ethereum) pour améliorer les performances.

**Avantages** :
- Frais réduits
- Transactions plus rapides
- Sécurité héritée du Layer 1

**Exemples** : StarkNet, Arbitrum, Optimism, zkSync

---

### Transaction / Tx
Opération enregistrée sur la blockchain (transfert de tokens, exécution de smart contract).

**Dans le bot** : Nombre de transactions = Indicateur d'activité du réseau.

---

### Smart Contract
Programme autonome exécuté sur la blockchain. Code immuable qui s'exécute automatiquement quand les conditions sont remplies.

**Exemple** : Pool de liquidité sur Ekubo (échange automatique STRK/ETH).

---

## 📱 Telegram

### Bot Telegram
Programme automatisé qui envoie des messages sur Telegram.

**Dans ce projet** : Envoie des alertes quand probabilité haussière > 75% ou < 25%.

---

### Token Bot
Clé secrète fournie par @BotFather permettant au bot d'envoyer des messages.

**Format** : `1234567890:ABCdefGHIjklMNOpqrsTUVwxyz`

---

### Chat ID
Identifiant unique d'une conversation Telegram (utilisateur ou groupe).

**Format** : `1393221901`

**Comment l'obtenir** : Envoyer un message au bot puis appeler l'API Telegram `/getUpdates`.

---

## 📈 Indicateurs Techniques

### Momentum
Force et vitesse d'une tendance de prix.

**Dans le bot** : Variation 1h et 24h mesurent le momentum.

---

### Volatilité
Amplitude des variations de prix.

**Exemples** :
- Bitcoin : Volatilité modérée (±5% par jour)
- Altcoin obscur : Haute volatilité (±30% par jour)

---

### Support / Résistance
- **Support** : Niveau de prix où la baisse s'arrête souvent (zone d'achat)
- **Résistance** : Niveau de prix où la hausse s'arrête souvent (zone de vente)

**Non utilisé dans le bot actuel** (amélioration future possible)

---

## 🔐 Sécurité

### KYC (Know Your Customer)
Vérification d'identité obligatoire sur les CEX (passeport, justificatif de domicile).

---

### Clé Privée / Private Key
Code secret donnant accès à vos cryptos. **NE JAMAIS LA PARTAGER**.

**Analogie** : Mot de passe de votre compte bancaire.

---

### Slippage
Différence entre le prix attendu et le prix réel d'exécution d'un ordre.

**Causes** :
- Faible liquidité
- Volatilité élevée
- Gros ordre

**Exemple** : Vous voulez acheter à $0.45 mais l'ordre s'exécute à $0.47 (slippage de 4.4%)

---

## 📚 Acronymes Courants

| Acronyme | Signification | Définition |
|----------|---------------|------------|
| **API** | Application Programming Interface | Interface pour accéder aux données |
| **ATH** | All-Time High | Prix historique le plus haut |
| **ATL** | All-Time Low | Prix historique le plus bas |
| **BTC** | Bitcoin | Première cryptomonnaie |
| **CEX** | Centralized Exchange | Plateforme centralisée |
| **DeFi** | Decentralized Finance | Finance décentralisée |
| **DEX** | Decentralized Exchange | Plateforme décentralisée |
| **ETH** | Ethereum | 2ème plus grande blockchain |
| **FOMO** | Fear Of Missing Out | Peur de rater une opportunité |
| **FUD** | Fear, Uncertainty, Doubt | Peur, incertitude et doute |
| **L1** | Layer 1 | Blockchain principale |
| **L2** | Layer 2 | Blockchain secondaire |
| **STRK** | StarkNet Token | Token natif de StarkNet |
| **TVL** | Total Value Locked | Valeur totale bloquée |
| **USD** | US Dollar | Dollar américain |

---

## 🎯 Concepts Clés pour Comprendre le Bot

### 1. Le bot est probabiliste
Il ne prédit pas l'avenir avec certitude, il calcule une probabilité basée sur des données actuelles.

**Analogie** : Météo à 70% de pluie ≠ il pleuvra à coup sûr

---

### 2. Plusieurs sources = Plus fiable
Le bot combine 6 sources de données pour réduire le risque de faux signaux.

**Si une seule source** : Facile de se tromper
**Si 6 sources alignées** : Plus de confiance

---

### 3. Pondération = Importance
Toutes les sources n'ont pas le même poids. CEX (30%) compte plus que Réseau (5%).

**Pourquoi** : Prix et volume CEX sont plus fiables que variation du nombre de transactions.

---

### 4. Normalisation = Comparaison
On convertit tout en scores 0.0-1.0 pour pouvoir combiner des données différentes (prix, volume, transactions).

**Sans normalisation** : Impossible de comparer "$150M de volume" avec "+30% de transactions"

---

### 5. Cache = Performance
Le bot ne fait pas 1000 appels API par minute, il stocke temporairement les données.

**Avantage** : Rapide, pas de ban par les APIs
**Inconvénient** : Données légèrement obsolètes (max 10 minutes)

---

## 💡 Conseils pour Débutants

### 1. Ne pas trader uniquement sur les signaux du bot
Le bot est un **outil d'aide à la décision**, pas une boule de cristal.

**Recommandations** :
- Faire ses propres recherches (DYOR : Do Your Own Research)
- Comprendre le projet StarkNet
- Ne pas investir plus que ce qu'on peut se permettre de perdre

---

### 2. Comprendre les limitations
- 35% du scoring est inutilisé (stubs)
- Pas de contexte macro (Bitcoin, Ethereum)
- Pas de détection des news importantes

---

### 3. Tester avant d'utiliser
- Observer les signaux sans trader pendant 1-2 semaines
- Comparer avec le prix réel après quelques heures/jours
- Ajuster les seuils selon votre tolérance au risque

---

### 4. Gérer le risque
- **Stop Loss** : Vendre automatiquement si perte > X%
- **Take Profit** : Vendre automatiquement si gain > X%
- **Diversification** : Ne pas mettre tous ses fonds sur STRK

---

## 🔗 Ressources pour Aller Plus Loin

### Sites d'information
- **CoinGecko** : https://www.coingecko.com/fr/pi%C3%A8ces/starknet
- **CoinMarketCap** : https://coinmarketcap.com/
- **StarkScan** : https://starkscan.co/

### Apprendre
- **Binance Academy** : https://academy.binance.com/fr
- **Coinbase Learn** : https://www.coinbase.com/learn
- **Documentation StarkNet** : https://docs.starknet.io/

### Communautés
- **Reddit** : r/starknet, r/CryptoCurrency
- **Discord** : Serveur officiel StarkNet
- **Twitter** : @Starknet, @ekubo_protocol

---

*Ce glossaire est conçu pour être compréhensible par quelqu'un qui n'a jamais fait de crypto. N'hésitez pas à le consulter régulièrement !*

*Dernière mise à jour : 2025-11-26*
