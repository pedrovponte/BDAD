# Álgebra Relacional

**1.** π nr (Aluno)

**2.** π cod,design (σ curso = 'AC' (Cadeira))

**3.** π Nome (Aluno) ∩ π Nome (Prof) ou 
π Aluno.Nome, Prof.Nome (σ Aluno.Nome=Prof.Nome ((Aluno)⨯(Prof)))

**4.** π Nome (Aluno) - π Nome (Prof) ou 
π Aluno.Nome ((Aluno) - ((Aluno) ⋉ (Prof)))

**5.** π Nome (Aluno) ∪ π Nome (Prof)

**6.** π Nome (σ Prova.cod = 'TS1' (Aluno ⨝ Prova)) ou
π Nome (Aluno ⋉ π nr (σ cod='TS1' Prova)) ou
π Aluno.Nome ( σ cod = 'TS1' (Aluno ⨝ Aluno.nr=Prova.nr Prova))

**7.** π Aluno.Nome (σ Cadeira.curso = 'IS' (Cadeira ⨝ Prova) ⨝ Aluno)

**8.** π Nome (σ cod = 'TS1' ∧ nota >= 10 (Aluno ⨝ Prova)) ∩ π Nome (σ cod = 'BD' ∧ nota >= 10 (Aluno ⨝ Prova)) ∩ π Nome (σ cod = 'EIA' ∧ nota >= 10 (Aluno ⨝ Prova))

**9.** γ max(Prova.nota) -> max (Prova)

**10.** γ avg(nota)->nota (σ cod='BD' Prova)

**11.** γ count(Aluno.nr) -> count (Aluno)

**12.** γ count(cod)->nr (σ curso='IS' (Cadeira)) ∪ γ count(cod)->nr (σ curso = 'AC' (Cadeira))