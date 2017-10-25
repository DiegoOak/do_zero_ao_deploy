# Removendo tarefas

Remoçao de tarefas é feita através da sua id e o código de status retrnado deve ser 204 NO CONTENT, pos nenhum conteúdo é retornado pela nossa api.

O teste para esta funcionalidade é

```python
def test_remover_tarefa():
    memdb.clear()
    tarefa1 = Tarefa('titulo 1', 'descrição')
    memdb[tarefa1.id] = tarefa1
    with app.test_client() as c:
        resp = c.delete('/task/{}'.format(tarefa1.id))
        assert resp.data == b''
        assert resp.status_code == 204
```

A implementação é simples como

```python
@app.route('/task/<int:id_tarefa>', methods=['DELETE'])
def remover(id_tarefa):
    try:
        remover_tarefa(id_tarefa)
        return '', 204
    except KeyError:
        return jsonify({'error': 'task not found'}), 404
```

Por fim, vamos adicionar um teste negativo para garantir que o sistema está funcionando corretamente mesmo em situação de falha.

```python

def test_erro_ao_remover_tarefa():
    memdb.clear()
    with app.test_client() as c:
        resp = c.delete('/task/42')
        assert resp.status_code == 404

```

:heavy_check_mark: Tudo funcionando? Ainda falta detalhar uma tarefa e editar uma tarefa.

[Ir para o passo 9 :arrow_right:](passo09.md)

[:leftwards_arrow_with_hook: Voltar ao README ](README.md)
