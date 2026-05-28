# Testes Automatizados em Go

Go possui suporte nativo a testes através do pacote padrão `testing`, dispensando frameworks externos para testes básicos.

## Criando um Arquivo de Teste
Os arquivos de teste devem obrigatoriamente terminar com `_test.go`.

```go
package calculadora

import "testing"

func TestSoma(t *testing.T) {
    resultado := Soma(2, 3)
    if resultado != 5 {
        t.Errorf("Esperava 5, mas obteve %d", resultado)
    }
}
```