## 180. Consecutive Numbers


> +-------------+---------+ <br>
> | Column Name | Type    | <br>
> +-------------+---------+ <br>
> | id          | int     | <br>
> | num         | varchar | <br>
> +-------------+---------+ <br>
> In SQL, id is the primary key for this table. <br>
> id is an autoincrement column starting from 1. <br>
 

Find all numbers that appear at least three times consecutively.

Return the result table in any order.

The result format is in the following example.

 
**Example 1:**
> **Input:** 
> Logs table:
> +----+-----+
> | id | num |
> +----+-----+
> | 1  | 1   |
> | 2  | 1   |
> | 3  | 1   |
> | 4  | 2   |
> | 5  | 1   |
> | 6  | 2   |
> | 7  | 2   |
> +----+-----+
> **Output:** 
> +-----------------+
> | ConsecutiveNums |
> +-----------------+
> | 1               |
> +-----------------+
> **Explanation:** 1 is the only number that appears consecutively for at least three times.
