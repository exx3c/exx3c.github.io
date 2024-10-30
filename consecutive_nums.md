<div style="width: 100%; border: 1px solid #dfe2e5; overflow: hidden; margin-bottom: 16px;">
 <div style="width: 100%; background-image: url('https://raw.githubusercontent.com/exx3c/exx3c.github.io/refs/heads/main/1_FzQPxYZJfrLZaoseXoIOuw.png'); background-size: cover; background-position: center; height: 110px;"></div>
</div>

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

## Solução:

### 1. Definimos os dados e schema e criamos um Dataframe Spark.

![image](https://github.com/user-attachments/assets/f87dc0d7-0b2d-4773-9577-9cc41e2be379)

### 2. Criamos uma janela de operação para que possamos utilizar a função ```lead()```, e duas novas colunas agregadas ao Dataframe. Onde a primeira e a segunda nova coluna representam o primeiro e o segundo valor das próximas linhas da coluna "num" respectivamente.

![image](https://github.com/user-attachments/assets/5922cc10-c025-48be-904b-5f86f85d732f)

### 3. Através da função where podemos então selecionar apenas as linhas em que as 3 colunas possuem o mesmo valor, ou seja, há 3 números iguais consecutivos. E por fim selecionamos apenas a coluna "num", excluimos os duplicados e renomeamos a coluna para o que o formato de output do problema seja satisfeito.

![image](https://github.com/user-attachments/assets/6ddf890b-fefc-4102-a8c7-f9cd8a2b200e)
