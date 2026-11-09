# 🏗️ Créer une application Streamlit pas à pas

(Open With Markdown Preview not Markdown Preview Enhanced - Video not supported on Enhanced)

---

## **Étape 1 : Créer le projet et le fichier principal**

1. **Créer un dossier pour l’application :**

```bash
mkdir mon_app_streamlit
cd mon_app_streamlit
```

2. **Créer le fichier principal `streamlit.py` :**

```bash
touch streamlit.py
```

> Ce fichier contiendra tout le code de l’application Streamlit.

Dès maintenant, vous pouvez lancer la commande suivante pour voir les modifications en temps réel (n'oubliez pas d'activer l'option Always Rerun )

```bash
streamlit run streamlit.py
```

3. **Créer le fichier requirements.txt :**

Dans ce tuto, nous allons créer un fichier `requirements.txt` avec les valeurs suivantes:

```
streamlit
pandas
plotly
```

Le fichier `requirements.txt` sert à lister toutes les librairies Python dont l'application a besoin, pour que Streamlit Cloud (ou tout autre serveur) puisse les installer automatiquement et exécuter  l'app sans erreur.

---

## **Étape 2 : Configurer la page**

Dans `streamlit.py`, ajouter la configuration de base de la page au tout début du fichier :

```python
import streamlit as st
import pandas as pd
import plotly.express as px


st.set_page_config(
    page_title="Dashboard Ventes",
    page_icon="📊",
    layout="wide",
    initial_sidebar_state="expanded"
)
```

* **page_title** : titre de l’onglet du navigateur
* **page_icon** : emoji ou image affichée dans l’onglet 
* **layout** : `'wide'` pour pleine largeur ou `'centered'` pour centré
* **initial_sidebar_state** : état initial de la sidebar (`'expanded'`, `'collapsed'`, `'auto'`)

> ⚠️ Cette commande doit être la **première dans le fichier**, avant tout autre composant Streamlit.



---

## **Étape 3 : Ajouter un menu multi-pages**

Pour créer plusieurs sections dans l’application, on peut utiliser la sidebar :

```python
page = st.sidebar.selectbox(
    "Navigation",
    ["Accueil", "Analyse", "Graphiques"]
)
```
* La variable `page` permet de déterminer quel contenu afficher.
* `st.sidebar.selectbox()` crée un menu déroulant dans la barre latérale.

On peut aussi remplacer le menu déroulant par l'affichage en liste verticale

```python
page = st.sidebar.radio(
    "Navigation",
    ["Accueil", "Analyse", "Graphiques"]
)
```

---

## **Étape 4 : Créer la page d'accueil**

```python
if page == "Accueil":
    st.title("Bienvenue sur le Dashboard")

    # Texte introductif
    st.write(
        "Ceci est la page d'accueil avec un résumé des informations principales."
    )

    # Lien vers la documentation officielle Streamlit
    st.markdown(
        "Pour en savoir plus sur Streamlit, visitez [la documentation](https://docs.streamlit.io/library)."
    )

    # Ajouter une image depuis une URL publique (version moderne)
    st.image(
        "https://streamlit.io/images/brand/streamlit-logo-secondary-colormark-darktext.png",
        caption="Logo Streamlit",
        use_container_width=True
    )

    # Ajouter un résumé ou indicateur simple
    st.metric(label="Nombre de visiteurs", value="1 234", delta="+12%")
    
    # Texte explicatif supplémentaire
    st.write(
        "Ce dashboard est conçu pour vous fournir une vue d'ensemble rapide de vos données et analyses."
    )
```

* **`if page == "Accueil"`**
  Vérifie si l’utilisateur a sélectionné la page “Accueil”. Tout le code suivant ne s’exécute que pour cette page, ce qui permet de créer plusieurs pages dans le dashboard.

* **`st.title()`**
  Affiche le titre principal de la page. Sert à introduire clairement le contenu et attirer l’attention de l’utilisateur.

* **`st.write()`**
  Affiche un texte simple ici il est utilisé pour donner une introduction ou un résumé du contenu de la page, aidant l’utilisateur à comprendre rapidement ce qu’il peut attendre.

* **`st.markdown("Pour en savoir plus sur Streamlit, visitez [la documentation](https://docs.streamlit.io/library).")`**
  Permet d’utiliser du markdown pour insérer un lien cliquable vers la documentation officielle de Streamlit, offrant une ressource externe pour approfondir les connaissances.

* **`st.image("URL", caption="Logo Streamlit", use_container_width=True)`**
  Affiche une image , l'image peut être local ou comme dans notre exemple provenir d'une URL publique. La légende explique ce que représente l’image et `use_container_width=True` adapte automatiquement sa largeur à l’espace disponible pour un rendu propre et responsive.

* **`st.metric(label="Nombre de visiteurs", value="1 234", delta="+12%")`**
  Affiche un indicateur clé (KPI) : un chiffre important avec une variation. Cela permet de montrer rapidement des données essentielles directement sur la page d’accueil.

* **`st.write("Ce dashboard est conçu pour vous fournir une vue d'ensemble rapide de vos données et analyses.")`**
  Affiche un texte explicatif final pour rappeler l’objectif du dashboard et guider l’utilisateur dans sa navigation.

Attention, l'indentation est importante et doit bien être respectée.

## **Étape 5 : Page Analyse**

Nous allons ajouter des widgets. Les widgets permettent à l’utilisateur d’interagir avec l’application :

```python 
elif page == "Analyse":
    st.title("Analyse des données")
    st.write("Cette page contient des tableaux et statistiques des ventes.")
    
    # Widgets interactifs
    option = st.selectbox(
        "Choisissez une catégorie",
        ["Catégorie A", "Catégorie B", "Catégorie C"]
    )
    valeur = st.slider(
        "Sélectionner une valeur", 0, 100, 50
    )
    
    st.write(f"Catégorie sélectionnée : {option}")
    st.write(f"Valeur choisie : {valeur}")

    # Sélecteurs de date
    st.date_input("Date de début")
    st.date_input("Date de fin")
    
    # Exemple de tableau
    data = pd.DataFrame({
        "Produit": ["Produit 1", "Produit 2", "Produit 3"],
        "Ventes": [100, 200, 150]
    })
    st.table(data)
    
    # Bouton pour télécharger le tableau
    st.download_button(
        label="Télécharger les données",
        data=data.to_csv(index=False).encode('utf-8'),
        file_name='ventes.csv',
        mime='text/csv'
    )

    # Lien vers la documentation
    st.link_button(
        "Voir la documentation du link_button",
        "https://docs.streamlit.io/develop/api-reference/widgets/st.link_button"
    )
```


* **`elif page == "Analyse"`**
  Vérifie si l’utilisateur a sélectionné la page “Analyse”. Tout le code qui suit s’exécute uniquement sur cette page.

* **`st.title("Analyse des données")`**
  Affiche le titre principal de la page pour introduire le contenu.

* **`st.write("Cette page contient des tableaux et statistiques des ventes.")`**
  Fournit un texte introductif expliquant l’objectif de la page.

* **`st.selectbox()`**
  Permet de choisir une catégorie parmi plusieurs options (A, B ou C).

* **`st.slider()`**
  Permet de sélectionner une valeur numérique minimale pour filtrer ou explorer les données.

* **`st.write(f"Catégorie sélectionnée : {option}")`**
  Affiche la catégorie choisie par l’utilisateur pour plus de clarté.

* **`st.write(f"Valeur choisie : {valeur}")`**
  Affiche la valeur choisie par l’utilisateur via le slider.

* **`st.date_input()`**
  Permet à l’utilisateur de sélectionner une date de début et une date de fin.

* **`st.table()`**
  Affiche le tableau des produits et de leurs ventes de manière statique.

* **`st.download_button()`**
  Permet à l’utilisateur de **télécharger le tableau** au format CSV.

  * `data=data.to_csv(index=False).encode('utf-8')` : convertit le DataFrame en CSV encodé UTF-8
  * `file_name='ventes.csv'` : nom du fichier téléchargé
  * `mime='text/csv'` : type MIME du fichier

* **`st.link_button()`**
  Affiche un bouton cliquable qui redirige vers la documentation officielle du widget `link_button`.

---

## **Étape 6 : Page Graphiques**

```python
elif page == "Graphiques":
    st.title("Visualisations interactives")
    
    # Dataset simulé
    df_data = pd.DataFrame({
        "Jour": ["Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi"],
        "Ventes": [100, 150, 200, 170, 220],
        "Commandes": [15, 60, 10, 36, 87],
        "Clients": [5, 8, 12, 7, 10]
    })
    
    # Sélection de la variable à visualiser
    variable = st.selectbox(
        "Choisissez la variable à visualiser", 
        ["Ventes", "Commandes", "Clients"]
    )
    
    # Colonnes pour graphiques côte à côte
    col1, col2 = st.columns(2)
    
    # Graphique linéaire dynamique
    col1.header(f"Évolution de {variable}")
    col1.line_chart(df_data.set_index("Jour")[variable])
    
    # Graphique à barres dynamique
    col2.header(f"Répartition de {variable} par jour")
    col2.bar_chart(df_data.set_index("Jour")[variable])
    
    # Graphique Plotly interactif
    st.subheader(f"Scatter Plot interactif de {variable}")
    fig = px.scatter(
        df_data,
        x="Jour",
        y=variable,
        size=variable,
        color=variable,
        color_continuous_scale=px.colors.sequential.Viridis,
        size_max=50,
        title=f"Scatter Plot de {variable} par jour"
    )
    st.plotly_chart(fig)
    
    # Expander pour détails supplémentaires
    with st.expander("Détails supplémentaires"):
        st.write("Données complètes utilisées pour les visualisations :")
        st.dataframe(df_data)
```

* **`elif page == "Graphiques"`**
  Vérifie si l’utilisateur a sélectionné la page “Graphiques”. Tout le code qui suit s’exécute uniquement sur cette page.

* **`st.title("Visualisations interactives")`**
  Affiche le titre principal pour introduire les différentes visualisations.

* **Création du dataset (`df_data`)**
  Contient les jours de la semaine et trois variables : `Ventes`, `Commandes`, `Clients`.
  Ce dataset alimente tous les graphiques de la page.

* **`st.selectbox()`**
  Permet à l’utilisateur de choisir la variable à visualiser (`Ventes`, `Commandes`, `Clients`).
  La sélection impacte tous les graphiques de la page.

* **`st.columns(2)`**
  Crée **deux colonnes côte à côte** :

  * `col1` : contient le graphique linéaire
  * `col2` : contient le graphique à barres
    Cela permet de présenter plusieurs graphiques sur la même ligne, ce qui rend la mise en page plus claire et lisible.

* **Graphique linéaire (`col1.line_chart`)**
  Montre l’évolution de la variable sélectionnée au fil des jours.
  L’axe X correspond aux jours de la semaine grâce à `df_data.set_index("Jour")[variable]`.

* **Graphique à barres (`col2.bar_chart`)**
  Montre la répartition ou la comparaison de la variable sélectionnée par jour.
  Dynamique : change automatiquement selon la variable choisie dans le `selectbox`.

* **Graphique Plotly (`px.scatter`)**
  Scatter Plot interactif de la variable sélectionnée :

  * Taille proportionnelle à la valeur de la variable
  * Couleur proportionnelle à la valeur
  * Titre dynamique indiquant la variable choisie
    Permet d’explorer visuellement les valeurs de manière plus interactive que les graphiques Streamlit natifs.

* **`st.expander()`**
  Permet de dérouler un bloc lorsque l'on clique dessus. Ici il contient les données complètes utilisées pour les visualisations, visibles à la demande.
  Permet à l’utilisateur de consulter facilement le dataset complet sans surcharger la page.


---

## **Étape 7 : Personnaliser la page**

Créer le dossier `.streamlit` et le fichier `config.toml` :

```python
[theme] 
primaryColor="#8B3A3A" 
backgroundColor="#FDF6F0" 
secondaryBackgroundColor="#F5EDE0" 
textColor="#2D2D2D" 
font="serif"

[global]
gatherUsageStats = true

[browser]
serverHeadless = false
```

* **`[theme]`**
  Définit l’apparence générale de l'application Streamlit :

  * `primaryColor = "#8B3A3A"` → couleur principale pour les boutons et éléments interactifs
  * `backgroundColor = "#FDF6F0"` → couleur de fond de la page principale
  * `secondaryBackgroundColor = "#F5EDE0"` → couleur de fond pour les encadrés ou sections secondaires
  * `textColor = "#2D2D2D"` → couleur du texte
  * `font = "serif"` → type de police utilisé pour tout le texte

* **`[global]`**
  Paramètres généraux de Streamlit :

  * `gatherUsageStats = true` → active l’envoi de statistiques anonymes à Streamlit (utile pour améliorer le produit)

* **`[browser]`**
  Paramètres liés à l’ouverture du navigateur :

  * `serverHeadless = false` → le navigateur s’ouvre automatiquement quand on lance l'application


**Toute modification du fichier `config.toml` ne sera prise en compte que lorsque l'on relance Streamlit.**

Il faut :

1. **Arrêter le serveur Streamlit** s’il tourne (Ctrl+C dans le terminal).
2. **Sauvegarder le fichier `config.toml`**.
3. **Relancer l'app** 

> Streamlit va alors lire la nouvelle configuration, appliquer le thème, la police, l’ouverture automatique du navigateur, etc.


---



## **Étape 7 : Deployer l'application depuis le dossier local**


### **1️⃣ Se placer dans le dossier projet**

Dans le terminal de VS Code :

```bash
cd chemin/vers/mon_app_streamlit
```

👉  Il faut être dans le dossier où se trouve `streamlit.py`

---

### **2️⃣ Initialiser un dépôt Git**

```bash
git init
```

👉 Cela transforme le dossier en dépôt Git **local**

---


### **3️⃣ Ajouter les fichiers**

```bash
git add .
```

👉 Ajoute tous les fichiers (streamlit.py, config.toml...)

---

### **4️⃣ Faire un premier commit**

```bash
git commit -m "Initial commit - app Streamlit"
```

👉 Sauvegarde une version du projet en local

---

### **5️⃣ Créer le repo distant sur GitHub**

* Aller sur GitHub
* Cliquer sur **New repository**
* Donner un nom, exemple "my_first_streamlit"
* Laisser la visibilité en public 
* **Ne pas ajouter de README** (important pour éviter les conflits)
* Ne pas ajouter de licence ou de .gitignore

---

### **6️⃣ Lier le projet local au repo GitHub**

Dans le terminal :

```bash
git remote add origin https://github.com/YourUsername/my_first_streamlit.git
git branch -M main
git push -u origin main
```

👉 Le code est maintenant en ligne

---

### **7️⃣ Déployer sur Streamlit Community Cloud**


Depuis le localhost, cliquez sur Deploy et suivez la vidéo suivante :

<div style="padding:64.97% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/1178016925?title=0&amp;byline=0&amp;portrait=0&amp;badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write; encrypted-media; web-share" referrerpolicy="strict-origin-when-cross-origin" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="deploy"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>


Le repo est automatiquement séléctionné car on part du projet local comme base de déploiement.

Une autre méthode est possible :
* Aller sur [Streamlit Cloud](https://streamlit.io/cloud)
* Cliquer sur **Deploying? Try:Free**
* Connectez vous depuis GitHub et cliquez sur Create App
* Choississez **Deploy a public app from GitHub**
* Sélectionner ensuite:

  * le repo GitHub
  * branche : `main`
  * fichier : `streamlit.py`

👉 Cliquer sur **Deploy**

---

### **Résultat**

* Une URL publique est générée
* L'app est accessible depuis n’importe où

---

### ❗ Important : localhost ≠ déploiement


* **localhost** → app visible uniquement sur l'ordinateur
* **Streamlit Cloud** → app accessible sur Internet



---

### 🧹 **Stopper l'app locale et supprimer l’app clooud (fin de démo)**

* Sur le terminal où vous avez lancé le streamlit cliquez sur `Ctrl + C` pour  mettre fin au hosting de l'application locale.

* Aller sur Streamlit Cloud
* Settings → **Delete app**



<div style="padding:64.97% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/1178017210?title=0&amp;byline=0&amp;portrait=0&amp;badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write; encrypted-media; web-share" referrerpolicy="strict-origin-when-cross-origin" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="delete"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>



 Nous sommes arrivés au bout de ce tuto, n'hésitez pas à consulter les différents [composants](https://streamlit.io/components) pour tester de nouvelles choses !
