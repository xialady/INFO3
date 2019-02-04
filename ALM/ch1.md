% Chapitre 1 - Titre
% Vincent danjean (cours) - Mica MURPHY (note) - Antoine SAGET (note)
% Lundi 21 Janvier 2019

# 0 - Informations

Mail : vincent.danjean@imag.fr

# 1 - Introduction aux systèmes d'exploitation (SE)

Exemples de systèmes d'exploitation (OS) : Linux, Windows, MacOS, Android (même noyau que Linux), iOS, Fushia, Solaris, AIX...

## I - À quoi sert un système d'exploitation (SE = OS) ?

- Simplifie la vie du programmeur (lien avec le matériel, drivers...)
- Abstraire le matériel :
  - Fichier/dossier : stockage
  - TCP/IP : réseau
  - Interfaces graphiques
  - I/O ou E/S
  - Processus : processeur/mémoire (partage d'un processeur par différents programmes)
- Arbitrage et protection

## II - Principe de fonctionnement d'un OS

On a besoin de certaines fonctionalités hard pour pouvoir construire un OS avec une protection entre les processus (W95 et Linux à partir du Intel i386).

1. Un processeur a **deux modes** de fonctionnement :

- **user** : instructions déjà vues en ALM1 (impossible de passer à l'autre mode)
- **noyau** : (kernel / superviseur / privilégié) : instructions supplémentaires, possiblité de passer à l'autre mode, reprogrammer les adresses accessibles.

- `swi` $\rightarrow$ nous amène dans un endroit précis du noyau
- Pour accéder aux périphériques exterieurs, on utilise `load` et `store` à des adresses spécifiques.

2. Un processeur avec une **MMU** (Memory Managment Unit) : en mode utilisateur on n'a pas accès à toutes les adresses $\rightarrow$ on ne peut pas dialoguer avec tout.

3. Une source d'interruption périodique :
quand le processeur reçoit un signal d'interruption (timer toutes les 10ms ou entrée clavier), il passe en mode noyau et lance une routine d'interruption au niveau de l'OS.

### III - Structure générale d'un OS

- User : programmes, libc
- Kernel : `pid`, `uid`, `time`, ordonnanceur, VFS (FAT, Ext4, NTFS, CDROM, DD) en lien avec le hardware et avec des drivers
- Hardware : processeur, disque, CD
- OS : Kernel + librairies (libc) + programmese

# 2 - Interruptions

## I - Rôles des interruptions

1) Réagir à des événements extérieurs
2) Élever les privilèges
3) Faire quelque chose quand le processeur détecte une erreur

> Si on fait un `load` ou un `store` à une adresse inexistante, si on utilise une instruction illégale (paramètre incorrect)... $\rightarrow$ le processeur utilise le méchanisme d'interruption pour "faire autre chose" et gérer l'erreur.

## II - Contraintes

- Le code à exécuter n'est pas choisi par le programme (car élévation de privilèges)
- Il faut être capable de revenir dans l'état exact dans lequel il était avant l'interruption (cela ne veut pas dire que l'on va retourner dedans si c'est le programme qui a causé cette même erreur)

> Dans le cas d'un accès à une adresse illégale -> segfault.
> Un autre exemple qu'on on accès à une adresse déplacée dans le swap : l'interruption ce charge de rappatrier dans la RAM puis dans recommencer à l'instruction précédente

## III - Les trois types d'interruptions

Vocabulaire :

- **Synchrone.** Le logiciel s'interrompt lui même. Donc la condition de revenir exactement au même état est plus faible. Elle n'est d'ailleurs pas respectée puisque l'interruption a une valeur de retour donc au moins un registre va changer.
- **Asynchrone.** Le matériel ne peut rien prévoir.
- **Explicite.** C'est un choix qui n'a pas été fait par le système.
- **Implicite.** C'est un choix qui a été fait par le système.

### 1) Interruptions logicielles

C'est une interruption synchrone et explicite.

- Appels systèmes (on peut générer cet appel manuellement en 3/4 lignes d'assembleur)
- Instruction assembleur spécifique / spéciale :
  - Quand on part (peu importe l'ordre) :
    - Sauver le mode actuel
    - Changer le mode (élévation de privilèges)
    - Sauver le `PC`
    - Changer le `PC` _dans la RAM ?_ à une adresse inaccessible en mode user :
      - Soit une adresse constante
      - Soit le chargement d'une valeur à une adresse constante
      - Soit le chargement d'une valeur d'un registre spécial
  - Retour d'interruption, quand on revient (on fait les deux en même temps) :
    - Remettre l'ancien mode (si on la faisait en deuxième cette instruction ne serait pas exécutée)
    - Remettre le `PC` (si on la faisait en deuxième on n'aurait plus les privilèges pour revenir à l'ancien `PC`)

### 2) Gestion des erreurs

C'est une interruption synchrone et implicite.

- Détection d'erreur par le processeur :
  - Sauver le mode actuel
  - Changer le mode (élévation de privilèges)
  - Sauver le `PC`
  - Changer le `PC` à une adresse inaccessible en mode user (souvent différente de celle utilisée pour les autres interruptions)
- Retour d'interruption :
  - Remettre l'ancien mode
  - Remettre le `PC`

### 3) Événement extérieur

C'est une interruption asynchrone et implicite.

- Interruption extérieure reçue par une broche d'interruption (une ou deux par processeur). On peut aussi avoir un contrôleur d'interruption.
- Signal sur la broche d'interruption
  - Sauver le mode actuel
  - Changer le mode (élévation de privilèges)
  - Sauver le `PC`
  - Changer le `PC` à une adresse inaccessible en mode user (souvent différente de celle utilisée pour les autres interruptions)
- Retour d'interruption :
  - Remettre l'ancien mode
  - Remettre le `PC`

Ici, le fait que ce soit asynchrone est problématique pour revenir dans l'état initial : des problèmes pour sauver PC et pour ne pas écraser les registres déjà utilisés

## IV - Techniques de réalisation des interruptions

### 1) Adresse du traitant

**Traitant** : la fonction exécutée quand une interruption survient.

- **indirecte** : `PC <- Mem[Constante]`, plusieurs instruction (utilisé sur les processeurs CISC : x86)
- **directe** : `PC <- Constante`, une seule instruction = un branchement (utilisé sur les processeurs RISC : Arm)

### 2) Sauvegarde et restauration des registres

Il faut remettre **tous** les registres à leur valeur initiale à la fin du traitant.

Les processeurs ont en général autant de registres de pile que de modes. Donc on va empiler les registres dans la pile propre au mode actuel -> la pile du système d'exploitation. `SP` correspond toujours à la pile dans le monde actuel.

- Duplication du registre `SP`
- Duplication d'autres registres et/ou utilisation de la pile privilégiée

USR     | FIQ     | IRQ     | SVC
--------|---------|---------|--------
R0-R7   |         |         |
R8-R12  | R8-R12  |         |
R13-R14 | R13-R14 | R13-R14 | R13-R14
R15     |         |         |
CPSR    |         |         |
        | SPSR    | SPSR    | SPSR

_Sauvegarde des registres en fonction des modes dans un processeur ARM_

```ARM
traitant_ARM:
  stmfd sp!, {r0-r11, fp, lr}
  bl    vrai_traitant
  ldmfd sp!, {r0-r11, fp, lr}
  sub   pc, lr, #4
```

### 3) Autorisation et masquage

```
   (Rinst <- [PC])
          |
          v
 (décode Rinst/PC++)
    |     |     |
    v     v     v
  (add, (bl,b) (ldr,
   sub)   |     str)
    |     |     |
    └-----┼-----┘
       ┌--┴--┐
pas    |     |
d'intr |     | interruption
et I=0 |     | et bit I = 1
       v     |
     début   |
             v
       (SPSR <- CPSR,
        CPSR <- mode IRQ,
        LR   <- PC,
        PC   <- 018)
             |
             v
           début
```

```
CPSR :
|ZNCV| ... |F|I|mode| I permet d'empêcher une interruption pendant qu'une autre est en cours
```















Bleu, Vert, Orange, Rose, Jaune, Bleu vif, Cyan, Jaune foncé, Rouge, Vert clair, Violet, Bleu foncé, Vert, Rouge clair, Vert vif, Bleu pastel.