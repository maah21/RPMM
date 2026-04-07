# Otimização da Oferta de Disciplinas — BCT Noturno UNIFESP

![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat&logo=python&logoColor=white)
![OR-Tools](https://img.shields.io/badge/OR--Tools-CP--SAT-FF6F00?style=flat&logo=google&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-1.x-013243?style=flat&logo=numpy&logoColor=white)
![openpyxl](https://img.shields.io/badge/openpyxl-Excel%20I%2FO-217346?style=flat&logo=microsoft-excel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Concluído-2ea44f?style=flat)
![Instituição](https://img.shields.io/badge/UNIFESP-ICT--SJC-003087?style=flat)

---

## Sobre o projeto

Este projeto foi desenvolvido na disciplina **Resolução de Problemas via Modelagem Matemática (RPMM)**, no Instituto de Ciência e Tecnologia da **Universidade Federal de São Paulo (UNIFESP)**, campus São José dos Campos.

O objetivo é **otimizar a oferta de disciplinas do curso BCT Noturno**, decidindo:

- quais disciplinas devem ser abertas;
- quantas turmas abrir por disciplina;
- qual letra da grade horária atribuir a cada turma;
- quais alunos alocar em cada turma.

A solução foi construída em **Python**, utilizando **Google OR-Tools (CP-SAT)** para modelagem e resolução do problema, além de **Pandas** e **NumPy** para tratamento dos dados, organização dos resultados e geração de relatórios em Excel.

---

## Contexto do problema

A grade do **BCT Noturno** segue uma estrutura fixa de letras **A–E**, em que cada letra corresponde a dois horários semanais:

|          | Seg | Ter | Qua | Qui | Sex |
|----------|:---:|:---:|:---:|:---:|:---:|
| 19h–21h  |  A  |  C  |  E  |  B  |  D  |
| 21h–23h  |  B  |  D  |  A  |  C  |  E  |

Com base nessa estrutura, o modelo busca gerar uma grade viável e eficiente, respeitando restrições acadêmicas e maximizando a satisfação dos estudantes a partir de um formulário de interesse.

---

## Objetivo

Maximizar a satisfação dos alunos na montagem da oferta de disciplinas, respeitando:

- grade horária fixa com letras **A–E**;
- disponibilidade individual dos estudantes;
- limite de disciplinas por aluno;
- capacidade mínima e máxima por turma;
- limite de até **2 turmas por disciplina**;
- ausência de conflito de horários.

---

## Modelagem matemática

O problema foi formulado como um **Problema de Programação Linear Inteira** e resolvido com o solver **CP-SAT** do Google OR-Tools.

### Variáveis de decisão

| Variável | Descrição |
|---|---|
| `open_disc[c,k]` | Indica se a turma `k` da disciplina `c` está aberta |
| `choose[c,k,li]` | Indica se a turma `k` da disciplina `c` foi atribuída à letra `L` |
| `x[s,c,k]` | Indica se o aluno `s` foi matriculado na turma `k` da disciplina `c` |
| `z[s,c,k,li]` | Relaciona a matrícula do aluno `s` à letra `L` da turma escolhida |

### Principais restrições

- Cada turma aberta deve escolher **exatamente uma letra**.
- A mesma disciplina não pode ter duas turmas com a mesma letra.
- Um aluno não pode cursar duas disciplinas na mesma letra.
- Cada turma aberta deve ter entre **3 e 5 alunos**.
- Cada aluno pode cursar no máximo **5 disciplinas**.
- Cada disciplina pode abrir no máximo **2 turmas**.

### Função objetivo

\[
\text{Maximizar } \sum_{s,c,k} W_{s,c} \cdot x_{s,c,k}
\]

Onde \(W_{s,c}\) representa o peso de interesse do aluno `s` pela disciplina `c`, calculado a partir das respostas do formulário:

- `+2` se a disciplina é do semestre;
- `+1` se a disciplina foi marcada como de interesse;
- multiplicação por `1.5` para alunos do turno noturno.

---

## Dados de entrada

O modelo utiliza dois arquivos principais:

- `respostas_formulario.xlsx`  
  Contém as respostas dos alunos, incluindo:
  - disciplinas de interesse;
  - disciplinas do semestre;
  - disponibilidade de horário.

- `Planilha Departamento 2025.xlsx`  
  Contém a oferta oficial de disciplinas e turmas do departamento.

---

## Tecnologias utilizadas

| Tecnologia | Aplicação no projeto |
|---|---|
| **Python** | Implementação do modelo, processamento e automação |
| **Google OR-Tools (CP-SAT)** | Resolução do problema de otimização |
| **Pandas** | Leitura, limpeza, cruzamento e exportação dos dados |
| **NumPy** | Construção de matrizes de pesos e disponibilidade |
| **openpyxl** | Escrita do arquivo final `.xlsx` com múltiplas abas |
| **Google Colab** | Ambiente de desenvolvimento e execução |

---

## Resultados gerados

Ao final da execução, o projeto gera automaticamente um arquivo Excel com múltiplas abas contendo:

- **Grade**  
  Disciplinas abertas, turmas, letras e horários atribuídos.

- **Resumo_Turmas**  
  Quantidade de turmas por disciplina, número de interessados e projeção de alunos reais.

- **Satisfacao**  
  Indicadores de satisfação individual dos alunos com base na alocação obtida.

Além disso, o notebook também permite gerar um **diagnóstico detalhado por disciplina**, incluindo:

- número de interessados no formulário;
- candidatos elegíveis;
- disponibilidade por letra;
- quantidade de alunos alocados;
- exemplos de alocação e alunos não atendidos.

---

## Como executar

O projeto foi desenvolvido no **Google Colab**.

### 1. Abrir o notebook

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/maah21/RPMM/blob/main/RPMM.ipynb)

### 2. Fazer upload dos arquivos de entrada

No Colab, envie os arquivos:

- `respostas_formulario.xlsx`
- `Planilha Departamento 2025.xlsx`

### 3. Instalar as dependências

```bash
!pip install -q ortools openpyxl