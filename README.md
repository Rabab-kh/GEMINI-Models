# GEMMINI-API

Le but de ce script est de forcer un modèle de langage (qui génère d'ordinaire du texte libre et imprévisible) à se comporter comme une API logicielle stricte qui renvoie toujours la même structure de données.

# Extracteur de Reçus Fiscaux Structuré avec Gemini 

Ce code permet d'automatise l'extraction des données du ticket de caisse à partir de text brut brut et mal formatés. Il utilise le modèle de langage **Gemini 2.5 Flash** via le  SDK `google-genai` pour transformer du texte non structuré en un objet de données Python standardisé et typé.


##  Comment ça marche ? (Explication Générale)

Au lieu de laisser l'IA répondre avec des phrases libres, nous lui passons un schéma strict défini en **Pydantic**. L'API de Google force  le modèle à suivre cette structure pendant la génération des mots. Le modèle n'a pas le droit de renvoyer autre chose qu'un format JSON valide.


## Architecture du Code

Le script est articulé autour de 3 étapes clés :

1. **La Définition du Contrat de Données (`Pydantic`) :**
   On définit une classe `RecuFiscal` qui hérite de `BaseModel`. Chaque champ possède un type strict (`str`, `float`, `list`) et une méthode `Field(description="...")`. Le LLM lit ces descriptions pour comprendre précisément ce qu'il doit extraire.
   
2. **L'Appel API Contraint (`google-genai`) :**
   On utilise la fonction `client.models.generate_content` avec le modèle `gemini-2.5-flash`. On y configure le `response_mime_type="application/json"` et on injecte le schéma Pydantic dans `response_schema`.


* ** Rq : Sécurité des données :** Intégration transparente et sécurisée des clés d'API via les variables d'environnement ou le gestionnaire de secrets Google Colab.
