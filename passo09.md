# Entregando tarefas

Só falta mais uma coisa para nossa "TODO list" estar completa. Precisamos ser capazes de  marcar uma tarefa como concluída.

```python
def test_entregando_tarefas():
    memdb.clear()
    tarefa = Tarefa('titulo', 'descrição')
    memdb[tarefa.id] = tarefa
    with app.test_client() as c:
        resp = c.put('/task/{}'.format(tarefa.id), data={'status': True})
        data = json.loads(resp.data.decode('utf-8'))
        assert resp.status_code == 200
        assert data['status'] == 'True'
```

E como fazer isto funcionar?

```
@app.route('/task/<int:id_tarefa>', methods=['PATCH', 'PUT'])
def editar(id_tarefa):
    try:
        t = Tarefa(request.form['titulo'],
                   request.form['descricao'],
                   request.form['status'])
        tarefa = editar_tarefa(id_tarefa, t)
        return jsonify({
            'id': tarefa.id,
            'titulo': tarefa.titulo,
            'descricao': tarefa.descricao,
            'status': tarefa.status,
        })
    except KeyError:
        return jsonify({'error': 'task not found'}), 404

```
:heavy_check_mark: Mais uma funcionalidade feita e testada! Parabéns! Vamos coloca-la no ar?


[Ir para o passo 11 :arrow_right:](passo11.md)

[:leftwards_arrow_with_hook: Voltar ao README ](README.md)
