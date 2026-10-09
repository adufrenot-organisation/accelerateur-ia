# APIA — Widget Grist Discussions (v2)

## Mise à jour sans perdre les avis

1. **Ne pas réimporter** la table `Commentaires` si elle contient déjà des avis. Ajouter seulement deux colonnes **Texte** `AuteurEmail` et `ParentId` (vides pour les anciens enregistrements). Conserver toutes les autres colonnes existantes.
2. Dans `Commentaires`, configurer la colonne `AuteurEmail` avec une **formule déclenchée** `user.Email` appliquée **à la création des enregistrements uniquement**. Cette colonne doit être alimentée par Grist, jamais par le widget. Les anciens avis sans email vérifié restent lisibles mais ne sont pas modifiables depuis le widget.
3. Créer une table technique `IdentiteWidget` avec `Jeton` (Texte), `Email` (Texte), `Nom` (Texte). Configurer des **formules déclenchées** sur `Email` : `user.Email` et `Nom` : `user.Name`, appliquées **à la création**. Vérifier que les champs sont bien alimentés à la création, puis tester le widget.
4. **Configurer des règles de contrôle d'accès (ACL) Grist** sur `Commentaires` : lecture et création autorisées aux contributeurs ; mise à jour et suppression autorisées uniquement si `rec.AuteurEmail == user.Email` ; refuser la modification directe de `AuteurEmail` et des champs d'identité/ownership, et empêcher de créer des enregistrements avec une identité falsifiée. Les détails des règles dépendent des règles ACL existantes du document. **Sans ACL, la restriction “mes avis seulement” est uniquement dans l'interface et n'est pas une garantie de sécurité.**
5. La table technique `IdentiteWidget` est utilisée pour découvrir l'identité de la session. La création/lecture/suppression de ses lignes doit être autorisée ; la visibilité des données d'identité et le nettoyage des lignes doivent être examinés dans le cadre de la sécurité du document.
6. Héberger `index.html` sur GitHub Pages (ou `apia_widget_grist.html` à une URL exacte) et renseigner son URL dans le widget personnalisé de Grist ; accorder **Full document access** seulement dans un document d'atelier dédié.

## Fonctionnalités
- Une seule vue, **sans interface Admin**.
- Fenêtre modale de discussion par élément du scénario, métier ou regroupement.
- Nouvel avis, réponse à un avis, édition et suppression **de ses propres avis**.
- Saisie `@` avec suggestions à partir des auteurs des avis existants. **Pas de notification native** envoyée aux utilisateurs mentionnés.
- Avis historiques conservés avec leurs identifiants, mais sans propriété vérifiable tant qu'ils n'ont pas `AuteurEmail`.
- Le widget n'utilise pas de nom saisi manuellement comme preuve d'identité.

## Attention à la confidentialité
Les avis et les mentions sont stockés dans le document Grist. Ne pas y inclure de données personnelles issues de dossiers AIPD réels.

## Limites
L'API des widgets Grist ne fournit pas directement l'identité de l'utilisateur ; la table technique et les formules déclenchées sont donc nécessaires. Les @mentions sont une aide à la rédaction, pas un système de notifications. Les règles ACL Grist sont indispensables pour garantir l'édition/suppression de ses seuls avis.
