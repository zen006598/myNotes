# Wire

refer:

- [Wire User Guide](https://github.com/google/wire/blob/main/docs/guide.md)

Wire 有兩大概念

- providers
- injectors

## Providers

- 一段有 return 的 function
- Provider 必須匯出，以便其他 package 能夠使用它，就像一般的函數一樣
- wire 會解析 provider 構建依賴關係

```go
type Foo struct {
    X int
}

func ProvideFoo() Foo {
    return Foo{X: 42}
}
```

## Provider Set

- 模組化: 把 provider 依照範圍做分組
- 可以把已經合併的 set 再次合併

```go
var SuperSet = wire.NewSet(ProvideFoo, ProvideBar, ProvideBaz)
var MegaSet = wire.NewSet(SuperSet, pkg.OtherSet)
```

## Injectors

- Injector 是由 Wire 自動生成的 function，負責「注入」依賴
- Injector 會根據定義好的 provider，連接 provider 將所有依賴組合在一起
- Injector 被產生於有呼叫 `wire.Build` 的 function

```go
func initializeBaz(ctx context.Context) (foobarbaz.Baz, error) {
    wire.Build(foobarbaz.MegaSet)
    return foobarbaz.Baz{}, nil
}
```

## 進階使用

`Bind()` provider return 實際返回, 讓 wire bind interface 接管 實際物件與 interface 的關聯
Struct Providers: `Struct()` 只需指定結構體中需要注入的導出欄位，Wire 就能自動生成對應的建構器函數，從而大幅減少樣板代碼、清晰表達依賴關係，並提升程式碼的可維護性和靈活性

```go
// after
type FooBar struct {
    MyFoo Foo
    MyBar Bar
}

func ProvideFooBar(foo Foo, bar Bar) FooBar {
    return FooBar{
        MyFoo: foo,
        MyBar: bar,
    }
}

var Set = wire.NewSet(
    ProvideFoo,
    ProvideBar,
    ProvideFooBar,
)

// before
// ProvideFooBar 可以做省略 使用 wire.Struct
// ProvideFooBar 會自動生成依賴
type FooBar struct {
    MyFoo Foo
    MyBar Bar
}

var Set = wire.NewSet(
    ProvideFoo,
    ProvideBar,
    // 移除 ProvideFooBar，改寫為以下
    wire.Struct(new(FooBar), "MyFoo", "MyBar"))

```
