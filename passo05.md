# Criando uma tarefa

:construction: daqui pra frente estamos em construção, me desculpe o transtorno. Espero que até dia 3 de novembro todo o tutorial

esteja revisado e finalizado(não inclui os bônus que devem ser entregue até a próxima semana)

TODO: descriçao sobre o que será feito

TODO: passo a passo no modelo TDD

    :x: texto de interação e levantamento do teste

    teste

    roda pytest

    descrição do problema

    :heavy_check_mark: proposta de solução

    código

    roda pytest

    descrição da solução

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

	todo.py
	test_todo.py

nothing added to commit but untracked files present (use "git add" to track)
```

Vemos dois arquivos não rastreados, precisamos avisar ao controle de versão que monitore estes arquivos.

`$ git add todo.py test_todo.py`

:floppy_disk: Agora vamos marcar esta versão como salva.

`git commit -m "inserindo uma tarefa"`

:octocat: Por fim envie ao github a versão atualizada do projeto.

`git push`

:warning: Não se esqueça de verificar a contrução(build) do projeto está ok lá no [travis](https://travis-ci.org/).

:sunglasses: Parabéns! Seu primeiro passo no desenvolvimento da nossa lista de afazeres está pronto? Mas e agora, como sei se uma tarefa foi realmente adicionada? Vamos ao próximo passo.

[Ir para o passo 6 :arrow_right:](passo06.md)

[:leftwards_arrow_with_hook: Voltar ao README ](README.md)
