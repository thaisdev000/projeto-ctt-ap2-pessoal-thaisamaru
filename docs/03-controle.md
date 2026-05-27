# Estruturas de Controle (If, For, Switch)

Go possui apenas uma estrutura de repetição: o `for`. Não existe `while` ou `do-while` em Go.

## Condicionais (If / Else)
```go
if x > 10 {
    fmt.Println("Maior que 10")
} else {
    fmt.Println("Menor ou igual a 10")
}
```
## O Único Loop: For
```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```