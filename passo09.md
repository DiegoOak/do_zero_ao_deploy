# Detalhando tarefas

Como a listagem de tarefas não apresenta a descrição, devemos fornecer uma maneira de consultar individualmente uma tarefa
para maiores detalhes.

```python

def test_detalhando_tarefa():
    memdb.clear()
    tarefa = Tarefa('titulo', 'descrição')
    memdb[tarefa.id] = tarefa
    with app.test_client() as c:
        resp = c.get('/task/{}'.format(tarefa.id))
        assert resp.status_code == 200
        data = json.loads(resp.data.decode('utf-8'))
        assert data['titulo'] == 'titulo'
        assert data['descricao'] == 'descrição'
```

:x: Com os testes falhando, vamos escrever o código necessário desta funcionalidade.

```python

@app.route('/task/<int:id_tarefa>', methods=['GET'])
def detalhar(id_tarefa):
    try:
        tarefa = recuperar_tarefa(id_tarefa)
        return jsonify({
            'id': tarefa.id,
            'titulo': tarefa.titulo,
            'descricao': tarefa.descricao,
            'status': tarefa.status,
        })
    except KeyError:
        return jsonify({'error': 'task not found'}), 404
```

:heavy_check_mark: OK! Os testes estão passando, mas e se eu procurar uma tarefa não existente?

```python

def test_erro_ao_detalhar_tarefa():
    memdb.clear()
    with app.test_client() as c:
        resp = c.get('/task/42')
        assert resp.status_code == 404
```
Vamos assegurar que este caso está previsto escrevendo um teste.

:heavy_check_mark: Mais uma funcionalidade feita e testada! Parabéns! Vamos continuar...


[Ir para o passo 10 :arrow_right:](passo10.md)

[:leftwards_arrow_with_hook: Voltar ao README ](README.md)
