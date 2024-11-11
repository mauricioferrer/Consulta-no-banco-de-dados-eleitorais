# Consulta SQL - Base de Dados Eleitorais

Este repositório contém consultas SQL aplicadas à base de dados `basedosdados.br_poder360_pesquisas.microdados`, com foco na análise de informações eleitorais, como pesquisas eleitorais, margens de erro, quantidade de entrevistas e a distribuição de dados por estado e ano. As consultas utilizam diferentes cláusulas para filtrar, agrupar e ordenar os resultados de acordo com requisitos específicos.

## Consultas

### 1. Seleção de Dados com Filtros e Ordenação
```sql
SELECT *
FROM `basedosdados.br_poder360_pesquisas.microdados`
WHERE cargo LIKE 'p%' 
  AND nome_municipio IS NOT NULL 
  AND ano IN (2002, 2014, 2020)
ORDER BY ano, data
LIMIT 10;
```

**Cláusulas utilizadas:**
- **WHERE**: Filtra os dados para selecionar apenas os registros onde:
  - O valor da coluna `cargo` começa com 'p' (usando o operador `LIKE`).
  - A coluna `nome_municipio` não seja nula.
  - O ano da pesquisa esteja entre 2002, 2014 ou 2020 (usando `IN`).
- **ORDER BY**: Ordena os resultados primeiro pelo ano (`ano`), e depois pela data (`data`).
- **LIMIT**: Limita a consulta a 10 registros.

### 2. Análise das Margens de Erro
```sql
SELECT MAX(margem_mais) AS max_margem,
       MIN(margem_mais) AS min_margem
FROM `basedosdados.br_poder360_pesquisas.microdados` 
WHERE margem_mais > 0
LIMIT 100;
```

**Cláusulas utilizadas:**
- **SELECT**: Utiliza as funções de agregação `MAX()` e `MIN()` para encontrar, respectivamente, a maior e a menor margem de erro (`margem_mais`).
- **WHERE**: Filtra os dados para considerar apenas as margens de erro que são maiores que 0 (`margem_mais > 0`).
- **LIMIT**: Limita os resultados a 100 registros.

### 3. Contagem de Estados Distintos
```sql
SELECT count(distinct sigla_uf) as qto_estados
FROM `basedosdados.br_poder360_pesquisas.microdados`
```

**Cláusulas utilizadas:**
- **SELECT**: Utiliza a função de agregação `count(distinct ...)` para contar o número de estados distintos (`sigla_uf`).
  
### 4. Análise de Quantidade de Entrevistas por Estado e Ano
```sql
SELECT ano, sigla_uf, 
       ROUND(AVG(quantidade_entrevistas)) as avg_entrevistas,
       CASE 
           WHEN AVG(quantidade_entrevistas) > 1000 THEN 'ALTA'
           WHEN AVG(quantidade_entrevistas) BETWEEN 815 AND 834 THEN 'MÉDIA'
           ELSE 'BAIXA' 
       END AS categoria_entrevistas
FROM `basedosdados.br_poder360_pesquisas.microdados`
WHERE ano > 2010 AND sigla_uf IS NOT NULL
GROUP BY ano, sigla_uf
HAVING avg_entrevistas > 800
ORDER BY ano DESC;
```

**Cláusulas utilizadas:**
- **SELECT**: Retorna o ano (`ano`), sigla do estado (`sigla_uf`), a média arredondada da quantidade de entrevistas (`AVG(quantidade_entrevistas)`) e uma categorização da média com base nos valores definidos (usando a cláusula `CASE`).
- **WHERE**: Filtra os dados para considerar apenas os anos maiores que 2010 e onde a sigla do estado (`sigla_uf`) não seja nula.
- **GROUP BY**: Agrupa os resultados por ano (`ano`) e sigla do estado (`sigla_uf`).
- **HAVING**: Filtra os grupos para incluir apenas aqueles com uma média de entrevistas superior a 800.
- **ORDER BY**: Ordena os resultados de forma decrescente com base no ano (`ano`).

## Descrição Geral das Cláusulas Utilizadas

- **`WHERE`**: Utilizada para filtrar registros com base em condições específicas.
- **`LIKE`**: Usado para realizar comparações com padrões de texto.
- **`IN`**: Usado para verificar se um valor está dentro de uma lista especificada.
- **`ORDER BY`**: Ordena os resultados de acordo com uma ou mais colunas.
- **`LIMIT`**: Restringe o número de registros retornados pela consulta.
- **`MAX()` e `MIN()`**: Funções de agregação que retornam, respectivamente, o valor máximo e mínimo de uma coluna.
- **`COUNT()`**: Função de agregação que retorna o número de registros distintos ou totais.
- **`AVG()`**: Função de agregação que calcula a média de uma coluna numérica.
- **`ROUND()`**: Função que arredonda o resultado de uma expressão numérica.
- **`CASE`**: Condicional SQL usada para criar categorias ou realizar decisões baseadas em valores.
- **`GROUP BY`**: Agrupa os resultados por uma ou mais colunas.
- **`HAVING`**: Filtra os grupos resultantes após o `GROUP BY`.
  
Essas consultas fornecem informações detalhadas sobre as pesquisas eleitorais, como margens de erro, quantidade de entrevistas, e a distribuição de dados por estado e ano. As cláusulas usadas ajudam a segmentar e organizar os dados para facilitar análises mais profundas.
