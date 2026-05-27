# Concorrência II: Channels

Para que as Goroutines conversem entre si de forma segura sem gerar condições de corrida, usamos **Channels** .

## Exemplo Prático com Canais
```go
package main
import "fmt"

func enviarDados(canal chan string) {
    canal <- "Olá do processo paralelo!" 
}

func main() {
    meuCanal := make(chan string)
    go enviarDados(meuCanal)
    
    mensagem := <-meuCanal 
    fmt.Println(mensagem)
}