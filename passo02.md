# Utilizando o pipenv para instalar dependências

Única bilbioteca que iremos precisar no nosso projeto é o micro framework flask. Para instalá-lo vamos utilizar o pipenv.

O [flask](http://flask.pocoo.org/) é um micro framework web bastante conhecido por sua arquitetura minimalista.

```bash
$ pipenv install flask
Creating a virtualenv for this project…
⠋Using base prefix '/usr'
New python executable in /home/cassiobotaro/.virtualenvs/todoapp-vVbOzvc4/bin/python
Installing setuptools, pip, wheel...done.

Virtualenv location: /home/cassiobotaro/.virtualenvs/todoapp-vVbOzvc4
Creating a Pipfile for this project…
Installing flask…
Collecting flask
  Using cached Flask-0.12.2-py2.py3-none-any.whl
Collecting itsdangerous>=0.21 (from flask)
Collecting Jinja2>=2.4 (from flask)
  Using cached Jinja2-2.9.6-py2.py3-none-any.whl
Collecting click>=2.0 (from flask)
  Using cached click-6.7-py2.py3-none-any.whl
Collecting Werkzeug>=0.7 (from flask)
  Using cached Werkzeug-0.12.2-py2.py3-none-any.whl
Collecting MarkupSafe>=0.23 (from Jinja2>=2.4->flask)
Installing collected packages: itsdangerous, MarkupSafe, Jinja2, click, Werkzeug, flask
Successfully installed Jinja2-2.9.6 MarkupSafe-1.0 Werkzeug-0.12.2 click-6.7 flask-0.12.2 itsdangerous-0.24

Adding flask to Pipfile's [packages]…
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
Updated Pipfile.lock (d3d473)!
```

Aqui podemos observar que foi criado um ambiente virtual e a instalação da biblioteca ocorreu neste ambiente, também podemos notar que outras bilbiotecas que são depêndencias do flask, foram instaladas.

Dois novos arquivos foram criados para controle das depêndencias, `Pipfile` e `Pipfile.lock`.

Neste momento deu diretório deve estar assim:
```
.
├── LICENSE
├── Pipfile
├── Pipfile.lock
└── README.md
```

Vamos testar nossa instalção então?

```python
$ python3
Python 3.6.2 (default, Jul 20 2017, 03:52:27)
[GCC 7.1.1 20170630] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import flask
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'flask'
>>>
```

Se entramos no python e tentamos importar a biblioteca flask, um erro é retornado dizendo que o módulo não foi encontrado.

Acontece que instalamos o flask somente no ambiente virtual. Para entrarmos no ambiente virtual digite `pipenv shell`.

```bash
$ python3
Python 3.6.2 (default, Jul 20 2017, 03:52:27)
[GCC 7.1.1 20170630] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import flask
>>>
```

Já dizia Uncle Bob, "Código sem testes é um código legado.". Vamos então instalar o pytest, que é uma ferramenta que auxilia na execução de testes.

O comando para instalção é `pipenv install pytest --dev`.

```bash
$ pipenv install pytest --dev
Installing pytest…
Collecting pytest
  Using cached pytest-3.2.3-py2.py3-none-any.whl
Collecting py>=1.4.33 (from pytest)
  Using cached py-1.4.34-py2.py3-none-any.whl
Requirement already satisfied: setuptools in /home/cassiobotaro/.virtualenvs/todoapp-vVbOzvc4/lib/python3.6/site-packages (from pytest)
Installing collected packages: py, pytest
Successfully installed py-1.4.34 pytest-3.2.3

Adding pytest to Pipfile's [dev-packages]…
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
Updated Pipfile.lock (bbaf41)!
```
Preste atenção no --dev, pois esta dependência é somente durante o desenvolvimento, ou seja, não precisamos da pytest para o nosso programa funcionar.


Instalado as dependências, vamos salvar uma primeira versão do nosso projeto com o nosso andamento?

Primeiro passo é checar o que foi feito até agora:
```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	Pipfile
	Pipfile.lock

nothing added to commit but untracked files present (use "git add" to track)
```

Vemos dois arquivos não rastreados, precisamos avisar ao controle de versão que monitore estes arquivos.

`$ git add Pipfile Pipfile.lock`

Agora vamos marcar esta versão como salva.

`git commit -m "adionando dependências do projeto"`

Por fim envie ao github a versão atualizada do projeto.

`git push`

Parabéns! Como um chef de cozinha, você já preparou suas ferramentas, daqui pra frente vamos cozinhar!

[Ir para o passo 3 :arrow_right:](passo03.md)

[:leftwards_arrow_with_hook: Voltar ao README ](README.md)
