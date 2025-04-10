go http/net

```go
package main

type todo struct {
	Title string
}

func main() {
	database.ConnectDB()

	mux := http.NewServeMux()
	mux.HandleFunc("POST /todo", func(w http.ResponseWriter, r *http.Request) {
		parameters := todo{}
		err := json.NewDecoder(r.Body).Decode(&parameters)
		if err != nil {
			http.Error(w, err.Error(), http.StatusBadRequest)
			return
		}
		todo, err:= todoRepos.CreateTodo(parameters.Title)
		if err != nil {
			http.Error(w, err.Error(), http.StatusInternalServerError)
			return
		}
		w.WriteHeader(http.StatusCreated)
		json.NewEncoder(w).Encode(todo)
	})

	mux.HandleFunc("/ping", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("pong"))
	})

	server := http.Server{
		Addr: ":8080",
		Handler: mux,
	}

	server.ListenAndServe()
}
```

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
