
# Tri par sélection

En anglais, cet algorithme est nommé « selection sort ».

## Le principe

On dispose de ``n`` données. On cherche la plus petite donnée et on la place en première position, puis on cherche la plus petite donnée parmi les données restantes et on la place en deuxième position, et ainsi de suite.

Si les données sont les éléments d’une liste ``liste``, l’alg

Pour chaque valeur de ``i``, on cherche dans la tranche liste ``[i:n]`` le plus petit élément et on l’échange avec ``liste[i]``.

L’**algorithme de tri par sélection** est souvent utilisé pour trier à la main des objets, comme des cartes à jouer, des livres, etc.

## Algorithme du minimum

```
i_mini ← i (indice du plus petit élèment)
mini ← liste[i]
pour j variant de i+1 à n-1
    si liste[j] < mini
        i_mini ← j
        mini ← liste[j]
```


Pour obtenir un **algorithme du tri sélection**, il ne reste qu’à insérer l’algorithme du minimum dans une boucle où ``i`` varie de 0 à ``n−2`` et pour chaque valeur de i à faire échange de ``liste[i]`` avec ``liste[i_mini]``.
La donnée en entrée est une liste de ``n`` éléments. Il n’y a pas de résultat renvoyé en sortie, la liste est modifiée en place.

## Algorithme du tri

```
POUR i VARIANT de i à n-1
    i_mini ← i
    mini ← liste[i]
    POUR j VARIANT de i+1 à n-1
        SI liste[j] < mini
            i_mini ← j
            mini ← liste[j]
    ECHANGER liste[i] et liste[i_mini]
```
Exemple avec la liste [7,4,3,2,9,5] et les éléments échangés.
[2,4,3,7,9,5] après échange de 2 et 7.


* Pour ``i`` égal à 0 
* Pour ``i`` égal à 1 
* Pour ``i`` égal à 2 
* Pour ``i`` égal à 3 
* Pour ``i`` égal à 4
* ``[2,3,4,7,9,5]`` après échange de 3 et 4. 
* ``[2,3,4,7,9,5]`` après aucun échange. 
* ``[2,3,4,5,9,7]`` après échange de 5 et 7. 
* ``[2,3,4,5,7,9]`` après échange de 7 et 9.
  
La liste passée en paramètre est modifiée en place. Donc pour utiliser cette fonction, il suffit d’écrire l’instruction ``tri_selection(liste)``. Si nous ne voulons pas modifier la liste passée en paramètre il faut en faire une copie et ensuite renvoyer une nouvelle liste qui est triée. On obtient alors le programme suivant :

``` javascript

function tri_selection(liste) {
  const liste_tri = liste;
  for (let i = 0; i < liste_tri.length; i++) {
    let i_mini = i;
    let mini = liste_tri[i];
    for (let j = i+1; j < liste_tri.length; j++) {
      if(liste_tri[j] < mini) {
        i_mini = j;
        mini = liste_tri[j];
        [liste_tri[i], liste_tri[i_mini]] = [liste_tri[i_mini], liste[i]]
      }
    }
  }
  return liste_tri;
}
```

Explications : 

Nous commençons par déclarer une fonction avec le mot clé ``function``.

* La ligne ``const liste_tri = liste;`` permet de faire une copie de  la liste passée en argument afin d'éviter de la modifier. 

  Ainsi : Nous passons dans notre fonction une liste, un tableau en javascript. Et nous retournons une nouvelle liste triée. 

  Avec le mot clé `const`, nous déclarons une variable, qui sera une référence constante. Même si nous modifions des éléments dans notre tableau, il n'y aura pas d'erreurs. (valable pour les tableaux et les objets JS). Par contre, la variable `i_mini` ne peut pas être déclarée avec `const` mais avec ``let`` car sa valeur change et son type est un entier. 


* Nous utilisons une boucle `for` pour nous déplacer dans notre liste. 
  Nous rappelons que les éléments d'un tableau sont rangés et placé à des indices allant de 0 pour le premier, à l'indice correspondant à la taille du tableau-1.

  Exemple : ["a", "b", "c"]
  
  * a est à l'indice 0.
  * b est à l'indice 1.
  * c est à l'indice 2.

  Il y a 3 éléments dans mon tableau, sa taille est de 3. Et le dernier est donc à l'indice : 3 - 1 = 2.

  Ma boucle `for` aurait pu être déclaré ainsi : 

  ``for (let i = 0; i <= liste_tri.length - 1; i++)`` mais pour limiter les erreurs et oublie de gestion de taille du tableau, nous obtons ici l'ecriture : ``i < liste_tri.length``, inférieur strictement.

* Pour inverser nos éléments de notre tableau : 
  `[liste_tri[i], liste_tri[i_mini]] = [liste_tri[i_mini], liste[i]]`

  Exemple : une liste ["a", 'b', 'c' , 'd' , 'e'].
  Si nous souhaitons inverser b et c seulement et conserver le reste : 
  [[1], [2]] = [[2], [1]]



