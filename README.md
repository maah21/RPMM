# 📐 Otimização da Oferta de Disciplinas — BCT Noturno UNIFESP

![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat&logo=python&logoColor=white)
![OR-Tools](https://img.shields.io/badge/OR--Tools-CP--SAT-FF6F00?style=flat&logo=google&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-1.x-013243?style=flat&logo=numpy&logoColor=white)
![Status](https://img.shields.io/badge/Status-Concluído-2ea44f?style=flat)
![Instituição](https://img.shields.io/badge/UNIFESP-ICT--SJC-003087?style=flat)

---

## 📌 Sobre o projeto

Trabalho desenvolvido na disciplina **Resolução de Problemas via Modelagem Matemática (RPMM)** 
do Instituto de Ciência e Tecnologia da **Universidade Federal de São Paulo (UNIFESP)**, 
campus São José dos Campos.

O modelo otimiza a oferta de disciplinas para o curso **BCT Noturno**, decidindo quais 
disciplinas abrir, em qual horário da grade (letras A–E) e quais alunos matricular em 
cada turma — tudo isso **maximizando a satisfação dos estudantes** com base em respostas 
de um formulário de interesse.

---

## 🎯 Problema

A grade do BCT Noturno usa uma estrutura fixa de **letras A–E**, onde cada letra representa 
dois horários semanais:

|          | Seg | Ter | Qua | Qui | Sex |
|----------|:---:|:---:|:---:|:---:|:---:|
| 19–21h   |  A  |  C  |  E  |  B  |  D  |
| 21–23h   |  B  |  D  |  A  |  C  |  E  |

O modelo decide:
- **Quais disciplinas abrir** (filtradas por demanda mínima)
- **Qual letra atribuir** a cada turma (sem conflito de horário por aluno)
- **Quais alunos matricular** em cada turma (respeitando disponibilidade individual)

---

## ⚙️ Modelagem

O problema é formulado como um **Programa Linear Inteiro (PLI)** e resolvido com o 
solver **CP-SAT do Google OR-Tools**.

### Variáveis de decisão

| Variável | Descrição |
|---|---|
| `open_disc[c,k]` | Turma `k` da disciplina `c` está aberta? |
| `choose[c,k,li]` | Turma `k` da disciplina `c` usa a letra `L`? |
| `x[s,c,k]` | Aluno `s` está matriculado na turma `k` da disciplina `c`? |
| `z[s,c,k,li]` | Aluno `s`, turma `k`, disciplina `c`, está na letra `L`? |

### Restrições principais

- Cada turma aberta escolhe **exatamente uma letra** (A–E)
- A mesma disciplina não pode ter duas turmas com a mesma letra
- Aluno não pode ter duas disciplinas na **mesma letra** (conflito de horário)
- Cada turma requer entre **3 e 5 alunos** para ser aberta
- Máximo de **5 disciplinas por aluno**
- Até **2 turmas por disciplina**

### Função objetivo

$$\text{Maximizar} \sum_{s,c,k} W_{s,c} \cdot x_{s,c,k}$$

Onde $W_{s,c}$ é o peso de interesse do aluno $s$ pela disciplina $c$, calculado com base 
nas respostas do formulário (+2 se é disciplina do semestre, +1 se é de interesse, ×1.5 
para alunos do turno noturno).

---

## 🔍 Resultados

- ✅ Solução **ótima ou viável** encontrada em até 120 segundos
- 📊 Grade gerada com disciplinas, turmas, letras e horários
- 👥 Relatório de satisfação individual por aluno
- 📁 Exportação automática para `.xlsx` com 3 abas:
  - **Grade** — disciplinas abertas com letra e horários
  - **Resumo_Turmas** — número de turmas e projeção de alunos reais
  - **Satisfacao** — satisfação percentual por aluno

---

## 🗂️ Estrutura do projeto
