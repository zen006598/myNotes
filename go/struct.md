# struct

```go
type Point struct{
  X int
  Y int
}
```

```go
p1 := Point{1, 2}
px := Point{X:1, Y:2}
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

type Player struct {
	Damage  int
	Defense int
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
