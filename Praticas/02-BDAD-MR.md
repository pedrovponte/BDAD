# Modelo Relacional

## Exercício 1

REFS (T, A, I, C, S#, R, S, E, V, N, PI, PF, Y, M)

D = { S → S#,R,E
      S# → S
      S, V, N → Y, M
      S, V, N, PI → T, PF
      A → I, C
    }

**a)** X = {S#, V, N, A}

X<sup>+</sup> = {S#, S, R, E, Y, M, I, C, V, N}

X não é uma chave pois a sua expansão não contém todos os atributos da relação.

**b)**

**Forma minimal de D**

S → S#
S → R
S → E
S# → S
S, V, N → Y
S, V, N → M
S, V, N, PI → T
S, V, N, PI → PF
A → I
A → C

Não existe redundância nem no lado direito nem no lado esquerdo

**Decomposição na 3ª Forma Normal**

R<sub>1</sub>(S#, R, E)
R<sub>2</sub>(S#, S)
R<sub>3</sub>(S, V, N, Y, M)
R<sub>4</sub>(S, V, N, PI, T, PF)
R<sub>5</sub>(A, I, C)

Key: (S, V, N, PI, A)

**c)** Menos espaço ocupado em disco; redução da informação repetida, reduzindo, assim, a possibilidade de introdução de informação inconsistente na base de dados

**d)**

| S (Sigla) | S# (ISSN) | R (Revista) | E (Editora) |
| :-------: | :-------: | :---------: | :---------: |
| A | 1 | a | b |
| B | 1 | a | b |

Para que isto não aconteça o SGBD deve ser capaz de definir restrições de
unicidade. Dessa forma é possível definir todas as chaves candidatas da
relação no momento da implementação da tabela. De notar que como o
conjunto de atributos da relação S# está contido no conjunto de atributos
de Revista, essa relação (S#) não é necessária, uma vez que a relação
Revista garante a preservação da DF: S# -> S.

**e)** Encontra-se na BCNF pois os lados esquerdos de todas as DF's são super chaves da respetiva relação.

## Exercício 2

R(A, B, C, D, E)

F={AB→C, DE→C, B→D}

**a)** {(A, B, E)}

**b)** 

| A | B | C | D | E |
| :-: | :-: | :-: | :-: | :-: |
| a | b | c | d<sub>1</sub> | e<sub>1</sub> |
| a<sub>2</sub> | b<sub>2</sub> | c | d | e |
| a<sub>3</sub> | b | c<sub>3</sub> | d | e<sub>3</sub> |

| A | B | C | D | E |
| :-: | :-: | :-: | :-: | :-: |
| a | b | c | d | e<sub>1</sub> |
| a<sub>2</sub> | b<sub>2</sub> | c | d | e |
| a<sub>3</sub> | b | c<sub>3</sub> | d | e<sub>3</sub> |

Esta decomposição não garante a junção sem perdas.

**c)**

| A | B | C | D | E |
| :-: | :-: | :-: | :-: | :-: |
| a | b | c | d<sub>1</sub> | e<sub>1</sub> |
| a<sub>2</sub> | b<sub>2</sub> | c | d | e |
| a<sub>3</sub> | b | c<sub>3</sub> | d | e<sub>3</sub> |
| a | b | c<sub>4</sub> | d<sub>4</sub> | e |

| A | B | C | D | E |
| :-: | :-: | :-: | :-: | :-: |
| a | b | c | d | e<sub>1</sub> |
| a<sub>2</sub> | b<sub>2</sub> | c | d | e |
| a<sub>3</sub> | b | c<sub>3</sub> | d | e<sub>3</sub> |
| a | b | c | d | e |

Esta decomposição garante a junção sem perdas.

**d)**

**Forma minimal de R**

AB→C
DE→C
B→D

Não existe redundância nem no lado esquerdo nem no lado direito

**Decomposição na 3ª Forma Normal**

R<sub>1</sub>(A, B, C)
R<sub>2</sub>(D, E, C)
R<sub>3</sub>(B, D)

Key: (A, B, E)

**e)** F = {A→D, BD→E, AC→E, DE→B}

A<sup>+</sup> = {A, D}
B<sup>+</sup> = {B}
C<sup>+</sup> = {C}
AB<sup>+</sup> = {A, B, D, E}
AC<sup>+</sup> = {A, C, E, D, B}
BC<sup>+</sup> = {B, C}
ABC<sup>+</sup> = {A, B, C, D, E}

Assim, o conjunto de DFs que se podem inferir do conjunto F e que envolvem sub-conjuntos de {A, B, C} são: AC -> B.

## Exercício 3

R (C, S, J, D, P, Q, V)

F={JP→C, SD→P, J→S}

**a)** {(J, D, Q, V)}

**b)** R1(S, D, P), R2 (J, S), R3 (C, J, D, Q, V)

Não está nem na 3a FN nem na 2a FN. A relação R3 tem como chave (J, D, Q, V). No entanto JD->C e assim, C não é funcionalmente dependente de JDQV mas só́ de parte, ou seja, de JD. De facto, J->S, SD -> P e PJ -> C.

**Decomposição na 3ª Forma Normal**

R<sub>1</sub>(S, D, P)
R<sub>2</sub>(J, S)
R<sub>3</sub>(J, P, C)
R<sub>4</sub>(J, D, Q, V)

**c)** A dependência funcional JP→C não é preservada. Basta verificar que os três atributos não estão presentes numa mesma relação.

**d)** F1={C→CSJDPQV, JP→C, SD→P, J→S}

~~C→C~~ Trivial
~~C→S~~ C→J e J→S
C→J
C→D
~~C→P~~ C→DJ, J→S e SD→P
C→Q
C→V
JP→C
SD→P
J→S

## Exercício 4

R(CPHSAN)

C – Cadeira; P – Professor; H – Hora; S – Sala; A – Aluno; N – Nota

* Cada cadeira tem um professor responsável;
* Só pode estar uma cadeira numa sala a uma hora;
* Um professor só pode estar numa sala a uma certa hora;
* Cada estudante só tem uma nota a cada cadeira;
* Um aluno só pode estar numa sala em cada instante.

**a)** 

C→P
S,H→C
P,H→S
A,C→N
A,H→S

**b)** {(A, H)}

**c)**

R<sub>1</sub>(C,P) preserva C→P
R<sub>2</sub>(S,H,C) preserva S→H
R<sub>3</sub>(P,H,S) preserva P,H→s
R<sub>4</sub>(A,C,N) preserva A,C→N
R<sub>5</sub>(A,H,S) preserva A,H→S; super-chave de R

**d)** Todas as relações em c. encontram-se na FNBC, pois os lados esquerdos das DFs são chave (logo super-chave) nas respetivas relações.