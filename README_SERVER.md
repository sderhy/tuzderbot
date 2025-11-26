# 🐧 STRK Bot - Version Serveur Linux

Version optimisée pour tourner en arrière-plan sur un serveur Linux, sans interface graphique.

## ✨ Fonctionnalités

- ✅ **Mode daemon** : tourne en arrière-plan 24/7
- ✅ **Alertes Telegram** : notifications automatiques
- ✅ **Dashboard web** : accessible à distance
- ✅ **Auto-restart** : redémarre automatiquement en cas de crash
- ✅ **Logs centralisés** : `/var/log/strk-bot/`
- ✅ **Systemd integration** : démarrage automatique au boot

---

## 🚀 Installation Rapide (Ubuntu/Debian)

### Méthode 1 : Script automatique (recommandé)

```bash
# 1. Télécharger les fichiers sur le serveur
scp strk_bot_server.py install_server.sh user@serveur:/tmp/

# 2. Se connecter au serveur
ssh user@serveur

# 3. Lancer l'installation
cd /tmp
sudo bash install_server.sh
```

### Méthode 2 : Installation manuelle

```bash
# 1. Installer les dépendances
sudo apt-get update
sudo apt-get install -y python3 python3-pip python3-venv

# 2. Créer les répertoires
sudo mkdir -p /opt/strk-bot
sudo mkdir -p /var/log/strk-bot

# 3. Copier le script
sudo cp strk_bot_server.py /opt/strk-bot/
sudo chmod +x /opt/strk-bot/strk_bot_server.py

# 4. Créer l'environnement virtuel
cd /opt/strk-bot
sudo python3 -m venv venv
sudo venv/bin/pip install flask requests

# 5. Configurer les permissions
sudo chown -R www-data:www-data /opt/strk-bot
sudo chown -R www-data:www-data /var/log/strk-bot

# 6. Installer le service systemd
sudo cp strk-bot.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable strk-bot
```

---

## ⚙️ Configuration

### 1. Éditer les credentials Telegram

```bash
sudo nano /opt/strk-bot/strk_bot_server.py
```

Modifier les lignes 23-24 :
```python
TELEGRAM_BOT_TOKEN = "VOTRE_TOKEN_ICI"
TELEGRAM_CHAT_ID = "VOTRE_CHAT_ID_ICI"
```

### 2. Personnaliser les paramètres (optionnel)

Dans le même fichier :

```python
# Port du serveur web (ligne 31)
SERVER_PORT = 5555

# Intervalle d'analyse en secondes (ligne 32)
UPDATE_INTERVAL = 30

# Seuils d'alerte (lignes 27-28)
ALERT_THRESHOLD_HIGH = 75  # Alerte ACHAT si ≥ 75%
ALERT_THRESHOLD_LOW = 25   # Alerte VENTE si ≤ 25%

# Cooldown entre alertes en secondes (ligne 29)
ALERT_COOLDOWN = 1800  # 30 minutes
```

---

## 🎮 Gestion du Service

### Démarrer le bot
```bash
sudo systemctl start strk-bot
```

### Arrêter le bot
```bash
sudo systemctl stop strk-bot
```

### Redémarrer le bot
```bash
sudo systemctl restart strk-bot
```

### Voir le statut
```bash
sudo systemctl status strk-bot
```

### Activer le démarrage automatique
```bash
sudo systemctl enable strk-bot
```

### Désactiver le démarrage automatique
```bash
sudo systemctl disable strk-bot
```

---

## 📊 Logs et Monitoring

### Voir les logs en temps réel
```bash
sudo tail -f /var/log/strk-bot/output.log
```

### Voir les erreurs
```bash
sudo tail -f /var/log/strk-bot/error.log
```

### Voir les 100 dernières lignes
```bash
sudo tail -n 100 /var/log/strk-bot/output.log
```

### Logs du service systemd
```bash
sudo journalctl -u strk-bot -f
```

---

## 🌐 Accès au Dashboard Web

Le dashboard est accessible à l'adresse :
```
http://VOTRE_IP_SERVEUR:5555
```

### Exemples :
- Local : `http://localhost:5555`
- Réseau local : `http://192.168.1.100:5555`
- Public : `http://votre-domaine.com:5555`

### Ouvrir le port dans le firewall (si nécessaire)

**Ubuntu/Debian (ufw) :**
```bash
sudo ufw allow 5555/tcp
```

**CentOS/RHEL (firewalld) :**
```bash
sudo firewall-cmd --permanent --add-port=5555/tcp
sudo firewall-cmd --reload
```

---

## 🔒 Sécurité et Production

### 1. Utiliser un reverse proxy (NGINX)

```nginx
# /etc/nginx/sites-available/strk-bot
server {
    listen 80;
    server_name votre-domaine.com;

    location / {
        proxy_pass http://127.0.0.1:5555;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Activer :
```bash
sudo ln -s /etc/nginx/sites-available/strk-bot /etc/nginx/sites-enabled/
sudo systemctl reload nginx
```

### 2. Ajouter HTTPS avec Let's Encrypt

```bash
sudo apt-get install certbot python3-certbot-nginx
sudo certbot --nginx -d votre-domaine.com
```

### 3. Protection par mot de passe (optionnel)

Éditer `strk_bot_server.py` et ajouter :

```python
from flask import request, Response

def check_auth(username, password):
    return username == 'admin' and password == 'votre_password'

def authenticate():
    return Response('Login Required', 401,
        {'WWW-Authenticate': 'Basic realm="Login Required"'})

@app.before_request
def require_auth():
    auth = request.authorization
    if not auth or not check_auth(auth.username, auth.password):
        return authenticate()
```

---

## 📈 Fichiers Générés

- `/opt/strk-bot/strk_cache.json` : Cache des données API
- `/opt/strk-bot/strk_signals.csv` : Historique des signaux
- `/var/log/strk-bot/output.log` : Logs du bot
- `/var/log/strk-bot/error.log` : Erreurs

---

## 🔧 Dépannage

### Le bot ne démarre pas
```bash
# Vérifier les logs
sudo journalctl -u strk-bot -n 50

# Vérifier les permissions
ls -la /opt/strk-bot

# Tester manuellement
cd /opt/strk-bot
sudo -u www-data venv/bin/python3 strk_bot_server.py
```

### Pas d'alertes Telegram
```bash
# Vérifier la configuration
sudo grep TELEGRAM /opt/strk-bot/strk_bot_server.py

# Tester l'API Telegram
curl -X POST "https://api.telegram.org/bot<TOKEN>/sendMessage" \
  -d "chat_id=<CHAT_ID>&text=Test"
```

### Port déjà utilisé
```bash
# Vérifier ce qui utilise le port 5555
sudo netstat -tulpn | grep 5555

# Changer le port dans le fichier (ligne 31)
sudo nano /opt/strk-bot/strk_bot_server.py
```

---

## 🆚 Différences avec la version Mac

| Fonctionnalité | Version Mac | Version Server |
|----------------|-------------|----------------|
| Interface graphique | ✅ PyWebview | ❌ Web uniquement |
| Démarrage | Manuel | ✅ Automatique (systemd) |
| Logs | Console | ✅ Fichiers `/var/log` |
| Accès distant | ❌ | ✅ |
| Auto-restart | ❌ | ✅ |
| Mode daemon | ❌ | ✅ |

---

## 📊 Monitoring Avancé (Optionnel)

### Health Check Endpoint

Le bot expose un endpoint de santé :
```bash
curl http://localhost:5555/health
```

Réponse :
```json
{"status": "ok", "timestamp": "2024-11-14T12:00:00"}
```

### Intégration avec Uptime Kuma / Monitoring

1. Installer Uptime Kuma :
```bash
docker run -d --restart=always -p 3001:3001 louislam/uptime-kuma:1
```

2. Ajouter un monitor HTTP avec l'URL :
```
http://localhost:5555/health
```

---

## 🚀 Mise en Production - Checklist

- [ ] Bot Token Telegram configuré
- [ ] Chat ID configuré
- [ ] Port 5555 ouvert dans le firewall (ou reverse proxy)
- [ ] Service systemd activé
- [ ] Logs vérifiés
- [ ] Dashboard accessible
- [ ] Test d'alerte Telegram envoyé
- [ ] Auto-restart configuré
- [ ] (Optionnel) HTTPS configuré
- [ ] (Optionnel) Monitoring configuré

---

## 📞 Support

Si vous rencontrez des problèmes :

1. Vérifier les logs : `sudo tail -f /var/log/strk-bot/output.log`
2. Vérifier le statut : `sudo systemctl status strk-bot`
3. Tester manuellement : `cd /opt/strk-bot && sudo -u www-data venv/bin/python3 strk_bot_server.py`

---

**Bon monitoring ! 🚀📈**
