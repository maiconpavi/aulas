Comentários são mensagens deixadas no meio do código para explicar algo que não está claro, não confunda com documentação. 

Se você acha necessário escrever comentário em uma certa parte do seu código, seu código não está legível o suficiente.

---
### Números mágicos

Números mágicos é qualquer constante escrita de forma literal em uma comparação ou atribuição de valores. Para evitar a necessidade de comentários e melhorar a legibilidade, utilize constantes globais.

#### Exemplo:
```python
# Status 5 é o status de mensagem enviada
if status == 5:
    message.mark_sent()
```

Poderia ser escrito como:
```python
MESSAGE_SENT = 5
...
if status == MESSAGE_SENT:
    message.mark_sent()
```

---
### Controle de fluxo extenso
Caso tenha um controle de fluxo do tipo `SE` (if's) com regras de negócio, utilize varáveis para trazer um sentido e nomear para uma comparação.

```python
# Um usuário pode editar uma mensagem se ele foi o autor da mensagem
# e a mensagem foi recebida recentemente ou se o usuário
# for administrador.
if (message.user.id == current_user.id and (
        message.delivered_time() is None or (datetime.now() - message.delivered_time()).seconds < 300 )) or current_user.type == User.Administrator:
    message.update_text(text)
```

Poderia ser escrito como:
```python
MESSAGE_UPDATE_TIMEFRAME = 5 * 60
...

user_is_author = message.user.id == current_user.id

is_recent = message.delivered_time() is None or (datetime.now() - message.delivered_time()).seconds < MESSAGE_UPDATE_TIMEFRAME

user_is_admin = current_user.type == User.Administrator

if user_is_author and is_recent or user_is_admin:
    message.update_text(text)
```

Ou extrair em uma função (ou método):
```python
MESSAGE_UPDATE_TIMEFRAME = 50 * 60

def can_edit_message(current_user: User, message: Message) -> bool:
    user_is_author = message.user.id == current_user.id

    is_recent = message.delivered_time() is None or (datetime.now() - message.delivered_time()).seconds < MESSAGE_UPDATE_TIMEFRAME
    
    user_is_admin = current_user.type == User.Administrator
    return user_is_author and is_recent or user_is_admin

...

if can_edit_message(current_user, message):
    message.update_text(text)
```


---
## Exceções

Tem algumas situações que pode ser necessário o uso de comentários, como por exemplo:

#### Optimizações de performance não obvias.
```python
def parse_id(id_str: str) -> int {
    # Já que usamos essa função com muita frequência
    # nós desenrolamos o loop já que medimos um ganho de
    # 20% na velocidade de execução fazendo isto.

    return (1000 * int(id_str[0]) +
            100  * int(id_str[1]) + 
            10   * int(id_str[2]) +
            1    * int(id_str[3])) 
        
}
```

#### Referência para artigos sobre matemática ou algoritmos
```rust
/// Partitions `v` into elements smaller than `pivot`, followed by elements greater than or equal
/// to `pivot`.
///
/// Returns the number of elements smaller than `pivot`.
///
/// Partitioning is performed block-by-block in order to minimize the cost of branching operations.
/// This idea is presented in the [BlockQuicksort][pdf] paper.
///
/// [pdf]: https://drops.dagstuhl.de/opus/volltexte/2016/6389/pdf/LIPIcs-ESA-2016-38.pdf
fn partition_in_blocks<T, F>(v: &mut [T], pivot: &T, is_less: &mut F) -> usize
```
