Opérations synchrones
############################

Une opération synchrone est une opération ou une tâche qu'on va réaliser dans notre programme et qui lorsqu'elle est réalisée retourne le résultat de cette opération ou de cette tâche immédiatement.

c'est à dire que lorsque l'on passe à l'opération suivante on sait que l'opération qui a été réalisé juste avant est complètement terminée et le résultat nous est fourni.

Par exemple: 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0
   
  const x = 10
  const y = 20
  if(x>1) {
    console.log('X est plus grand que 1');
  }
  if(y<1) {
    console.log('Y est plus grand que 1'); 
  }
  // Calcul du total
  const total = x + y;
  console.log('Le total est' + total);


Dans cet exemple, en première ligne nous déclarons une constante ``x`` qui est assignée à la valeur ``10``. Quand nous passons à la deuxième ligne nous sommes sûr à ce moment là que dans notre programme nous avons une constante qui s'appelle ``x`` et que cette valeur est assigné à la valeur ``10``, c'est à dire qu'à la ligne 2 nous sommes sûr que l'opération 1 a été complètement terminée et nous pouvons la considérer comme acquise.
A la ligne 2, nous assignons une nouvelle constante qui a été assigné à la valeur ``20``.
Nous avons la certitude que ``x`` et ``y`` existent et ont bien été affectée aux valeurs désignées par notre programme, nous pouvons donc les utiliser dans la suite du programme. 

Le programme s'execute de lignes en lignes, et réalise successivement les deux tests de la ligne 3 et 6. 
Les instructions de notre programme seront exécutés les unes à la suite des autres. L'instruction suivante s'exécute quand la précédente est achevée. 


Opérations asynchrones
############################

Une **opération asynchrone** est aussi exécutée d'**instructions** en **instructions**, toutefois lorsque l'on passe à la suivante, on ne peut pas avoir la garantie que le résultat de l'instruction précédente existe déjà. 
Nous sommes habitué à ce type d'opération, par exemple lorsque nous exécutons un programme qui ouvre un fichier pour être lu : Il faut un certain temps pour le localiser sur un disque, lire l'information et la restituer, et ce temps est variable selon les ressources disponibles de votre ordinateur. 
On ne peut pas garantir que lorsque le programme passe à l'instruction suivante, le fichier soit déjà lu et accessible par l'utilisateur. 
On pourrait simplement demander au programme de mettre en pause l'exécution de la suite des instructions en attendant que le fichier soit lu mais cela bloquerait l'application le temps nécessaire. 
Pour ne pas bloquer le programme, on peut alors demander à l'application d'éxécuter d'autres tâches qui n'auraient pas besoin de l'information en attente,  puis de s'en occuper de nouveau lorsque le traitement est fini. 

Nous allons voir comment réaliser des opérations asynchrones avec **Javascript**.

Nous allons créer un script basique qui va demander à l'utilisateur de saisir son prénom et qui l'affichera dans une console.

Nous écrirons le code via NodeJS pour s'affranchir d'une page HTML pour exécuter le javascript et se focaliser sur le concept. 


.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0
   
  

