# struct

## 基礎結構

命名規則 編譯會錯
大寫開頭：public
小寫開頭：private

```go
// type 結構名稱 struct
type Point struct{
	// 欄位名稱 資料型態
  X int
  Y int
}
```

### 建立實體

```go
p1 := Point{1, 2}
p2 := Point{X:1, Y:2}
```

## 記憶體分配

複製所有欄位的值，不是參考型別，指向同一變數

```go
package main

func main() {
	p1 := Person{Hight: 180, Weight: 70}
	p2 := p1
	p2.Hight = 190
	println(p1.Hight)
	println(p2.Hight)
}

type Person struct {
	Hight  int
	Weight int
}
// output 180
// output 190
```

## 建構子

go 類似 OOP 但不完全是 OOP

```go
type Person struct {
    Name string
    age  int
}

func NewPerson(name string, age int) Person {
    return Person{Name: name, age: age}
}

```

## 物件方法

```go
type Person struct {
    Name string
    Age  int
}

// 方法定義在 Person 的 receiver 上
func (p Person) GetAge() {
    Printf(p.Age)
}

// 使用 pointer 來修改內容
// 可以想成
// void SetAge(int a) {
//     this.Height = a;
// }
func (p *Person) SetAge(newAge int) {
    p.Age = newAge
}

func main() {
    p := Person{Name: "Alice", Age: 30}
    p.GetAge()

    p.SetAge(35)
    p.GetAge()
}
// output 30
// output 35
```

```go
type Player struct {
	Damage  int
	Defense int
}
```

```go
package main

import "fmt"

func main() {
	p1 := NewPlayer()
	p1.Attack()

	warrior := Warrior{Player{15, 10}}
	magician := Magician{NewPlayer()}
	warrior.Attack()
	magician.Attack()
}


// 建構子
//refer https://stackoverflow.com/questions/18125625/constructors-in-go
func NewPlayer() Player {
	return Player{
		Damage:  10,
		Defense: 5,
	}
}
// class method
func (p *Player) Attack() {
	fmt.Printf("Player deals %d damage\n", p.Damage)
}
// inherit
type Warrior struct {
	Player
}
// override inherit method
func (p *Warrior) Attack() {
	fmt.Printf("Warrior deals %d damage\n", p.Damage)
}

type Magician struct {
	Player
}

```
