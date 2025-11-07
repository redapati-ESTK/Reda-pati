# Reda-pati
#include <stdio.h>
#include <stdlib.h>

/* --------- Fonction PGCD (Algorithme d’Euclide) --------- */
int pgcd_etapes(int a, int b) {
    int r, q;
    printf("\n=== Étape 1 : Calcul du PGCD par l’algorithme d’Euclide ===\n");
    printf("Divisions euclidiennes successives :\n");

    while (b != 0) {
        q = a / b;
        r = a % b;
        printf("→ %d = %d × %d + %d\n", a, b, q, r);
        a = b;
        b = r;
    }
    printf("PGCD trouvé : %d\n", abs(a));
    return abs(a);
}

/* --------- Algorithme d’Euclide étendu avec traçage --------- */
int euclide_etendu_etapes(int a, int b, int *x, int *y, int niveau) {
    if (b == 0) {
        *x = 1;
        *y = 0;
        printf("Niveau %d → Retour : (%d, %d) donne x = 1, y = 0\n", niveau, a, b);
        return a;
    }

    int x1, y1;
    int q = a / b;
    int r = a % b;

    printf("Niveau %d → Division : %d = %d × %d + %d\n", niveau, a, b, q, r);
    int d = euclide_etendu_etapes(b, r, &x1, &y1, niveau + 1);

    *x = y1;
    *y = x1 - q * y1;

    printf("Niveau %d → Remontée : d = %d, x = %d, y = %d (car x = y1, y = x1 - q*y1 = %d - %d×%d)\n",
           niveau, d, *x, *y, x1, q, y1);

    return d;
}

/* --------- Résolution de l’équation ax + by = c --------- */
void equation_diophantienne(int a, int b, int c) {
    printf("\n=== TP1 : Équation diophantienne linéaire ===\n");
    printf("Équation : %d·x + %d·y = %d\n", a, b, c);

    /* Étape 1 — PGCD */
    int d = pgcd_etapes(a, b);

    /* Étape 2 — Vérification d’existence */
    printf("\n=== Étape 2 : Vérification d’existence des solutions ===\n");
    if (c % d != 0) {
        printf("→ Le PGCD %d ne divise pas %d : aucune solution entière.\n", d, c);
        return;
    }
    printf("→ Le PGCD %d divise %d : il existe des solutions entières.\n", d, c);

    /* Étape 3 — Algorithme d’Euclide étendu */
    printf("\n=== Étape 3 : Calcul détaillé des coefficients de Bézout ===\n");
    int x0, y0;
    euclide_etendu_etapes(a, b, &x0, &y0, 1);
    printf("\nRésultat final Bézout : %d·(%d) + %d·(%d) = %d\n", a, x0, b, y0, d);

    /* Étape 4 — Calcul de la solution particulière */
    printf("\n=== Étape 4 : Calcul d’une solution particulière ===\n");
    int facteur = c / d;
    int xp = x0 * facteur;
    int yp = y0 * facteur;

    printf("On sait que %d = %d × %d\n", c, facteur, d);
    printf("Donc on multiplie les coefficients de Bézout (%d, %d) par %d :\n", x0, y0, facteur);
    printf("xp = %d × %d = %d\n", x0, facteur, xp);
    printf("yp = %d × %d = %d\n", y0, facteur, yp);
    printf("→ Solution particulière : (xp, yp) = (%d, %d)\n", xp, yp);

    /* Étape 5 — Forme générale des solutions */
    printf("\n=== Étape 5 : Forme générale des solutions ===\n");
    int alpha = b / d;
    int beta = -a / d;
    printf("Les solutions générales sont données par :\n");
    printf("(x, y) = (%d + %d·k, %d + %d·k),  k ∈ Z\n", xp, alpha, yp, beta);

    printf("\n=== Fin du programme ===\n");
}

/* --------- Programme principal --------- */
int main() {
    int a, b, c;

    printf("Entrez a : ");
    scanf("%d", &a);
    printf("Entrez b : ");
    scanf("%d", &b);
    printf("Entrez c : ");
    scanf("%d", &c);

    equation_diophantienne(a, b, c);
    return 0;
}

