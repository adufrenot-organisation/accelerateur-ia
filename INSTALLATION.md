# Atelier APIA — Widget Grist — organisation actualisée

Instance cible : https://grist.numerique.gouv.fr/

## Contenu
- `apia_widget_grist.html` : widget View/Admin avec commentaires persistés dans Grist.
- `Elements.csv` : référentiel des éléments commentables (facultatif pour le fonctionnement du widget, utile pour l'administration).
- `Commentaires.csv` : modèle de table obligatoire, initialement vide.
- `Decisions.csv` : modèle facultatif pour formaliser les arbitrages.

## Installation
1. Dans Grist, créer un **nouveau document dédié** « Atelier APIA — Métiers ».
2. Importer `Commentaires.csv` dans une table nommée **exactement `Commentaires`**. Vérifier les identifiants de colonnes : `CommentId`, `ElementId`, `Auteur`, `Texte`, `Position`, `CreeLe`, `ModifieLe`. Définir ces colonnes comme **Texte** (notamment les dates ISO).
3. Importer éventuellement `Elements.csv` et `Decisions.csv` dans les tables du même nom.
4. Héberger `apia_widget_grist.html` sur une URL HTTPS accessible aux personnes autorisées, idéalement sur une infrastructure institutionnelle maîtrisée. Le fichier local `file://` ne peut pas être utilisé comme URL de widget partagé.
5. Dans le document Grist : **Ajouter nouveau > Ajouter un widget > Personnalisé (Custom)**, sélectionner la table `Commentaires` comme source, puis saisir l'URL HTTPS dans les options du widget.
6. Accorder **Accès complet au document** au widget : indispensable à l'API Grist pour lire/écrire les commentaires. Utiliser un document dédié sans données sensibles supplémentaires.
7. Donner les droits Grist nécessaires aux participants. Tester la publication avec un compte autorisé, puis ouvrir le document avec un second compte pour vérifier la visibilité du commentaire.

## Limites et sécurité
- Les commentaires sont partagés et persistants dans Grist, non dans `localStorage`. Le nom affiché est **déclaratif** : ce n'est pas une preuve d'identité. Les accès sont contrôlés par les permissions Grist, pas par le champ Nom.
- Le widget demande un **accès complet au document**, pas seulement à la table des commentaires. Ne pas mélanger ce document avec des données confidentielles.
- La modification/suppression se fait dans la table Grist, pas dans l'interface du widget. L'attribution à un nom ne constitue pas une autorisation à modifier uniquement ses propres lignes.
- Le rafraîchissement s'effectue sur changement de la table sélectionnée et via le bouton **Actualiser** ; ce n'est pas une messagerie temps réel garantie.
- Le script `grist-plugin-api.js` est chargé depuis `docs.getgrist.com` lors de l'ouverture ; le fichier widget est autonome en HTML/CSS/JS métier, mais **le fonctionnement collaboratif exige Grist et l'accès réseau**.
- Ne pas inclure de dossiers AIPD réels, de secrets ni de données personnelles sensibles dans les commentaires d'atelier.
- Aucun déploiement ni modification de votre instance Grist n'a été effectué automatiquement.

## Version actualisée — répartition des compétences
- **AI Software Engineer** : développement et intégration IA, RAG et agents, dédié à chaque produit.
- **LLM & Context Engineer** : prompts, contextes, traces Langfuse, évaluations, qualité, latence, tokens et coûts ; compétence mutualisée et progressivement dimensionnée.
- **DevOps Engineer** : CI/CD, déploiements, Kubernetes et exploitation applicative ; mutualisé.
- **ML Engineer** : adaptation / fine-tuning selon besoin, expertise transverse.
- **MLOps / AI Platform Engineer** : serving, modèles internes, GPU, endpoints, versionnement et exploitation des modèles ; plateforme transverse.
- Pilotage : **Product Owner / Tech Lead** ; Product Manager mutualisé selon dimensionnement.

### Mise à jour d'un document Grist existant
1. Sauvegarder le document Grist et le widget HTML précédent.
2. Remplacer le fichier HTML hébergé par celui du kit (même URL possible), puis actualiser le widget.
3. Ne **pas réimporter `Commentaires.csv`** dans la table existante : les commentaires sont conservés dans Grist.
4. `Elements.csv` est un référentiel facultatif : pour le mettre à jour, synchroniser les lignes sur **ElementId** (ne pas dupliquer la table sans nécessité).
5. Les identifiants `fusion0` à `fusion4` et `role0` à `role4` restent inchangés : les anciens commentaires restent attachés à leur hypothèse ou à leur rôle, avec un marquage « hypothèse initiale » ou « ancien découpage » lorsque nécessaire. `fusion5` et `role5` sont nouveaux.
6. Les anciennes contributions peuvent contenir des opinions relatives à l'organisation précédente : elles restent visibles comme historique et ne valent pas validation de la cible.

Le widget ne modifie pas à distance l'instance https://grist.numerique.gouv.fr/.
