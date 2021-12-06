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
  - [Activités](#activités)
  - [Concaténation des actions](#concaténation-des-actions)

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

**Remarque importante :**
**l'ordre d'apparition des capteurs à de l'importance. On ne peut pas modifier cette ordre, il est immutable**

---

## Activités

Une activité est considérée comme étant un essemble d'actions.
Pour pouvoir représenter cette séquence, on va implémenter une classe Activités qui représentera un graphe. Ce graphe sera la concaténation de tous les sous-graphes d'actions.

On définit une activité par 5 paramètres :

- Le **nom** de l'activité, par exemple "sleeping", "having lunch", etc.
- L'**heureMin** c'est-à-dire l'heure à laquelle l'activité peut  commencer (dans une journée exemple 8h30)
- L'**heureMax** c'est-à-dire l'heure à laquelle l'activité peut  se terminer (dans une journée exemple l'activité termine au plus tard 22h15)
- La **duree** la durée de l'activité dans une journée
- L'**actionList** l'ensemble des actions qui composent l'activité

**Remarque : [heureMin:heureMax] représente la plage horaire durant laquelle une activité peut avoir lieu**

Une activité contrairement à une action ne respecte pas un ordre immutable. On peut générer plusieurs combinaisons d'actions, l'odre n'a pas d'importance.
Par ailleurs, on peut avoir des répétiotions d'actions.

---

## Concaténation des actions

Comme dit précédemment, pour avoir une activité on va concaténer des graphes d'actions.
Pour cela on va relier le dernier élément d'un sous graphe au première élément d'un autre sous graphe. On va donc générer un arc.

Or un arc a une étiquette qui est la durée inter-action. Cette durée sera calculée de la manière suivante :

- on calcule la durée inter-action totale ie la durée de l'activité dont on soustrait la durée de chaque action. On note cette durée totale D.
- on instaure une liste qui va donner un poid à chacun des arcs inter-action(probabilité et la somme donne 1). On note p_i la probabilité de l'arc inter-action i.
- on attribue à chaque arc la proportion de la durée interaction totale lui correspondant à savoir D*p_i.

**Remarque importante :**
**La concaténation des activités fonctionne sur le même principe.**
