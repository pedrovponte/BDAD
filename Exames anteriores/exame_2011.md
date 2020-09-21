# Teórica

1. 
    i. B
    ii. B
    iii. A
    iv. --
    v. C
    vi. D
    vii. D
    viii. --
    ix. --
    x. --

# Pratica

3a. 
    
    Crete Table Paciente (
        numUtente Integer Not Null,
        nome Varchar2(100),
        dataNascimento Date,
        sexo Varchar2(1),
        morada Varchar2(200),
        telf Varchar2(9),
        notas Varchar2(200),
        codPostal Varchar2(8),
        Constraint Paciente_PK Primary Key (numUtente)
    )

    Create Table Analise_Tipo (
        tipoAnalise Integer Not Null Autoincrement,
        nome Varchar2(100),
        Constraint Analise_Tipo_PK Primary Key (tipoAnalise)
    )

    Create Table Analise (
        idAnalise Integer Not Null Autoincrement,
        tipoAnalise Integer Not Null,
        numUtente Integer Not Null,
        dataHora Date,
        valor Number,
        notas Varchar2(200),
        bool_alerta Integer,
        Constraint Analise_PK Primary Key (idAnalise),
        Constraint Analise_FK1 Foreign Key (tipoAnalise) References Analise_Tipo(tipoAnalise),
        Constraint Analise_FK2 Foreign Key (numUtente) References Paciente(numUtente)
    )

    Create Table Paciente_Limite (
        numUtente Integer Not Null,
        tipoAnalise Integer Not Null,
        limInf Number,
        limSup Number,
        Constraint Paciente_Limite_PK Primary Key (numUtente, tipoAnalise),
        Constraint Paciente_Limite_FK1 Foreign Key (numUtente) References Paciente (numUtente),
        Constraint Paciente_Limite_FK2 Foreign Key (tipoAnalise) References Analise_Tipo(tipoAnalise)
    )

3b. 