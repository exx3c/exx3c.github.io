<div style="width: 100%; border: 1px solid #dfe2e5; overflow: hidden; margin-bottom: 16px;">
 <div style="width: 100%; background-image: url('https://raw.githubusercontent.com/exx3c/exx3c.github.io/refs/heads/main/snowflake.png'); background-size: cover; background-position: center; height: 110px;"></div>
</div>

# Snowflake: Primeiros Passos com Pipeline de Dados no Snowflake

Este artigo apresenta conceitos essenciais do Snowflake para Engenharia de Dados, com exemplos práticos de ingestão, transformação e análise de dados. É uma introdução acessível para quem deseja entender as funcionalidades básicas da plataforma e aprender a executar operações simples e essenciais de maneira eficiente.

## Snowflake Notebook:
```
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
