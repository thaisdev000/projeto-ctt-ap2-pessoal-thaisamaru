# Concorrência I: Goroutines

A concorrência é um dos pontos mais fortes do Roadmap de Go. Uma **Goroutine** é uma thread leve gerenciada pelo próprio Go runtime.

## Como disparar uma Goroutine
Basta adicionar a palavra-chave `go` antes da chamada de uma função:

```go
go executarTarefaPesada()
```
>[!NOTE]
Múltiplas Goroutines rodam simultaneamente de forma assíncrona.
[!NOTE]
```bash
go version
