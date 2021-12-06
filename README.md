# Génération de Datasests

Après avoir renommé les capteurs dans tous les fichiers, nous allons générer des datasets de manière à simuler le comportement des personnes âgées dans leur appartement.

On distinguera deux types de personnes agées :

- les personnes agées autonomes (AA)
- les personnes agées non autonomes (ANA)

---

 Tables of contents

- [Génération de Datasests](#génération-de-datasests)
  - [Action](#action)
    - [Une histoire de graphes](#une-histoire-de-graphes)

---

## Action

Dans un premier temps on a définit une classe action.

- **Qu'est ce qu'une action?**

On considère une action comme étant une succession d'activité de capteurs.

Prenons un exemple : 
> ["SDB+",0,600,"WC+",40,3600,"WC-"]

Ici on considère cette séquence étant une action. La personne vivant dans l'appartement à ouvert la porte de la salle de bain puis à utiliser les toilettes.

### Une histoire de graphes

Pour pouvoir représenter cette séquence, on va implémenter une classe Action qui représentera un graphe. Le graphe sera orienté et sans cycles.

Pour cela on utilisera la librairie python _networkx_.

 Le graphe se décomposera en deux parties :

- les noeuds du graphes correspondront à un capteur activé ou non.
- les arcs eux correspondront à la durée qui s'écoule entre deux capteurs.

Le choix des durées se fera de manière à respecter les durées minimun et maximum données. 

Reprenons l'exemple de plus tôt ie :
> ["SDB+",0,600,"WC+",40,3600,"WC-"]

Notre graphe sera composée de trois noeuds et de deux arcs. 
![image](https://github.com/uvsq22006756/prevadom_image/blob/main/ex_action.jpg)

Grâce à la librairie _random_ on va pouvoir choisir une valeur de durée pour chaque arc.

Ici par exemple pour l'arc partant de SBD+ vers WC+ on ca choisir aléatoirement une durée comprise entre les bornes 0 et 600 secondes.

![image](https://github.com/uvsq22006756/prevadom_image/blob/main/ex_action_2.jpg)
