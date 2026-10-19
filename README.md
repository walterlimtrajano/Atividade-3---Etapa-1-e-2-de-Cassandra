# Atividade Prática 3 - Banco de Dados Colunar (Apache Cassandra)

## 📌 Descrição do Projeto
Este repositório contém a documentação e os registros de execução dos comandos CQL (*Cassandra Query Language*) referentes à **Atividade Prática 3** do módulo de Bancos de Dados NoSQL. 

O objetivo principal desta atividade foi aplicar conceitos práticos de manipulação de um banco de dados colunar, incluindo a criação de *Keyspaces*, tabelas, inserção de dados, atualizações, remoções e consultas utilizando índices secundários.

---

## 🖼️ Registros de Execução e Descrição das Imagens

### **Etapa 1: Criação de Keyspace, Tabela e Inserção de Dados**
* **Imagem 1 (`etapa1.png`):**
  * **Conexão e Keyspace:** Inicialização da interface `cqlsh` e criação do *Keyspace* `unipe` com a estratégia de replicação `SimpleStrategy` e fator de replicação `3`.
  * **Definição da Tabela:** Seleção do *keyspace* (`use unipe;`) e criação da tabela `cadastro` com as colunas `id` (chave primária do tipo `uuid`), `nome` (`text`) e `cargo` (`text`).
  * **Inserção de Dados:** Inserção dos 6 registros iniciais referentes aos professores, alunos e funcionários informados na especificação da atividade.

### **Etapa 2: Atualização, Remoção e Consultas Filtradas**
* **Imagem 2 (`etapa2.png`):**
  * **Atualização de Dados (`UPDATE`):** Alteração bem-sucedida do cargo do registro referente a *Theo* de `'aluno'` para `'funcionario'` através da sua chave primária (`id`).
  * **Remoção de Registro (`DELETE`):** Exclusão do registro de *Afonso* via chave primária (`id`).
  * **Criação de Índice (`CREATE INDEX`):** Criação de um índice secundário na coluna `cargo` para permitir a execução de consultas filtradas por esta propriedade.
  * **Consultas de Validação (`SELECT`):**
    * Busca por `cargo = 'professor'`, retornando apenas o registro do professor *Thyago* (uma vez que *Afonso* foi excluído).
    * Busca por `cargo = 'aluno'`, retornando apenas a aluna *Fernanda* (confirmando que *Theo* passou para a categoria de funcionário).

---

## 💻 Script dos Comandos Utilizados

### **Etapa 1**
```sql
-- 1. Criação e uso do Keyspace
CREATE KEYSPACE unipe WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': '3'};
USE unipe;

-- 2. Criação da tabela
CREATE TABLE cadastro (
    id uuid PRIMARY KEY,
    nome text,
    cargo text
);

-- 3. Inserção dos registros iniciais
INSERT INTO cadastro (id, nome, cargo) VALUES (1a8d6a80-33df-11e5-a151-feff819cdc9f, 'Thyago', 'professor');
INSERT INTO cadastro (id, nome, cargo) VALUES (1a8d649a-33df-11e5-a151-feff819cdc9f, 'Afonso', 'professor');
INSERT INTO cadastro (id, nome, cargo) VALUES (a70ca7ff-6d57-4f89-be89-08421c432bb7, 'Fernanda', 'aluno');
INSERT INTO cadastro (id, nome, cargo) VALUES (04b57f0e-33df-11e5-a151-feff819cdc9f, 'Theo', 'aluno');
INSERT INTO cadastro (id, nome, cargo) VALUES (c4f408dd-00f3-488e-8800-050d2775bbc7, 'Sophia', 'funcionario');
INSERT INTO cadastro (id, nome, cargo) VALUES (04b57c98-33df-11e5-a151-feff819cdc9f, 'Leonardo', 'funcionario');
```

### **Etapa 2**
```sql
-- 1. Alterar o cargo do Theo para funcionario
UPDATE cadastro SET cargo = 'funcionario' WHERE id = 04b57f0e-33df-11e5-a151-feff819cdc9f;

-- 2. Excluir o registro de Afonso
DELETE FROM cadastro WHERE id = 1a8d649a-33df-11e5-a151-feff819cdc9f;

-- 3. Criar índice secundário na coluna 'cargo'
CREATE INDEX ON cadastro (cargo);

-- 4. Consultar professores cadastrados
SELECT * FROM cadastro WHERE cargo = 'professor';

-- 5. Consultar alunos cadastrados
SELECT * FROM cadastro WHERE cargo = 'aluno';
```

---

## 🛠️ Tecnologias Utilizadas
* **Database:** Apache Cassandra
* **CLI:** CQL Shell (`cqlsh`)
* **Ambiente:** WSL / Linux Terminal