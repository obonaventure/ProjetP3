-- DESCRIPTION --

Ce petit fichier texte pr�sente comment utiliser les informations obtenues
avec Lizard et avec le calcul du temps CPU pour faire du feedback textuel (pour
l'�tudiant) et par tag custom (pour les tuteurs). L'exercice pilote est 
"Palindrome", vous pouvez vous y r�f�rer pour voir comment cela est fait en
pratique.

-- HOWTO : LIZARD --

Lizard est un outil qui analyse pour nous le nombre de lignes que contient un code
ainsi que sa complexit� cyclomatique (cens�e repr�senter la complexit� du code,
autrement dit la difficult� pour une personne tierce de comprendre le code en
question). 

Ces deux informations sont maintenant disponibles pour chaque t�che
et pour chaque question de chacune de ces t�ches dans les tags customs associ�s 
(visibles lorsqu'on t�l�charge une soumission dans "submission.test" au champ
"custome"). Ces tags sont d�nomm�s ainsi : cc_q1 et nlines_q1 pour la premi�re
question, cc_q2 et nlines_q2 pour la seconde, etc. Attention cependant que si une
des questions n'est pas de type "multiline code", les num�ros peuvent �tre d�cal�s.
Par exemple si la premi�re question n'est pas un code alors cc_q1 r�f�rera � la q2.
Cela sera am�lior� plus tard.

En plus de ces tags customs, il est possible de donner un feedback textuel �
l'�tudiant (car celui-ci n'a pas acc�s aux tags customs). Pour cela, il faut cr�er
un fichier avec le nom "lizard.txt" au m�me emplacement que l� o� se trouve le
fichier "run". Ce fichier contient un entier par ligne et deux entiers par
question (donc deux lignes s'il n'y a qu'une question). La premi�re ligne est
le threshold pour la complexit� cyclomatique de la question 1 et la deuxi�me pour
le nombre de lignes de cette m�me question. S'il y a une deuxi�me question (ou
plus), il faut alors toujours respecter cet ordre (et commencer par la question 1
puis la question 2, etc.). Ce threshold sert � v�rifier que le code de l'�tudiant
ne les d�passe pas. Si c'est le cas, un feedback est fourni en disant que le code
de l'�tudiant est trop compliqu� ou trop long et en lui indiquant les valeurs
calcul�es sur la solution pr�sente dans "student/solutions/student_sol_code.c".
Il est � noter que ce feedback arrive que la question soit r�ussie ou rat�e et
que ces deux "tests" n'ont aucun impact sur la r�ussite de la question.

-- HOWTO : CPU --

Pour le temps CPU, on ne le calcule que dans les questions o� cela est n�cessaire.
A l'�criture de ce tutoriel, le seul exercice o� cela est fait est la t�che pilote
"Palindrome". Pour cette mesure, il existe �galement un tag custom et un feedback
textuel.

Pour les utiliser, il faut cr�er un test sp�cial dans "tests.c" qui va calculer le
temps CPU pris par la fonction de l'�tudiant et ensuite celui pris par la fonction
solution (disponible dans "student/solutions/student_code_sol.c"). Ce test doit
bien s�r �tre effectu� sur une donn�e de grande taille pour le rendre
pertinent. Il est ind�pendant entre chaque question et donc peut se faire que sur
une partie des questions d'une t�che. Pour exemple, dans "Palindrome", le test 
s'appelle test_CPU() et vaut pour 1/10 des points de la t�che (n'h�sitez pas � 
aller regarder comment cela est fait avant de le faire pour votre t�che).

Ce test sp�cial va alors faire deux choses avec les temps calcul�s. D'abord il va
�crire dans un fichier "cpu.txt" dans le r�pertoire courant le temps pris par la
fonction de l'�tudiant (ATTENTION : pour ce faire il est imp�ratif d'utiliser
fprintf, puisque cela sera analys� par le code python du run qui ne peut lire que
des strings, et de cr�er le fichier � l'ouverture, c'est-�-dire utiliser le mode 
"w+" pour la fonction fopen). S'il y a plusieurs questions qui doivent �tre 
analys�es, il faut �crire un flottant par ligne dans l'ordre des questions.
Ensuite, le test v�rifie que le temps pris par la fonction de l'�tudiant n'est pas
sup�rieure � une certaine valeur (dans "Palindrome" nous avons mis 
"studtime > 2*soltime"). Si c'est le cas, le test est rat� et un feedback textuel 
est fait � l'�tudiant en lui indiquant le temps pris par chacune des deux 
fonctions (celle de l'�tudiant et celle de la solution).

Le fichier cr�� quant � lui est utilis� dans le fichier "run" pour cr�er les tags
customs en rapport avec le CPU. Pour ce faire, il regarde si le fichier existe et
si oui, il lit ligne par ligne le(s) flottant(s) pr�sent(s) dans le fichier et
output les tags de nom cpu_q1, cpu_q2, etc. Attention cependant que le chiffre est
en rapport avec la premi�re question analys�e et pas avec la premi�re question de
la t�che ! Si par exemple vous analysez seulement la question 2 d'une t�che,
le tag en rapport avec ce test sera cpu_q1 car c'est la premi�re question que vous
analysez (une am�lioration est possible et sera s�rement faite plus tard en
rempla�ant le fichier texte par un fichier JSON ou YAML ou XML qui permettra
d'�tablir des paires question/temps CPU plus facilement).
