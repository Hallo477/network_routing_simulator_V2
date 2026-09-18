# Simulateur de Routage Réseau

Application web interactive permettant de construire un graphe représentant un réseau de routeurs, puis d'y appliquer différents algorithmes de graphes pour calculer des chemins, des distances globales ou une topologie à coût minimal — avec simulation de panne et possibilité d'annulation.

Réalisé dans le cadre du cours **Questions Spéciales de Programmation Avancée** (M1, Faculté des Sciences Informatiques, Université Catholique du Congo).

## Aperçu

Le simulateur permet de :

- construire librement un graphe (nœuds = routeurs, arêtes = liens pondérés) ;
- visualiser ce graphe sur un canevas interactif, avec repositionnement des nœuds à la souris ;
- calculer le plus court chemin entre deux routeurs avec **Dijkstra**, **Bellman-Ford** ou **Floyd-Warshall** ;
- calculer l'**arbre couvrant minimal** du réseau avec **Kruskal** ;
- simuler la panne d'un routeur (suppression d'un nœud) et observer le recalcul automatique d'un chemin alternatif ;
- **annuler** la dernière suppression ou réinitialisation et revenir à l'état précédent du graphe.

## Fonctionnalités

### Construction du graphe
- Ajout de nœuds (routeurs) nommés librement
- Ajout d'arêtes pondérées, avec option de lien bidirectionnel
- Suppression d'un nœud (et de ses arêtes) ou d'une arête isolée
- Réinitialisation complète du graphe
- Statistiques en temps réel : nombre de nœuds, d'arêtes, densité

### Visualisation
- Rendu du graphe sur `<canvas>`, avec flèches et poids affichés
- Repositionnement des nœuds par glisser-déposer
- Mise en évidence du chemin ou de l'arbre couvrant calculé

### Algorithmes de graphes

| Algorithme | Problème résolu | Complexité temporelle | Complexité spatiale |
|---|---|---|---|
| Dijkstra | Plus court chemin, source unique, poids positifs | O((V + E) log V) | O(V) |
| Bellman-Ford | Plus court chemin, source unique, poids négatifs possibles | O(V × E) | O(V) |
| Floyd-Warshall | Plus courts chemins entre toutes les paires | O(V³) | O(V²) |
| Kruskal | Arbre couvrant à coût minimal | O(E log E) | O(V + E) |

Après chaque calcul, l'application affiche le nombre réel d'opérations effectuées et le compare à l'estimation théorique.

### Tolérance aux pannes (redondance réactive)
Lorsqu'un nœud faisant partie d'un chemin actif est supprimé, l'application :
- invalide la route si le nœud supprimé était le départ ou l'arrivée ;
- relance automatiquement l'algorithme et affiche le chemin alternatif si un chemin existe encore ;
- signale l'absence d'alternative si le graphe restant ne permet plus de relier les deux extrémités.

Chaque cas est accompagné d'une notification visuelle (toast).

### Annulation (undo)
Avant toute suppression de nœud, d'arête, ou réinitialisation, l'état complet du graphe (nœuds, arêtes, positions, route active) est sauvegardé. Un bouton dédié permet de revenir à l'état précédent, y compris lorsque la suppression avait déclenché un recalcul automatique de chemin.

## Correspondance avec les protocoles de routage réels

| Protocole | Catégorie | Algorithme associé |
|---|---|---|
| OSPF | État de liens (IGP) | Dijkstra |
| RIP | Vecteur de distance (IGP) | Bellman-Ford |
| BGP | Vecteur de chemin, inter-domaines (EGP) | Routage par politique — non modélisé directement |
| STP (IEEE 802.1D) | Prévention de boucle, couche 2 | Kruskal |

## Utilisation

Aucune installation n'est nécessaire.

1. Cloner ou télécharger le dépôt.
2. Ouvrir le fichier `simulateur_de_routage_réseau_V2.html` dans un navigateur (Chrome, Firefox, Edge...).
3. Construire un graphe, choisir un algorithme, puis lancer un calcul.

```bash
git clone <URL-du-dépôt>
cd <dossier-du-dépôt>
```

Puis ouvrir `simulateur_de_routage_réseau_V2.html` directement, ou via un serveur local :

```bash
python3 -m http.server 8000
# puis ouvrir http://localhost:8000/simulateur_de_routage_réseau_V2.html
```

## Structure du projet

```
Simulateur-de-Route-R-seau_V2/
└── simulateur_de_routage_réseau_V2.html   # Application complète (HTML + CSS + JavaScript)
```

Fichier unique, sans dépendance externe ni build.

## Captures d'écran

<!-- Insérer ici une capture de la vue d'ensemble de l'interface -->

<!-- Insérer ici une capture d'un calcul de chemin (Dijkstra / Bellman-Ford / Floyd-Warshall) -->

<!-- Insérer ici une capture de l'arbre couvrant minimal (Kruskal) -->

<!-- Insérer ici une capture de la notification affichée après suppression d'un nœud -->

## Limites connues

- Un seul chemin actif à la fois ; aucun chemin de secours pré-calculé en parallèle
- La suppression d'une arête ne déclenche pas le même recalcul automatique que la suppression d'un nœud
- Aucune persistance des données (le graphe est perdu au rechargement de la page)
- L'historique d'annulation est limité aux vingt dernières actions

## Pistes d'évolution

- Redondance proactive (chemins de secours pré-calculés)
- Simulation de panne de lien (suppression d'arête déclenchant un recalcul)
- Modélisation simplifiée d'un routage par politique inspiré de BGP
- Export / import de la topologie au format JSON
- Journal des événements de panne, de reprise et d'annulation

## Auteur

**Richard Kalonji**
Master en Réseaux et Télécommunications — Université Catholique du Congo

## Licence

© 2026 Richard Kalonji — Tous droits réservés.
Projet académique — usage pédagogique.
