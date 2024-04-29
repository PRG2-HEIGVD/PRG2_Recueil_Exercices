### Question 1: Lorsque le programme est exécuté avec les mêmes arguments que pour l'exercice précédent, quelle différence observe-t-on? Quel rôle joue les 2 OS dans cette différence de comportement?

<details>
<summary>Solution</summary>
<p></p>

L'exécution sous Linux produit l'output suivant:

~~~
$ 09-04-bufferedPrint 10 0
buffer size = 10 characters
flush never
12
Enter the first number: 12
12
Enter the second number: Enter the third number: 
Average is : 12.000000
~~~

On constate que les prompts sont affichés en entier mais après que l'on ait saisi les inputs. 

Cette différence de comportement peut être en partie expliquée par le fait que l'ordre de buffering passé avec _setvbuf()_ est traité de manière différente par les 2 OS:
- Sous Windows: le buffering est effectué en mode bloc avec des blocs de 10 caractères, comme demandé
- sous Linux: le buffering est en fait effectué en mode texte, le buffer est vidé lorsque un '\n' y est écrit

</details>

### Question 2: Si on analyse plus avant l'output produit dans les mêmes conditions que pour la question précédente, on peut aussi noter une "bizarrerie" dans l'affichage sous Linux. Quelle est-elle? Comment l'expliquer?

<details>
<summary>Solution</summary>

<p></p>
La différence de stratégie de buffering n'explique pas tout le comportement que nous observons dans l'output de la question précédente. En effet, si on regarde le code source de <i>09-04-bufferedPrint.c</i> on peut noter que les invites sont affichées par des appels à <i>printf()</i> sans <i>'\n'</i>. Les 3 invites ne devraient donc pas être affichées à l'écran avant que le résultat du calcul de ne le soit, puisque c'est ici seulement que le premier <i>'\n'</i> est écrit dans le buffer de <i>stdout</i>. Or, les messages d'invite sont affichés:

- le premier, après avoir saisi 1 input,
- les 2 suivants, après avoir saisi les 2 inputs suivants.

Cette observation plus attentive ne peut pas être expliquée avec la seule différence de stratégie de buffering entre les 2 OS. Pour l'expliquer il faut d'abord savoir que, sur les systèmes Posix:

1) _stdin_ et _stdout_ appliquent toutes deux une stratégie de buffering par ligne lorsque elles sont en mode buffering.
2) _stdout_ est automatiquement vidée lorsqu'une tentative de lecture sur _stdin_ nécessite une lecture depuis le terminal.

Ensuite, il faut comprendre les appels à _scanf()_ et leur influence sur _stdin_. Sans rentrer dans les détails, _scanf()_ ne consomme pas le _'\n'_ après le nombre lu. C'est le _scanf()_ suivant qui le fait pour atteindre le premier caractère qui correspond au format attendu: "%d" attend un chiffre. La lecture de la ligne sur _stdin_, et donc le vidage de _stdout_, est déclenchée en retard d'un _scanf()_, ce qui esplique le phénomène observé.

</details>
