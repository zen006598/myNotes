gin http method: https://gin-gonic.com/zh-tw/docs/examples/http-method/

```go
router := gin.Default()

	router.GET("/someGet", getting)
	router.POST("/somePost", posting)
	router.PUT("/somePut", putting)
	router.DELETE("/someDelete", deleting)
	router.PATCH("/somePatch", patching)
	router.HEAD("/someHead", head)
	router.OPTIONS("/someOptions", options)

	// default :8080
	router.Run()
```

binding json form data
https://gin-gonic.com/zh-tw/docs/examples/bind-form-data-request-with-custom-struct/

```go
type StructB struct {
    NestedStruct StructA
    FieldB string `form:"field_b"`
}

type StructD struct {
    NestedAnonyStruct struct {
        FieldX string `form:"field_x"`
    }
    FieldD string `form:"field_d"`
}
// html checkbox
type myForm struct {
    Colors []string `form:"colors[]"`
}
```

```go
c.Bind(&b)
c.ShouldBindJSON(&structD) //指定 json
c.ShouldBindWith(&structD, binding.Form)
c.ShouldBindQuery(&structD)
c.ShouldBindUri(&person) // path value
```

wire
dotenv
envconfig
