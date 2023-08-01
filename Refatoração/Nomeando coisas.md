Uma das partes mais difíceis da computação é nomear coisas, seguindo alguns padrões é possível facilitar.


### Não escreva variáveis com uma só letra.

```python
def partition(arr: list, l: int, h: int) -> int:
    i = ( l - 1 )
    x = arr[h]
  
    for j in range(l, h):
        if arr[j] <= x:
            i = i + 1
            arr[i], arr[j] = arr[j], arr[i]
  
    arr[i + 1], arr[h] = arr[h], arr[i + 1]
    return (i + 1)
```

O que é `l` e `h` ? Não use em escopos maiores de um loop.
```python
def partition(arr: list, low: int, high: int) -> int:
    i = (low - 1)
    high_val = arr[high]
  
    for j in range(low, high):
        if arr[j] <= high_val:
            i = i + 1
            arr[i], arr[j] = arr[j], arr[i]
  
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return (i + 1)
```

O retorno é o `pivot` (`i + 1`), é possível deixar ainda mais claro mudando um pouco as operações que envolvem o `i`.

```python
def partition(arr: list, low: int, high: int) -> int:
    pivot = low
    high_val = arr[high]
  
    for i in range(low, high):
        if arr[i] <= high_val:
            arr[pivot], arr[i] = arr[i], arr[pivot]
            pivot += 1
  
    arr[pivot], arr[high] = arr[high], arr[pivot]
    return pivot
```

Ainda é possível melhorar ainda mais, no próximo assunto iremos tratar sobre.

### Não coloque o tipo no nome da variável.
Antes da existências de [LSP](https://microsoft.github.io/language-server-protocol/)'s essa prática ainda tinha uma utilidade, porém hoje em dia isso deve estar expresso na tipagem.

#### De
```python
def execute(delay_in_seconds: int):
	...
```

#### Para
```python
def execute(delay: timedelta):
	...
```

### Inversão 
#### De
```python
def partition(arr, low, high):
    pivot = low
    high_val = arr[high]
  
    for i in range(low, high):
        if arr[i] <= high_val:
            arr[pivot], arr[i] = arr[i], arr[pivot]
            pivot += 1
  
    arr[pivot], arr[high] = arr[high], arr[pivot]
    return pivot
```

#### Para
```python
def partition(arr, low, high):
    pivot = low
    high_val = arr[high]
  
    for i in range(low, high):
        if arr[i] > high_val:
	        continue
		arr[pivot], arr[i] = arr[i], arr[pivot]
		pivot += 1
  
    arr[pivot], arr[high] = arr[high], arr[pivot]
    return pivot
```

Nesse caso a melhoria na legibilidade pode não ser clara de início, porém seja necessário adicionar mais condições ficará mais fácil dessa forma.