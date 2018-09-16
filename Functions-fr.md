# <a name="functions"></a>Fonctions

* [Définition d'une fonction](#def)
* [Fonction d'affichage (retour console)](#print-function)
* [Fonction 'range' (liste d'entiers)](#range-function)
* [Fonction de connaissance du type de variable](#type-function)
* [Scope (portée) des variables](#variable-scope)

<br>

### <a name="def"></a>Définition d'une fonction

```python
#!/usr/bin/python3

# ----- Fonction sans argument -----
def greeting():
    print("-----------------------------")
    print("         Hello World         ")
    print("-----------------------------")

greeting()

# ----- Fonction avec arguments -----
def sum_two_numbers(num1, num2):
    total = num1 + num2
    print("{} + {} = {}".format(num1, num2, total))

sum_two_numbers(3, 4)

# ----- Fonction avec une valeur de retour -----
def num_square(num):
    return num * num

my_num = 3
print(num_square(2))
print(num_square(my_num))
```

* Le mot-clé `def` est utilisé pour définir un corps de fonction 
* Les fonctions doivent être définies avant tout usage 
* Une erreur de syntaxe commune (courante), est d'oublier `:` à la fin de l'instruction de définition d'une fonction
* Les blocs (corps) des fonctions, structures de contrôle, etc, sont distingués grâce à l'indentation 
    * l'indentation recommandée est de 4 espaces consécutifs 
    * [Bonnes pratiques de style - documentation Python docs](https://docs.python.org/3/tutorial/controlflow.html#intermezzo-coding-style)
* Par défaut, la valeur de retour de `return` (comme son absence) est `None` 
* [Comment les variables sont passées aux fonctions en Python](http://robertheaton.com/2014/02/09/pythons-pass-by-object-reference-as-explained-by-philip-k-dick/)
* `format` est traité dans le sujet suivant 

```
$ ./functions.py 
-----------------------------
         Hello World         
-----------------------------
3 + 4 = 7
4
9
```

**Valeur par défaut des arguments**

```python
#!/usr/bin/python3

# ----- Fonction avec un argument avec une valeur par défaut -----
def greeting(style_char='-'):
    print(style_char * 29)
    print("         Hello World         ")
    print(style_char * 29)

print("Default style")
greeting()

print("\nStyle character *")
greeting('*')

print("\nStyle character =")
greeting(style_char='=')
```

* Souvent, les fonctions peuvent avoir un comportement par défaut et, si nécessaire, permettent de donner un argument pertinent

```
$ ./functions_default_arg_value.py 
Style par défaut
-----------------------------
         Hello World         
-----------------------------

Si le style passé est "*"
*****************************
         Hello World         
*****************************

Si le style passé est "=" 
=============================
         Hello World         
=============================
```

* Les triples guillemets de commentaires permettent de décrire l'objectif / le fonctionnement de la fonction et sont généralement  repris par l'outil de génération de documentation 
* Pour éviter d'alourdir les exemples de ce tutoriel, les docstrings des programmes et de la définition des fonctions ne sont pas repris 
    * Voir le chapitres sur les [docstrings](./Docstrings.md) pour les exemples et discussions à ce propos 

```python
def num_square(num):
    """
    retourne le carré d'une variable 
    """

    return num * num
```

**Lectures complémentaires**

Il y a de nombreuses manières d’appeler une fonction et d’autres types de déclarations ; merci de vous référer aux liens fournis pour davantage d’information 

* [définir une fonction – documentation Python](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)
* [fonctions natives de Python – documentation Python](https://docs.python.org/3/library/functions.html)

<br>

### <a name="print-function"></a>Fonction d’affichage (retour console) 

* Par défaut, la fonction `print` ajoute systématiquement une ligne à la fin de son retour 
* Un tel comportement peut être modifié en passant une autre chaîne de caractères à l’argument `end` de cette fonction 

```python
>>> print("hi")
hi
>>> print("hi", end='')
hi>>> 
>>> print("hi", end=' !!\n')
hi !!
>>> 
```

* Cette fonction d’[aide](https://docs.python.org/3/library/functions.html#help) peut être utilisée pour obtenir la documentation par l’interpréteur 
* Appuyer sur `q` pour revenir à l’aide 

```python
>>> help(print)

Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
    
    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.
```

* Plusieurs arguments peuvent être passés à la fonction `print` grâce au séparateur d’arguments `,` lors de son appel. 
* La valeur par défaut de l’argument `sep`, est un espace simple 

```python
>>> a = 5
>>> b = 2

>>> print(a+b, a-b)
7 3

>>> print(a+b, a-b, sep=' : ')
7 : 3

>>> print(a+b, a-b, sep='\n')
7
3
```

* Lorsque les variables sont envoyées à la console (affichage), la méthode [__str__](https://docs.python.org/3/reference/datamodel.html#object.__str__) est appelé et permet de donner une représentation en chaîne de caractère (représentation humaine) 
* Ainsi une conversion explicite n’est pas nécessaire à moins qu’une concaténation soit nécessaire 

```python
>>> greeting = 'Hello World'
>>> print(greeting)
Hello World
>>> num = 42
>>> print(num)
42

>>> print(greeting + '. We are learning Python')
Hello World. We are learning Python
>>> print(greeting, '. We are learning Python', sep='')
Hello World. We are learning Python

>>> print("She bought " + num + " apples")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: Can't convert 'int' object to str implicitly

>>> print("She bought " + str(num) + " apples")
She bought 42 apples
```

* Comme alternative, vous pouvez modifier `sep` avec plusieurs arguments 

```python
>>> print("She bought", num, "apples")
She bought 42 apples

>>> items = 15
>>> print("No. of items:", items)
No. of items: 15

>>> print("No. of items:", items, sep='')
No. of items:15
```

* Pour rediriger le flux d’affichage à [stderr](https://stackoverflow.com/questions/3385201/confused-about-stdin-stdout-and-stderr) (erreur) au lieu du flux par défaut `stdout`, il vous suffit de modifier l’argument `file` 
* See also [sys.exit()](https://docs.python.org/3/library/sys.html#sys.exit)

```python
>>> import sys
>>> print("Error!! Not a valid input", file=sys.stderr)
Error!! Not a valid input
```

* La fonction `str.format()` peut être utilisée pour mettre en forme les chaîne de caractères et permettre l’utilisation de plusieurs variables d’une façon plus élégante que la seule concaténation 

```python
>>> num1 = 42
>>> num2 = 7

>>> '{} + {} = {}'.format(num1, num2, num1 + num2)
'42 + 7 = 49'

# Ou sauver la représentation du formatage (expression) voulu dans une variable et l’utiliser ensuite au besoin 
>>> op_fmt = '{} + {} = {}'
>>> op_fmt.format(num1, num2, num1 + num2)
'42 + 7 = 49'
>>> op_fmt.format(num1, 29, num1 + 29)
'42 + 29 = 71'

# et bien sûr l’expression peut être utiliser dans la fonction ‘print’ directement 
>>> print('{} + {} = {}'.format(num1, num2, num1 + num2))
42 + 7 = 49
```

* utilisation d’argument de type ‘nombre’ 

```python
>>> num1
42
>>> num2
7
>>> print("{0} + {1} * {0} = {2}".format(num1, num2, num1 + num2 * num1))
42 + 7 * 42 = 336
```

* formatage de nombres – spécification utilisé en utilisant l’argument optionnel du nombre, suivi par `:` puis le type de formation du nombre 

```python
>>> appx_pi = 22 / 7
>>> appx_pi
3.142857142857143

# limitation de la partie décimale (nombre de chiffres après la virgule) 
# la valeur est arrondie à cette fin 
>>> print("{0:.2f}".format(appx_pi))
3.14
>>> print("{0:.3f}".format(appx_pi))
3.143

# alignement 
>>> print("{0:<10.3f} and 5.12".format(appx_pi))
3.143      and 5.12
>>> print("{0:>10.3f} and 5.12".format(appx_pi))
     3.143 and 5.12

# remplissage de zéros 
>>> print("{0:08.3f}".format(appx_pi))
0003.143
```

* Changement de base (10, 2, etc) 

```python
>>> print("42 in binary = {:b}".format(42))
42 in binary = 101010
>>> print("42 in octal = {:o}".format(42))
42 in octal = 52
>>> print("241 in hex = {:x}".format(241))
241 in hex = f1

# Ajouter ‘#’ pour un préfixe 0b/0o/0x (binaire, octal, hexa) 
>>> print("42 in binary = {:#b}".format(42))
42 in binary = 0b101010

>>> hex_str = "{:x}".format(42)
>>> hex_str
'2a'

# Vous pouvez aussi utiliser la fonction native 
>>> format(42, 'x')
'2a'
>>> format(42, '#x')
'0x2a'

# Convertir une chaîne de caractère en entier 
>>> int(hex_str, base=16)
42
>>> int('0x2a', base=16)
42
```

* Similaire au préfixe d’affichage brut `r`, l’utilisation du préfixe`f` permet la représentation de nombre 
    * Introduit depuis Python v3.6
* Similaire à `str.format()`, la variable ou l’expression est spécifié par `{}`

```python
>>> num1 = 42
>>> num2 = 7
>>> f'{num1} + {num2} = {num1 + num2}'
'42 + 7 = 49'
>>> print(f'{num1} + {num2} = {num1 + num2}')
42 + 7 = 49

>>> appx_pi = 22 / 7
>>> f'{appx_pi:08.3f}'
'0003.143'

>>> f'{20:x}'
'14'
>>> f'{20:#x}'
'0x14'
```

**Lectures complémentaires**

* [formatage de chaînes de caractères – documentation Python](https://docs.python.org/3/library/string.html#formatstrings) – pour davantage d’informations ou d’exemples 
* [f-string – documentation Python](https://docs.python.org/3/reference/lexical_analysis.html#f-strings) – pour davantage d’exemples et de mises en garde 

<br>

### <a name="range-function"></a>Fonction ‘liste d’entiers’ 

Note du traducteur : il n’y a d’équivalent de ‘range’ en français, l’expression ‘liste d’entiers’ semble être la plus appropriée. 

* Par défaut les arguments sont les suivants : `start=0` et `step=1` – vous pouvez donc les utiliser ou ne pas en tenir en compte et passer vos propres arguments 
    * `range(stop)`
    * `range(start, stop)`
    * `range(start, stop, step)`
* Veuillez noter que le retour de la fonction `range` l’inclue par la valeur définie à `stop` – ce sera toujours l’entier précédent la valeur stop 
* Voir le chapitre sur les [listes](./Lists.md) pour l’introduction et les explications de cette partie 
* [‘ranges’ – documentation Python](https://docs.python.org/3/library/stdtypes.html#typesseq-range) – pour davantage d’informations et d’exemples 

```python
>>> range(5)
range(0, 5)

>>> list(range(5))
[0, 1, 2, 3, 4]

>>> list(range(-2, 2))
[-2, -1, 0, 1]

>>> list(range(1, 15, 2))
[1, 3, 5, 7, 9, 11, 13]

>>> list(range(10, -5, -2))
[10, 8, 6, 4, 2, 0, -2, -4]
```

<br>

### <a name="type-function"></a>Fonction déterminant le type 

Cette fonction est utile pour connaître le type de données ou de la variable qui est lui passées en argument. 

```python
>>> type(5)
<class 'int'>

>>> type('Hi there!')
<class 'str'>

>>> type(range(7))
<class 'range'>

>>> type(None)
<class 'NoneType'>

>>> type(True)
<class 'bool'>

>>> arr = list(range(4))
>>> arr
[0, 1, 2, 3]
>>> type(arr)
<class 'list'>
```

<br>

### <a name="variable-scope"></a>Scope (portée) des variables 

```python
#!/usr/bin/python3

def print_num():
    print("Yeehaw! num is visible in this scope, its value is: " + str(num))

num = 25
print_num()
```

* Les variables définit avant l’appel à la fonction sont également visibles dans la définition de la fonction 
* [Valeur des arguments par défaut – documentation Python](https://docs.python.org/3/tutorial/controlflow.html#default-argument-values) – voir la description pour connaître quand les valeurs par défaut sont évaluées (interprétées) 

```
$ ./variable_scope_1.py 
Yeehaw! num is visible in this scope, its value is: 25
```

Que se passe-t-il lorsqu’une variable est déclarée dans un bloc, est utilisé en dehors de ce dernier ? 

```python
#!/usr/bin/python3

def square_of_num(num):
    sqr_num = num * num

square_of_num(5)
print("5 * 5 = {}".format(sqr_num))
```

* Ici `sqr_num` est déclaré au sein de la fonction `square_of_num` et n’est pas accessible en dehors 

```
$ ./variable_scope_2.py 
Traceback (most recent call last):
  File "./variable_scope_2.py", line 7, in <module>
    print("5 * 5 = {}".format(sqr_num))
NameError: name 'sqr_num' is not defined
```

Une des solutions pour contourner un tel comportement, est l’utilisation du mot-clé `global` 
```python
#!/usr/bin/python3

def square_of_num(num):
    global sqr_num
    sqr_num = num * num

square_of_num(5)
print("5 * 5 = {}".format(sqr_num))
```

* Désormais vous pouvez accéder à la variable `sqr_num` en dehors de la définition de la fonction 

```
$ ./variable_scope_3.py
5 * 5 = 25
```

Si le nom de la variable est identique dans un tel cas à l’intérieur et à l’extérieur de la fonction (conflit de nom), le changement dans un bloc de fonction n’affecte pas la variable extérieure à celui-ci. 

```python
#!/usr/bin/python3

sqr_num = 4

def square_of_num(num):
    sqr_num = num * num
    print("5 * 5 = {}".format(sqr_num))

square_of_num(5)
print("Whoops! sqr_num is still {}!".format(sqr_num))
```

* Notez que l’utilisation de `global sqr_num` affectera ici `sqr_num` la variable en dehors du bloc définissant la fonction. 

```
$ ./variable_scope_4.py 
5 * 5 = 25
Whoops! sqr_num is still 4!
```

**Lectures complémentaires**

* [Exemple de portée – documentation Python](https://docs.python.org/3/tutorial/classes.html#scopes-and-namespaces-example)
* [la déclaration ‘global’ – documentation Python](https://docs.python.org/3/reference/simple_stmts.html#the-global-statement)
