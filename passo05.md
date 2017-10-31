# Criando uma tarefa

Agora vamos começar a desenvolver nossa interface web.

Utilizaremos a técnica de TDD, sendo assim vamos começar escrevendo os testes.

A primeira funcionalidade é a criação de um tarefa na "TODO list". Quando uma nova tarefa é criada corretamente, a mesma deve possuir status `False`, ou seja ainda não foi feita. O código de status da resposta deve ser 201.

Crie um novo arquivo de testes para os testes do app. Estou utilizando um arquivo chamado `test_main.py`.

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

:x: Oh não! Agora os testes estão quebrando.

Vamos corrigi-los escrevendo a funcionalidade de criação de uma tarefa. Crie um arquivo `main.py` com o seguinte conteúdo.

```
from flask import Flask, jsonify, request

app = Flask('Meu app')

tarefas = []


@app.route('/task', methods=['POST'])
def criar():
    # recebe o título e a descrição através do corpo da requisição
    titulo = request.json['titulo']
    descricao = request.json['descricao']
    # utiliza estes valores para criar uma tarefa
    # a função criar tarefa foi desenvolvida e testada no passo anterior
    tarefa = {
        'titulo': titulo,
        'descricao': descricao,
        'status': False
    }
    tarefas.append(tarefa)
    # retorna o resultado em um formato json
    return jsonify(tarefa)
```

:x: ops! Parece que os testes ainda não estão passando!

O código de status da reuisição não é o correto.

No retorno da função acrescente o valor 201 como visto abaixo.

```python
    return jsonify(tarefa), 201
...

...
```
:heavy_check_mark: Testes passando! Vamos prosseguir. Preciso me assegurar que o caso negativo também esteja correto.

E se eu me esquecer da descrição da tarefa?

```python
def test_criar_tarefa_sem_descricao():
    with app.test_client() as c:
        # o código de status deve ser 400 indicando um erro do cliente
        resp = c.post('/task', data=json.dumps({'titulo': 'titulo'}),
                      content_type='application/json')
        assert resp.status_code == 400
```

Vamos corrigir isto, o código ficará assim:

```python
from flask import Flask, jsonify, request, abort

app = Flask('Meu app')

tarefas = []


@app.route('/task', methods=['POST'])
def criar():
    # recebe o título e a descrição através do corpo da requisição
    titulo = request.json['titulo']
    descricao = request.json.get('descricao')
    if not descricao:
        abort(400)
    # utiliza estes valores para criar uma tarefa
    # a função criar tarefa foi desenvolvida e testada no passo anterior
    tarefa = {
        'titulo': titulo,
        'descricao': descricao,
        'status': False
    }
    tarefas.append(tarefa)
    # retorna o resultado em um formato json
    return jsonify(tarefa), 201

```

A faixa de código de status que vai de 400 até 499 é destinada a erros do cliente, e neste caso ele está tentando enviar um válor inválido.

:heavy_check_mark: Verifique se os testes agora passam. E se eu não passar o título? Precisamos corrigir isto também.

Um teste para esta condição seria:

```python
def test_criar_tarefa_sem_titulo():
    with app.test_client() as c:
        # o código de status deve ser 400 indicando um erro do cliente
        resp = c.post('/task', data=json.dumps({'descricao': 'descricao'}),
                      content_type='application/json')
        assert resp.status_code == 400
```

e o código final

```python
from flask import Flask, jsonify, request, abort

app = Flask('Meu app')

tarefas = []


@app.route('/task', methods=['POST'])
def criar():
    # recebe o título e a descrição através do corpo da requisição
    titulo = request.json.get('titulo')
    descricao = request.json.get('descricao')
    if not descricao or not titulo:
        abort(400)
    # utiliza estes valores para criar uma tarefa
    # a função criar tarefa foi desenvolvida e testada no passo anterior
    tarefa = {
        'titulo': titulo,
        'descricao': descricao,
        'status': False
    }
    tarefas.append(tarefa)
    # retorna o resultado em um formato json
    return jsonify(tarefa), 201
```

TODO: testar de forma manual

:heavy_check_mark: Parabéns, temos a nossa primeira funcionalidade adicionada ao sistema!

Antes de avançar para o próximo passo salve esta versão do código.

Primeiro passo é checar o que foi feito até agora:
```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	main.py
	test_main.py

nothing added to commit but untracked files present (use "git add" to track)
```

Vemos dois arquivos não rastreados, precisamos avisar ao controle de versão que monitore estes arquivos.

`$ git ad main.py test_main.py`

:floppy_disk: Agora vamos marcar esta versão como salva.

`git commit -m "inserindo uma tarefa"`

:octocat: Por fim envie ao github a versão atualizada do projeto.

`git push`

:sunglasses: Parabéns! Seu primeiro passo no desenvolvimento da nossa lista de afazeres está pronto? Mas e agora, como sei se uma tarefa foi realmente adicionada? Vamos ao próximo passo.

[Ir para o passo 6 :arrow_right:](passo06.md)

[:leftwards_arrow_with_hook: Voltar ao README ](README.md)
