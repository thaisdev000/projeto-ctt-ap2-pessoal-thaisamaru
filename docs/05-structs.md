# Structs e Métodos

Go não possui classes ou herança tradicional. Em vez disso, utilizamos **Structs** para agrupar dados e métodos.

## Exemplo de Struct
```go
type Aluno struct {
    Nome  string
    Idade int
}

func main() {
    a := Aluno{Nome: "Thais", Idade: 21}
    fmt.Println(a.Nome)
}