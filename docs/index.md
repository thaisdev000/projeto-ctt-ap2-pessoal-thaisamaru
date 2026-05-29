---
icon: lucide/rocket
---

# Linguagem Go

### Descubra mais sobre a linguagem Golang!

## Instalando a linguagem Go

* [`zensical new`][new] - Create a new project
* [`zensical serve`][serve] - Start local web server
* [`zensical build`][build] - Build your site

  [new]: https://zensical.org/docs/usage/new/
  [serve]: https://zensical.org/docs/usage/preview/
  [build]: https://zensical.org/docs/usage/build/

## Sintaxe básica

## Váriaveis

## Estrutura de controles
### If
### For
### Switch

## Arrays, Slices e Maps
[cite_start]Para guardar grupos de informações, o Go usa três estruturas fundamentais: Arrays, Slices e Maps[cite: 175].

| Estrutura | Como funciona por dentro | Por que é bom? |
| :--- | :--- | :--- |
| **Array** | [cite_start]Lista com tamanho que nunca muda [cite: 184] | [cite_start]Muito rápido, mas pouco flexível[cite: 184]. |
| **Slice** | [cite_start]Uma visão dinâmica de um array [cite: 184] | [cite_start]Fácil de passar para funções sem gastar memória[cite: 184]. |
| **Map (Novo)** | [cite_start]Baseado em Swiss Tables e grupos de 16 [cite: 184] | [cite_start]A busca é extremamente veloz e gasta menos CPU[cite: 184]. |

---

### Arrays vs. Slices (Arrays Dinâmicos)

[cite_start]Os Arrays têm tamanho fixo e são pouco usados no dia a dia[cite: 176]. [cite_start]O que usamos mesmo são os **Slices**, que funcionam como listas que crescem sozinhas conforme você adiciona itens[cite: 177]. [cite_start]Por trás das câmeras, um Slice é apenas uma "janela" para un Array real, guardando o endereço dos dados, o tamanho atual e a capacidade total[cite: 178, 179].

```go
package main

import "fmt"

func main() {
    // Criando um Slice dinâmico
    numeros := []int{1, 2, 3}
    numeros = append(numeros, 4)
    fmt.Println(numeros)
}
```
### Maps (Chave e Valor)
```go
capital := make(map[string]string)
capital["SP"] = "São Paulo"
capital["RJ"] = "Rio de Janeiro"
```

## Structs

## Métodos

## Tratamento de erros

## Goroutines

## Channels

## Gereciamento de pacotes (Go modules)

## Testes automatizados
