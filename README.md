# API com Django Rest Framework

## *Preparação do ambiente:*

* Instalação do Python

* Crie uma pasta onde ficará seu código

* Crie uma venv com o comando no terminal dentro da pasta criada 
```
python3 -m venv ./venv 
``` 

* Ative seu ambiente virtual com o comando no terminal:
```
source venv/bin/activate
```

* Instalação do Django dentro do ambiente virtualizado com o comando :
```
pip install django
```

* Crie um projeto para manter toda a configuração de nossa aplicação, com o comando:
```
django-admin startproject setup.
```

* Faça download do postman entrando no link: https://www.postman.com/downloads/ , descompacte e depois instale.

* Você pode usar alguma IDE de escolha própria para abrir o projeto criado, no meu caso vou utilizar o Pycharm:
Vá em File-> open -> "pasta_do_projeto"

* Através do terminal do Pycharm localize a pasta da venv para que possamos inicia-lá com o comando:
```
source venv/bin/activate
``` 

* Ainda no terminal do Pycharm inicie a aplicação com o comando abaixo, posteriormente você pode acessar a url http://localhost:8000/ para testar se aplicação esta rodando:
```
python manage.py runserver
```
* Como criar uma app:
```
python manage.py startapp escola
```
* Vá em views.py que faz o controle das requisições e renderize  um json com as requisições que estão chegando.EX:
Vá em escola -> views.py
```
from django.http import JsonResponse

def alunos(request):
    pass
```

## *O que é uma API*

API é um núcleo comum de funcionalidades que pode ser usado por várias aplicações diferentes. 

A API será o contato direto entre uma aplicação e o servidor assim fazendo requisições e enviando respostas.


## *Requisição GET*


Aula Requisição GET : 2:34

--------------------------------------------------------------------------------
# Readme Glossary

# Title

## Subtitle

_Italico_

**Negrito**

**_Negrito e italico_**

* lista objeto1
* lista objeto2

```
bash
print("")
```



