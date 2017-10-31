# Listando tarefas

A listagem de tarefas deve retornar uma lista com todas as tarefas. Seu código de status é sempre 200 ainda que esteja vazia.

Traduzindo isto em um teste automatizado que deve ser acrescentado ao arquivo de testes `test_main.py`.

```python
from main import tarefas

def test_listar_tarefa_sem_conteudo():
    tarefas.clear()
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
    return jsonify(tarefas)
```

Agora vamos checar uma lista com algum conteúdo.

```python
def test_listar_tarefa_com_conteudo():
    tarefas.clear()
    tarefas.append({'titulo': 'titulo', 'descricao': 'descricao'})
    with app.test_client() as c:
        resp = c.get('/task')
        data = json.loads(resp.data.decode('utf-8'))
        assert resp.status_code == 200
        assert len(data) == 1

```

:heavy_check_mark: É isto, não esqueça de verifcar os testes! Agora temos uma listagem das nossas tarefas.

[Ir para o passo 7 :arrow_right:](passo07.md)

[:leftwards_arrow_with_hook: Voltar ao README ](README.md)
