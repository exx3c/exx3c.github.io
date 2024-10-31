<div style="width: 100%; border: 1px solid #dfe2e5; overflow: hidden; margin-bottom: 16px;">
 <div style="width: 100%; background-image: url('https://raw.githubusercontent.com/exx3c/exx3c.github.io/refs/heads/main/1_FzQPxYZJfrLZaoseXoIOuw.png'); background-size: cover; background-position: center; height: 110px;"></div>
</div>

# Apache Spark + Databricks: Manipulando Dataframes com PySpark

Para este cenário, selecionei um problema do LeetCode originalmente voltado para solução com pandas e adaptei a resolução para o ambiente Databricks, utilizando PySpark. Embora pandas seja ideal para manipulação de dados em pequena escala, PySpark é projetado para processamento distribuído, permitindo trabalhar com grandes volumes de dados de maneira mais eficiente.

## Problema: 180. Consecutive Numbers (LeetCode)

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+ 
| id          | int     |
| num         | varchar | 
+-------------+---------+ 
In SQL, id is the primary key for this table.
id is an autoincrement column starting from 1.
 ```


Find all numbers that appear at least three times consecutively.

Return the result table in any order.

The result format is in the following example.


**Example 1:**
```
Input:
Logs table:
+----+-----+
| id | num |
+----+-----+
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |
+----+-----+
Output: 
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
Explanation: 1 is the only number that appears consecutively for at least three times.
```

---

## Solução:

A estratégia original com pandas consistia em duplicar a coluna 'num' e, em seguida, usar a função ```shift()``` para deslocar cada coluna duplicada uma posição acima. Isso permitia que, ao combinarmos as colunas, cada linha contivesse o segundo e terceiro valores consecutivos para cada item inicial. Com PySpark, essa operação é simplificada graças à função ```lead()```, que permite acessar diretamente o valor da próxima linha ao aplicar uma operação de janela. Isso facilita a obtenção dos valores consecutivos de forma nativa e eficiente.

### Etapas:

#### 1. Definimos os dados e o esquema (schema) e, em seguida, criamos um DataFrame.

![image](https://github.com/user-attachments/assets/f87dc0d7-0b2d-4773-9577-9cc41e2be379)

#### 2. Criamos uma janela de operação que nos permite utilizar a função ```lead()``` para adicionar duas novas colunas ao DataFrame. A primeira coluna representa o próximo valor na sequência da coluna 'num', enquanto a segunda coluna captura o valor subsequente. Dessa forma, cada linha contém o valor atual junto aos dois próximos valores consecutivos da coluna 'num'.

![image](https://github.com/user-attachments/assets/5922cc10-c025-48be-904b-5f86f85d732f)

#### 3. Utilizamos a função ```where()``` para filtrar apenas as linhas em que as três colunas possuem o mesmo valor, indicando a presença de três números iguais consecutivos. Em seguida, selecionamos apenas a coluna 'num', removemos os valores duplicados e renomeamos a coluna para atender ao formato de saída especificado pelo problema.

![image](https://github.com/user-attachments/assets/6ddf890b-fefc-4102-a8c7-f9cd8a2b200e)

---

## Recursos e Referências

- [Documentação do PySpark](https://spark.apache.org/docs/latest/api/python/)
- [Guia de Janelas no PySpark](https://sparkbyexamples.com/pyspark/pyspark-window-functions/)

---

## Agradecimentos e Contato

Obrigado por acompanhar este artigo! Fique à vontade para enviar sugestões ou se apenas quer dizer olá, fique à vontade para me contatar!

- **Email:** [gabriel.dultra.239@gmail.com](mailto:gabriel.dultra.239@gmail.com)
- **LinkedIn:** [gabriel-dultra](https://www.linkedin.com/in/gabriel-dultra/)
- **GitHub:** [exx3c](https://github.com/exx3c/)

---

© [Gabriel Dultra] - Todos os direitos reservados.
