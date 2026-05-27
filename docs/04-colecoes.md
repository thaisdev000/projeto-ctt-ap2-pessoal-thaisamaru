# Arrays, Slices e Maps

O roadmap divide as coleções em três pilares fundamentais: Arrays, Slices e Maps.

## Slices (Arrays Dinâmicos)
Os Slices são muito mais comuns que arrays tradicionais em Go por serem expansíveis.
```go
numeros := []int{1, 2, 3}
numeros = append(numeros, 4)
```
## Maps (Chave e Valor)
```go
capital := make(map[string]string)
capital["SP"] = "São Paulo"
capital["RJ"] = "Rio de Janeiro"
```