# 🌿 Identification des Plantes Médicinales par Application Mobile

> Application mobile intelligente destinée aux tradipraticiens pour l'identification automatique des plantes médicinales à partir de photos, avec accès hors ligne aux fiches ethnobotaniques.

---

## 📋 Table des matières

- [Description du projet](#-description-du-projet)
- [Fonctionnalités](#-fonctionnalités)
- [Architecture technique](#-architecture-technique)
- [Pipeline IA](#-pipeline-ia)
- [Stack technologique](#-stack-technologique)
- [Installation](#-installation)
- [Utilisation](#-utilisation)
- [Structure du projet](#-structure-du-projet)
- [Évaluation du modèle](#-évaluation-du-modèle)
- [Auteur](#-auteur)

---

## 📖 Description du projet

Ce projet présente une application mobile innovante permettant l'**identification automatique de plantes médicinales** à partir de photos de feuilles ou d'écorces. 

L'application utilise des techniques avancées de **Deep Learning** (Mask RCNN pour la segmentation + InceptionResNetV2 pour la classification) exécutées entièrement **hors ligne** sur l'appareil mobile. Elle est destinée principalement aux **tradipraticiens** (médecins traditionnels utilisant les plantes médicinales) opérant dans des zones à connectivité limitée.

### Objectifs
- Identifier automatiquement une plante médicinale depuis une photo
- Fournir la fiche ethnobotanique complète (usages, toxicité, remèdes)
- Fonctionner **sans connexion internet**
- Être accessible sur smartphones milieu de gamme

---

## ✨ Fonctionnalités

| Fonctionnalité | Description |
|---|---|
| 📸 **Identification par photo** | Prendre une photo ou importer depuis la galerie |
| 🔌 **Mode hors ligne** | Toutes les fonctions disponibles sans internet |
| 🗂️ **Fiche ethnobotanique** | Nom scientifique, usages médicinaux, origine géographique |
| ☠️ **Avertissement toxicité** | Signalement des plantes dangereuses et parties toxiques |
| 🧪 **Remèdes naturels** | Recettes et préparations traditionnelles |
| 📜 **Historique** | Journal de toutes les identifications précédentes |
| ⭐ **Favoris** | Sauvegarder les plantes consultées fréquemment |
| 👤 **Compte utilisateur** | Inscription et connexion personnalisée |

---

## 🏗️ Architecture technique

```
📸 Photo (mobile)
       ↓
🔲 Mask RCNN
   Segmentation → isolation de la feuille/écorce
       ↓
✂️  Crop automatique de la zone segmentée
       ↓
🧠 InceptionResNetV2 (TFLite)
   Classification → identification de l'espèce
       ↓
🗃️  Base SQLite + plants_data.json
   Récupération de la fiche ethnobotanique
       ↓
📱 Affichage Flutter
   Résultat + informations complètes
```

---

## 🤖 Pipeline IA

### 1. Segmentation — Mask RCNN
- Détecte et **isole précisément** la région de la feuille ou de l'écorce dans l'image
- Élimine le fond (sol, table, autres objets)
- Basé sur les poids pré-entraînés **COCO**

### 2. Classification — InceptionResNetV2
- Architecture pré-entraînée sur **ImageNet**
- **Fine-tuning** sur un dataset de plantes médicinales (Kaggle)
- Images redimensionnées en **224×224 pixels**
- Exporté en format **TensorFlow Lite (.tflite)** pour inférence mobile

### 3. Évaluation
- **Top-1 accuracy** : précision de la première proposition
- **Top-3 accuracy** : la bonne réponse dans les 3 premières
- **Temps d'inférence** : mesuré sur smartphone milieu de gamme

---

## 🛠️ Stack technologique

### Machine Learning (Python)
- `TensorFlow / Keras` — entraînement du modèle
- `Mask RCNN` — segmentation d'instance
- `OpenCV` — traitement d'images
- `Jupyter Notebook` — expérimentation et entraînement
- `TensorFlow Lite` — déploiement mobile

### Application Mobile
- `Flutter (Dart)` — framework mobile cross-platform
- `TFLite Flutter` — inférence du modèle sur mobile
- `SQLite (sqflite)` — base de données locale utilisateur
- `CameraX` — capture d'image en temps réel

### Données
- `plants_data.json` — fiches ethnobotaniques des plantes
- `SQLite` — historique, favoris, comptes utilisateurs
- Dataset : [Indian Medicinal Leaf Dataset (Kaggle)](https://www.kaggle.com/)

---

## 🚀 Installation

### Prérequis
- [Flutter SDK](https://flutter.dev/docs/get-started/install) installé
- [Android Studio](https://developer.android.com/studio) ou VS Code avec plugin Flutter
- Un émulateur Android ou un appareil physique

### Étapes

```bash
# 1. Cloner le repository
git clone https://github.com/yassineSandalMahamat/identification-plantes-medicinales.git
cd identification-plantes-medicinales

# 2. Aller dans le dossier Flutter
cd flutter_app

# 3. Installer les dépendances
flutter pub get

# 4. Vérifier que les assets sont en place
# → flutter_app/assets/models/medicinal_plants_model.tflite
# → flutter_app/assets/data/plants_data.json

# 5. Lancer l'application
flutter run
```

---

## 📱 Utilisation

1. **Lancer** l'application sur votre smartphone
2. **Se connecter** ou créer un compte
3. **Prendre une photo** d'une feuille ou d'une écorce
4. **Attendre** quelques secondes (traitement hors ligne)
5. **Consulter** le résultat : nom, usages, toxicité, remèdes
6. **Sauvegarder** dans les favoris si besoin

---

## 📁 Structure du projet

```
identification-plantes-medicinales/
│
├── 📁 models/
│   ├── medicinal_plants_model.ipynb     # Notebook entraînement
│   └── medicinal_plants_model.tflite    # Modèle TFLite (mobile)
│
├── 📁 data/
│   ├── plants_data.json                 # Fiches ethnobotaniques
│   └── kaggle_dataset_link.txt          # Lien dataset d'entraînement
│
├── 📁 flutter_app/
│   ├── lib/
│   │   ├── main.dart                    # Point d'entrée
│   │   ├── screens/                     # Écrans de l'application
│   │   ├── models/                      # Modèles de données
│   │   ├── services/                    # Services (IA, DB)
│   │   ├── repositories/               # Accès aux données
│   │   └── widgets/                     # Composants UI
│   ├── assets/
│   │   ├── models/                      # Modèle .tflite
│   │   └── data/                        # plants_data.json
│   └── pubspec.yaml
│
└── README.md
```

---

## 📊 Évaluation du modèle

| Métrique | Valeur |
|---|---|
| Top-1 Accuracy | ~85% |
| Top-3 Accuracy | ~94% |
| Temps d'inférence (mobile) | < 2 secondes |
| Nombre d'espèces | 22 classes |
| Taille du modèle (.tflite) | ~50 MB |

---

## 👤 Auteur

**Yassine Sandal Mahamat**  
🔗 GitHub : [@yassineSandalMahamat](https://github.com/yassineSandalMahamat)

---

## 📄 Licence

Ce projet est à usage académique. Il s'inspire des travaux open source de :
- [khadija-oubaha/medicinal-plants-app](https://github.com/khadija-oubaha/medicinal-plants-app)
- [AarohiSingla/Plant-Disease-Detection-Using-Mask-R-CNN](https://github.com/AarohiSingla/Plant-Disease-Detection-Using-Mask-R-CNN)

---

*Projet réalisé dans le cadre d'un travail académique sur l'Intelligence Artificielle appliquée à la botanique médicinale.*
