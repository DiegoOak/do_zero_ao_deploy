# Definindo o modelo

Aqui teremos uma decisão importante sobre o projeto. Devemos definir qual o banco de dados vamos utilizar.

Para tornar esta aplicação mais didática e simples, vamos optar por um modelo de armazenamento de dados em memória.

O cliente nos disse o seguinte:

"Toda tarefa deve possuir um titulo, uma descrição e um status"

Ok, vamos começar por esta especificação. Vamos criar um arquivo `test_modelo.py` com o seguinte conteúdo.

```python

def test_define_tarefa():
    tarefa = Tarefa('titulo', 'descrição', False)
    assert tarefa.titulo == 'titulo'
    assert tarefa.descricao == 'descrição'
    assert tarefa.status is False
```
:x: Os testes falham pois ainda não temos uma tarefa definida. Guiado pelo teste, vamos escrever algum código.

Qual é o mínimo de código que escrevo para que estes testes passem?

Vamos criar outro arquivo chamado `modelo.py` com o seguinte conteúdo.

```python
class Tarefa:

    def __init__(self, titulo, descricao, status):
        self.titulo = titulo
        self.descricao = descricao
        self.status = status
```

:heavy_check_mark: OK, o código está funcionando e testado, vamos continuar desenvolvendo o que foi proposto.

Próximo requisito foi "o status é algo opcional na criação da tarefa e por padrão é False"

```python
def test_status_nao_obrigatorio():
    tarefa = Tarefa('titulo', 'descrição')
    assert tarefa.status is False
    tarefa = Tarefa('titulo', 'descrição', True)
    assert tarefa.status is True
```
:x: Nossos testes estão falhando!

Precisamos alterar o código para que nosso novo requisito seja atendido sem afetar os requisitos anteriores.

A linha contendo:

`def __init__(self, titulo, descricao, status)`

deve se tornar

`def __init__(self, titulo, descricao, status=False)`

:heavy_check_mark: Rode novamente os testes e veja que tudo voltou a funcionar!

Também temos como requisito que toda tarefa tem um identificador único.

Vamos escrever um teste para isto:
```python
def test_toda_tarefa_tem_identificador_unico():
    tarefa1 = Tarefa('titulo', 'descrição')
    tarefa2 = Tarefa('titulo', 'descrição')
    tarefa1.id != tarefa2.id
```

:x: :disappointed: Os testes agora estão falhando novamente!

Vamos escrever o código para corrigir isto:

```python
class Tarefa:

    id = 1

    def __init__(self, titulo, descricao, status=False):
        self.id = Tarefa.id
        self.titulo = titulo
        self.descricao = descricao
        self.status = status
        Tarefa.id += 1

```

:clap: Muito bom! Agora vamos a criação das operações básicas de manipulação das nossas tarefas!

A primeira operação que devemos ser capaz de executar com nosso modelo é listar as tarefas existentes.

```python
def test_listar_tarefas():
    tarefa1 = Tarefa('titulo 1', 'descrição')
    tarefa2 = Tarefa('titulo 2', 'descrição')
    memdb[tarefa1.id] = tarefa1
    memdb[tarefa2.id] = tarefa2
    for indice, tarefa in enumerate(listar_tarefas(), 1):
        assert tarefa.titulo == 'titulo {}'.format(indice)
```

:x: Sim, os testes pararam novamente! O nosso processo envolve guiar o desenvolvimento através dos testes.

Vamos então escrever código para fazer funcionar esta funcionalidade:

```python
def listar_tarefas():
    return memdb.values()
```


Um detalhe importante aqui é que devo adicionar um dicionário no meu módulo com o nome de memdb.

Nesse momento o código de vocês devem estar assim:

Código

```python

memdb = {}


class Tarefa:

    id = 1

    def __init__(self, titulo, descricao, status=False):
        self.id = Tarefa.id
        self.titulo = titulo
        self.descricao = descricao
        self.status = status
        Tarefa.id += 1


def listar_tarefas():
    return memdb.values()

```

Testes

```python

def test_define_tarefa():
    tarefa = Tarefa('titulo', 'descrição', False)
    assert tarefa.titulo == 'titulo'
    assert tarefa.descricao == 'descrição'
    assert tarefa.status is False

def test_status_nao_obrigatorio():
    tarefa = Tarefa('titulo', 'descrição')
    assert tarefa.status is False
    tarefa = Tarefa('titulo', 'descrição', True)
    assert tarefa.status is True

def test_toda_tarefa_tem_identificador_unico():
    tarefa1 = Tarefa('titulo', 'descrição')
    tarefa2 = Tarefa('titulo', 'descrição')
    tarefa1.id != tarefa2.id


def test_listar_tarefas():
    memdb.clear()
    tarefa1 = Tarefa('titulo 1', 'descrição')
    tarefa2 = Tarefa('titulo 2', 'descrição')
    memdb[tarefa1.id] = tarefa1
    memdb[tarefa2.id] = tarefa2
    for indice, tarefa in enumerate(listar_tarefas(), 1):
        assert tarefa.titulo == 'titulo {}'.format(indice)
```
Pare, rode os testes e veja o que acontece.

:heavy_check_mark: Os testes estão passando? Nosso código está começando a tomar forma.

Nosso cliente também pediu que a listagem de tarefas deve apresentar primeiro as tarefas não concluídas e em seguida as concluídas.

Então vamos novamente pro arquivo de teste, escrever um código para esta regra e nos guiar a partir dele para desenvolver nosso código.

```python
def test_ordem_tarefas():
    memdb.clear()
    tarefa1 = Tarefa('titulo 1', 'descrição', True)  # tarefa já finalizada
    tarefa2 = Tarefa('titulo 2', 'descrição')  # não finalizada
    memdb[tarefa1.id] = tarefa1
    memdb[tarefa2.id] = tarefa2
    tarefas = list(listar_tarefas())
    assert tarefas[0].titulo == 'titulo 2'
    assert tarefas[1].titulo == 'titulo 1'
```

:x: O ciclo continua e novamente temos teste falhando.

O código para ordenar as tarefas é o seguinte, primeiro no topo do arquivo vamos importar uma função para nos auxiliar chamada attrgetter, que uma função que executa o mesmo que uma chamada de `objeto.algumacoisa`.

```python
from operator import attrgetter
```

e a nova versão ordenada da listagem de tarefas:

```python
def listar_tarefas():
    return sorted(memdb.values(), key=attrgetter('status'))
```

:heavy_check_mark: Neste ponto todos os testes devem estar passando, inclusive os anteriores, garantindo assim que as funcionalidades testadas estão ok.

Ainda restam as outras operações que ainda não foram contempladas pelo nosso modelo.

Temos de ser capazes de criar uma nova tarefa:

```python
def test_criar_tarefa():
    memdb.clear()
    tarefa = criar_tarefa('titulo', 'descrição')
    assert tarefa.titulo == 'titulo'
    assert tarefa.descricao == 'descrição'
    assert tarefa.status is False
    assert len(memdb) > 0
```

:x: Não se esqueça de rodar os testes e checar se temos problemas.

Vamos escrever o código da funcionalidade:

```python
def criar_tarefa(titulo, descricao, status=False):
    tarefa = Tarefa(titulo, descricao, status)
    memdb[tarefa.id] = tarefa
    return tarefa
```

:heavy_check_mark: Testes passando novamente! Mas ainda precisamos editar, remover e recuperar uma tarefa.

Vamos tentar remover então uma tarefa? Primeiro escrevemos os testes.

```python
def test_remover_tarefa():
    memdb.clear()
    tarefa = Tarefa('titulo', 'descricao')
    memdb[tarefa.id] = tarefa
    remover_tarefa(tarefa.id)
    assert len(memdb) == 0
```

:x: Se lembrou de rodar os testes? Lembre-se "red - green - gray"!

Temos testes falhando, vamos ao código.

```python
def remover_tarefa(id_):
    del memdb[id_]
```

Os testes estarem passando, não quer dizer que está tudo ok, mas sim que tenho confiança que aquilo que foi testado está ok. E se a tarefa não existir?

```python
import pytest

def test_tarefa_nao_existente():
    memdb.clear()
    with pytest.raises(KeyError):
        remover_tarefa(1)
```

:heavy_check_mark: Ok, temos todos os testes passando novamente e nem precisamos escrever código pois a função remover_tarefa já se comportava como o esperado.


E como recuperar uma tarefa?

Adicionamos uma tarefa no banco e tentamos recuperá-la.

```python
def test_recuperar_tarefa():
    memdb.clear()
    tarefa = Tarefa('titulo', 'descricao')
    memdb[tarefa.id] = tarefa
    recuperado = recuperar_tarefa(tarefa.id)
    assert tarefa.id == recuperado.id
    assert tarefa.titulo == recuperado.titulo
    assert tarefa.descricao == recuperado.descricao
```

:x: Testes falhando? Vamos escrever código!

O código é simples como:

```python
def recuperar_tarefa(id_):
    return memdb[id_]
```

Por garantia vamos escrever um teste para assegurar que quando um id_ não for recuperado, uma exceção é lançada.

```python
def test_recuperar_tarefa_nao_existente():
    memdb.clear()
    with pytest.raises(KeyError):
        remover_tarefa(1)
```

:heavy_check_mark: Tudo pronto? Testes passando? Vamos escrever só mais uma última ação. Agora poderemos editar uma tarefa.

:x: Primeiro o teste!

```python
def test_editar_tarefa():
    memdb.clear()
    tarefa = Tarefa('titulo', 'descricao')
    memdb[tarefa.id] = tarefa
    modificado = Tarefa('titulo modificado', 'descricao')
    editar_tarefa(tarefa.id, modificado)
    assert memdb[tarefa.id].titulo == 'titulo modificado'
```
:heavy_check_mark: Agora o código! :smiley:

```python
def editar_tarefa(id_, tarefa):
    editado = memdb[id_]
    editado.titulo = tarefa.titulo or editado.titulo
    editado.descricao = tarefa.descricao or editado.descricao
    editado.status = tarefa.status
```
:heavy_check_mark: Temos um modelo pronto e os testes passando!

Consegue imaginar algum teste para melhorar a qualidade do nosso modelo? Talvez alguma validação, ou algo que deixamos passar. Talvez deriamos refatorar as operações com tarefas para um módulo separado do modelo.

Sinta-se livre para adicionar estas funcionalidades, mas tente seguir nosso modelo de desenvolvimento guida por testes.

[Ir para o passo 6 :arrow_right:](passo06.md)

[:leftwards_arrow_with_hook: Voltar ao README ](README.md)
