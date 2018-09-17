# <a name="executing-external-commands"></a>Exécution de commandes externes 

Note de traduction : dans ce contexte, certains termes n’ont pas été francisés (Shell, wildcards, etc). 

* [Appel de commandes Shell](#calling-shell-commands)
* [Appel de commandes Shell avec expansion](#calling-shell-commands-with-expansion)
* [Gérer les commandes de flux sortants et les redirections](#getting-command-output-and-redirections)

Certaines parties affichées peuvent être différentes en fonction de votre environnement de travail (basé sur votre nom d’utilisateur, votre répertoire de travail, etc). 

<br>

### <a name="calling-shell-commands"></a> Appel de commandes Shell 

```python
#!/usr/bin/python3

import subprocess

# Exécuter la commande externe 'date'
subprocess.call('date')

# Passer (énoncer) des options et des arguments à la commande 
print("\nToday is ", end="", flush=True)
subprocess.call(['date', '-u', '+%A'])

# Autre exemple 
print("\nSearching for 'hello world'", flush=True)
subprocess.call(['grep', '-i', 'hello world', 'hello_world.py'])
```

* L’instruction `import` est utilisé pour charger le module `subprocess` [qui est un standard dans la librairie (bibliothèque de modules) de Python](https://docs.python.org/3/library/index.html); 
* La fonction `call` depuis le module `subprocess` est une des solutions pour appeler une commande extérieure ; 
* En passant l’argument `flush` à la valeur `True` (par défaut, celle étant `False`), vous êtes sûr qu’un message sera affiché avant `subprocess.call` ; 
* Pour transmettre des arguments à la commande, une liste de chaînes de caractère est passée et non une chaîne de caractère unique. 

```
$ ./calling_shell_commands.py 
Tue Jun 21 18:35:33 IST 2016

Today is Tuesday

Searching for 'hello world'
print("Hello World")
```

**Lecture complémentaire**

* [Module `subprocess` – documentation Python](https://docs.python.org/3/library/subprocess.html)
* [Module `os.system` – documentation Python](https://docs.python.org/3/library/os.html#os.system)
    * [article concernant la différenre entre les deux](https://www.quora.com/Whats-the-difference-between-os-system-and-subprocess-call-in-Python)
* [instruction d’importation `import` – documentation Python](https://docs.python.org/3/reference/simple_stmts.html#import)

<br>

### <a name="calling-shell-commands-with-expansion"></a>Appel de commandes Shell avec expansion 

```python
#!/usr/bin/python3

import subprocess

# Exécution sans expansion 
print("No shell expansion when shell=False", flush=True)
subprocess.call(['echo', 'Hello $USER'])

# Exécution avec l’expansion Shell 
print("\nshell expansion when shell=True", flush=True)
subprocess.call('echo Hello $USER', shell=True)

# Echapper la chaîne si nécessaire 
print("\nSearching for 'hello world'", flush=True)
subprocess.call('grep -i \'hello world\' hello_world.py', shell=True)
```

* Par défaut `subprocess.call` n’est pas étendu [wildcards du Shell](https://github.com/learnbyexample/Linux_command_line/blob/master/Shell.md#wildcards), effecturer une [substitution de commande](http://mywiki.wooledge.org/CommandSubstitution), etc
* Vous pouvez outrepassé ce comportement en passant la valeur `True` à l’argument `shell` 
* Notez que la commande entière est maintenant passée en tant que chaîne et non en tant que liste de chaînes 
* Les chaînes de caractères doivent être échappées en fonction de la composition de la commande 
* Utilisez `shell=True` seulement si vous êtes sûr que la commande doit être exécutée dans l’environnement Shell (avec les droits associés donc) – sinon cela représente une possible [faille de sécurité](https://stackoverflow.com/questions/3172470/actual-meaning-of-shell-true-in-subprocess)
    * [Constructeur `subprocess.Popen` – documentation Python](https://docs.python.org/3/library/subprocess.html#popen-constructor)

```
$ ./shell_expansion.py 
No shell expansion when shell=False
Hello $USER

shell expansion when shell=True
Hello learnbyexample

Searching for 'hello world'
print("Hello World")
```

* Dans certains cas, vous pouvez remplacer l’échappement en utilisant intelligemment des doubles et des simples guillemets comme suit : 

```python
# avec des doubles guillemets au sein de guillemets simples
subprocess.call('grep -i "hello world" hello_world.py', shell=True)

# ou l’inverse 
subprocess.call("grep -i 'hello world' hello_world.py", shell=True)
```

* [Redirection des sorties de commandes Shell](https://github.com/learnbyexample/Linux_command_line/blob/master/Shell.md#redirection) peuvent être utilisés comme d’ordinaire 

```python
# pour plus de clarté, utilisez des variables et évitez les longues chaînes 
cmd = "grep -h 'test' report.log test_list.txt > grep_test.txt"
subprocess.call(cmd, shell=True)
```

**Solution alternative à l’utilisation de `Shell=True` **

```python
>>> import subprocess, os
>>> subprocess.call(['echo', 'Hello', os.environ.get("USER")])
Hello learnbyexample
0
```

* `os.environ.get("USER")` permet de récupérer la variable d’environnement du script Python `USER`
* `0` est la valeur de retour / de sortie, indiquant la bonne réalisation de la commande. C'est un avertissement de l'interpréteur python qui affiche également la valeur de retour. 

<br>

### <a name="getting-command-output-and-redirections"></a>Gérer les commandes de flux sortants et les redirections 

```python
#!/usr/bin/python3

import subprocess

# La sortie inclut les messages d’erreur 
print("Getting output of 'pwd' command", flush=True)
curr_working_dir = subprocess.getoutput('pwd')
print(curr_working_dir)

# Obtenir le statut et les retours de la commande exécutée  
# Toute autre valeur que `0` est considéré comme une commande ayant rencontrée une erreur 
ls_command = 'ls hello_world.py xyz.py'
print("\nCalling command '{}'".format(ls_command), flush=True)
(ls_status, ls_output) = subprocess.getstatusoutput(ls_command)
print("status: {}\noutput: '{}'".format(ls_status, ls_output))

# Vous pouvez supprimer les messages d’erreurs si vous le souhaitez 
# subprocess.call() renvoie le statut de la commande et qui peut être utilisé 
print("\nCalling command with error msg suppressed", flush=True)
ls_status = subprocess.call(ls_command, shell=True, stderr=subprocess.DEVNULL)
print("status: {}".format(ls_status))
```

* La sortie de `getstatusoutput()` est un `tuple` – plus d’informations et d’exemples dans les prochains chapitres 
* `getstatusoutput()` et `getoutput()` sont des fonctions dites héritées
* D’autres fonctions plus récentes (et donc sécurisée) existent et peuvent doivent être utilisées 
    * [`subprocess.check_output` – documentation Python](https://docs.python.org/3/library/subprocess.html#subprocess.check_output)
    * [`subprocess.run` – documentation Python](https://docs.python.org/3/library/subprocess.html#subprocess.run)

```
$ ./shell_command_output_redirections.py 
Getting output of 'pwd' command
/home/learnbyexample/Python/python_programs

Calling command 'ls hello_world.py xyz.py'
status: 2
output: 'ls: cannot access xyz.py: No such file or directory
hello_world.py'

Calling command with error msg suppressed
hello_world.py
status: 2
```
