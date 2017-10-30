# Criando uma tarefa

Agora vamos começar a desenvolver nossa interface web.

Continuaremos empregando o TDD, sendo assim vamos começar escrevendo os testes.

A primeira funcionalidade é a criação de um tarefa na "TODO list". Quando uma nova tarefa é criada corretamente, a mesma deve possuir status `False`, ou seja ainda não foi feita. O código de status da resposta deve ser 201.

Crie um novo arquivo de testes para os testes do app. Estou utilizando um arquivo chamado `test_tarefa.py`.

Transformando estas espeficificações em código, resulta em algo como:

```python
import json
from main import app


def test_criar_tarefa():
    with app.test_client() as c:
        # realiza a requisição utilizando o verbo POST
        resp = c.post('/task', data={'titulo': 'titulo',
                                     'descricao': 'descricao'})
        # é realizada a análise e transformação para objeto python da resposta
        data = json.loads(resp.data.decode('utf-8'))
        # 201 CREATED é o status correto aqui
        assert resp.status_code == 201
        assert data['titulo'] == 'titulo'
        assert data['descricao'] == 'descricao'
        # qaundo a comparação é com True, False ou None, utiliza-se o "is"
        assert data['status'] is False
```

Criamos um contexto onde c é um cliente para nossa aplicação. Utilizamos esse cliente para simular um acesso a nossa api e
verificamos se o conteúdo retornado está correto, assim como o código de status.

Um detalhe importante aqui é que o conteúdo retornado(resp.data) é uma sequencia de bytes, logo precisamos fazer uma conversão para string antes de fazer o parser do mesmo. Isto e feito através da função `.decode('utf-8')`.

Obs: lembre-se de colocar as importações no topo do arquivo.

:x: Oh não! Agora os testes estão quebrando novamente.

Vamos corrigi-los escrevendo a funcionalidade de criação de uma tarefa. Crie um arquivo `main.py` com o seguinte conteúdo.

```
from flask import Flask, jsonify, request
from modelo import criar_tarefa, memdb

app = Flask('Meu app')


@app.route('/task', methods=['POST'])
def criar():
    memdb.clear()
    # recebe o título e a descrição através do corpo da requisição
    titulo = request.form['titulo']
    descricao = request.form['descricao']
    # utiliza estes valores para criar uma tarefa
    # a função criar tarefa foi desenvolvida e testada no passo anterior
    tarefa = criar_tarefa(titulo, descricao)
    # retorna o resultado em um formato json
    return jsonify({
        'id': tarefa.id,
        'titulo': tarefa.titulo,
        'descricao': tarefa.descricao,
        'status': tarefa.status,
    })
```

:x: ops! Parece que os testes ainda não estão passando!

O código de status da reuisição não é o correto.

No retorno da função acrescente o valor 201 como visto abaixo.

```python
    ...
    return jsonify({
        'id': tarefa.id,
        'titulo': tarefa.titulo,
        'descricao': tarefa.descricao,
        'status': tarefa.status,
    })
    return jsonify({
        'id': tarefa.id,
        'titulo': tarefa.titulo,
        'descricao': tarefa.descricao,
        'status': tarefa.status,
    }), 201
    ...
```
:heavy_check_mark: Testes passando! Vamos prosseguir. Preciso me assegurar que o caso negativo também esteja correto.

```python
from modelo import memdb

def test_erro_ao_criar_tarefa():
    memdb.clear()
    with app.test_client() as c:
        resp = c.post('/task', data={'titulo': 'titulo'})
        assert resp.status_code == 400
```

:warning: Um aviso importante: As importações sempre virão no início do arquivo.

A faixa de código de status que vai de 400 até 499 é destinada a erros do cliente, e neste caso ele está tentando enviar um válor inválido.

:heavy_check_mark: Verifique se os testes continuam passando e vamos para o proximo passo.

[Ir para o passo 6 :arrow_right:](passo06.md)

[:leftwards_arrow_with_hook: Voltar ao README ](README.md)
