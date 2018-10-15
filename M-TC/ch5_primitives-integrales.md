% Chapitre 5 - Primitives et intégrales
% Manel TAYACHI (cours) - Mica MURPHY (note) - Antoine SAGET (note)
% Vendredi 12 Octbre 2018

# A) Primitives

**Définition.** Soit $g:[a, b] \rightarrow \mathbb{R}$ (continue).
Une **primitive** de *g* est une fonction $G:[a, b] \rightarrow \mathbb{R}$ dérivable tq $G' = g$

*Exemple.* Soit $g(x) = x^2$, les fonctions $G_1 : \begin{array}{cc}\mathbb{R} \rightarrow \mathbb{R} \\ x \mapsto {x^3 \over 3}\end{array}$ et $G_2 : \begin{array}{cc}\mathbb{R} \rightarrow \mathbb{R} \\ x \mapsto {x^3 \over 3} + 12\end{array}$ sont des primitives de $g$.

> Il n'y a pas unicité de la primitive

**Proposition.** Si $G_1$ et $G_2$ sont deux primitive de $g$ , alors $G_1 - G_2 =$ cste.
De plus, toute les primitives de $g$ sont de la forme $G_1 + c$ où $c$ est une constante

**Notation.** On note $\int^a_bg(x)\ dx$ l'ensemble des primitves de g.

*Exemple.*
$$
\int x^2\ dx = {x^3 \over 3} + Cste \\
(=\{x \mapsto  {x^3 \over 3} + C | C \in \mathbb{R}\})$$

**Primitives usuelles**

Soit $b\in \mathbb{R},\ n \in \mathbb{Z}\backslash\{-1\},\ \alpha \in \mathbb{R}\backslash\{-1\}$

$f(x)$      | $\int f(x)\ dx$
------------|-------------------------------------------
$b$         | $bx + Cste$
$ax$        | $a {x^2 \over 2} + Cste$
$x^n$       | ${x^{n + 1} \over n + 1} + Cste$
$1 \over x$ | $ln|x| + Cste$
$\cos(x)$   | $\sin(x) + Cste$
$\sin(x)$   | $-\cos(x) + Cste$
$e^x$       | $e^x + Cste$
$\ln(x)$    | $x\ln(x) - x + Cste$
$x^\alpha$  | ${x^{\alpha + 1} \over \alpha + 1} + Cste$

\pagebreak

# B) Intégrales

## 1) Formule de somme

- $\int(g_1(x) + g_2(x))\ dx = \int g_1(x)\ dx + \int f_2(x)\ dx$

*Exemple.* $\int (2x^2 + 1)\ dx = {2x^3 \over 3} + x + Cste$

- $\int\lambda g(x)\ dx = \lambda \int g(x)\ dx$

> Remarque. On a pas de formule pour le produit (*IPP*)ni pour le quotient...
> On a pas on plus de formule pour la composée (*changement de variable*)

## 2) Calcul intégral

$g:[a, b] \rightarrow \mathbb{R}$ (continue)
$\int_a^b g(t)dt =$ aire algébrique (avec signe) entre le graphe de $g$ et l'axe des abscisses entre $a$ et $b$

![Schéma de la correspondance entre intégrale et aire algébrique]()
$\int^b_a g(t)dt = A_1 - A_2 + A_3$

> Remarque. Lorsqu'il y a des bornes, $\int_a^b g(t) dt \in \mathbb{R}$

*Exemple.* $\int^1_0 x\ dx$
![Schéma de int^1_0 x\ dx]()

$\int_0^1 x\ dx = {1 \over 2}$

![Schéma de int^1_-1 x\ dx]()
$\int^1_{-1}x\ dx = A_2 - A_1$

### C) Lien primitives et intégrales

**Théorème.** (Théorème fondamental de l'analyse)
Soit $g:[a, b] \rightarrow \mathbb{R}$ continue et soir $G$ une primitive de $g$.
Alors $\int^b_a g(t)dt = [G(t)]^{t = b}_{t = a} = G(b) - G(a)$ $\blacksquare$

*Exemple.*
$\int^1_0 x^2\ dx = [{x^3 \over 3}]^{x = 1}_{x = 0} = {1 \over 3}$
![Schéma correspondant]()

$\int^{\pi/2}_0 \cos(x)\ dx = [\sin(x)]^{x = \pi/2}_{x = 0} = \sin(\pi/2) = 1$
![Schéma correspondant]()

**Corollaire** Soit $g:[a, b] \rightarrow \mathbb{R}$ continue.
Alors $\forall c \in [a, b]$,
$$G_c : \begin{array}{cc}[a,b] \rightarrow \mathbb{R} \\ x \mapsto \int_c^x g(t)\ dt\end{array}$$
est une primitive de $g$.

\pagebreak

# D) Méthodes de calcul d'une intégrale

## 1) Chasles

$a, b, c \in \mathbb{R}$

$$
\int^b_a g(t)\ dt = \int^c_a g(t)\ dt + \int^b_c g(t)\ dt
$$

## 2) Utiliser les symétries

  - Si $f$ pair $\forall a > 0$

    $$
    \int^a_0 f(x)\ dx = \int^0_{-a} f(x)\ dx
    $$
    ![Schéma d'une fonction paire]()

    Donc

    $$
    \int^a_{-a} f(x)\ dx = 2\int^a_0 f(x)\ dx
    $$

  - Si $f$ impaire, $\forall a > 0$
    $$
    \int^a_0 f(x)dx = -\int^0_{-a} f(x)\ dx
    $$
    ![Schéma d'une fonction impaire]()

    Donc $\int_{-a}^a f(x)\ dx = 0$

    *Exemple* $\int_{-\pi/2}^{\pi/2}\sin(x)\ dx = 0$

  - On peut utiliser d'autres "symétries" comme la périodicité (exemple avec $\cos$ et $\sin$)

\pagebreak

## 3) Changement de variable

$f : [a,b] \rightarrow \mathbb{R}$ continue

> Rappel des fonctions bijectives. $f : x \rightarrow y$ bijective si $\forall y \in Y,\ \exists! x \in X$ tq $f(x) = y$
$f : [c,d] \rightarrow [a,b]$ dérivable et bijective
(En pratique on vérifie que $u([c, d]) = [a, b]$ et que $u$ est strictement monotone)

$$\boxed{\int_c^d f(u(t))\ u'(t)\ dt = \int_{u(c)}^{u(d)} f(x)\ dx}$$

$$"u' = {du \over dt} \Rightarrow u'\ dt = du"$$

*Exemple.* $\int^{1 \over 4}_0 xe^{x^2}\ dx$
$$
u:\begin{array}{cc}[0, {1 \over 2}] \rightarrow [0, {1 \over 4}] \\ x \mapsto x^2 \end{array}
$$
bijectif et $C^1$

$u'(x) = 2x\ "du = 2xdx"$
Donc
$$
I + {1 \over to} \int^{1 \over 2}_0 e^{x^2}2x\ dx = {1 \over 2} \int^{u({1 \over 2})_{u(0)} e^u\ du} \\
= {1 \over 2} \int^{1 _over 4}_0 e^u\ du \\
= {1 \over 2} [e^u]^{1 \over 4}_0 = {1 \over 2}(e^{1 \over 4} - 1)
$$


*Exemple.*
Soit $\int_0^{1/4} {\ln(1-\sqrt{x}) \over \sqrt{x}}\ dx$, choisissons $u : \begin{array}{cc}[0, {1 \over 4}] \rightarrow [{1 \over 2}, 1] \\ x \mapsto 1-\sqrt{x} \end{array}$

![Schéma de u]()

$u$ est strictement décroissante, $u([0, {1 \over 4}]) = [{1 \over 2}, 1]$ et $u'(x) = {-1 \over 2\sqrt{x}}$

Donc
$$
J = -2 \int^{1 \over 4}_0 \ln(1 - \sqrt{x})({-dx \over 2 \sqrt{x}})
$$
$$
= -2 \int^{u(1)}_{u(0)} ln(u)\ du = -2 \int^{1 \over 2}_1 ln(u)\ du \\
= 2 \int_{1/2}^1 \ln{u}\ du \\
$$
$$
= 2[u\ln(u) - u]_{1/2}^1 = 2\Big(-1 - \big({1 \over 2}\ln({1 \over 2}) - {1 \over 2}\big)\Big) \\
= \boxed{ln(2) - 1}
$$

\pagebreak

## 4) Intégration par parties (IPP)

*Rappel.*
$$
(uv)' = u'v + uv' \\
\int (uv)' = \int u'v + \int uv'$$

Donc
$$
[u(x)v(x)]^b_a = \int^b_a u'(x)v(x)\ dx + \int^b_a u(x)v'(x)\ dx
$$
Donc $\boxed{\int_a^b u'(x)v(x)\ dx = \Big[u(x)v(x)\Big]_a^b - \int_a^b u(x)v'(x)\ dx}$

*Exemple.*

$$
\int^1_0 xe^x\ dx = [xe^x]^1_0 - \int^1_0 u(x)v'(x)\ dx
$$

Prenons $u(x) = e^x$, $u'(x) = e^x$, $v(x) = x$ et $v'(x) = 1$

Donc $I = e - \int_a^1 e^x\ dx = e - \big[e^x\big]_0^1 = e - (e-1) = \boxed{1}$

*Exemple.*

$$
J = \int_0^{\pi/2} (x^2 + 1)\cos(x)\ dx
$$

Prenons $u(x) = \sin(x)$, $u'(x) = \cos(x)$, $v(x) = x^2+1$ et $v'(x) = 2x$

Donc $J = \Big[(x^2+1)\sin(x)\Big]_0^{\pi/2} - \int_0^{\pi/2} 2x\sin(x)\ dx = {\pi^2 \over 1} + 1 - 2\int_0^{\pi/2} x\sin(x)\ dx = \boxed{1}$

Prenons $u(x) = -\cos(x)$, $u'(x) = \sin(x)$, $v(x) = x$ et $v'(x) = 1$

Donc $\int^{\pi \over 2}_0 xsin(x)\ dx = \Big[-x cos(x)\Big]^{\pi \over 2}_0 + \int^{\pi \over 2}_0 \cos(x)\ dx = \Big[\sin(x)\Big]^{\pi \over 2}_0 = 1$

Donc $\boxed{J = {\pi^2 \over 4} - 1}$

## 5) Décomposition en éléments simples

- **But.** intégrer des fonctions de la forme $P(x) \over Q(x)$ avec $P$ et $Q$ deux polynôes (on appelle ça des fonctions rationnelle en $x$)
- **Étape 1.**
    - si le degré $P <$ le degré de $Q$, alors on ne fait rien
    - si le degré $P \ge$ le degré de $Q$, on va se ramener à une fraction rationnelle ${\widetilde{P} \over \widetilde{Q}}$ avec la décomposition $P <$ le degré de $Q$
      Pour cela, on fait la division euclidienne de $P$ par $Q$.
      C'est-à-dire $P = LQ + R$ avec $L$ et $R$ deux polynômes tq degré $R <$ degré $Q$
      Ansi ${P \over Q} = L + {R \over Q}$
      En pratique, comment trouve-t-on $L$ et $R$ ?

    *Exemple* $P = X^5 + X^4 - X^2 + 1 \\ Q = X^2 - 1$

    ![Division euclidienne de P par Q]()

    Donc $P(x) = \underset{L(X)}{(X^3 + X^3 + X)}\underset{Q(X)}{(X^2 - 1)}+\underset{X+1}{R(X)}$