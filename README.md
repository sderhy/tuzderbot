
## 🤖 **Setup du Bot Telegram (5 minutes)**

### **Étape 1 : Créer ton bot Telegram**

1. Ouvre Telegram et cherche **@BotFather**
1. Envoie `/newbot`
1. Donne un nom : `STRK Alert Bot`
1. Donne un username : `strk_alert_bot` (ou autre disponible)
1. **BotFather te donne un TOKEN** comme : `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`

### **Étape 2 : Récupérer ton Chat ID**

1. Cherche ton bot dans Telegram et envoie `/start`
1. Va sur cette URL (remplace TOKEN) :

```
https://api.telegram.org/bot<TON_TOKEN>/getUpdates
```

1. Tu verras un JSON avec `"chat":{"id":123456789}` → c’est ton **CHAT_ID**

-----

## 📦 **Installation de la librairie**

```bash
pip3 install python-telegram-bot
```

-----

## 🔥 **Version améliorée avec Telegram**

-----

## ⚙️ **Configuration Telegram en 3 lignes**

Dans le fichier `strk_bot_app.py`, remplace ces lignes (vers le haut) :

```python
# 🔔 CONFIGURATION TELEGRAM (à remplir)
TELEGRAM_BOT_TOKEN = "123456789:ABCdefGHIjklMNOpqrsTUVwxyz" # Ton token @BotFather
TELEGRAM_CHAT_ID = "123456789" # Ton chat ID
```

-----

## 🔔 **Comment fonctionnent les alertes**

### **Seuils par défaut** :

- **ACHAT** : probabilité hausse **≥ 75%**
- **VENTE** : probabilité hausse **≤ 25%**

### **Cooldown intelligent** :

- **30 minutes** entre 2 alertes identiques (évite le spam)
- Les alertes contiennent :
- Signal (ACHAT/VENTE)
- Probabilité exacte
- Prix actuel
- Variation 24h
- Volume 24h (pour ACHAT)

### **Exemple de message Telegram** :

```
🚀 ALERTE STRK BOT 🚀

📍 Signal : ACHAT
📊 Probabilité : 78.3%
💰 Prix actuel : $0.1456

📈 Variation 24h : +8.54%
💵 Volume 24h : $287.5M

🎯 Confiance élevée pour un mouvement haussier

⏰ 14:35:22
```

-----

## 🎨 **Personnaliser les seuils**

Tu peux ajuster les seuils dans la config :

```python
ALERT_THRESHOLD_HIGH = 80 # Plus strict (alerte si >80%)
ALERT_THRESHOLD_LOW = 20 # Plus strict (alerte si <20%)
ALERT_COOLDOWN = 3600 # 1h entre alertes
```

-----

## 🧪 **Tester ton bot Telegram**

1. Lance l’app :

```bash
python3 strk_bot_app.py
```

1. **Pour forcer un test manuel**, ajoute cette ligne temporairement :

```python
# Dans la fonction api_data(), juste avant return jsonify(result)
send_telegram_alert("TEST", 85, 0.14, "Test de connexion")
```

-----

## 📊 **Bonus : Statistiques Telegram (optionnel)**

Si tu veux aussi un **rapport quotidien automatique**, ajoute ce thread :

```python
def daily_report():
"""Envoie un rapport quotidien à 9h"""
while True:
now = datetime.now()
if now.hour == 9 and now.minute == 0:
# Lire le CSV pour stats
try:
import pandas as pd
df = pd.read_csv(LOG_FILE)
avg_prob = df['prob_haussier'].mean()

report = f"""
📊 RAPPORT QUOTIDIEN STRK

📈 Prob. moyenne 24h : {avg_prob:.1f}%
📍 Signaux : {df['signal'].value_counts().to_dict()}
💰 Prix moyen : ${df['price'].mean():.4f}
"""
send_telegram_alert("RAPPORT", avg_prob, df['price'].iloc[-1], report)
except:
pass

time.sleep(60) # Vérifier chaque minute

# Dans le __main__, ajouter :
report_thread = threading.Thread(target=daily_report, daemon=True)
report_thread.start()
```

-----

## 🚀 **Résumé de ce qui se passe maintenant**

✅ **App Mac** avec interface moderne
✅ **Données live** (CoinGecko + Ekubo + StarkScan)
✅ **Alertes Telegram** quand signal fort
✅ **Cooldown** anti-spam
✅ **Logging** dans CSV
✅ **Cache** pour éviter rate limits

**Tu es prêt ! 🎯**
