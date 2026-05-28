# Tratamento de Erros (Error Handling)

Em Go, erros não são exceções (`try/catch`). Erros são valores de retorno normais de uma função.

## Padrão de Tratamento
```go
resultado, err := FazerCalculo()
if err != nil {
    log.Println("Erro ao calcular:", err)
    return
}
```
```go
fmt.Println("Sucesso:", resultado)
```