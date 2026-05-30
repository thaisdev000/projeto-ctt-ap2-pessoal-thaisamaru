---
icon: lucide/layers
---

# Arrays, Slices e Maps: Anatomia e Performance

No Go, gerenciar coleções de dados de forma eficiente exige entender como o *runtime* manipula a memória. A escolha entre um Array, um Slice ou um Map afeta diretamente o uso de CPU e o tempo de resposta do seu sistema.

---
## Introdução

Diferente de outras linguagens onde coleções são sempre ponteiros ocultos, o Go expõe o comportamento real da memória:

* **Arrays** são blocos fixos e contínuos. Mudar o tamanho exige criar um array novo. Passar um array para uma função **copia todos os elementos na memória**.
* **Slices** são apenas ponteiros leves (headers de 24 bytes) que apontam para um array oculto. Passá-los para funções é ultra-rápido.
* **Maps** são tabelas hash dinâmicas baseadas no algoritmo de *Swiss Tables*, que lêem grupos de dados em paralelo a nível de hardware.

## Arrays
Um **Array** em Go é um bloco contíguo de memória com um tamanho fixo determinado em tempo de compilação. Diferente de linguagens como Java ou C#, onde os arrays são ponteiros implícitos, no Go os arrays são **valores**.

### Comportamento de cópia por valor
Passar um array gigante como argumento para uma função faz com que o Go duplique todos os elementos na pilha de execução (*Stack*). Para evitar esse overhead de performance, a engenharia de software em Go utiliza ponteiros para arrays ou, na grande maioria dos casos, **Slices**.

```go
package main
import "fmt"
func modificarArray(arr [3]int) {
    arr[0] = 999 // Altera apenas a cópia local dentro da função
}
func modificarComPonteiro(arr *[3]int) {
    arr[0] = 999 // Altera o array original na memória
}
func main() {
    meuArray := [3]int{10, 20, 30}

    modificarArray(meuArray)
    fmt.Println("Após cópia por valor:", meuArray) // [10 20 30]
    modificarComPonteiro(&meuArray)
    fmt.Println("Após passagem por ponteiro:", meuArray) // [999 20 30]
}
```
## Slice Header

Um Slice é uma abstração construída sobre um array oculto . Internamente, um slice não armazena dados; ele é uma estrutura leve de 24 bytes conhecida como Slice Header:

### Componentes do header:
* Data Pointer : Endereço do primeiro elemento acessível através do slice (não necessariamente o índice zero do array oculto).
* Length : O número de elementos expostos no slice atual.
* Capacity : O número máximo de elementos que o slice pode conter antes de forçar uma nova alocação de memória.

```go
package main
import "fmt"
func main() {
    // Alocação explícita com make(tipo, len, cap)
    // Evita realocações desnecessárias se você já sabe o limite dos dados
    sliceOtimizado := make([]int, 3, 6)
    sliceOtimizado[0] = 100
    fmt.Printf("Ponteiro implícito: %v | len: %d | cap: %d\n", sliceOtimizado, len(sliceOtimizado), cap(sliceOtimizado))
}
```
## Slicing Mechanics
O fatiamento de estruturas obedece à sintaxe v[baixo:alto]. O índice de início (baixo) é inclusivo, enquanto o índice de término (alto) é exclusivo.

### Compartilhamento de Memória Subjacente
Quando você cria um slice a partir de outro, ambos apontam para o mesmo array oculto. Modificações em um afetarão o outro de forma imediata, contanto que o slice modificado não tenha estourado sua capacidade e gerado uma realocação.

```go
package main
import "fmt"
func main() {
    // Array original que serve de suporte físico
    dados := [5]string{"A", "B", "C", "D", "E"}   
    // Fatia do índice 1 ao 3 (Pega os elementos 1 e 2 -> "B" e "C")
    fatia1 := dados[1:3]
    // A capacidade calcula do início da fatia até o fim físico do array original
    fmt.Printf("Fatia 1: %v | len: %d | cap: %d\n", fatia1, len(fatia1), cap(fatia1)) // len: 2, cap: 4
    // Alteração destrutiva: afeta o array original 'dados'
    fatia1[0] = "ZETA"
    fmt.Println("Array original modificado:", dados) // [A ZETA C D E]
}
```
### O Perigo do Vazamento de Memória (Memory Leak)
Como detalhado na documentação do LabEx, se você tiver um array imenso na memória e extrair uma fatia minúscula dele, o Garbage Collector não poderá liberar o array imenso. O ponteiro contido no cabeçalho da sua pequena fatia mantém todo o array original vivo no Heap.

* Como evitar: Se precisar apenas de um fragmento, use a função copy() para instanciar um novo array oculto independente e isolado.
```go
// Forma incorreta (Mantém o arquivo inteiro de 10MB na memória para ler 4 bytes)
func lerMetadadosIncorreto(arquivoCompleto []byte) []byte {
    return arquivoCompleto[0:4] 
}

// Forma correta (Isola os dados e permite a limpeza do arquivo gigante)
func lerMetadadosCorreto(arquivoCompleto []byte) []byte {
    metadados := make([]byte, 4)
    copy(metadados, arquivoCompleto[0:4])
    return metadados
}
```

## O Mecanismo do append e Estratégias de Crescimento
A função append() insere elementos no fim do slice de forma inteligente. Se len == cap, o Go ativa sua rotina de realocação dinâmica:

* O runtime calcula o tamanho do novo bloco de memória.
* Um novo array oculto é alocado em uma área distinta.
* Os dados existentes são copiados para a nova área via cópia de blocos de memória (memmove).
* O ponteiro interno do slice é atualizado para o novo endereço.

### Curva de Crescimento Suavizada (Versões Recentes de Go)
Para evitar desperdício excessivo em coleções massivas, o algoritmo do Go não dobra a capacidade indefinidamente. Para volumes menores, o fator é 2.0x. À medida que o slice cresce além do limiar base, a taxa reduz gradualmente em direção a 1.25x de incremento por alocação.

## Maps
Seguindo a engenharia descrita no Blog Oficial do Go e consolidada com as mudanças de arquitetura recentes, os Maps em Go são implementados como tabelas hash altamente otimizadas baseadas no conceito de Swiss Tables.

### Arquitetura de Busca Paralela (Hardware SIMD)
Os mapas dividem seus pares de chave-valor em blocos estruturados. Em vez de varrer elemento por elemento de forma linear quando ocorre uma colisão de hash, as versões estáveis do Go organizam grupos de metadados de 16 elementos simultâneos. O processador utiliza instruções vetoriais nativas (SIMD) para checar o grupo inteiro de uma vez a nível de hardware, reduzindo o tempo médio de busca para O(1) com altíssima eficiência de cache.
```go
package main
import "fmt"
func main() {
    // Alocação ideal especificando capacidade inicial para mitigar re-hashing
    tabelaPrecos := make(map[string]float64, 50)    
    tabelaPrecos["Go"] = 0.0
    tabelaPrecos["Rust"] = 14.50
    // O Idioma Comma-OK: Segurança na leitura de chaves
    valor, encontrado := tabelaPrecos["Python"]
    if !encontrado {
        fmt.Println("Chave inexistente. Retornou o valor zero do tipo:", valor) // 0
    }
}
```
!!! warning "`Insegurança Concorrente`"
    Maps nativos não possuem travas contra acessos simultâneos. Se duas Goroutines escreverem no mesmo mapa juntas, o programa crasha na hora.

### Resumo

| Critério Técnico | Array | Slice | Map |
| :--- | :--- | :--- | :--- |
| **Tipo de Alocação** | Estática e Contígua | Dinâmica (Ponteiro) | Dinâmica (Swiss Tables) |
| **Passagem de Parâmetro** | Cópia de todos os elementos | Cópia do Header (24 bytes) | Cópia de Ponteiro de Estrutura |
| **Uso Recomendado** | Buffers de tamanho fixo, Criptografia | Listas de dados, Iterações gerais | Dicionários, Índices, Busca Rápida |
| **Custo de Busca** | O(n) linear | O(n) linear | O(1) constante por SIMD |
