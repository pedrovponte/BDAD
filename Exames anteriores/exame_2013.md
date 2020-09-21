# Teoria

1. B
2. D
3. C
4. A
5. C
6. --
7. B
8. C
9. --
10. B

# Prática

2. 
    1. Tornar os lados direitos singulares:
        * A->B
        * AC->D
        * BD->C
        * B->C
        * B->E
        * E->A

    2. Verificar se há dependências funcionais redundantes: 

        * A->B: {A}<sup>+</sup>={A} $\rightarrow$ não redundante;
        * AC->D: {AC}<sup>+</sup>={A, C, B, E} $\rightarrow$ não redundante;
        * BD->C: {BD}<sup>+</sup>={B, D, C, E, A} $\rightarrow$ redundante (removida daqui para a frente);
        * B->C: {B}<sup>+</sup>={B} $\rightarrow$ não redundante;
        * B->E: {B}<sup>+</sup>={B} $\rightarrow$ não redundante;
        * E->A: {E}<sup>+</sup>={E} $\rightarrow$ não redundante;

    3. Verificar se não há redundância nos atributos do lado esquerdo:
        * removendo A de AC->D: {C}<sup>+</sup>={C};
        * removendo C de AC->D: {A}<sup>+</sup>={A, B, C, E};
        * Logo, não se pode remover nem A nem C desta relação pois dessa forma não seria possivel atingir D.

    4. Forma minimal das dependências funcionais:

        * A->B
        * AC->D
        * B->C
        * B->E
        * E->A
    
    5. Relações
        R<sub>1</sub>(A, B)
        R<sub>2</sub>(A, C, D)
        R<sub>3</sub>(B, C, E)
        R<sub>4</sub>(E, A)

        {A, B}<sup>+</sup>={A, B, C, E, D} $\rightarrow$ R<sub>1</sub> é uma chave, logo superchave de R

        Como há pelo menos uma relação que é superchave, então não necessitamos de adicionar mais nenhuma.

3a. Select hostname, Pessoa.nome
    From Servidor, Pessoa
    Where Servidor.vulneravel = "sim" AND idResponsavel = idPessoa;

3b. Select Servidor.hostname, Bug.descricao, Pessoa.nome
    From Servidor, Bug, Pessoa, AplicacaoServidor, Aplicacao
    Where Bug.vulnerabilidade = "sim" AND
    Servidor.idResponsavel = Pessoa.idPessoa AND
    Aplicacao.idAplicacao = Bug.idAplicacao AND
    AplicacaoServidor.idAplicacao = Aplicacao.idAplicacao AND
    AplicacaoServidor.idServidor = Servidor.idServidor
    Order By Servidor.hostname;

3c. Select hostname
    From Servidor, Pessoa, AplicacaoServidor, Bug
    Where servidor.hostname like "alu%" AND
    Pessoa.idPessoa = Servidor.idResponsavel AND
    Pessoa.mail = "joao.almeida@cica.pt" AND
    AplicacaoServidor.idServidor = Servidor.idServidor AND
    Bug.idAplicacao = AplicacaoServidor.idAplicacao
    GROUP BY Servidor.hostname
    HAVING count(*) > 0;

3d. Select Aplicacao.nome
    From Aplicacao, Bug
    Where Bug.idAplicacao = Aplicacao.idAplicacao
    Group By Aplicacao.nome
    Order By count(*)
    Limit 1;

3e. Drop Trigger if exists AdicionaVulnerabilidade;
    Create Trigger AdicionaVulnerabilidade
    After insert on AplicacaoServidor
    For Each Row
    When exists (
        Select *
        From Aplicacao, Bug
        Where Aplicacao.idAplicacao = New.idAplicacao AND Bug.idAplicacao = New.idAplicacao AND Bug.vulnerabilidade = "sim"
    )
    Begin
        Update Servidor
        Set vulneravel = "sim"
        Where idServidor = New.idServidor;
    END;

3f. Drop Trigger if exists AdicionaBug;
    Create Trigger AdicionaBug
    After insert on Bug
    For each row
    When New.vulnerabilidade = "sim"
    Begin
        Update Servidor
        Set vulneravel = "sim"
        Where EXISTS (
            Select *
            From AplicacaoServidor
            Where AplicacaoServidor.idAplicacao = New.idAplicacao AND AplicacaoServidor.idServidor = Servidor.idServidor
        );
        Update Bug
        Set prioridade = 1
        Where Bug.idBug = New.idBug;
    End;