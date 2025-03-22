# 指標變數

資料變數 `num := 1`
取得該資料變數的記憶體位置 `&變數名稱`
存放到指標變數 `var 新變數 *資料型態 = &變數名稱`
`*資料型態` 指標資料型態
`*變數名稱` 反解指標變數

```go
package main

import "fmt"

func main() {
	num := 10             // 資料變數
	fmt.Println(num)      // output: 10
	fmt.Println(&num)     // &變數 取得記憶體位置, output: 0x1400000e1a0
	var place *int = &num // 變數指標 *type 存放記憶體位置
	fmt.Println(place)    // output: 0x1400000e1a0
	fmt.Println(*place)   // 反解記憶體位置 *變數名稱 output: 10
	*place = 20           // 反解記憶體位置並修改記憶體上的值
	fmt.Println(num)      // output: 20
}
```

```
10
0x14000098020
0x14000098020
10
20
```

## function 參數傳遞

pass by value

```go
package main

import "fmt"

func main() {
	num := 10
	addTen(num)
	fmt.Println(num) // output : 10
}
// 沒有副作用
func addTen(n int) int {
	n += 10
	return n
}

```

pass by pointer

```go
package main

import "fmt"

func main() {
	num := 10
	addTen(&num)
	fmt.Println(num)
}

func addTen(n *int) {
	*n += 10
}

```
