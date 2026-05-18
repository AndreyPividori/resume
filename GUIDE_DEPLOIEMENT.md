# 🚀 Guide de déploiement — Portfolio Andrey Pividori
## GitHub Pages + OVH (pividori.fr) + EmailJS

---

## VUE D'ENSEMBLE

```
Tu écris sur ton PC
      ↓  git push
GitHub (dépôt + Pages)  ←→  ton navigateur accède via pividori.fr/resume
      ↑
OVH DNS pointe vers GitHub Pages
      ↑
EmailJS reçoit les messages de contact → andrey@pividori.fr
```

Temps estimé : **30 à 45 minutes** la première fois.

---

## ÉTAPE 1 — Installer Git sur ton PC

1. Va sur **https://git-scm.com/download/win** (ou Mac)
2. Installe avec les options par défaut
3. Ouvre **Git Bash** (Windows) ou le Terminal (Mac) et vérifie :
   ```bash
   git --version
   # → git version 2.x.x
   ```
4. Configure ton identité (une seule fois) :
   ```bash
   git config --global user.name "Andrey Pividori"
   git config --global user.email "andrey@pividori.fr"
   ```

---

## ÉTAPE 2 — Créer le dépôt GitHub

1. Crée un compte sur **https://github.com** si tu n'en as pas
   → Nom d'utilisateur suggéré : `andreypividori` ou `pividori`

2. Crée un **nouveau dépôt public** :
   - Clique sur **"New repository"**
   - Nom : `resume` *(exactement ce mot — c'est l'URL finale)*
   - Visibilité : **Public**
   - Ne coche rien (pas de README auto)
   - Clique **"Create repository"**

3. Tu obtiens une URL du type :
   `https://github.com/andreypividori/resume`

---

## ÉTAPE 3 — Préparer les fichiers en local

1. Crée un dossier sur ton PC, par exemple :
   ```
   C:\Users\Andrey\Documents\portfolio-resume\
   ```

2. Copie dans ce dossier les 4 fichiers fournis :
   ```
   portfolio-resume/
   ├── index.html        ← ton portfolio (fichier principal)
   ├── CNAME             ← contient : pividori.fr
   ├── .nojekyll         ← fichier vide, obligatoire pour GitHub Pages
   └── README.md         ← description du projet
   ```

3. Ouvre Git Bash **dans ce dossier** (clic droit → "Git Bash Here") et exécute :
   ```bash
   git init
   git add .
   git commit -m "Initial commit — portfolio Andrey Pividori"
   git branch -M main
   git remote add origin https://github.com/TON_USERNAME/resume.git
   git push -u origin main
   ```
   *(Remplace `TON_USERNAME` par ton nom d'utilisateur GitHub)*

   GitHub va te demander de te connecter → suis les instructions.

---

## ÉTAPE 4 — Activer GitHub Pages

1. Sur GitHub, va dans ton dépôt `resume`
2. Clique **Settings** (onglet en haut)
3. Dans le menu gauche : **Pages**
4. Sous "Source" : sélectionne **"Deploy from a branch"**
5. Branch : **main** / Folder : **/ (root)**
6. Clique **Save**

⏳ Attends 2-3 minutes, puis teste :
`https://TON_USERNAME.github.io/resume`

---

## ÉTAPE 5 — Configurer OVH pour pividori.fr

> Objectif : `pividori.fr` → GitHub Pages

1. Connecte-toi à **https://www.ovh.com** → Espace client
2. Va dans **Noms de domaine** → `pividori.fr` → **Zone DNS**
3. **Supprime** les enregistrements A existants qui pointent ailleurs (si présents)
4. **Ajoute 4 enregistrements A** (GitHub Pages IPs) :

   | Type | Sous-domaine | Cible |
   |------|-------------|-------|
   | A | @ (vide) | 185.199.108.153 |
   | A | @ (vide) | 185.199.109.153 |
   | A | @ (vide) | 185.199.110.153 |
   | A | @ (vide) | 185.199.111.153 |

5. **Ajoute un enregistrement CNAME** :

   | Type | Sous-domaine | Cible |
   |------|-------------|-------|
   | CNAME | www | TON_USERNAME.github.io. |

6. ⏳ La propagation DNS prend **entre 15 minutes et 24h**

7. Une fois propagé, GitHub Pages activera automatiquement HTTPS (🔒)
   grâce au fichier `CNAME` déjà dans ton dépôt.

8. **Test final** : `https://pividori.fr` doit afficher ton portfolio
   et `https://pividori.fr/resume` aussi (même fichier).

---

## ÉTAPE 6 — Configurer EmailJS (formulaire de contact)

> Permet de recevoir les messages de contact sur andrey@pividori.fr sans backend.
> Gratuit jusqu'à **200 emails/mois**.

### 6.1 Créer le compte EmailJS

1. Va sur **https://www.emailjs.com** → Sign Up (gratuit)
2. Connecte-toi

### 6.2 Ajouter ton service email

1. Menu gauche → **Email Services** → **Add New Service**
2. Choisis **Gmail** (ou ton fournisseur)
3. Clique **Connect Account** → connecte `andrey@pividori.fr`
4. Nomme le service : `portfolio_contact`
5. Note le **Service ID** (ex: `service_abc123`)

### 6.3 Créer le template d'email

1. Menu gauche → **Email Templates** → **Create New Template**
2. Configure comme suit :

   **To email** : `andrey@pividori.fr`
   **From name** : `{{from_name}}`
   **Reply To** : `{{reply_to}}`
   **Subject** : `[Portfolio] {{subject}}`

   **Contenu (Body)** :
   ```
   Nouveau message depuis ton portfolio !

   De : {{from_name}}
   Email : {{from_email}}
   Objet : {{subject}}

   Message :
   {{message}}
   ```

3. Clique **Save** → note le **Template ID** (ex: `template_xyz456`)

### 6.4 Récupérer ta Public Key

1. Menu gauche → **Account** → **General**
2. Copie ta **Public Key** (ex: `user_AbCdEfGhIjKlMnOp`)

### 6.5 Mettre à jour index.html

Ouvre `index.html` dans un éditeur (VS Code recommandé) et remplace
les 3 lignes en haut du fichier :

```javascript
// AVANT (lignes ~12-14)
const EMAILJS_PUBLIC_KEY  = 'VOTRE_PUBLIC_KEY';
const EMAILJS_SERVICE_ID  = 'VOTRE_SERVICE_ID';
const EMAILJS_TEMPLATE_ID = 'VOTRE_TEMPLATE_ID';

// APRÈS — avec tes vraies valeurs
const EMAILJS_PUBLIC_KEY  = 'user_AbCdEfGhIjKlMnOp';   // ta vraie clé
const EMAILJS_SERVICE_ID  = 'service_abc123';            // ton vrai service ID
const EMAILJS_TEMPLATE_ID = 'template_xyz456';           // ton vrai template ID
```

### 6.6 Pousser la mise à jour

```bash
# Dans Git Bash, dans ton dossier portfolio-resume/
git add index.html
git commit -m "Configure EmailJS contact form"
git push
```

⏳ Attends 1-2 minutes → teste le formulaire sur ton site en ligne.

---

## ÉTAPE 7 — Workflow pour les futures mises à jour

Quand tu veux modifier ton portfolio (nouvelles expériences, etc.) :

```bash
# 1. Modifie index.html avec ton éditeur
# 2. Dans Git Bash :
git add .
git commit -m "Mise à jour : [description de la modif]"
git push
# → Mise en ligne automatique en 1-2 minutes
```

---

## RÉCAPITULATIF FINAL

| Élément | URL / Valeur |
|---------|-------------|
| 🌐 Site en ligne | `https://pividori.fr` |
| 📄 URL portfolio | `https://pividori.fr/resume` |
| 🔧 Dépôt GitHub | `https://github.com/TON_USERNAME/resume` |
| ✉️ Contact | EmailJS → `andrey@pividori.fr` |
| 💰 Coût total | **0 €/mois** (GitHub Pages + EmailJS gratuits) |

---

## EN CAS DE PROBLÈME

**Le site ne s'affiche pas après OVH :**
→ Attends 24h (propagation DNS lente possible)
→ Vérifie dans GitHub Pages (Settings > Pages) que le domaine custom est bien `pividori.fr`

**Le formulaire n'envoie pas :**
→ Ouvre la console du navigateur (F12) et regarde les erreurs
→ Vérifie que les 3 clés EmailJS sont bien renseignées dans index.html

**Page 404 sur GitHub :**
→ Vérifie que le fichier s'appelle bien `index.html` (pas `Index.html`)
→ Vérifie que GitHub Pages est activé sur la branche `main`

---

*Guide préparé pour Andrey Pividori — Mai 2025*
