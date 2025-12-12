# ⚡ Análise de Geração de Energia (SQL/MySQL)

Este repositório contém um projeto de modelagem de dados e consultas SQL avançadas focado em um cenário de **monitoramento de geração de energia**. O script abrange desde a criação do esquema relacional (DDL) até a inserção de dados (DML) e extração de relatórios analíticos (DQL).

## 📂 Sobre o Projeto

O objetivo é simular um sistema onde clientes possuem geradores de energia distribuídos em diferentes estados. O banco de dados permite rastrear a produção diária de energia e responder a perguntas de negócios essenciais para a gestão da eficiência energética.

### 🗂 Estrutura do Banco de Dados (`energia`)

O modelo relacional foi construído com as seguintes entidades:

* **tb_uf:** Tabela de domínio com os Estados (UF).
* **tb_cliente:** Clientes proprietários dos geradores, vinculados a um estado.
* **tb_gerador:** Equipamentos geradores, vinculados a um cliente.
* **tb_registro_geracao:** Tabela fato contendo o registro diário de energia (kWh) por gerador.



## 🧠 Consultas e Conceitos Aplicados

O script demonstra proficiência nos seguintes conceitos de SQL:

* **Relacionamentos:** Uso de `JOIN` para cruzar dados de 4 tabelas simultaneamente.
* **Agregação:** Utilização de `SUM`, `AVG` e `ROUND` para cálculos estatísticos.
* **Filtragem Avançada:** Uso de `BETWEEN` para intervalos de datas e filtros textuais.
* **Tratamento de Nulos:** Uso de `LEFT JOIN` com `IS NULL` para encontrar geradores inativos (sem produção).
* **Ordenação e Limites:** `ORDER BY` e `LIMIT` para criar rankings (Top 1, Top 10).

### 🔎 Perguntas de Negócio Respondidas

O script soluciona as seguintes questões estratégicas:
1.  **Ranking de Clientes:** Quem gerou mais energia em um período específico?
2.  **Performance Individual:** Qual o gerador mais eficiente de um cliente específico (ex: *Libero Corp.*)?
3.  **Auditoria de Falhas:** Existem geradores que não registraram nenhuma produção no mês?
4.  **Métricas Regionais:** Qual a média diária de geração dos clientes situados no Paraná (PR)?
5.  **Picos de Produção:** Qual foi o dia com a maior soma global de geração de energia?

## 🛠️ Tecnologias

* **SGBD:** MySQL 8.0+
* **Linguagem:** SQL (Structured Query Language)

## ▶️ Como Executar

1.  Certifique-se de ter o MySQL instalado.
2.  Clone este repositório.
3.  Abra o arquivo `script.sql` (ou o arquivo principal do repositório) no seu cliente SQL (MySQL Workbench, DBeaver, etc.).
4.  **Ordem de Execução:**
    * Execute primeiro os comandos de `CREATE TABLE` e `INSERT` (que estão no final do script).
    * Em seguida, execute as consultas `SELECT` (no início do script) para ver as análises.

## 📄 Exemplo de Consulta

Trecho de código para identificar geradores sem produção (Inatividade):

```sql
SELECT
    g.nome_gerador, c.nome_cliente
FROM
    tb_gerador g
JOIN
    tb_cliente c ON g.id_cliente = c.id_cliente
LEFT JOIN 
    tb_registro_geracao rg ON g.id_gerador = rg.id_gerador 
    AND rg.data_registro BETWEEN '2024-01-01' AND '2024-01-31'
WHERE
    rg.id_registro IS NULL; -- Filtra onde não houve "match" de registro
