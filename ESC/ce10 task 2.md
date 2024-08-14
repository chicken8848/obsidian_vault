
| UCid | TCid | Functions/modules | input   | expected_output | time_date_executed |
| ---- | ---- | ----------------- | ------- | --------------- | ------------------ |
|      | 1    | findmax           | []      | error           |                    |
|      | 2    | findmax           | [null]  | error           |                    |
|      | 3    | findmax           | [&a]    | error           |                    |
|      | 4    | findmax           | [5]     | 5               |                    |
|      | 5    | findmax           | [5,1,3] | 5               |                    |
|      | 6    | findmax           | [3,1,5] | 5               |                    |
|      | 7    | findmax           | [1,5,3] | 5               |                    |
|      | 8    | findmax           | null    | error           |                    |
