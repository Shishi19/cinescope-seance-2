# Note — Réparer la fenêtre modale

| # | Problème rencontré | Impact utilisateur | Correction et vérification |
|---|---|---|---|
| 1 | « Réserver » était un `<div onClick>` : ni rôle `button`, ni focus, ni activation par Entrée/Espace. | Un utilisateur clavier ou lecteur d’écran ne peut pas ouvrir la réservation. | Remplacé par un `<button type="button">`. Vérifié : Tab atteint le bouton, rôle `button` et nom « Réserver Après l’aube » dans le panneau Accessibilité. |
| 2 | La fenêtre était un `<div>` sans rôle ni nom, le focus restait derrière, et le fond restait navigable. La croix était un `<span>` sans nom ni focus. | Aucune annonce d’une fenêtre, focus perdu dans la page masquée, impossible de fermer au clavier. | `<dialog>` natif ouvert avec `showModal()` (rôle `dialog`, `aria-labelledby` vers le `<h2>`, fond inerte, Tab piégé). Croix en `<button aria-label="Fermer la réservation">`. Vérifié : nom calculé « Réserver … », focus sur la croix à l’ouverture, trois Tab restent dans la fenêtre. |
| 3 | Aucune fermeture clavier et aucun retour du focus. | Escape sans effet, et le focus retombe au début de la page : il faut tout reparcourir. | `onClose` du `<dialog>` (déclenché par Escape, la croix et « Revenir aux films ») : le focus revient au bouton mémorisé via `event.currentTarget`, donc au bon film. Vérifié avec Escape sur « La mémoire des murs » et Entrée sur la croix pour « Après l’aube » : le focus revient chaque fois au bouton du film demandé. |

Contraintes respectées : aucun `tabIndex`, éléments natifs, indicateur de focus conservé.
