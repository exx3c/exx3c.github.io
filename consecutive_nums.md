<div style="width: 100%; border: 1px solid #dfe2e5; overflow: hidden; margin-bottom: 16px;">
 <div style="width: 100%; background-image: url('https://media.licdn.com/dms/image/v2/D4E16AQFxAQHoxQmq6w/profile-displaybackgroundimage-shrink_350_1400/profile-displaybackgroundimage-shrink_350_1400/0/1719354406463?e=1733961600&v=beta&t=SPUztuTiGKKWeEHkbGfSGljAo_cb2sNuUZmz14ux58k'); background-size: cover; height: 190px;"></div>
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
