# Guide Complet : Configuration Keycloak et Tests - Service User

Ce document contient **toutes les étapes de configuration manuelle de Keycloak** de A à Z, ainsi que tous les tests à effectuer.

---

## 📋 Table des Matières

1. [Démarrage du Système](#1-démarrage-du-système)
2. [Accès Initial à Keycloak](#2-accès-initial-à-keycloak)
3. [Création du Realm](#3-création-du-realm)
4. [Création du Client](#4-création-du-client)
5. [Création des Rôles](#5-création-des-rôles)
6. [Configuration des Mappers](#6-configuration-des-mappers)
7. [Création des Utilisateurs](#7-création-des-utilisateurs)
8. [Tests Complets](#8-tests-complets)

---

## 1. Démarrage du Système

### Étape 1.1 : Arrêter les Conteneurs Existants

```bash
cd /home/aimerou/Documents/Mes-projets/projet-scala/gestion_des_etudiants
docker-compose down
```

**Résultat attendu** :
```
Stopping service-user ... done
Stopping keycloak ... done
Stopping keycloak-db ... done
...
Removing service-user ... done
Removing keycloak ... done
```

### Étape 1.2 : Démarrer Tous les Services

```bash
docker-compose up -d
```

**Résultat attendu** :
```
Creating network "gestion_des_etudiants_microservices-net" ... done
Creating volume "gestion_des_etudiants_keycloak-db-data" ... done
Creating ms-config-server ... done
Creating ms-discover-server ... done
Creating keycloak-db ... done
Creating keycloak ... done
Creating service-user ... done
...
```

### Étape 1.3 : Surveiller le Démarrage

```bash
docker-compose logs -f keycloak
```

**Attendez de voir** :
```
keycloak | Running the server in production mode. DO NOT use this configuration in production.
keycloak | Listening on: http://0.0.0.0:8080
keycloak | Keycloak 25.0 started in XXXms
```

**Appuyez sur `Ctrl+C`** pour arrêter de suivre les logs.

### Étape 1.4 : Vérifier que Tous les Services Sont Healthy

```bash
docker-compose ps
```

**Vérifiez que tous les services affichent** : `Up (healthy)`

**Attendez 2-3 minutes** si certains services ne sont pas encore healthy.

---

## 2. Accès Initial à Keycloak

### Étape 2.1 : Ouvrir Keycloak dans le Navigateur

1. Ouvrez votre navigateur (Chrome, Firefox, etc.)
2. Accédez à l'URL : **http://localhost:8180**

**Ce que vous devriez voir** :
```
┌─────────────────────────────────────────┐
│         Welcome to Keycloak             │
│                                         │
│  [Administration Console]               │
│                                         │
└─────────────────────────────────────────┘
```

### Étape 2.2 : Cliquer sur "Administration Console"

Cliquez sur le bouton **"Administration Console"**.

### Étape 2.3 : Se Connecter

**Page de connexion affichée** :
```
┌─────────────────────────────────────────┐
│         Sign in to your account         │
│                                         │
│  Username or email                      │
│  [________________]                     │
│                                         │
│  Password                               │
│  [________________]                     │
│                                         │
│         [Sign In]                       │
│                                         │
└─────────────────────────────────────────┘
```

**Entrez** :
- **Username** : `admin`
- **Password** : `admin`

**Cliquez sur** : `Sign In`

### Étape 2.4 : Vérification de la Connexion

**Vous devriez voir** le tableau de bord Keycloak avec :
- En haut à gauche : Menu déroulant affichant `master`
- Menu de gauche : Realm settings, Clients, Users, etc.

---

## 3. Création du Realm

### Étape 3.1 : Accéder au Menu des Realms

1. En haut à gauche, cliquez sur le **menu déroulant** qui affiche `master`
2. Vous verrez une liste avec :
   - `master` (sélectionné)
   - `Create Realm` (bouton)

### Étape 3.2 : Cliquer sur "Create Realm"

Cliquez sur **"Create Realm"**.

### Étape 3.3 : Remplir le Formulaire de Création

**Formulaire affiché** :
```
┌─────────────────────────────────────────────────────┐
│  Create realm                                       │
│                                                     │
│  Realm name *                                       │
│  [_______________________________________]          │
│                                                     │
│  ☐ Enabled                                          │
│                                                     │
│  [Cancel]                    [Create]               │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Remplissez** :
1. **Realm name** : Tapez exactement `gestion-etudiants`
2. **Enabled** : Cochez la case ✅

**Cliquez sur** : `Create`

### Étape 3.4 : Vérification

**Message de succès affiché** :
```
✅ Success! Realm created
```

**Vérifiez** :
- Le menu déroulant en haut à gauche affiche maintenant `gestion-etudiants`
- Vous êtes sur la page "Realm settings"

---

## 4. Création du Client

### Étape 4.1 : Accéder au Menu Clients

Dans le **menu de gauche**, cliquez sur **"Clients"**.

**Page affichée** :
```
┌─────────────────────────────────────────────────────┐
│  Clients                                            │
│                                                     │
│  [Create client]                                    │
│                                                     │
│  List of clients:                                   │
│  - account                                          │
│  - account-console                                  │
│  - admin-cli                                        │
│  - broker                                           │
│  - realm-management                                 │
│  - security-admin-console                           │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Étape 4.2 : Cliquer sur "Create client"

Cliquez sur le bouton **"Create client"**.

### Étape 4.3 : Écran 1/3 - General Settings

**Formulaire affiché** :
```
┌─────────────────────────────────────────────────────┐
│  Add Client                                         │
│  Step 1 of 3: General Settings                     │
│                                                     │
│  Client type *                                      │
│  ○ OpenID Connect  ○ SAML                           │
│                                                     │
│  Client ID *                                        │
│  [_______________________________________]          │
│                                                     │
│  Name                                               │
│  [_______________________________________]          │
│                                                     │
│  Description                                        │
│  [_______________________________________]          │
│                                                     │
│  [Back]                          [Next]             │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Remplissez** :
1. **Client type** : Sélectionnez `OpenID Connect` (déjà sélectionné par défaut)
2. **Client ID** : Tapez exactement `gestion-etudiants-client`
3. **Name** : (optionnel) Tapez `Gestion Étudiants Client`
4. **Description** : (optionnel) Tapez `Client pour l'application de gestion des étudiants`

**Cliquez sur** : `Next`

### Étape 4.4 : Écran 2/3 - Capability Config

**Formulaire affiché** :
```
┌─────────────────────────────────────────────────────┐
│  Add Client                                         │
│  Step 2 of 3: Capability config                    │
│                                                     │
│  ☐ Client authentication                            │
│  ☐ Authorization                                    │
│                                                     │
│  Authentication flow                                │
│  ☑ Standard flow                                    │
│  ☐ Direct access grants                             │
│  ☐ Implicit flow                                    │
│  ☐ Service accounts roles                           │
│  ☐ OAuth 2.0 Device Authorization Grant             │
│  ☐ OIDC CIBA Grant                                  │
│                                                     │
│  [Back]                          [Next]             │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Configurez** :
1. **Client authentication** : Laissez **DÉCOCHÉ** ❌ (client public)
2. **Authorization** : Laissez **DÉCOCHÉ** ❌
3. **Authentication flow** :
   - **Standard flow** : **COCHÉ** ✅ (déjà coché)
   - **Direct access grants** : **COCHEZ** ✅ (IMPORTANT !)
   - **Implicit flow** : Laissez **DÉCOCHÉ** ❌
   - **Service accounts roles** : Laissez **DÉCOCHÉ** ❌
   - **OAuth 2.0 Device Authorization Grant** : Laissez **DÉCOCHÉ** ❌
   - **OIDC CIBA Grant** : Laissez **DÉCOCHÉ** ❌

**Cliquez sur** : `Next`

### Étape 4.5 : Écran 3/3 - Login Settings

**Formulaire affiché** :
```
┌─────────────────────────────────────────────────────┐
│  Add Client                                         │
│  Step 3 of 3: Login settings                       │
│                                                     │
│  Root URL                                           │
│  [_______________________________________]          │
│                                                     │
│  Home URL                                           │
│  [_______________________________________]          │
│                                                     │
│  Valid redirect URIs                                │
│  [_______________________________________] [+]       │
│                                                     │
│  Valid post logout redirect URIs                    │
│  [_______________________________________] [+]       │
│                                                     │
│  Web origins                                        │
│  [_______________________________________] [+]       │
│                                                     │
│  [Back]                          [Save]             │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Remplissez** :

1. **Root URL** : Laissez vide
2. **Home URL** : Laissez vide
3. **Valid redirect URIs** :
   - Tapez `http://localhost:3000/*` puis cliquez sur `+`
   - Tapez `http://localhost:8088/*` puis cliquez sur `+`
4. **Valid post logout redirect URIs** :
   - Tapez `http://localhost:3000/*` puis cliquez sur `+`
   - Tapez `http://localhost:8088/*` puis cliquez sur `+`
5. **Web origins** :
   - Tapez `http://localhost:3000` puis cliquez sur `+`
   - Tapez `http://localhost:8088` puis cliquez sur `+`

**Cliquez sur** : `Save`

### Étape 4.6 : Vérification

**Message de succès** :
```
✅ Success! Client created successfully
```

**Vérifiez** :
- Vous êtes sur la page de détails du client `gestion-etudiants-client`
- L'onglet "Settings" est actif

---

## 5. Création des Rôles

### Étape 5.1 : Accéder au Menu Realm Roles

Dans le **menu de gauche**, cliquez sur **"Realm roles"**.

**Page affichée** :
```
┌─────────────────────────────────────────────────────┐
│  Realm roles                                        │
│                                                     │
│  [Create role]                                      │
│                                                     │
│  List of roles:                                     │
│  - default-roles-gestion-etudiants                  │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Étape 5.2 : Créer le Rôle ADMIN

#### 5.2.1 : Cliquer sur "Create role"

Cliquez sur le bouton **"Create role"**.

#### 5.2.2 : Remplir le Formulaire

**Formulaire affiché** :
```
┌─────────────────────────────────────────────────────┐
│  Create role                                        │
│                                                     │
│  Role name *                                        │
│  [_______________________________________]          │
│                                                     │
│  Description                                        │
│  [_______________________________________]          │
│                                                     │
│  [Cancel]                    [Save]                 │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Remplissez** :
1. **Role name** : Tapez exactement `ADMIN` (en majuscules)
2. **Description** : Tapez `Administrateur système`

**Cliquez sur** : `Save`

#### 5.2.3 : Vérification

**Message de succès** :
```
✅ Success! Role created
```

### Étape 5.3 : Créer le Rôle ENSEIGNANT

#### 5.3.1 : Retourner à la Liste des Rôles

Cliquez sur **"Realm roles"** dans le menu de gauche.

#### 5.3.2 : Cliquer sur "Create role"

Cliquez sur le bouton **"Create role"**.

#### 5.3.3 : Remplir le Formulaire

**Remplissez** :
1. **Role name** : Tapez exactement `ENSEIGNANT` (en majuscules)
2. **Description** : Tapez `Enseignant`

**Cliquez sur** : `Save`

### Étape 5.4 : Créer le Rôle ETUDIANT

#### 5.4.1 : Retourner à la Liste des Rôles

Cliquez sur **"Realm roles"** dans le menu de gauche.

#### 5.4.2 : Cliquer sur "Create role"

Cliquez sur le bouton **"Create role"**.

#### 5.4.3 : Remplir le Formulaire

**Remplissez** :
1. **Role name** : Tapez exactement `ETUDIANT` (en majuscules)
2. **Description** : Tapez `Étudiant`

**Cliquez sur** : `Save`

### Étape 5.5 : Vérification Finale

Cliquez sur **"Realm roles"** dans le menu de gauche.

**Vous devriez voir** :
```
┌─────────────────────────────────────────────────────┐
│  Realm roles                                        │
│                                                     │
│  List of roles:                                     │
│  - ADMIN                                            │
│  - ENSEIGNANT                                       │
│  - ETUDIANT                                         │
│  - default-roles-gestion-etudiants                  │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 6. Configuration des Mappers

Les mappers permettent d'inclure les rôles dans le JWT token.

### Étape 6.1 : Accéder au Client

1. Dans le **menu de gauche**, cliquez sur **"Clients"**
2. Dans la liste, cliquez sur **"gestion-etudiants-client"**

### Étape 6.2 : Accéder aux Client Scopes

1. Cliquez sur l'onglet **"Client scopes"** (en haut de la page)

**Page affichée** :
```
┌─────────────────────────────────────────────────────┐
│  Client scopes                                      │
│                                                     │
│  Assigned client scopes                             │
│  - gestion-etudiants-client-dedicated               │
│  - email                                            │
│  - profile                                          │
│  - roles                                            │
│  - web-origins                                      │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Étape 6.3 : Cliquer sur le Scope Dédié

Dans la section **"Assigned client scopes"**, cliquez sur **"gestion-etudiants-client-dedicated"**.

### Étape 6.4 : Accéder aux Mappers

Cliquez sur l'onglet **"Mappers"** (en haut de la page).

**Page affichée** :
```
┌─────────────────────────────────────────────────────┐
│  Mappers                                            │
│                                                     │
│  [Add mapper]  ▼                                    │
│                                                     │
│  List of mappers:                                   │
│  (vide ou quelques mappers par défaut)              │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Étape 6.5 : Ajouter un Mapper

#### 6.5.1 : Cliquer sur "Add mapper"

Cliquez sur le bouton **"Add mapper"** puis sélectionnez **"By configuration"**.

**Liste affichée** :
```
┌─────────────────────────────────────────────────────┐
│  Choose mapper type                                 │
│                                                     │
│  - Audience                                         │
│  - Audience Resolve                                 │
│  - Group Membership                                 │
│  - Hardcoded claim                                  │
│  - Hardcoded Role                                   │
│  - User Attribute                                   │
│  - User Client Role                                 │
│  - User Property                                    │
│  - User Realm Role                                  │
│  - User Session Note                                │
│  ...                                                │
│                                                     │
└─────────────────────────────────────────────────────┘
```

#### 6.5.2 : Sélectionner "User Realm Role"

Trouvez et cliquez sur **"User Realm Role"**.

#### 6.5.3 : Configurer le Mapper

**Formulaire affiché** :
```
┌─────────────────────────────────────────────────────┐
│  Add mapper                                         │
│                                                     │
│  Name *                                             │
│  [_______________________________________]          │
│                                                     │
│  Mapper type                                        │
│  User Realm Role                                    │
│                                                     │
│  ☐ Multivalued                                      │
│                                                     │
│  Token Claim Name *                                 │
│  [_______________________________________]          │
│                                                     │
│  Claim JSON Type                                    │
│  [String ▼]                                         │
│                                                     │
│  ☐ Add to ID token                                  │
│  ☐ Add to access token                              │
│  ☐ Add to userinfo                                  │
│                                                     │
│  [Cancel]                    [Save]                 │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Remplissez** :
1. **Name** : Tapez exactement `roles`
2. **Mapper type** : `User Realm Role` (déjà sélectionné)
3. **Multivalued** : **COCHEZ** ✅
4. **Token Claim Name** : Tapez exactement `roles`
5. **Claim JSON Type** : Sélectionnez `String`
6. **Add to ID token** : **COCHEZ** ✅
7. **Add to access token** : **COCHEZ** ✅
8. **Add to userinfo** : **COCHEZ** ✅

**Cliquez sur** : `Save`

### Étape 6.6 : Vérification

**Message de succès** :
```
✅ Success! Mapper created
```

**Retournez sur l'onglet "Mappers"** et vérifiez que le mapper `roles` apparaît dans la liste.

---

## 7. Création des Utilisateurs

### Étape 7.1 : Créer l'Utilisateur ADMIN

#### 7.1.1 : Accéder au Menu Users

Dans le **menu de gauche**, cliquez sur **"Users"**.

#### 7.1.2 : Cliquer sur "Add user"

Cliquez sur le bouton **"Add user"**.

#### 7.1.3 : Remplir le Formulaire

**Formulaire affiché** :
```
┌─────────────────────────────────────────────────────┐
│  Create user                                        │
│                                                     │
│  ☐ Email verified                                   │
│  ☐ Enabled                                          │
│                                                     │
│  Username *                                         │
│  [_______________________________________]          │
│                                                     │
│  Email                                              │
│  [_______________________________________]          │
│                                                     │
│  First name                                         │
│  [_______________________________________]          │
│                                                     │
│  Last name                                          │
│  [_______________________________________]          │
│                                                     │
│  [Cancel]                    [Create]               │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Remplissez** :
1. **Email verified** : **COCHEZ** ✅
2. **Enabled** : **COCHEZ** ✅
3. **Username** : Tapez `admin`
4. **Email** : Tapez `admin@example.com`
5. **First name** : Tapez `Admin`
6. **Last name** : Tapez `User`

**Cliquez sur** : `Create`

#### 7.1.4 : Définir le Mot de Passe

**Vous êtes maintenant sur la page de détails de l'utilisateur.**

1. Cliquez sur l'onglet **"Credentials"** (en haut)

**Page affichée** :
```
┌─────────────────────────────────────────────────────┐
│  Credentials                                        │
│                                                     │
│  [Set password]                                     │
│                                                     │
└─────────────────────────────────────────────────────┘
```

2. Cliquez sur le bouton **"Set password"**

**Popup affichée** :
```
┌─────────────────────────────────────────────────────┐
│  Set password                                       │
│                                                     │
│  Password *                                         │
│  [_______________________________________]          │
│                                                     │
│  Password confirmation *                            │
│  [_______________________________________]          │
│                                                     │
│  ☐ Temporary                                        │
│                                                     │
│  [Cancel]                    [Save]                 │
│                                                     │
└─────────────────────────────────────────────────────┘
```

3. **Remplissez** :
   - **Password** : Tapez `admin123`
   - **Password confirmation** : Tapez `admin123`
   - **Temporary** : Laissez **DÉCOCHÉ** ❌ (IMPORTANT !)

4. **Cliquez sur** : `Save`

5. **Popup de confirmation** :
   ```
   Are you sure you want to set a password for the user?
   [Cancel]  [Save password]
   ```

6. **Cliquez sur** : `Save password`

#### 7.1.5 : Assigner le Rôle ADMIN

1. Cliquez sur l'onglet **"Role mapping"** (en haut)

**Page affichée** :
```
┌─────────────────────────────────────────────────────┐
│  Role mapping                                       │
│                                                     │
│  [Assign role]                                      │
│                                                     │
│  Assigned roles:                                    │
│  - default-roles-gestion-etudiants                  │
│                                                     │
└─────────────────────────────────────────────────────┘
```

2. Cliquez sur le bouton **"Assign role"**

**Popup affichée** :
```
┌─────────────────────────────────────────────────────┐
│  Assign role to admin                               │
│                                                     │
│  Filter by realm roles  ▼                           │
│                                                     │
│  ☐ ADMIN                                            │
│  ☐ ENSEIGNANT                                       │
│  ☐ ETUDIANT                                         │
│  ☐ default-roles-gestion-etudiants                  │
│  ...                                                │
│                                                     │
│  [Cancel]                    [Assign]               │
│                                                     │
└─────────────────────────────────────────────────────┘
```

3. **Cochez** la case devant `ADMIN` ✅

4. **Cliquez sur** : `Assign`

#### 7.1.6 : Vérification

**Vous devriez voir** :
```
┌─────────────────────────────────────────────────────┐
│  Role mapping                                       │
│                                                     │
│  Assigned roles:                                    │
│  - ADMIN                                            │
│  - default-roles-gestion-etudiants                  │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Étape 7.2 : Créer l'Utilisateur ENSEIGNANT

**Répétez les étapes 7.1.1 à 7.1.6** avec les informations suivantes :

- **Username** : `enseignant`
- **Email** : `enseignant@example.com`
- **First name** : `Jean`
- **Last name** : `Dupont`
- **Password** : `enseign123`
- **Temporary** : ❌ DÉCOCHÉ
- **Rôle** : `ENSEIGNANT`

### Étape 7.3 : Créer l'Utilisateur ETUDIANT

**Répétez les étapes 7.1.1 à 7.1.6** avec les informations suivantes :

- **Username** : `etudiant`
- **Email** : `etudiant@example.com`
- **First name** : `Marie`
- **Last name** : `Martin`
- **Password** : `etud123`
- **Temporary** : ❌ DÉCOCHÉ
- **Rôle** : `ETUDIANT`

### Étape 7.4 : Vérification Finale

1. Cliquez sur **"Users"** dans le menu de gauche
2. Vous devriez voir **3 utilisateurs** dans la liste :
   - `admin`
   - `enseignant`
   - `etudiant`

---

## 8. Tests Complets

### Test 1 : Vérification des Services

#### Test 1.1 : Keycloak Accessible

```bash
curl -I http://localhost:8180/realms/gestion-etudiants
```

**Résultat attendu** :
```
HTTP/1.1 200 OK
Content-Type: application/json
...
```

#### Test 1.2 : Service-User Accessible

```bash
curl http://localhost:8081/actuator/health
```

**Résultat attendu** :
```json
{
  "status": "UP"
}
```

---

### Test 2 : Authentification Keycloak Directe

#### Test 2.1 : Obtenir Token Admin

```bash
curl -X POST http://localhost:8180/realms/gestion-etudiants/protocol/openid-connect/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=password" \
  -d "client_id=gestion-etudiants-client" \
  -d "username=admin" \
  -d "password=admin123"
```

**Résultat attendu** :
```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI...",
  "expires_in": 300,
  "refresh_expires_in": 1800,
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI...",
  "token_type": "Bearer",
  "not-before-policy": 0,
  "session_state": "abc123-def456-...",
  "scope": "profile email"
}
```

#### Test 2.2 : Vérifier le Token sur jwt.io

1. **Copiez** la valeur de `access_token` (le long texte commençant par `eyJ...`)
2. Allez sur **https://jwt.io**
3. **Collez** le token dans la section "Encoded" (à gauche)
4. **Vérifiez** le payload (section "Decoded" à droite) :

**Payload attendu** :
```json
{
  "exp": 1736709123,
  "iat": 1736708823,
  "jti": "abc123-def456-...",
  "iss": "http://localhost:8180/realms/gestion-etudiants",
  "aud": "account",
  "sub": "xyz789-...",
  "typ": "Bearer",
  "azp": "gestion-etudiants-client",
  "session_state": "...",
  "acr": "1",
  "realm_access": {
    "roles": [
      "ADMIN",
      "default-roles-gestion-etudiants"
    ]
  },
  "roles": [
    "ADMIN"
  ],
  "scope": "profile email",
  "sid": "...",
  "email_verified": true,
  "preferred_username": "admin",
  "given_name": "Admin",
  "family_name": "User",
  "email": "admin@example.com"
}
```

**Points critiques à vérifier** :
- ✅ `"roles": ["ADMIN"]` est présent
- ✅ `"preferred_username": "admin"` est correct
- ✅ `"email": "admin@example.com"` est correct
- ✅ `"iss"` contient `http://localhost:8180/realms/gestion-etudiants`

#### Test 2.3 : Token Enseignant

```bash
curl -X POST http://localhost:8180/realms/gestion-etudiants/protocol/openid-connect/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=password" \
  -d "client_id=gestion-etudiants-client" \
  -d "username=enseignant" \
  -d "password=enseign123"
```

**Vérifiez** : Le token doit contenir `"roles": ["ENSEIGNANT"]`

#### Test 2.4 : Token Étudiant

```bash
curl -X POST http://localhost:8180/realms/gestion-etudiants/protocol/openid-connect/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=password" \
  -d "client_id=gestion-etudiants-client" \
  -d "username=etudiant" \
  -d "password=etud123"
```

**Vérifiez** : Le token doit contenir `"roles": ["ETUDIANT"]`

---

### Test 3 : Enregistrement via Service-User

#### Test 3.1 : Créer un Nouvel Utilisateur

```bash
curl -X POST http://localhost:8081/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "email": "test@example.com",
    "password": "password123",
    "firstName": "Test",
    "lastName": "User",
    "roles": ["ETUDIANT"]
  }'
```

**Résultat attendu** :
```json
{
  "id": 1,
  "username": "testuser",
  "email": "test@example.com",
  "firstName": "Test",
  "lastName": "User",
  "keycloakId": "abc123-def456-ghi789-...",
  "roles": ["ETUDIANT"]
}
```

#### Test 3.2 : Vérifier dans Keycloak

1. Retournez sur **Keycloak** (http://localhost:8180)
2. Allez dans **Users**
3. Recherchez `testuser`
4. **Vérifiez** :
   - ✅ L'utilisateur existe
   - ✅ Email : `test@example.com`
5. Cliquez sur l'utilisateur puis sur **"Role mapping"**
6. **Vérifiez** :
   - ✅ Le rôle `ETUDIANT` est assigné

---

### Test 4 : Login via Service-User

#### Test 4.1 : Login Admin

```bash
curl -X POST http://localhost:8081/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "admin",
    "password": "admin123"
  }'
```

**Résultat attendu** :
```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI...",
  "expires_in": 300,
  "refresh_expires_in": 1800,
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI...",
  "token_type": "Bearer"
}
```

#### Test 4.2 : Login Testuser

```bash
curl -X POST http://localhost:8081/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "password": "password123"
  }'
```

**Résultat attendu** : Même format que Test 4.1

---

### Test 5 : Endpoints Protégés

#### Test 5.1 : Sans Token (doit échouer)

```bash
curl -X GET http://localhost:8081/api/users
```

**Résultat attendu** :
```
401 Unauthorized
```

#### Test 5.2 : Avec Token Admin (doit réussir)

```bash
# Étape 1 : Obtenir le token
TOKEN=$(curl -s -X POST http://localhost:8081/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"admin123"}' \
  | grep -o '"access_token":"[^"]*' | cut -d'"' -f4)

# Étape 2 : Afficher le token (pour vérification)
echo "Token: $TOKEN"

# Étape 3 : Utiliser le token
curl -X GET http://localhost:8081/api/users \
  -H "Authorization: Bearer $TOKEN"
```

**Résultat attendu** : `200 OK` avec liste des utilisateurs

#### Test 5.3 : Avec Token Étudiant (doit échouer)

```bash
# Étape 1 : Obtenir le token étudiant
TOKEN=$(curl -s -X POST http://localhost:8081/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"etudiant","password":"etud123"}' \
  | grep -o '"access_token":"[^"]*' | cut -d'"' -f4)

# Étape 2 : Essayer d'accéder
curl -X GET http://localhost:8081/api/users \
  -H "Authorization: Bearer $TOKEN"
```

**Résultat attendu** : `403 Forbidden`

---

### Test 6 : Swagger UI

#### Test 6.1 : Accéder à Swagger

Ouvrez votre navigateur :
```
http://localhost:8088/service-user/swagger-ui.html
```

#### Test 6.2 : Tester Login

1. Trouvez **`POST /auth/login`**
2. Cliquez sur **"Try it out"**
3. **Entrez** :
```json
{
  "username": "admin",
  "password": "admin123"
}
```
4. Cliquez sur **"Execute"**
5. **Vérifiez** : Réponse 200 OK avec `access_token`
6. **Copiez** le `access_token`

#### Test 6.3 : Autoriser avec le Token

1. En haut de la page Swagger, cliquez sur **"Authorize"** (icône de cadenas)
2. **Popup affichée** :
```
┌─────────────────────────────────────────┐
│  Available authorizations               │
│                                         │
│  Bearer (http, Bearer)                  │
│  Value: [___________________]           │
│                                         │
│  [Authorize]  [Close]                   │
│                                         │
└─────────────────────────────────────────┘
```
3. **Collez** le token dans le champ "Value"
4. Cliquez sur **"Authorize"**
5. Cliquez sur **"Close"**

#### Test 6.4 : Tester Endpoint Protégé

1. Trouvez **`GET /api/users`**
2. Cliquez sur **"Try it out"**
3. Cliquez sur **"Execute"**
4. **Vérifiez** : Réponse 200 OK avec liste des utilisateurs

---

## 🎯 Checklist Finale de Validation

Avant de considérer la configuration terminée, cochez chaque élément :

### Configuration Keycloak

- [ ] Realm `gestion-etudiants` créé
- [ ] Client `gestion-etudiants-client` créé
- [ ] Client : "Direct access grants" activé
- [ ] Client : Valid redirect URIs configurées
- [ ] Client : Web origins configurées
- [ ] Rôle `ADMIN` créé
- [ ] Rôle `ENSEIGNANT` créé
- [ ] Rôle `ETUDIANT` créé
- [ ] Mapper `roles` créé et configuré
- [ ] Mapper : "Multivalued" activé
- [ ] Mapper : "Add to access token" activé
- [ ] Utilisateur `admin` créé
- [ ] Utilisateur `admin` : mot de passe non temporaire
- [ ] Utilisateur `admin` : rôle ADMIN assigné
- [ ] Utilisateur `enseignant` créé
- [ ] Utilisateur `enseignant` : rôle ENSEIGNANT assigné
- [ ] Utilisateur `etudiant` créé
- [ ] Utilisateur `etudiant` : rôle ETUDIANT assigné

### Tests

- [ ] Test 1.1 : Keycloak accessible ✅
- [ ] Test 1.2 : Service-User accessible ✅
- [ ] Test 2.1 : Token admin obtenu ✅
- [ ] Test 2.2 : Rôle ADMIN dans le token ✅
- [ ] Test 2.3 : Token enseignant obtenu ✅
- [ ] Test 2.4 : Token étudiant obtenu ✅
- [ ] Test 3.1 : Enregistrement utilisateur ✅
- [ ] Test 3.2 : Utilisateur visible dans Keycloak ✅
- [ ] Test 4.1 : Login admin via service-user ✅
- [ ] Test 4.2 : Login testuser via service-user ✅
- [ ] Test 5.1 : Accès refusé sans token ✅
- [ ] Test 5.2 : Accès autorisé avec token admin ✅
- [ ] Test 5.3 : Accès refusé avec token étudiant ✅
- [ ] Test 6 : Swagger UI fonctionnel ✅

---

## 🚨 Dépannage

### Problème : "Invalid client credentials"

**Symptôme** : Erreur lors de l'obtention du token

**Causes possibles** :
1. Le client n'existe pas
2. "Direct access grants" n'est pas activé
3. Le nom du client est incorrect

**Solution** :
1. Vérifiez que le client `gestion-etudiants-client` existe
2. Vérifiez dans les paramètres du client que "Direct access grants" est **COCHÉ**

### Problème : "Invalid user credentials"

**Symptôme** : Erreur lors du login

**Causes possibles** :
1. Mot de passe incorrect
2. Mot de passe temporaire non changé
3. Utilisateur désactivé

**Solution** :
1. Vérifiez le mot de passe
2. Dans Keycloak, allez dans Users → Credentials
3. Vérifiez que "Temporary" est **DÉCOCHÉ**

### Problème : Les rôles n'apparaissent pas dans le token

**Symptôme** : Le payload JWT ne contient pas `"roles"`

**Causes possibles** :
1. Le mapper n'est pas configuré
2. Le mapper n'est pas activé
3. Les rôles ne sont pas assignés

**Solution** :
1. Vérifiez le mapper dans Client scopes → Mappers
2. Vérifiez que "Add to access token" est **COCHÉ**
3. Vérifiez que l'utilisateur a bien le rôle assigné

### Problème : 401 Unauthorized sur service-user

**Symptôme** : Service-user rejette tous les tokens

**Causes possibles** :
1. Service-user ne peut pas contacter Keycloak
2. La configuration `issuer-uri` est incorrecte

**Solution** :
```bash
# Vérifier que Keycloak est accessible depuis Docker
docker exec service-user curl http://keycloak:8080/realms/gestion-etudiants

# Vérifier les logs de service-user
docker-compose logs service-user | grep -i keycloak
```

---

## 📚 Résumé des URLs

| Service | URL | Credentials |
|---------|-----|-------------|
| Keycloak Admin | http://localhost:8180 | admin / admin |
| Keycloak Realm | http://localhost:8180/realms/gestion-etudiants | - |
| Service-User API | http://localhost:8081 | - |
| Service-User Swagger | http://localhost:8088/service-user/swagger-ui.html | - |
| Eureka Dashboard | http://localhost:8761 | - |
| Gateway | http://localhost:8088 | - |
| Keycloak DB (PostgreSQL) | localhost:5432 | keycloak / keycloak123 |

---

## 📝 Informations Importantes

### Credentials par Défaut

**Keycloak Admin** :
- Username : `admin`
- Password : `admin`

**Utilisateurs de Test** :
- Admin : `admin` / `admin123`
- Enseignant : `enseignant` / `enseign123`
- Étudiant : `etudiant` / `etud123`

**Base de Données Keycloak** :
- Database : `keycloak`
- User : `keycloak`
- Password : `keycloak123`

### Configuration Client

- **Client ID** : `gestion-etudiants-client`
- **Client Type** : OpenID Connect
- **Access Type** : Public
- **Direct Access Grants** : Enabled

### Rôles

- `ADMIN` : Accès complet
- `ENSEIGNANT` : Accès enseignant
- `ETUDIANT` : Accès étudiant

---

**✅ Configuration terminée !** Si tous les tests passent, votre système est prêt pour le développement.
