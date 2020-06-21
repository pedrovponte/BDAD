1. D
2. E
3. E
4. B
5. C
6. A
7. C
8. E
9. A
10. B
11. C
    |                         |   |
    |:-----------------------:|:--------------------------------------------:|
    |{A}<sup>+</sup>={A}      |{A, E}<sup>+</sup>={A, B, C, D, E, F, G, H}   |
    |{B}<sup>+</sup>={B, E, }   |{B, C, D}<sup>+</sup>={A, B, C, D, E, F, G, H}|
    |{C}<sup>+</sup>={C}      |{A, C}<sup>+</sup>={A, C}                     |
    |{D}<sup>+</sup>={D, F, H, B, A, E, C, G}|{A, C, D}<sup>+</sup>={A, B, C, D, E, F, G, H}|
    |{E}<sup>+</sup>={E, B, C}|
    |{F}<sup>+</sup>={F, B, A, E, C, D, G, H}|
    |{G}<sup>+</sup>={G}      |
    |{H}<sup>+</sup>={H}      |

12. Para que uma relação de encontre na forma normal de Boyce-Codd, é necessário que para qualquer dependência funcional não trivial A<sub>1</sub>A<sub>2</sub>...A<sub>n</sub>$\rightarrow$B<sub>1</sub>B<sub>2</sub>...B<sub>m</sub> em R, {A<sub>1</sub>A<sub>2</sub>...A<sub>n</sub>} é uma superchave de R, ou seja, o lado esquerdo de qualquer dependência funcional deve ser uma superchave.

    {F}<sup>+</sup>={BAECDGHF};
    {B}<sup>+</sup>={BEC};

    Como a partir da DF B$\rightarrow$E não é possível obter o conjunto R, então R não se encontra na forma normal de Boyce-Codd.

13. Pessoa(id, pais, filiacao) $\rightarrow$pais, id não nulo **Area cientifica?**;
    
    Sessao(horaInicio, horaFim, idA->Autor, idO->Orador) $\rightarrow$idA, horaInicio, horaFim não nulo; horaInicio < horaFim;
    
    PublicacaoSessao(idP->Publicacao, idS->Sessao, ordem) $\rightarrow$ idO, idS, ordem nao nulo; ordem > 0;

    Orador(idP->Pessoa, idS->Sessao, biografia) $\rightarrow$idP, idS, biografia nao nulo;
    
    Participante(idP->Pessoa, dataInscricao, tipoInscricao, preco) $\rightarrow$ idP, dataInscricao, tipoInscricao, preco nao nulo; preco > 0;

    Autor(idP->Pessoa) $\rightarrow$;

    Revisor(idP->Pessoa) $\rightarrow$ idP nao nulo;

    Publicação(id, titulo, resumo, palavrasChave) $\rightarrow$ id, idA não nulo; palavrasChave.size >= 1 && palavrasChave.size <= 5;

    PalavraPublicacao(idP->Publicacao, palavra); idP nao nulo; maximo de 5 palavras por publicacao e minimo de 1;

    AutorPublicacao(idA->Autor, idP->Publicacao, ordem) $\rightarrow$ idA, idP nao nulo; ordem > 0;

    Revisao(idP->Publicacao, idR->Revisor, classificacao, descricao) $\rightarrow$ idP, idR nao nulos; classificacao > 0;

14. Select Estudante.nome as "Estudante"
    From Estudante, Curso
    Where Estudante.nome like "%a%" AND Curso.nome = "MIEIC" AND Estudante.curso = Curso.id;

15. Select E1.nome
    From Estudante E1, Amizade
    Where E1.id = Amizade.ID1 and Amizade.ID2 IN (Select id From Estudante Where nome like "Gabriel%");

16. Select E0.nome
    From Estudante E0, Estudante E1, Estudante E2, Estudante E3, Estudante E4, Estudante E5, Amizade A1, Amizade A2, Amizade A3, Amizade A4, Amizade A5
    Where A1.ID1 = E0.id AND A2.ID1 = E0.id AND A3.ID1 = E0.id AND A4.ID1 = E0.id AND A5.ID1 = E0.id AND A1.ID2 = E1.id AND A2.ID2 = E2.id AND A3.ID2 = E3.id AND A4.ID2 = E4.id AND A5.ID2 = E5.id AND E1.anoCurricular = 1 AND E2.anoCurricular = 2 AND E3.anoCurricular = 3 AND E4.anoCurricular = 4 AND E5.anoCurricular = 5;

17. CREATE TABLE AmizadeTransitiva (
      ID1 int REFERENCES Estudante(ID),
      ID2 int REFERENCES Estudante(ID),
      PRIMARY KEY (ID1, ID2)
    );

    INSERT INTO AmizadeTransitiva
        Select DISTINCT A1.ID1, A2.ID2
        From Amizade A1, Amizade A2
        Where A1.ID2 = A2.ID1
        And A1.ID1 <> A2.ID2
        And A2.ID2 NOT IN (
            Select A3.ID2 From Amizade A3 Where A3.ID1 = A1.ID1
        )
        ORDER BY A1.ID1, A2.ID2;

18. Select E1.Nome, E2.Nome
    From Estudante E1, Estudante E2, Amizade
    Where E1.ID = Amizade.ID1 AND E2.ID = Amizade.ID2 AND E1.curso <> E2.curso AND E1.ID < E2.ID;

19. Create Trigger AmizadesRemove
    After Delete on Amizade
    For each row
    Begin
        Delete from Amizade where (ID1, ID2) = (Old.ID1, Old.ID2) OR (ID1, ID2) = (Old.ID2, Old.ID1);
    End;

    Create Trigger AmizadeInsert
    After Insert on Amizade
    For each row
    Begin
        Insert into Amizade values (New.ID2, New.ID1);
    END;

    Create Trigger AmizadeUpdate
    Before Update on Amizade
    For each row
    Begin
        Select raise(ignore);
    End;
