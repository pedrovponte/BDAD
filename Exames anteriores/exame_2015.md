1. C
2. 
3. C
4. D
5. B
6. B
7. E
8. E
9. B
10. D
11. A chave da relação é {A,F}, pois a partir deste conjunto é possível obter todos os atributos da relação.

    * A->B: {A}<sup>+</sup>={A} $\rightarrow$ não redundante;
    * B->C: {B}<sup>+</sup>={B} $\rightarrow$ não redundante;
    * B->D: {B}<sup>+</sup>={B} $\rightarrow$ não redundante;
    * AD->E: {AD}<sup>+</sup>={A, D, B, C, E} $\rightarrow$ redundante;

    * removendo A de AD->E: {D}<sup>+</sup>={D};
    * removendo D de AD->E: {A}<sup>+</sup>={A, B, C, D, E};
    * Logo, D é irrelevante na relação AD->E, chegamos a E com ou sem ele;

    Forma minimal das dependências funcionais:

    * A->B
    * B->C
    * B->D
    * A->E

    Relações:

    * R<sub>1</sub>(A, B, E)
    * R<sub>2</sub>(B, C, D)
    * R<sub>3</sub>(A, F)

12. Uma relação está na Forma Normal de Boyce-Codd se e só se para qualquer dependência funcional não trivial A<sub>1</sub>A<sub>2</sub>...A<sub>n</sub>$\rightarrow$B<sub>1</sub>B<sub>2</sub>...B<sub>m</sub> em R, {A<sub>1</sub>A<sub>2</sub>...A<sub>n</sub>} é uma superchave de R. O lado esquerdo de qualquer dependência funcional deve ser uma superchave.

**R(A, B, E)**

* A->B
* A->E

A é chave da relação, logo superchave. Assim, não viola a BCNF.

**R(B, C, D)**

* B->C
* B->D

B é chave da relação, logo superchave. Assim, não viola a BCNF.

**R(A, F)**

* A->F

A é chave da relação, logo superchave. Assim, não viola a BCNF.

Assim, as relações obtidas encontram-se na Forma Normal de Boyce-Codd.

13. Equipamento(referencia, dataAquisicao, fornecedor, tipo)
    Sala(id);
    EquipamentoSala(idS->Sala, idE->Equipamento);
    Reserva(id, dataInicio, dataFim);
    ReservaSala(idR->Reserva, duracao, idP->Professor);
    ReservaEquipamento(idR->Reserva);
    Pessoa(id);
    Tecnico(idP->Pessoa);
    Utilizador(idP->Pessoa);
    Aluno(idU->Utilizador);
    Professor(idU->Utilizador);
    TecnicoReserva(idR->Reserva, idT->Tecnico);
    TecnicoEquipamento(idE->Equipamento, idT->Tecnico);
    TecnicoSala(idS->Sala, idT->Tecnico);
    UtilizadorEquipamento(idU->Utilizador, idR->ReservaEquipamento);
    ProfessorSala(idP->Professor, idR->ReservaSala)
    Helpdesk(idT->Tecnico);
    Redes(idT->Tecnico);
    Sistemas(idT->Tecnico);
    Microinformatica(idT->Tecnico);
    ApoioTecnico(id, dataColocacao, dataResolucao, estado, comentariosUtilizador, comentariosTecnico);
    UtilizadorPedido(idU->Utilizador, idA->ApoioTecnico);
    TecnicoPedido(idT->Tecnico, idA->ApoioTecnico);

14. Select caption
    From Photo
    Where julianday(uploadDate) - julianday(creationDate) = 2 AND user IN
    (Select id From User Where name = "Daniel Ramos");

15. Select distinct name
    From User
    Where id not IN (Select user From Photo);

16. Select AVG(users) AS "Media"
    From (Select photo, COUNT(user) AS users
        From AppearsIn
        Where photo IN (
            Select photo
            From Likes
            Group By photo
            Having COUNT(user) > 3
        )
    Group By photo);

17. Drop View if exists Friend1;
    Create View Friend1 As
    Select Friend.user2 AS "user"
    From User, Friend
    Where name = "Daniel Ramos" and user1 = id;

    Drop View if exists Friend2;
    Create View Friend2 As
    Select Friend.user2
    From Friend1, Friend
    Where Friend.user1 In Friend1;

    Select Distinct caption
    From Photo, AppearsIn
    Where id = photo And (AppearsIn.user in Friend1 Or AppearsIn.user in Friend2);

18. Delete From Photo
    Where uploadDate < "2010-01-10" AND id IN (
        Select AppearsIn.photo
        From AppearsIn
        Group BY AppearsIn.photo
        Having count(AppearsIn.user) < 2
    );

19. Drop Trigger if exists likesPhoto;
    Create Trigger likesPhoto
    After Insert On AppearsIn
    For each row
    When (
        Select Likes.user
        From Likes
        Where New.user = Likes.user AND New.photo = Likes.photo
    ) IS NULL
    Begin
        Insert into Likes Values (New.user, New.photo);
    End;