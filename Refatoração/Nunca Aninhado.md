Manter uma quantidade de indentações inferior a 3 é importante para manter uma legibilidade miníma.

```python
def calculate(bottom, top):
	if top > bottom:
		numbers_sum = 0

		for number in range(bottom, top + 1):
			if number % 2 == 0:
				numbers_sum += number

		return numbers_sum
	else:
		return 0
```

Se isso danifica sua visão e sua mente somente olhando, iremos melhorar essa situação.

### Extração
Uma das formas de diminuir o nível de indentação é extraindo parte da lógica em uma função.

#### De
```python
def calculate(bottom, top):
	if top > bottom:
		numbers_sum = 0

		for number in range(bottom, top + 1):
			if number % 2 == 0:
				numbers_sum += number

		return numbers_sum
	else:
		return 0
```

#### Para
```python
def filter_number(number):
	if number % 2 == 0:
		return number
	return 0

def calculate(bottom, top):
	if top > bottom:
		numbers_sum = 0

		for number in range(bottom, top + 1):
			numbers_sum += filter_number(number)

		return numbers_sum
	else:
		return 0
```

### Inversão
Se uma condição for invertida é possível quebrar o fluxo previamente.

```python
def filter_number(number):
	if number % 2 == 0:
		return number
	return 0

def calculate(bottom, top):
	if top > bottom:
		numbers_sum = 0

		for number in range(bottom, top + 1):
			numbers_sum += filter_number(number)

		return numbers_sum
	else:
		return 0
```

#### Para
```python
def filter_number(number):
	if number % 2 == 0:
		return number
	return 0

def calculate(bottom, top):
	if top <= bottom:
		return 0
	numbers_sum = 0

	for number in range(bottom, top + 1):
		numbers_sum += filter_number(number)

	return numbers_sum		
```

Você pode transformar um operador `AND` para um `OR` e vice e versa, esse processo pode ser útil para a inversão. Um `a AND b` é igual a `NOT (NOT a OR NOT b)`, ou seja, para trocar um `AND` por um `OR` basta negar (aplicar o operador `NOT`) no entrada e na saída do operador.
Então se você precisa inverter algo que possuí operador você pode omitir o `NOT` da saída pois a inversão da inversão é igual a não fazer nada. Portanto se quiser inverter `a AND b` ficará igual a: `NOT a or NOT b`.

### Exemplo
#### De
```python
def calc(a, b):
	if a and b:
		# do calc
		...
	else:
		return 0
```
#### Para
```python
def calc(a, b):
	if not a or not b:
		return 0
	# do calc
	...
```
