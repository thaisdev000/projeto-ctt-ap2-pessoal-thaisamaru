# Sintaxe Básica e Variáveis

Em Go, a declaração de variáveis pode ser feita de forma explícita ou utilizando a inferência de tipo rápida.

## Declaração de Variáveis
```go
package main

import "fmt"

func main() {
    var nome string = "Impacta"
    idade := 20
    
    fmt.Printf("Nome: %s, Idade: %d\n", nome, idade)
}
```
>
O operador := só pode ser utilizado dentro de funções.
