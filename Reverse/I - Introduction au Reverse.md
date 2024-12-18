<br>
Notes qui proviennent du cours de [Reverse.zip](https://reverse.zip)


### Sections et Segments

Pour commencer cette introduction, on prendra pour exemple ce programme C très simple :    
```c
#include "stdio.h"

int main()
{
        puts("Hello world!\n");
}
```


> **TL-DR** : Un **segment** est une zone mémoire qui contient **plusieurs sections** qui ont les mêmes attributs (ex : Lecture seule, exécutable …).
> ![[Pasted image 20241218204011.png]]
> **Program Header Table**: Liste les **segments** du programme.
> **Section Header Table** : Liste les **sections** du programme.


Les sections n'ont plus réellement d'utilité dès lors que le programme exécuté. Elles ont du sens lorsque l'OS va devoir allouer les différentes zones mémoires du processus. 
En section, on retrouve par exemple :  
- Section d'initialisation `.init` 
- Section qui contient le code `.text` (le code de la fonction `main()` par exemple.)
- Section de fin `.fini`

> Toutes les sections ci-dessus ont un point commun, elles se retrouveront dans le même segment, et ce par rapport à leur **droits**. Que ce soit l'intiialisation, le code ou la fin, les sections devront avoir les droits **RX**, puisque le contenu doit être lu, et des instructions devront être exécutées.


> Une section qui contient des données non modifiables, comme `.rodata` sera dans un segment qui ne comporte que le droit **R** (puisque rien besoin d'écrire ou d'exécuter). En prenant l'exemple du programme C ci-dessus, on aurait la chaîne `"Hello World!\n"` puisqu'elle est en paramètre d'une fonction, et jamais modifiée.


En reprenant le programme ci-dessus, pour afficher les différents segments de son ELF *(donc le code compilé, l'exécutable)*, on utilise :  
```bash
readelf -l helloworld
```
![[Pasted image 20241218205139.png]]

On voit bien, en bas de l'affichage pour les `Segments Section...` que le Segment n°3 comporte bien les sections `.init`, `.text` et `.fini` comme expliqué ci-dessus.

On remarque que le segment 11 est vide, c'est normal, il contient la **pile d'exécution** appelée **stack**. Elle contient les variables locales, et fonctionne en **LIFO**.

Pour revenir en haut de l'image, ce sont les segments qui sont affichés. On peut voir que la permission d'exécution, normalement **X**, est représentée par un **E**. On retrouve donc bien pour le segment 3 *(le deuxième LOAD)* les permissions **RE**.

### Représentation d'un processus
On peut essayer de comprendre l'agencement d'un processus en mémoire :  
![[Pasted image 20241218210549.png]]
> La _heap_ est désignées par “tas” en français mais cela **n’a rien à voir** avec la structure de données nommée [tas](https://fr.wikipedia.org/wiki/Tas_(informatique)). On l’appelle tas car il y a un tas de choses dedans allouées dynamiquement et qui sont souvent hétérogènes.
> 
> Etant donné que le **tas** est dédié à **l’allocation dynamique** de données, c’est un segment que l’on ne voit pas dans le programme tant qu’il n’est pas exécuté car cette zone mémoire est créée dynamiquement au lancement du programme.
> 
> Il en est de même pour la pile qui est une zone mémoire allouée lors du lancement du programme.

Concernant les adresses....
> Ici l’adresse la plus basse `0x00000000` a été mise tout en haut mais cela ne signifie pas qu’un processus est chargé à cette adresse. C’est d’ailleurs jamais le cas.
> 
> L’adresse `0x00000000` sur ce schéma permet juste de garder en tête que l’on représente les adresses basses vers le haut et les adresses hautes vers le bas.
> 
> De la même manière, la pile d’exécution n’atteint pas l’adresse `0x7fffffff`.

_On a pas d'explications par rapport à la convention que les adresses hautes soient représentées en bas et inversement._

<br>

Pour mieux comprendre le schéma ci-dessus, on peut faire un lien avec la programmation :  
- Dans le segment ayant les droits **RE** : Les instructions des différentes fonctions du programme, comme `main()`
- Dans le segment ayant les droits **R** (comme .rodata) : Les données non modifiables telles que les chaînes en argument de fonction etc.
- Dans le segment ayant les droits **RW** (comme .data) : Les **variables globales** (déclarées en dehors de toute fonction), les **variables statiques**...
- Dans le **tas**/ segment **DYNAMIC**: Les variables allouées dynamiquement (malloc en C), dont on ne connaît pas la taille avant l'exécution du programme. (inputs...)
- Dans la **pile**/ segment **GNU_STACK** : Les **variables locales**, c'est à dire la majorité des variables utilisées qui ne **sont pas allouées dynamiquement** et déclarées au sein des fonctions (sans **static**). Du style `int a = 123`

