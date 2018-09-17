# <a name="getting-user-input"></a>Disposer des entrées « utilisateur » 

Note de traduction : les « entrées » ici indiquées, sont celles du clavier. 

* [Entrée d’entiers](#integer-input)
* [Entrée de décimaux (nombre à virgule flottante)](#floating-point-input)
* [Entrée de texte](#string-input)

<br>

### <a name="integer-input"></a>Entrée d’entiers 

```python
#!/usr/bin/python3

usr_ip = input("Merci d’entrer un entier (chiffre ou nombre) : ")

# Vous devez explicitement convertir l’entrée dans le type désiré dans le cas présent 
usr_num = int(usr_ip)
sqr_num = usr_num * usr_num

print("Le carré de l’entier entré est : {}".format(sqr_num))
```

* Testons désormais avec des entiers et une chaîne de caractères 
* [Entiers littéraux – documentation Python](https://docs.python.org/3/reference/lexical_analysis.html#integer-literals)

```
$ ./user_input_int.py 
Merci d’entrer un entier (chiffre ou nombre) : 23
Le carré de l’entier entré est : 529

$ ./user_input_int.py 
Merci d’entrer un entier (chiffre ou nombre) : abc
Traceback (most recent call last):
  File "./user_input_int.py", line 6, in <module>
    usr_num = int(usr_ip)
ValueError: invalid literal for int() with base 10: 'abc'
```

<br>

### <a name="floating-point-input"></a>Entrée de décimaux (nombre à virgule flottante) 

```python
#!/usr/bin/python3

usr_ip = input("Merci d’entrer un nombre décimal : ")

# Là encore vous devez explicitement convertir l’entrée dans le type désiré 
usr_num = float(usr_ip)
sqr_num = usr_num * usr_num

# Fixe la limite de la partie décimale du nombre entré, à deux chiffres après la virgule 
print("Le carré du nombre entré est : {0:.2f}".format(sqr_num))
```

* La [notation scientifique E](https://en.wikipedia.org/wiki/Scientific_notation#E_notation) peut parfois être utilisée lorsqu’elle est requise 
* [Entrée de décimaux littéraux – documentation Python](https://docs.python.org/3/reference/lexical_analysis.html#floating-point-literals)
* [Entrée de décimaux (nombre à virgule flottante)  – documentation Python](https://docs.python.org/3/tutorial/floatingpoint.html)

```
$ ./user_input_float.py 
Merci d’entrer un nombre décimal : 3.232
Le carré du nombre entré est : 10.45

$ ./user_input_float.py 
Merci d’entrer un nombre décimal : 42.7e5
Le carré du nombre entré est : 18232900000000.00

$ ./user_input_float.py 
Merci d’entrer un nombre décimal : abc
Traceback (most recent call last):
  File "./user_input_float.py", line 6, in <module>
    usr_num = float(usr_ip)
ValueError: could not convert string to float: 'abc'
```

<br>

### <a name="string-input"></a>Entrée d’une chaîne de caractères 

```python
#!/usr/bin/python3

usr_name  = input("Bonjour toi ! Quel est ton nom? ")
usr_color = input("Et quelle est ta couleur préférée? ")

print("{}, j’aime la couleur {}".format(usr_name, usr_color))
```

* Ici vous n’avez pas de conversion à effectuer, ni aucun caractère de nouvelle ligne à prendre en compte contrairement à Perl 

```
$ ./user_input_str.py 
Bonjour toi ! Quel est ton nom? Developpez.com
Et quelle est ta couleur préférée? Blanc 
Developpez.com, j’aime la couleur Blanc 
```
