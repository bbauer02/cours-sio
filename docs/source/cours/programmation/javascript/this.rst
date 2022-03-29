Comprendre les fonctions et le mot clé ``This`` (ES2017)
###########################################################

Le mot clé ``This``
***************************

Javascript a beaucoup évolue depuis ses premières version, et beaucoup de concept de base ont vu leur fonctionnement modifié. 

Il est donc nécessaire de faire un tour d'horizon et revoir ensemble les fondamentaux de Javascript. 

Dans un premier temps, prenons une fonction simple 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0
   
  function myFunction() {
    console.log(this===window); 
  }
  myFunction() // affiche : "true"

Cette fonction ne fait pas grand chose ormi comparer la stricte égalité de ``this`` avec ``window``.

``window`` est une variable globale qui represente la fenêtre du navigateur. Si l'on écrirait ce code depuis **NodeJs**, nous aurions mis à la place ``global``.

En executant ce bout de code, nous voyons que la valeur de retour est ``true``.

Compliquons légérement les choses, créons un objet qui contiendra une fonction qui fera la meme chose que précédement.

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  var myObject = {
    myFunction: function() {
      console.log(this === myObject);
    }
  }

  myObject.myFunction(); // affiche : "true"

L'utilisation de cet exemple permet de montrer que le pointeur ``this`` est placé sur l'objet ``myObject`` et non plus sur ``window``.
Et cela fonctionne comme nous pourrions l'attendre, ``this`` sert à appeler le contexte dans lequel une fonction est associée.
Et c'est le comportement que l'on retrouve dans des langages orientés objets comme le **C#** ou **java**.

.. warning:: 

  Le terme **pointeur** employé ici n'a rien à voir avec le concept de pointeur en C. 

Changeons maintenant légérement les choses dans notre code. Nous allons garder exactement la même déclaration de l'objet ``myObject``, mais lieu d'appeler ``myFunction`` depuis l'objet directement nous allons créer une variable ``myFunction`` qui contiendra ``myObject.myFunction``.

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 7,8

  var myObject = {
    myFunction: function() {
      console.log(this === myObject);
    }
  }

  var myFunction = myObject.myFunction;
  myFunction(); // affiche : "false"

Nous voyons que maintenant ``this`` n'est plus égale au contexte de la fonction ``myFunction`` mais à ``window``.

.. admonition:: Régle Javascript
   
   

  Si il est appelé depuis ``obj.func()`` alors ``this`` égale ``obj``. C'est à dire depuis la fonction elle-même dans son contexte.

  Sinon , ``this`` égale ``window`` ou ``global``.

  C'est comme cela que les choses fonctionnent avec Javascript.


Prenons un autre exemple : 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 3, 5

  var myObject = {
    myFunction: function() {
      console.log(this===myObject); // affiche: "true"
      setTimeout(function() {
        console.log(this === myObject); // affiche: "false"
        console.log(this === window); // affiche: "true"
      },0);
    }
  }
  myObject.myFunction();

Nous retrouvons ``myObject`` mais nous y avons ajouté une fonction asynchrone ``setTimeout`` (Nous aurions pu utiliser n'importe quelle autre fonction qui possède un ``callback``).

Et nous appelons la fonction ``myFunction`` depuis l'objet directement.
nous constatons qu'à la ligne 3, ``this`` est "égale" à ``myObject``. 
Alors qu'à la ligne 5, dans la fonction **callback**, ``this`` égale à ``window`` à la place de ``myObject``.

Pourquoi cela ? Pour comprendre, il faut se rapporter à la règle émise plus haut. 

A la ligne 3, ``this`` est invoqué depuis ``myFunction`` par l'intermédiaire de la référence à l'objet ``myObject`` elle-même.
Or, à la ligne 5, ``this`` est appelé depuis une fonction anonyme, qui n'est référencé dans aucun objet. C'est la fonction ``setTimeout`` qui l'appelle. 
Donc ``this`` égal à ``window``.

Expression de fonction vs Déclaration de fonction
******************************************************

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  function myFunctionDeclaration() {}

  var myFunctionExpression = function() {};

Depuis ES5, il possible de déclarer des fonctions Javascript de 2 manières différentes comme le montre le code ci-dessus.

Ces déclarations semblent différentes mais font exactement la même chose. 

Toutefois comme nous l'avons expliqué dans le cours sur le ``hoisting`` il existe une différence lors de la déclaration et de l'invocation de la fonction. 

Nous pouvons parfaitement utiliser une fonction déclarée après son appel, car le ``hoisting`` va se charger de remonter la déclaration tout en haut du script. 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  myfunction(); // Affiche: "Hello"

  function myFunction() {
    console.log("hello");
  }

Par contre nous aurons une erreur en utilisant la syntaxe suivante : 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  myfunction(); // Affiche: Uncaught ReferenceError: myfunction is not defined"

  var myFunction = function () {
    console.log("hello");
  }

Car le mécanisme de ``hoisting`` sépare les variable en deux parties: La déclaration et l'affectation. Il déplace la partie déclarative en haut du script et laisse l'affectation là où elle a été mise. 

Dans notre code ``var myFunction`` est considéré comme une déclaration de variable et c'est ce qu'elle est : une variable auquelle est affectée une référence à une fonction anonyme. Et à la ligne 1, ``myFunction`` égale à ``undefined``.

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  var myFunction;

  myfunction();

  myFunction = function () {
    console.log("hello");
  }

Expressions de fonction nommée
**********************************

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  var myFunction = function myOtherFunction(recurse) {
	if(recurse) {
		myFunction(false); // OK
		myOtherFunction(false); // OK
	}
  };

  myFunction(true); // OK 
  myOtherFunction(true); // ReferenceError

Nous avons déclaré une fonction nommée ``myOtherFunction`` dont la référence est assignée à la variable ``myFunction``.

A l'intérieur de ``myOtherFunction``, nous appelons : ``myFunction`` et ``myOtherFunction``, et nous avons le droit de le faire. 

Par contre, si à l'extérieur nous appelons ``myOtherFunction`` directement, nous aurons un message d'erreur de référence. Seul l'appel par ``myFunction`` sera valide. 

``Call``, ``apply`` et ``bind`` : initialisation manuelle de ``this``
**************************************************************************

Précédement, nous avons mis en évidence que ``this`` est de nouveau assigné à ``global`` ou ``window`` s'il est utilisé au sein d'une fonction asynchrone.

Etudions avec ce script comment changer la valeur de ``this`` dans une fonction avec les méthodes ``call``, ``apply`` et ``bind``. 

``call``
------------------------------



Etudions le cas de la méthode ``call`` :


.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  var myObject = {
    myFunction: function(a, b) {
      console.log(a + ' ' + b); // affiche : "Hello world"
      console.log(this === myObject); // False
      console.log(this === myOtherObject); // True
    }
  }

  var myOtherObject = {}

  myObject.myFunction.call(myOtherObject, 'hello', 'world');   
  
Nous créons un objet quelconque : ``myOtherObject``.
Nous appellons la méthode ``myFunction`` de l'objet ``myObject``, mais nous souhaitons que la référence de ``this`` de ``myFunction`` soit celle d'un autre objet extérieur, ``myOtherObject``. Cela est possible grâce à la méthode ``call``, qui prend en premier argument l'objet dont vous voulons utiliser la référence et les autres arguments suivants seront ceux nécessaires à l'utilisation de la fonction ``myFunction``.

``apply``
------------------------------

Il existe une autre syntaxe qui fait exactement la même chose que ``call`` mais avec la méthode ``apply``. La seule différence réside dans la manière dont sont passées les arguments à la fonction : ils sont placés dans un tableau. 

.. code:: 

  myObject.myFunction.apply(myOtherObject, ['hello', 'world']);


``bind``
------------------------------

Et finalement nous avons ``bind`` qui fonctionne presque pareil que ``call`` ormi du fait qu'il sépare la procédure d'utilisation en deux étapes séparées. 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 11,12

  var myObject = {
    myFunction: function(a, b) {
      console.log(a + ' ' + b); // affiche : "Hello world"
      console.log(this === myObject); // False
      console.log(this === myOtherObject); // True
    }
  }

  var myOtherObject = {}

  var myFunction = myObject.myFunction.bind(myOtherObject);
  myFunction('hello', 'world');  
  
Nous obtenons une nouvelle fonction qui possède un contexte de ``this`` prédéfinie, qui n'est pas celui de l'objet parent dans laquelle la fonction est déclarée,  mais de l'objet ``myOtherObject`` passé en argument à la méthode ``bind``. Nous l'avons assigné à une variable qui peut être ensuite utilisé comme une fonction classique. 

``bind`` est typiquement utilisé si nous avons besoin de forcer le pointeur de ``this`` d'une fonction **callback** par exemple.

Notation abrégée des objets
******************************

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 7,11
   
  const myObject = {
    myFunction() { 
      console.log(this === myObject);
    }
  }; 

  myObject.myFunction(); // true

  const myFunction = myObject.myFunction;

  myFunction(); // false  

Nous utilisons ici ``const`` pour déclarer notre objet ``myObject`` à la place de ``var``. 
Et contrairement aux exemples précédents nous avons déclaré la fonction ``myFunction`` directement en la nommant sans utiliser le mot clé ``function`` et sans l'avoir assigné à une clé d'objet comme : ``objectKey : function() {}``.

Cette nouvelle syntaxe introduite par **ECMA2015** permet de raccourcir les déclarations de fonction dans un objet Javascript tout en restant lisible. 

A la ligne 7, nous voyons que le pointeur ``this`` de la fonction ``myFunction`` est égale à son objet parent.
Toutefois lorsque nous faisons un alias à la ligne 9, ``this`` change de valeur. 
Cette syntaxe offre donc le même comportement pour la valeur de ``this`` qu'avec une syntaxe avec les ``:`` et le mot clé ``function``.

Fonctions fléchées
******************************

Es2015 ajoute une nouvelle syntaxe pour la déclaration des fonctions en javascript : **Les fonctions fléchées**. 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  const myFunction = () => { 
	  console.log(this === windows ); // True
  }
  myFunction();
  

Rappelez vous maintenant de cet exemple vu plus haut :

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 3, 5

  var myObject = {
    myFunction: function() {
      console.log(this===myObject); // affiche: "true"
      setTimeout(function() {
        console.log(this === myObject); // affiche: "false"
        console.log(this === window); // affiche: "true"
      },0);
    }
  }
  myObject.myFunction();

Nous en avons conclu que le pointeur de ``this`` changeait dans la fonction anonyme **callback** de ``setTimeout`` pour prendre celui de ``windows`` ou ``global``.

Réécrivons ce bout de code avec les nouvelles notations abordées précédement : 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  var myObject = {
    myFunction() {
      console.log(this===myObject); // affiche: "true"
      setTimeout(() => { 
        console.log(this === myObject); // affiche: "true"
        console.log(this === window); // affiche: "false"
      },0);
    }
  }
  myObject.myFunction();

Nous constatons que les résultats sont inversés.  En utilisant les fonctions fléchés comme ci-dessus nous conservons le pointeur de ``this``, qui correspond à l'objet ``myObject``.

Fonctions fléchées et ``.call()``
**************************************

Vous vous rappelez de la méthode ``.call()`` ? 

Etudions son comportement avec les fonctions fléchées. 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  const myObject = {};

  const myFunction = () => {
    console.log(this === myObject);
  };

  myFunction(); // False

  myFunction.call(myObject); // False 

Nous avons déclaré un objet quelconque ``myObject``. 
Nous souhaitons déplacer le pointeur de ``this`` vers l'objet ``myObject`` avec la méthode ``.call()`` comme nous l'avons vu avec les fonctions déclarée avec le mot clé ``function``. 

Et contre toute attente, nous faisons le constat que cela n'est pas possible !

Une fonction fléchée est une alternative compacte aux expressions de fonctions traditionnelles. 
elles ne peuvent pas être utilisé cependant dans toutes les situations. 

* ``this`` et ``super`` dans leur corps ne peuvent pas se lier à leur parent, nous ne devons donc pas les utiliser comme méthode d'un objet. 

* Les fonctions fléchées n'ont pas accès au mot clé : ``new.target``. 

* Les fonctions fléchées ne peuvent pas être utilisées par les méthodes ``call``, ``apply`` et ``bin``. 

* Les fonctions fléchées ne peuvent pas être utilisée comme constructeur. 

* Les fonctions fléchées ne peuvent pas utiliser ``yield`` dans leur corps. 


Les propriétés d'instance dans ES2017
****************************************

Avant la mise en place de ES2017, si vous vouliez ajouter une propriété à une classe, nous devions l'ajouter dans le constructeur comme suit : 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 3

  class MyClass {
    constructor() {
      this.myProperty = 10;
    }
  }

  const myInstance = new MyClass();
  console.log(myInstance.myProperty); // 10

C'était extrêmement verbeux, et pouvait rendre complexe la lecture du constructeur. Maintenant, nous pouvons déclarer directement les propriétés en dehors du constructeur : 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  class MyClass {
    myProperty = 10;
  }

  const myInstance = new MyClass();
  console.log(myInstance.myProperty); // 10

Mais cela entraine quelques implications particuliaire spécialement autour des fonctions.
En effet grâce à cette implémentation les méthodes d'une classe peuvent être de la forme d'une fonction fléchée et être considéré comme étant membre de la classe : 

.. code-block:: javascript
  :linenos:
  :emphasize-lines: 0

  class MyClass {

    myFunction = () => {
      console.log( this instanceof MyClass); // True
    };
  }

  const myInstance = new MyClass();
  const myFunction = myInstance.myFunction;

  myFunction();

Et ainsi, même en créant un alias de la méthode dans une variable, elle sera toujours considéré comme étant membre de l'instance de la classe qui l'a initié, comme cela fonctionne dans des langage comme java, C# ou C++. 