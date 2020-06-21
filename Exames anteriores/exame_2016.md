1. C
2. A
3. E
4. C
5. C
6. B
7. E
8. E
9. A
10. B
11. B
    | | |
    |:------------------------:|:--------------------------:|
    |{A}<sup>+</sup>={A}       | {AF}<sup>+</sup>={A, F, B, E} |
    |{B}<sup>+</sup>={B, E}    | {ACD}<sup>+</sup>={A, C, D} |
    |{C}<sup>+</sup>={C}       | {AFC}<sup>+</sup>={A, F, C, B, D, G, H, E} |
    |{D}<sup>+</sup>={D}       |
    |{E}<sup>+</sup>={E}       |
    |{F}<sup>+</sup>={F, B, E} |
    |{G}<sup>+</sup>={G, H}    |
    |{H}<sup>+</sup>={H}       |

12. Uma relação R está na Forma Normal de Boyce-Codd se e só se para qualquer dependência funcional não trivial A<sub>1</sub>A<sub>2</sub>...A<sub>n</sub>$\rightarrow$B<sub>1</sub>B<sub>2</sub>...B<sub>m</sub> em R, {A<sub>1</sub>A<sub>2</sub>...A<sub>n</sub>} é uma superchave de R. O lado esquerdo de qualquer dependência funcional deve ser uma superchave.

    A chave da relação é {A, F, C}.

    A primeira relação, ABC$\rightarrow$ não é uma superchave de R pois não contém a chave de R.

    Logo, a relação R não está na Forma Normal de Boyce-Codd.

13. Produto(id, nome, descricao, categoria) $\rightarrow$ id Unique;
    
    PrecoProduto(id, idProduto->Produto, preco) $\rightarrow$ id Unique, preco > 0;

    Fornecedor(id) $\rightarrow$ id Unique;

    FornecedorProduto(idF->Fornecedor, idP->Produto, idPr->PrecoProduto);

    Encomenda(id, idF->Fornecedor) $\rightarrow$ id Unique;

    EncomendaProduto(idE->Encomenda, idP->Produto, quantidade, idP->PrecoProduto) $\rightarrow$ quantidade > 0;

    Loja(id, contacto, localizacao) $\rightarrow$ id Unique;

    ProdutoLoja(idP->Produto, idL->Loja, quantidade) $\rightarrow$ quantidade > 0;

    Cliente(id, nome) $\rightarrow$ id Unique;

    Compra(id, idC->Cliente, idL->loja) $\rightarrow$ id Unique;

    ProdutoCompra(idC->Compra, idP->Produto, idP->Preco);

14. Select E.nome AS "Estudante", C.nome AS "Curso"
    From Estudante E, Curso C
    Where E.anoCurricular = 3 AND E.curso = C.id;

15. Select E1.nome AS "nome"
    From Estudante E1, Amizade
    Where E1.id = Amizade.ID1
    Group By Amizade.ID1
    Having count(amizade.ID2) > 3;

16. Select E1.nome AS "Nome", E1.anoCurricular AS "Ano Curricular"
    From Estudante E1
    Where E1.id IN 
    (Select E1.id 
    From Estudante E2, Amizade
    Where E1.anoCurricular = E2.anoCurricular AND Amizade.ID1 = E1.id AND Amizade.ID2 = E2.id AND E1.id <> E2.id) AND E1.id NOT IN
    (Select E1.id
    From Estudante E2, Amizade
    Where E1.anoCurricular <> E2.anoCurricular AND Amizade.ID1 = E1.id AND Amizade.ID2 = E2.id AND E1.id <> E2.id)
    ORDER BY E1.anoCurricular, E1.nome;

17. Drop View Friend;
    Drop View Friend2;
    Drop View Friend3;

    Create View Friend AS
    Select Amizade.ID2
    From Amizade, Estudante
    Where Amizade.ID1 = Estudante.id AND Estudante.nome = "Miguel Sampaio";

    Create View Friend2 AS
    Select Amizade.ID2
    From Amizade, Friend
    Where Amizade.ID1 IN Friend;

    Create View Friend3 AS
    Select Amizade.ID2 AS ID
    From Amizade, Friend2
    Where Amizade.ID1 IN Friend2;

    Select DISTINCT Friend3.ID AS "ID"
    From Friend3;

18. Select Estudante.nome AS "nome", Estudante.anoCurricular
    From Estudante, Amizade
    Where Estudante.ID = Amizade.ID1
    Group By Amizade.ID1
    Having count(Amizade.ID2) = (
        Select Max(num)
        From (
            Select count(Amizade.ID2) AS "num"
            From Estudante, Amizade
            Where Estudante.ID = Amizade.ID1
            Group By Amizade.ID1
        )
    );

19. Create Trigger AddFriends
    After Insert On Estudante
    For each row
    Begin
        Insert into Amizade Values
            Select (New.ID, Estudante.ID)
            From Estudante
            Where Estudante.curso = New.curso AND New.ID <> Estudante.ID;
        Insert into Amizade Values
            Select Estudante.ID, New.ID
            From Estudante
            Where Estudante.curso = New.curso AND New.ID <> Estudante.ID;
    End;  
