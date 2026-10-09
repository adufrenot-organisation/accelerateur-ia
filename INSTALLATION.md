# APIA — Discussions v5 : mentions filtrées et notifications dans le widget

## Mise à jour sans perte de commentaires

Remplacer le fichier `index.html` publié sur GitHub Pages et recharger le widget Grist. **Ne pas réimporter la table `Commentaires` existante**. Vérifier ses colonnes techniques : `CommentId`, `ElementId`, `Auteur`, `AuteurEmail`, `Texte`, `Position`, `CreeLe`, `ModifieLe`, `ParentId` (ParentId : Texte). Les nouveaux commentaires enregistrent `AuteurEmail` pour permettre de reconnaître leurs auteurs.

## Mentions @

La saisie de `@` puis de lettres filtre immédiatement les utilisateurs par nom ou email, sans distinction d'accents ni de casse. Sélectionner un résultat pour insérer un jeton `@Prenom_Nom` ; ce jeton est associé à l'email de l'annuaire lors de la publication. Les utilisateurs proviennent de la table `AnnuaireUtilisateurs` (`Nom`, `Email`, `Actif`) : elle doit être **synchronisée avec les personnes ayant réellement accès au document**, via l'API de permissions Grist ou une procédure d'administration. Le widget seul ne peut pas récupérer automatiquement toute cette liste. Le CSV livré ne contient aucun utilisateur réel.

## Notifications internes

Importer la nouvelle table `NotificationsAvis` si elle n'existe pas, avec les colonnes `DestinataireEmail`, `AuteurEmail`, `CommentId`, `ElementId`, `Lien`, `Statut`. À la publication d'un commentaire contenant une mention valide, ou d'une réponse à un avis, une ligne est créée pour chaque destinataire. Le bouton cloche 🔔 du widget affiche le nombre de notifications non lues, leur liste et permet d'ouvrir le fil concerné, avec surlignage du commentaire. La notification passe à `Lue` au clic. **Aucun email, aucun service d'envoi**. Les notifications apparaissent lorsque le widget est ouvert ou actualisé ; elles ne sont pas des notifications globales de l'interface Grist.

## Droits et sécurité

Le widget utilise `IdentiteWidget` pour identifier la session (colonnes déclenchées `Email=user.Email`, `Nom=user.Name`, selon la configuration de Grist). Les boutons Modifier et Supprimer n'apparaissent que pour les avis dont `AuteurEmail` correspond à l'utilisateur courant. Les commentaires historiques sans email doivent être attribués après vérification par un administrateur. **Ces contrôles d'interface ne sont pas une garantie de sécurité** : définir des règles ACL Grist sur `Commentaires` et `NotificationsAvis` afin que les utilisateurs ne puissent modifier/supprimer que leurs propres lignes et ne lire que leurs propres notifications. Vérifier que les règles autorisent les créations attendues et tester avec deux comptes. L'usage d'un widget en accès complet dans un document partagé demande une revue de sécurité des ACL.

## Limites

Le lien dans `NotificationsAvis.Lien` pointe vers l'URL du widget et son ancre ; selon la manière dont GitHub Pages est intégré dans Grist, ouvrir cette URL hors Grist ne donnera pas accès aux données. **La navigation fiable est celle de la cloche à l'intérieur du widget**. Les utilisateurs doivent ouvrir le document pour voir leurs notifications. Cette version n'a pas été testée sur votre instance Grist.
