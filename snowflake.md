<div style="width: 100%; border: 1px solid #dfe2e5; overflow: hidden; margin-bottom: 16px;">
 <div style="width: 100%; background-image: url('https://raw.githubusercontent.com/exx3c/exx3c.github.io/refs/heads/main/snowflake.png'); background-size: cover; background-position: center; height: 110px;"></div>
</div>

# Snowflake: Primeiros Passos com Pipeline de Dados no Snowflake

Este artigo apresenta conceitos essenciais do Snowflake para Engenharia de Dados, com exemplos práticos de ingestão, transformação e análise de dados. É uma introdução acessível para quem deseja entender as funcionalidades básicas da plataforma e aprender a executar operações simples e essenciais de maneira eficiente.

## Snowflake Notebook (Pipeline de Dados):

<h3>Etapa 1: Criação da Tabela de Clientes</h3>

```sql
CREATE OR REPLACE TABLE Clientes (
    ClienteID INT,
    Nome STRING,
    Idade INT,
    Genero CHAR(1),
    Cidade STRING,
    DataCadastro DATE,
    ValorCompra FLOAT
)
```
<table>
 <tr>
  <th></th>
  <th>status</th>
 </tr>
 <tr>
  <td>0</td>
  <td>Table CLIENTES successfully created.</td>
 </tr>
</table>

---

<h3>Etapa 2: Ingestão de Dados para Tabela Clientes</h3>
<p>Nessa etapa os dados em CSV já estão previamente carregados em um Stage Interno.</p>

```sql
COPY INTO Clientes
FROM @DATA/clientes_dados.csv

FILE_FORMAT = (
    TYPE = 'CSV',
    FIELD_OPTIONALLY_ENCLOSED_BY = '"',
    SKIP_HEADER = 1
)

ON_ERROR = 'CONTINUE'
```

<table>
 <tr>
  <th></th>
  <th>file</th>
  <th>status</th>
  <th>rows_parsed</th>
  <th>rows_loaded</th>
  <th>error_limit</th>
  <th>errors_seen</th>
 </tr>
 <tr>
  <td>0</td>
  <td>data/clientes_dados.csv</td>
  <td>LOADED</td>
  <td>1,000</td>
  <td>1,000</td>
  <td>1,000</td>
  <td>0</td>
 </tr>
</table>

---

<h3>Etapa 2.1: Visualização dos Primeiros Registros</h3>

```sql
SELECT * FROM Clientes LIMIT 20
```

<table>
  <thead>
    <tr>
     <th></th>
      <th>ClienteID</th>
      <th>Nome</th>
      <th>Idade</th>
      <th>Genero</th>
      <th>Cidade</th>
      <th>DataCadastro</th>
      <th>ValorCompra</th>
    </tr>
  </thead>
  <tbody>
    <tr>
     <td>0</td>
      <td>1</td>
      <td>Alice</td>
      <td>25</td>
      <td>F</td>
      <td>São Paulo</td>
      <td>2023-01-10</td>
      <td>150.50</td>
    </tr>
    <tr>
     <td>1</td>
      <td>2</td>
      <td>Bob</td>
      <td>45</td>
      <td>M</td>
      <td>Rio de Janeiro</td>
      <td>2022-12-15</td>
      <td>200.75</td>
    </tr>
    <tr>
     <td>2</td>
      <td>3</td>
      <td>Charlie</td>
      <td>35</td>
      <td>M</td>
      <td>Belo Horizonte</td>
      <td>2023-01-20</td>
      <td>320.90</td>
    </tr>
  </tbody>
</table>

---

<h3>Etapa 3: Criação da Tabela ETL_Clientes</h3>
<p>Criamos uma tabela separadamente para trabalharmos a transformação dos dados.</p>

```sql
CREATE OR REPLACE TABLE ETL_Clientes AS
SELECT * FROM Clientes
```

<table>
 <tr>
  <th></th>
  <th>status</th>
 </tr>
 <tr>
  <td>0</td>
  <td>Table ETL_CLIENTES successfully created.</td>
 </tr>
</table>

---

<h3>Etapa 4: Adicionando a Coluna FaixaEtaria</h3>

```sql
ALTER TABLE ETL_Clientes
ADD COLUMN FaixaEtaria STRING
```

<table>
 <tr>
  <th></th>
  <th>status</th>
 </tr>
 <tr>
  <td>0</td>
  <td>Statement executed successfully.</td>
 </tr>
</table>

---

<h3>Etapa 5: Atualizando FaixaEtaria Baseado na Idade</h3>

```sql
UPDATE ETL_Clientes
SET FaixaEtaria = CASE
    WHEN Idade < 18 THEN 'Jovem'
    WHEN Idade >= 18 AND Idade < 65 THEN 'Adulto'
    WHEN Idade >= 65 THEN 'Idoso'
    ELSE 'Indefinido'
END
```

<table>
 <tr>
  <th></th>
  <th>numbers of rows updated</th>
 </tr>
 <tr>
  <td>0</td>
  <td>1,000</td>
 </tr>
</table>

---

<h3>Etapa 6: Criando uma VIEW ClientesMedias</h3>
<p>Criamos uma VIEW extraindo insights importantes sobre a tabela Clientes.</p>

```sql
CREATE OR REPLACE VIEW ClientesMedias AS
    SELECT Genero, FaixaEtaria,
        COUNT(FaixaEtaria) AS Quantidade,
        CAST(AVG(Idade) AS INT) AS IdadeMedia,
        ROUND(AVG(ValorCompra), 2) AS ValorCompraMedio
    FROM ETL_Clientes
    GROUP BY FaixaEtaria, Genero
    ORDER BY FaixaEtaria, Genero
```

<table>
 <tr>
  <th></th>
  <th>status</th>
 </tr>
 <tr>
  <td>0</td>
  <td>View CLIENTESMEDIAS successfully created.</td>
 </tr>
</table>

---

<h3>Visualizando a VIEW ClientesMedias</h3>

```sql
SELECT * FROM ClientesMedias
```

<table>
  <thead>
    <tr>
     <th></th>
      <th>Genero</th>
      <th>FaixaEtaria</th>
      <th>Quantidade</th>
      <th>IdadeMedia</th>
      <th>ValorCompraMedio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
     <td>0</td>
      <td>F</td>
      <td>Adulto</td>
      <td>464</td>
      <td>40</td>
      <td>2,513.14</td>
    </tr>
    <tr>
     <td>1</td>
      <td>M</td>
      <td>Adulto</td>
      <td>435</td>
      <td>41</td>
      <td>2,534.93</td>
    </tr>
   <tr>
     <td>2</td>
      <td>F</td>
      <td>Idoso</td>
      <td>41</td>
      <td>67</td>
      <td>2,351.51</td>
    </tr>
   <tr>
     <td>3</td>
      <td>M</td>
      <td>Idoso</td>
      <td>60</td>
      <td>67</td>
      <td>2,146.78</td>
    </tr>
  </tbody>
</table>

---

## Snowflake Dashboards (Análise de Dados):
O Snowflake também oferece a funcionalidade de criação de Dashboards e Gráficos para análise visual. Aqui exploramos um pouco dessas funcionalidades utilizando a tabela ```Clientes``` como fonte de dados.

<h2>Relacionando Idade com ValorCompra através do Gráfico de Barras</h2>

![image](https://github.com/user-attachments/assets/d60aceb4-0c1f-4c55-b49b-975aa7406511)
<br>
<h2>Analisando médias com Heatgrid através da relação entre Cidade e FaixaEtaria</h2>

![image](https://github.com/user-attachments/assets/253735c4-9614-4b62-b370-3e26b61dc12e)

# Conclusão

## Recursos e Referências

- [Snowflake Reference Documentation](https://docs.snowflake.com/en/reference)
- [SQL Statements Examples](https://www.w3schools.com/sql/default.asp)

## Agradecimentos e Contato

Obrigado por acompanhar este artigo! Fique à vontade para enviar sugestões ou se apenas quer dizer olá, fique à vontade para me contatar!

- **Email:** [gabriel.dultra.239@gmail.com](mailto:gabriel.dultra.239@gmail.com)
- **LinkedIn:** [gabriel-dultra](https://www.linkedin.com/in/gabriel-dultra/)
- **GitHub:** [exx3c](https://github.com/exx3c/)

---

© [Gabriel Dultra] - Todos os direitos reservados.
