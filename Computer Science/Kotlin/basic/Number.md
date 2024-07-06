Kotlin sudah menyedialan build-in type dari [[numbers]].

| Type    | Size (bits) | Min value                         | Max value                           |
| ------- | ----------- | --------------------------------- | ----------------------------------- |
| `Byte`  | 8           | -128                              | 127                                 |
| `Short` | 16          | -32768                            | 32767                               |
| `Int`   | 32          | -2,147,483,648 (-231)             | 2,147,483,647 (231 - 1)             |
| `Long`  | 64          | -9,223,372,036,854,775,808 (-263) | 9,223,372,036,854,775,807 (263 - 1) |

##### Catatan-catatan
1. ketika melakukan [[Initialize]] tanpa memberikan sebuah type yang explisit, maka Kotlin akan melihat, apakah value tersebut masuk dalam range [[Int]], jika iya masih masuk dalam range [[Int]], maka type tersebut [[Int]]. Apabila lebih dari max value [[Int]], maka akan dianggap sebagai type dari [[Long]]