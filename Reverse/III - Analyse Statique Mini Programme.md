**Analyse statique**: Analyse du programme sans l'exécuter ni utiliser de débogueur.

Pour cet exemple, on va prendre une architecture **x86** (32 bits).

```c
int main()  
{  
 int a = 2;  
 int b = 3;  
  
 return a+b;  
}
```
On souhaite le compiler en 32 bits, et non pas en 64, donc commande un peu plus longue : 
```
gcc -m32 -fno-pie main.c -o exe
```

> A noter qu'il faut installer ce paquet au préalable : `sudo apt-get install gcc-multilib`


Après avoir compilé le programme, on obtient un exécutable `exe`. On peut utiliser la commande `file` pour avoir des informations dessus :  
```bash
┌──(naxyl㉿kali)-[~/Desktop/Reverse/Learning/Test]
└─$ file exe 
exe: ELF 32-bit LSB pie executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=dd0d2aa9ca29ab6815c9088c6e2765a29da4d3df, for GNU/Linux 3.2.0, not stripped
```
- **ELF** : Exécutable linux
- **LSB** (Least Significant Byte) : Il s'agit donc d'un programme [*Little Endian*](https://reverse.zip/posts/introduction_au_reverse_partie_4/#le-boutisme).
- **dynamically linked** : Les librairies sont chargées dynamiquement à l'exécution, donc pas directement inclues dans le programme.
- **not stripped** : Les symboles ne sont pas supprimés et sont présents dans le programme. Un symbole est une information, principalement une string et utilisé pour le débogueur. Le **nom d'une variable** **globale** ou un **nom de fonction** est un symbole.

Si on lance le programme, on obtient bien le résultat de la fonction : 
```bash
┌──(naxyl㉿kali)-[~/Desktop/Reverse/Learning/Test]
└─$ ./exe | echo $?
5
```


Maintenant l'objectif, c'est de reverse le programme, de façon statique. On va donc devoir retrouver le segment qui comporte le code, plus précisément les **opcodes**.

--> On va donc utiliser un outil qui transforme le code brut du programme (pas le code source attention) en instructions assembleurs afin qu'on les comprenne.

IDAAAAAAAAAAAAAAA !

On arrive donc là dessus :  
![[Pasted image 20241218233231.png]]

Ca fait super peur en tant que premier programme à reverse. Une explication de l'interface est donnée [ici](https://reverse.zip/posts/introduction_au_reverse_partie_5/#apprendre-%C3%A0-utiliser-ida-freeware), cela ne sert donc à rien que je réitère.






