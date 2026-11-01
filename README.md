# Undercover

Le jeu d'ambiance Undercover, en une page web, pour y jouer à plusieurs autour d'un seul téléphone.

**Jouer : https://zaneriis.github.io/undercover/**

## Le principe

Les civils reçoivent tous le même mot secret. Les undercovers en reçoivent un mot proche, sans savoir qu'ils sont les intrus. Mr. White n'a aucun mot et doit deviner de quoi tout le monde parle.

À chaque tour, chacun décrit son mot en un mot ou une courte phrase, puis le groupe vote pour éliminer un joueur.

- Les **civils** gagnent en éliminant tous les imposteurs.
- Les **imposteurs** gagnent s'il ne reste plus qu'un seul civil.
- **Mr. White** éliminé a droit à une dernière chance : s'il devine le mot des civils, il gagne.

## Comment jouer

Ouvre la page sur un téléphone, ajoute les prénoms dans l'ordre où vous êtes assis, règle le nombre d'undercovers et de Mr. White, puis passe le téléphone. Chacun maintient la carte appuyée pour lire son mot, qui se cache dès qu'on relâche.

L'appli désigne celui qui ouvre le tour, gère le vote, révèle le rôle de l'éliminé et compte les points d'une partie à l'autre.

Barème : 2 points par civil en cas de victoire des civils, 10 points pour un undercover, 6 points pour Mr. White.

## Trois écarts de difficulté

Le menu d'accueil propose trois listes de mots, qui changent complètement le rythme des parties :

| Liste | Exemple | Effet |
| --- | --- | --- |
| Mots proches | Chat / Chien | L'undercover peut se fondre dans la masse ; les parties durent. |
| Mots éloignés | Chat / Cheval | Il faut rester vague pour ne pas se trahir. |
| Mots très éloignés | Chat / Aspirateur | L'undercover tient rarement deux tours ; parties courtes et bruyantes. |

## Le hasard

Trois précautions, parce qu'un tirage naïf donne vite l'impression d'être truqué :

- **Les mots.** Chaque liste a son propre sac : une paire ne peut pas revenir tant que toutes les autres de sa liste ne sont pas sorties. Un tirage indépendant redonnerait une paire déjà vue au bout d'une dizaine de parties.
- **Les rôles.** Chaque joueur porte une dette, qui monte quand il est civil et descend quand il est imposteur, et qui pondère le tirage suivant. Le résultat reste aléatoire, mais personne n'enchaîne quatre fois le rôle d'undercover. Sur 6 000 parties simulées à six joueurs, chacun ressort imposteur entre 999 et 1 001 fois pour 1 000 attendues.
- **L'ouverture du tour.** Celui qui a le moins souvent ouvert est choisi, départagé au hasard en cas d'égalité, et le compte est mémorisé d'une partie à l'autre. Mr. White n'ouvre jamais, il n'aurait aucun indice pour parler.

Les entiers sont tirés avec `crypto.getRandomValues` et rejet des valeurs hors plage, pour éviter le biais du modulo que laisse un simple `% n`.

Le panneau « Qui a eu quels rôles » affiche les comptes réels par joueur, pour vérifier tout ça en cours de soirée.

## Faire tourner le projet

Pas de dépendances, pas d'étape de build : `index.html` contient tout le jeu. Ouvre le fichier dans un navigateur, ou sers le dossier :

```sh
python3 -m http.server 8000
```

## Modifier les paires de mots

Les trois listes sont les constantes `PAIRS`, `PAIRS_LOIN` et `PAIRS_LUNE`, au début du `<script>`, rassemblées ensuite dans `DECKS`. Chaque entrée est un couple `["un mot", "l'autre mot"]` ; l'appli tire la paire au hasard et décide dans quel sens l'attribuer, donc l'ordre à l'intérieur du couple n'a pas d'importance. Pour en ajouter, il suffit d'allonger la liste voulue.

Pour créer une quatrième liste, ajoutez une entrée à `DECKS` avec un `name`, un `sample`, un `desc` et vos `pairs` : le sélecteur de l'accueil et les sacs de tirage s'y adaptent tout seuls.

## Données locales

Les prénoms, les scores, l'historique des rôles et les paires déjà sorties sont gardés dans le `localStorage` du navigateur, sous la clé `undercover-v1`. Rien ne quitte l'appareil, et il n'y a aucun serveur. Le bouton « Oublier l'historique » remet les compteurs à zéro.

## Licence

MIT — voir [LICENSE](LICENSE).
