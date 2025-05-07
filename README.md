# Découverte de l'Overflow d'un char en C

## Introduction

Lorsqu'une variable stocke une valeur qui dépasse sa capacité maximale, on parle de "débordement" ou "overflow". Cet exercice simple vous aidera à visualiser ce phénomène avec le type `char`.

## Représentation d'un char en mémoire

Voici comment un `char` est stocké en mémoire :

```
REPRÉSENTATION D'UN CHAR EN MÉMOIRE (8 BITS)
--------------------------------------------

     1 OCTET (8 bits)
  ┌─────────────────┐
  │                 │
  │  Un caractère   │
  │                 │
  └─────────────────┘

   ┌─┬─┬─┬─┬─┬─┬─┬─┐
   │7│6│5│4│3│2│1│0│  ← Position des bits (de droite à gauche)
   └─┴─┴─┴─┴─┴─┴─┴─┘
    │ │ │ │ │ │ │ │
    │ │ │ │ │ │ │ └─→ 2⁰ = 1
    │ │ │ │ │ │ └───→ 2¹ = 2
    │ │ │ │ │ └─────→ 2² = 4
    │ │ │ │ └───────→ 2³ = 8
    │ │ │ └─────────→ 2⁴ = 16
    │ │ └───────────→ 2⁵ = 32
    │ └─────────────→ 2⁶ = 64
    └───────────────→ 2⁷ = 128 (bit de signe pour signed char)


TYPES DE CHAR EN C
------------------

1. UNSIGNED CHAR (0 à 255)
   ┌─┬─┬─┬─┬─┬─┬─┬─┐
   │0│0│0│0│0│0│0│0│ = 0   (Valeur minimale)
   └─┴─┴─┴─┴─┴─┴─┴─┘

   ┌─┬─┬─┬─┬─┬─┬─┬─┐
   │1│1│1│1│1│1│1│1│ = 255 (Valeur maximale)
   └─┴─┴─┴─┴─┴─┴─┴─┘

2. SIGNED CHAR (-128 à 127)
   ┌─┬─┬─┬─┬─┬─┬─┬─┐
   │1│0│0│0│0│0│0│0│ = -128 (Valeur minimale)
   └─┴─┴─┴─┴─┴─┴─┴─┘
    ↑
    Bit de signe (1 = négatif)

   ┌─┬─┬─┬─┬─┬─┬─┬─┐
   │0│1│1│1│1│1│1│1│ = 127  (Valeur maximale)
   └─┴─┴─┴─┴─┴─┴─┴─┘
    ↑
    Bit de signe (0 = positif)
```

## Le phénomène d'overflow

Voici ce qui se passe lors d'un overflow :

```
● UNSIGNED CHAR: Quand on dépasse 255

   ┌─┬─┬─┬─┬─┬─┬─┬─┐
   │1│1│1│1│1│1│1│1│ = 255
   └─┴─┴─┴─┴─┴─┴─┴─┘
       + 1
       ↓
   ┌─┬─┬─┬─┬─┬─┬─┬─┐
   │0│0│0│0│0│0│0│0│ = 0   (Retour à zéro!)
   └─┴─┴─┴─┴─┴─┴─┴─┘

● SIGNED CHAR: Quand on dépasse 127

   ┌─┬─┬─┬─┬─┬─┬─┬─┐
   │0│1│1│1│1│1│1│1│ = 127
   └─┴─┴─┴─┴─┴─┴─┴─┘
       + 1
       ↓
   ┌─┬─┬─┬─┬─┬─┬─┬─┐
   │1│0│0│0│0│0│0│0│ = -128 (Valeur change de signe!)
   └─┴─┴─┴─┴─┴─┴─┴─┘
```

## Exercice

### Partie 1 : Observer l'overflow d'un unsigned char

Créez un programme C simple qui :

1. Déclare une variable `compteur` de type `unsigned char` et lui assigne la valeur 254
2. Affiche sa valeur
3. Incrémente la variable d'une unité (compteur = compteur + 1)
4. Affiche sa nouvelle valeur
5. Incrémente la variable d'une unité à nouveau
6. Affiche sa valeur finale

Voici le code de départ :

```c
#include <stdio.h>

int main() {
    unsigned char compteur = 254;

    printf("Valeur initiale: %d\n", compteur);

    // Incrémentez et affichez la valeur

    // Incrémentez et affichez la valeur encore une fois

    return 0;
}
```

### Partie 2 : Observer l'overflow d'un signed char

Créez un second programme qui :

1. Déclare une variable `nombre` de type `signed char` et lui assigne la valeur 127
2. Affiche sa valeur
3. Incrémente la variable d'une unité
4. Affiche sa nouvelle valeur

Puis :

1. Déclare une autre variable `nombre_negatif` de type `signed char` et lui assigne la valeur -128
2. Affiche sa valeur
3. Décrémente la variable d'une unité (nombre_negatif = nombre_negatif - 1)
4. Affiche sa nouvelle valeur

Voici le code de départ :

```c
#include <stdio.h>

int main() {
    signed char nombre = 127;

    printf("Valeur maximale d'un signed char: %d\n", nombre);

    // Incrémentez et affichez la valeur


    signed char nombre_negatif = -128;

    printf("Valeur minimale d'un signed char: %d\n", nombre_negatif);

    // Décrémentez et affichez la valeur

    return 0;
}
```

## Résultats attendus

Pour la Partie 1, votre programme devrait afficher :

```
Valeur initiale: 254
Après incrémentation: 255
Après seconde incrémentation: 0
```

Pour la Partie 2, votre programme devrait afficher :

```
Valeur maximale d'un signed char: 127
Après incrémentation: -128

Valeur minimale d'un signed char: -128
Après décrémentation: 127
```

## Questions de réflexion

1. Qu'observez-vous quand un `unsigned char` dépasse sa valeur maximale (255) ?
2. Qu'observez-vous quand un `signed char` dépasse sa valeur maximale (127) ?
3. Qu'observez-vous quand un `signed char` descend en dessous de sa valeur minimale (-128) ?

## Pour aller plus loin

Essayez d'observer ce qui se passe si vous :

1. Incrémentez un `unsigned char` avec une valeur plus grande, par exemple :

   ```c
   unsigned char c = 250;
   c = c + 10;  // Que vaut c maintenant ?
   ```

2. Additionnez deux `unsigned char` dont la somme dépasse 255 :

   ```c
   unsigned char a = 200;
   unsigned char b = 100;
   unsigned char somme = a + b;  // Que vaut somme ?
   ```

3. Essayez de produire un overflow avec d'autres types de variables :

   ```c
   // Overflow avec un short (généralement 16 bits, -32768 à 32767)
   short s = 32767;
   s = s + 1;  // Que vaut s maintenant ?

   // Overflow avec un unsigned short (généralement 16 bits, 0 à 65535)
   unsigned short us = 65535;
   us = us + 1;  // Que vaut us maintenant ?

   // Overflow avec un int (généralement 32 bits)
   int i = 2147483647;  // Valeur maximale pour un int sur la plupart des systèmes
   i = i + 1;  // Que vaut i maintenant ?
   ```

4. Explorez ce qui se passe lors d'opérations de multiplication :

   ```c
   unsigned char x = 200;
   unsigned char y = 2;
   unsigned char produit = x * y;  // Que vaut produit ?
   ```

5. Affichez un `char` en tant que caractère plutôt qu'en tant que nombre :

   ```c
   // Rappel: En C, un char est à la fois un petit nombre et un caractère
   char lettre = 'A';  // La lettre A a le code ASCII 65

   // Affichage comme nombre
   printf("Valeur numérique: %d\n", lettre);

   // Affichage comme caractère
   printf("Caractère: %c\n", lettre);

   // Incrémentation pour passer à la lettre suivante
   lettre = lettre + 1;
   printf("Caractère suivant: %c\n", lettre);  // Devrait afficher 'B'

   // Que se passe-t-il si on continue d'incrémenter?
   ```

---

_Comprendre l'overflow est essentiel pour éviter des bugs subtils dans vos programmes. Ce phénomène se produit avec tous les types entiers en C, chacun ayant ses propres limites basées sur sa taille en bits._
