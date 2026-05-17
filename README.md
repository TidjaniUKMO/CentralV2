# BétonPro SCADA v2.0
## Système de Supervision Industrielle - Centrale à Béton

### Schneider TM200CE24R · Modbus TCP · PostgreSQL 18.3 · React/TypeScript

---

## 🏭 PRÉSENTATION

BétonPro SCADA est un système complet de contrôle et supervision pour centrale à béton.
Il intègre un frontend SCADA moderne, une communication Modbus TCP vers l'automate Schneider,
une base PostgreSQL et un simulateur PLC virtuel pour les tests.

---

## 🗂️ STRUCTURE DU PROJET

```
betonpro-scada/
├── src/
│   ├── App.tsx                    # Point d'entrée application
│   ├── main.tsx                   # Bootstrap React
│   ├── index.css                  # Styles globaux SCADA
│   ├── types/
│   │   └── index.ts               # Types TypeScript industriels complets
│   ├── store/
│   │   └── index.ts               # Store Zustand centralisé
│   ├── i18n/
│   │   └── index.ts               # Traductions FR/EN/AR
│   ├── services/
│   │   └── simulator.ts           # Simulateur PLC virtuel
│   ├── components/
│   │   └── Layout.tsx             # Layout navigation SCADA
│   ├── pages/
│   │   ├── LoginPage.tsx          # Authentification
│   │   ├── DashboardPage.tsx      # Tableau de bord temps réel
│   │   ├── SynopticPage.tsx       # Synoptique animé centrale
│   │   ├── ProductionPage.tsx     # Gestion production/gâchées
│   │   ├── RecipesPage.tsx        # Module recettes CRUD
│   │   ├── AlarmsPage.tsx         # Système alarmes industrielles
│   │   ├── ClientsPage.tsx        # Clients/Camions/Chauffeurs
│   │   ├── ParametersPage.tsx     # Temporisations + Impulsions
│   │   ├── CommunicationPage.tsx  # Modbus TCP monitoring
│   │   ├── UsersPage.tsx          # Gestion utilisateurs/rôles
│   │   ├── MaintenancePage.tsx    # Commandes manuelles
│   │   └── DeveloperPage.tsx      # Config développeur (masquable)
│   └── docs/
│       ├── SQL_POSTGRESQL.sql     # Script base de données complet
│       └── PLC_LADDER_TM200CE24R.md # Documentation programme API
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
└── README.md
```

---

## 🚀 INSTALLATION ET DÉMARRAGE

### Prérequis
- Node.js 18+
- npm 9+
- PostgreSQL 18.3 (pour backend)

### Installation
```bash
git clone https://github.com/your-org/betonpro-scada
cd betonpro-scada
npm install
npm run dev
```

### Build production
```bash
npm run build
# → dist/index.html (fichier unique, deployable)
```

---

## 👤 COMPTES UTILISATEURS

| Utilisateur  | Mot de passe | Rôle          | Accès |
|-------------|-------------|---------------|-------|
| admin       | admin123    | Administrateur | Tout  |
| operateur   | oper123     | Opérateur      | Production, Alarmes, Synoptique |
| maintenance | maint123    | Maintenance    | Paramètres, Maintenance, Lecture |

---

## 🔌 COMMUNICATION MODBUS TCP

### Configuration automate
```
Automate: SCHNEIDER TM200CE24R
IP: 192.168.1.100
Port: 502
Unit ID: 1
Timeout: 3000 ms
Refresh: 200 ms
```

### Mapping principal
| Adresse Modbus | Type | Signal | Description |
|---|---|---|---|
| 0 | Coil | VER_T1_OUV | Vérin trémie 1 ouverture |
| 1 | Coil | VER_T1_FER | Vérin trémie 1 fermeture |
| 8 | Coil | VIS_CIM1 | Vis doseuse silo 1 |
| 10 | Coil | POMPE_EAU1 | Pompe eau 1 |
| 100 | HR | POIDS_AGR | Poids balance agrégats × 0.1 kg |
| 102 | HR | COURANT_MAL | Courant malaxeur × 0.1 A |
| 120 | HR | ETAT_SEQ | État séquence automatique |

---

## 🏗️ BASE DE DONNÉES POSTGRESQL

```sql
-- Création base
psql -U postgres -f src/docs/SQL_POSTGRESQL.sql

-- Connexion
psql -U betonpro_app -d betonpro
```

### Tables principales
- `scada.users` — Utilisateurs et authentification
- `scada.plant_config` — Configuration centrale
- `scada.hoppers` — Trémies agrégats (dynamiques)
- `scada.silos` — Silos ciment (dynamiques)
- `scada.recipes` — Recettes béton
- `scada.production_batches` — Historique gâchées
- `scada.alarms` — Journal alarmes
- `scada.sensor_data` — Archive capteurs (partitionné)
- `scada.modbus_config` — Configuration Modbus
- `scada.timers` — Temporisations paramétrables
- `scada.impulse_config` — Configuration impulsions

---

## ⚙️ FONCTIONNALITÉS

### 1. Dashboard Temps Réel
- KPIs production du jour
- Graphiques tendances
- État équipements
- Niveaux trémies et silos
- Alarmes actives

### 2. Synoptique Animé
- Visualisation complète centrale
- Trémies avec niveaux
- Balance agrégats
- Skip / Tapis incliné
- Silos ciment
- Réservoirs eau/adjuvant
- Malaxeur avec animation

### 3. Production
- Sélection recette + client + camion
- Validation cohérence recette vs config
- Suivi temps réel séquence (7 étapes)
- Historique complet avec recherche

### 4. Recettes
- CRUD complet
- Validation anti-doublons
- Détail agrégats par trémie
- Détail ciment par silo
- Duplication rapide
- Historique utilisation

### 5. Alarmes
- Acquittement individuel/global
- 3 niveaux priorité (critique/avert./info)
- Journal complet
- Génération test simulateur

### 6. Paramètres (Exploitant)
- 10 temporisations paramétrables
- Gestion impulsions vérins:
  - Seuil déclenchement (%)
  - Durée ON (ms)
  - Durée OFF (ms)
  - Visualisation signal

### 7. Configuration Développeur (Admin)
- Nombre trémies (jusqu'à 8)
- Mode déchargement (vérin/tapis)
- Présence vibreur
- Silos ciment (jusqu'à 4)
- Réservoirs eau/adjuvant
- Type skip/tapis incliné
- Mapping Modbus complet
- **Masquable pour exploitant final**

### 8. Communication Modbus
- Monitoring connexion PLC
- Tableau mapping complet (27 tags)
- Journal erreurs
- Test connexion
- Configuration IP/port/timeout

### 9. Clients / Camions / Chauffeurs
- CRUD complet avec recherche
- Type client (entreprise/particulier)
- Liaison camion ↔ client
- Liaison chauffeur ↔ client

### 10. Utilisateurs
- 3 rôles: admin / opérateur / maintenance
- Permissions modulaires (lire/écrire/exécuter)
- Audit last login

### 11. Simulateur PLC
- Simulation valeurs temps réel (500ms)
- Scénario production automatique
- Vitesse ajustable (×1 à ×10)
- Simulation bruit capteurs

### 12. Multilingue
- Français (défaut)
- English
- عربي (RTL)

### 13. Thèmes
- Sombre (défaut) — Style SCADA industriel
- Clair — Bureautique

---

## 🔧 PROGRAMME PLC (TM200CE24R)

Voir documentation complète: `src/docs/PLC_LADDER_TM200CE24R.md`

Sections:
- INIT — Initialisation démarrage
- SÉCURITÉS — Interlocks (EA, protections)
- WATCHDOG — Surveillance communication SCADA
- MODES — Sélection AUTO/MANU
- SÉQUENCE_AUTO — Machine d'états 7 étapes
- VÉRINS — Commande et mode impulsionnel
- MOTEURS — Contacteurs et protections
- SKIP — Benne montante avec sécurités
- MALAXEUR — Moteur et temporisations
- ALARMES — Génération et gestion
- COMMUNICATION — Modbus TCP registres

---

## 📊 DIAGRAMMES ARCHITECTURE

```
┌─────────────────────────────────────────────────────────┐
│                    ARCHITECTURE GLOBALE                  │
│                                                         │
│  ┌──────────────────┐         ┌──────────────────────┐  │
│  │   NAVIGATEUR WEB │◄───────►│   REACT FRONTEND     │  │
│  │  (SCADA Client)  │  HTTP   │   Tailwind + Zustand │  │
│  └──────────────────┘         └──────────┬───────────┘  │
│                                          │               │
│  ┌──────────────────┐         ┌──────────▼───────────┐  │
│  │   POSTGRESQL 18  │◄───────►│   NODE.JS BACKEND    │  │
│  │   Base données   │  SQL    │   API REST + WS      │  │
│  └──────────────────┘         └──────────┬───────────┘  │
│                                          │Modbus TCP     │
│  ┌──────────────────┐                   │               │
│  │   SIMULATEUR     │    ┌──────────────▼───────────┐  │
│  │   PLC Virtuel    │    │   SCHNEIDER TM200CE24R   │  │
│  │  (sans automate) │    │   192.168.1.100:502      │  │
│  └──────────────────┘    └──────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

---

## 📋 DONNÉES D'EXEMPLE

### Recette B25 (Béton C25/30)
```json
{
  "code": "B25",
  "name": "Béton B25",
  "aggregates": [
    {"hopper": "Sable 0/4", "weight_kg": 700},
    {"hopper": "Gravier 4/8", "weight_kg": 300},
    {"hopper": "Gravier 8/16", "weight_kg": 600}
  ],
  "cement": [{"silo": "CEM I 52.5", "weight_kg": 350}],
  "water_liters": 175,
  "adjuvant_liters": 3.5,
  "mixing_time_s": 90,
  "total_kg": 2128.5
}
```

---

## 📜 LICENCE

© 2024 BétonPro Engineering — Système industriel professionnel
Usage réservé aux centrales à béton équipées de Schneider TM200CE24R
