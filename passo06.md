# Listando tarefas

A listagem de tarefas deve retornar uma lista com todas as tarefas, mas sem suas respectivas descrições. Seu código de status é sempre 200 ainda que esteja vazia.

Traduzindo isto em um teste automatizado que deve ser acrescentado ao arquivo de testes `test_tarefa.py`.

```python
def test_listar_tarefa_sem_conteudo():
    memdb.clear()
    with app.test_client() as c:
        resp = c.get('/task')
        data = json.loads(resp.data.decode('utf-8'))
        assert resp.status_code == 200
        assert data == []

```

E  o código:

```python
@app.route('/task', methods=['GET'])
def listar():
    tarefas = []
    for tarefa in listar_tarefas():
        tarefas.append({
            'id': tarefa.id,
            'titulo': tarefa.titulo,
            'status': tarefa.status,
        })
    return jsonify(tarefas)
```

Agora vamos checar uma lista com algum conteúdo.

:warning: Para este teste iremos precisar de uma Tarefa, então não se esqueça de realizar a importação.

```python
from modelo import Tarefa
```

```python
def test_listar_tarefa_com_conteudo():
    memdb.clear()
    tarefa1 = Tarefa('titulo 1', 'descrição')
    memdb[tarefa1.id] = tarefa1
    with app.test_client() as c:
        resp = c.get('/task')
        data = json.loads(resp.data.decode('utf-8'))
        assert resp.status_code == 200
        assert len(data) == 1
        assert 'descricao' not in data[0]
```

:heavy_check_mark: E isto, não esqueça de verifcar os testes! Agora temos uma listagem das nossas tarefas.

[Ir para o passo 7 :arrow_right:](passo07.md)

[:leftwards_arrow_with_hook: Voltar ao README ](README.md)
