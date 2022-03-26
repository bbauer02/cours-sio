Hoisting 
############

Le **Hoisting** est un concept propre au moteur Javascript sur la manière d'exécuter les scripts. 
Cela permet à Javascript de répertorier les variables ou les fonctions qui seront utilisées **AVANT** leur utilisation dans un script. 

Prenons un exemple pour comprendre : 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0
   
   let result = 1;

   console.log(addOne(3)); // Retourne 4
   console.log(result); // Retourne 4

   function addOne(numToAdd) {
    result = result + numToAdd;
    return result;
   }

Ici, nous appelons ``addOne`` à la **ligne 3** alors que nous l'avons déclaré plus bas, de la **ligne 6 à 9**, nous devrions avoir un message d'erreur.
Mais grâce au mécanisme de **Hoisting**, Javascript a été capable de parcourir notre fichier avant qu'il ne s'execute et a mémorisé toutes les déclarations de fonctions. 

Mais il faut garder à l'esprit que cela n'est valable que dans le cadre d'une déclaration de fonction avec le mot clé ``function``. 

Si nous avions une fonction anonyme stockée dans une variable comme suit : 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 6,7,8,9
   
   let result = 1;

   console.log(addOne(3)); // Retourne 4
   console.log(result); // Retourne 4

   const addOne = function (numToAdd) {
    result = result + numToAdd;
    return result;
   }

La variable ``addOne`` sera bien entendu référencée par le mécanisme du **Hoisting** mais il ne sera pas possible d'appeler la fonction associée. 

Soyez donc vigilant si vous utilisez cette syntaxe. 

Pour comprendre ce que fait le **hoisting** nous allons proposer ce script simple : 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  let var1 = 100;
  let var2 = increment(var1);
  let var3 = decrement(var2);

  function increment(value) {
    return value++;
  }

  const decrement = function(value) {
    return value--;
  } 
  
.. code-block:: javascript
  :caption: Le même script réorganisé par le moteur Javascript : Hosting
  :linenos:
  :emphasize-lines: 0
  
  function increment(value) {
    return value++;
  }

  let var1;
  let var2;
  let var3;
  const decrement;

  var1 = 100;
  var2 = increment(var1);
  var3 = decrement(var2);
  decrement = function(value) {
    return value--;
  } 


La déclaration de la fonction ``increment`` est bien déplacée, levée au haut du script, car elle a été déclarée avec le mot clé ``function``.

Les variables ``var1``, ``var2``, ``var3`` viennent ensuite, mais aucune valeur ne leur a été affecté, elles sont définies pour le moment comme ``undefined``.

La fonction ``decrement`` est considérée comme étant une variable de par sa syntaxe déclarative. 



.. admonition:: A retenir
   
   Le moteur de **Javascript**, fait un traitement du script avant de l'**exécuter**. 
   Il va bouger les déclarations de variables tout en haut du script, ainsi que les déclarations de fonctions. 

   Puis les affectations des valeurs seront faites uniquement lors de l'execution. 
  
