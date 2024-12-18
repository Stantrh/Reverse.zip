**L’assembleur** ou **langage machine**, est le langage le plus bas niveau qu’il puisse y avoir. Par “plus bas niveau” on entend qu’il s’agit d’un langage compris **directement** par l’ordinateur et plus précisément par le **processeur**.

En fait, c’est le **processeur** qui se charge **d’exécuter** toutes les instructions assembleur. Que ce soit les accès en mémoire vive (RAM), les calculs, les appels de fonctions, bref, c’est lui **LE cerveau** de l’ordinateur.

> Pour voir l'architecture de notre PC sous Linux : 
> ```bash
> lscpu | grep Arch
> ```
> Sous Windows : 
> ```powershell
> `$env:PROCESSOR_ARCHITECTURE`
> ```
> 
> Pour ma part :  
> ```bash
> ┌──(naxyl㉿kali)-[~]
└─$ lscpu | grep Arch
Architecture:                         x86_64
> ```

> Selon le type de processeur, on retrouve plusieurs types de **langages assembleur** (*x86*, *ARM*, *MIPS*, *RISC-V.*..).
> 
> **x86** : Processeurs utilisés par AMD et Intel
> **ARM**: Processeurs utilisés sur la majorité des smartphones, Mac


Pour illustrer cette différence, prenons le programme le plus basique qui existe :  
```c
int rien()  
{ 	
	return 0; 
}   
```

On a un [site](https://godbolt.org/) qui nous permet de voir comment le code sera compilé selon les différents langages assembleurs. Pour les 4 langages assembleurs *les plus utilisés*.

**x86_64** : 
```asm6502
	push rbp       
	mov rbp, rsp       
	mov eax, 0       
	pop rbp       
	ret
```

**ARM** : 
```armasm
	mov     
	w0, 0       
	ret
```

**MIPS** : 
```arm-asm
	addiu   $sp,$sp,-8       
	sw      $fp,4($sp)       
	move    $fp,$sp       
	move    $2,$0       
	move    $sp,$fp       
	lw      $fp,4($sp)       
	addiu   $sp,$sp,8       
	jr      $31       nop
```

**RISC-V** : 
```asmatmel
addi    sp,sp,-16       
sd      s0,8(sp)       
addi    s0,sp,16       
li      a5,0       
mv      a0,a5       
ld      s0,8(sp)       
addi    sp,sp,16       
jr      ra`
```



**NOTE** : 
Il est important de savoir qu'au sein d'une même architecture (un langage d'assembleur, genre ARM par exemple), il existe plusieurs versions. 
- **x86**: Assembleur d'Intel et AMD, version 32 bits.
- **x86_64**: Idem, mais version 64 bits. Aussi appelée **AMD_64**.

Du coup, la différence entre de l'assembleur **32 bits** et **64 bits** réside dans la façon dont le processeur va traiter les données. 
- Dans du **64 bits** le processeur peut traiter une donnée de **64 bits** d'un coup.
- Dans du **32 bits** le processeur doit traiter une donnée de **64 bits** en **deux** fois (*32 bits par 32 bits*)

Ce qui permet de traiter une donnée, c'est ce qu'on appelle un **registre**. Un processeur dispose d'environ 10 ou 20 **registres** qui correspondent à des petites zones mémoires pour faire des opérations telles que des calculs, déplacements de valeurs, stockage, etc...
--> C'est bien plus performant qu'utiliser la RAM, car accessible directement.

*Ce qui est bien, c'est qu'un OS 64 bits est rétrocompatible, il peut exécuter des programmes 32 bits !*

> Dans le langage, les principales différences syntaxiques sont donc le **nom des registres utilisés**. Par exemple, en **x86**, on utilise `eax` contre `rax` en **x86_64**.



### Architectures
On peut classer les architectures en deux classes :  
- **CISC** --> ***AMD_64*** par exemple. Le nombre d'octets pour représenter une instruction n'est **pas fixe**. Ca peut être 1 octet, 3 octets, 4 octets, 5 etc... jusqu'à 15 octets.
- **RISC** --> ***ARM*** par exemple. Le nombre d'octets pour représenter une instruction est **fixe**. 2 ou 4 octets pour ARM.


⚠️ **Ressource utile et importante** : [Le Boutisme](https://reverse.zip/posts/introduction_au_reverse_partie_4/#le-boutisme) ⚠️



