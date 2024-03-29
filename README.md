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
## *Iniciando o projeto*

* Ativar VENV -> Comando runserver

* Você pode usar alguma IDE de escolha própria para abrir o projeto criado, no meu caso vou utilizar o Pycharm:
Vá em File-> open -> "pasta_do_projeto"

* Através do terminal do Pycharm localize a pasta da venv para que possamos ativá-la com o comando:
```
source venv/bin/activate
``` 

* Ainda no terminal do Pycharm inicie a aplicação com o comando abaixo, posteriormente você pode acessar a url http://localhost:8000/ para testar se aplicação esta rodando:
```
python manage.py runserver
```

## *Como criar uma app*
```
python manage.py startapp escola
```
* Vá em views.py que faz o controle das requisições e renderize  um json com as requisições que estão chegando.EX:
Vá em escola -> views.py
```
from django.http import JsonResponse

def alunos(request):
    if request.method == 'GET':
        aluno = {'id':1, 'nome':'Guilherme'}
        return JsonResponse(aluno)
```

## *Como cadastrar uma url*

* Quando for acessada uma URL ser visualizado o json com as requisições feitas por essa página. EX:

Acesse setup -> urls.py e importe alunos da view de escola:
```
from escola.views import alunos
```

e crie um novo path para o retorno do json com os alunos
```
urlpatterns = [
    path('alunos/', alunos),
]
```

## *O que é uma API*

API é um núcleo comum de funcionalidades que pode ser usado por várias aplicações diferentes. 

A API será o contato direto entre uma aplicação e o servidor assim fazendo requisições e enviando respostas.

OBS: uma API não renderiza uma página web.

## *Django Rest Framework*

é uma ferramenta para a construção de APIs e vai ajudar a gerenciar as urls e requisições no django.

* Instalação:

Acesse o link: https://www.django-rest-framework.org/ para ver os comandos de instalação necessários.

Abra o terminal dentro do projeto que esta trabalhando, com sua venv ativa e digite os comandos:
```
pip install djangorestframework

pip install markdown
```
Acesse o arquivo de setup -> settings.py para indicar que o novo app django rest foi instalado.
```#Application definition

INSTALLED_APPS = [
    'rest_framework',
]
```
## *Criando um modelo*

* Para que os dados da view reflitam os dados do banco de dados é necessário que um modelo seja criado. EX: no arquivo models.py crie uma classe para manter todas as informações de um aluno e um método para representar esse objeto aluno faça o mesmo para curso.

Acesse escola -> models.py 
```
from django.db import models

class Aluno(models.Model):
    nome = models.Charfield(max_length=30)
    rg = models.Charfield(max_length=9)
    cpf = models.Charfield(max_length=11)
    data_nascimento = models.Datefield()

    def __str__(self):
        return self.nome

class Curso(models.Model):
    NIVEL = (
        ('B', 'Básico'),
        ('I', 'Intermediário'),
        ('A', 'Avançado')
    )    
    codigo_curso = models.Charfield(max_length=10)
    descricao = models.Charfield(max_length=100)
    nivel = models.Charfield(max_length=1, choices=NIVEL, blank=False, null=False, default='B')

    def __str__(self):
        return self.descricao 
```

* Após a criação do modelo é necessário fazer sua migração para o banco de dados. EX: 

Acesse ao arquivo setup -> settings.py acrescente nos apps instalados escola.

```
INSTALLED_APPS = [
    'escola',
]
```
No terminal com sua venv ativada insira o comando:
```
python manage.py makemigrations

python manage.py migrate
```

## *Django Admin*

* Para configurar o Admin acesse o arquivo admin.py. EX:

Acesse o arquivo escola -> admin.py
```
from django.contrib import admin
from .models import Aluno, Curso

class Alunos(admin.ModelAdmin):
    list_display = ('id','nome','rg','cpf','data_nascimento')
    list_display_links = ('id', 'nome')
    search_fields = ('nome',)
    list_per_page = 20

admin.site.register(Aluno, Alunos)

class Cursos(admin.ModelAdmin):
    list_display = ('id','codigo_curso','descricao')
    list_display_links = ('id', 'codigo_curso')
    search_fields = ('codigo_curso',)

admin.site.register(Curso, Cursos)

```

## *Criar um super user*

* acesse o terminal com sua venv ativada e crie o super user. EX:
```
python manage.py createsuperuser
```
insira o email do super usuario e depois a senha
login: darth@alura.com
senha: 123
 
## *Criar uma view:*

* vá em escola -> views.py:

```
from django.http import JsonResponse

def alunos(request):
    if request.method == 'GET':
        aluno = {'id':1, 'nome':'Guilherme'}
        return JsonResponse(aluno) 
```
## *Dica:*

* Você pode fazer uma busca geral em seu projeto por uma palavra chave
ajudando a identificar um arquivo ou algum trecho de código. Para fazer 
essa busca você pode ir em Navigate -> Search Everywhere ou utilizar o 
atalho "double click SHIFT" que funciona pressionando rapidamente a tecla 
SHIFT duas vezes

## *Pegar um aluno no BD e retornar um json:*

* Vá em admin.py e em models.py para configurar a classe alunos e cursos:

indicar o formato json no models.py para que a API do Django entenda
de forma que a API leia o JSON e a aplicação veja em python;

Isso é feito pelo arquivo serializer, que converte informações entre a API (JSON) e a aplicação (Python) também serve como
um filtro para os dados que deseja ser disponibilizados para 
API ou não. 

Vá no seu projeto e crie um novo arquivo chamado seriallizer.py

Ex: escola-> criar novo arquivo -> dê o nome de serializer.py

```
from rest_framework import serializers
from escola.models import Aluno,Curso

class AlunoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Aluno
        fields = ['id','nome','rg','cpf','data_nascimento']

class CursoSerializer(serializers.ModelSerializer):
    class Meta:        
        model = Curso    
        fields = '__all__'

```

OBS: O Serializer permite que dados sejam convertidos para a forma
python nativa para que a RM do Python possa entender facilmente 
renderizadas em JSON, XML ou até outros tipos.

* É necessário que a view indique quais dados foram filtrados ou 
selecionados pelo serializer. 

Viewset - este permite que a pessoa que está desenvolvendo a API
consiga pensar na modelagem de negócio. 

Vá em escola -> views.py 


```
from rest_framework import viewsets
from escola.models import Aluno, Curso
from escola.serializer import AlunoSerializer, CursoSerializer

class AlunoViewSet(viewsets.ModelViewSet):
    """"Exibindo todos os alunos e alunas""""
    queryset = Aluno.objects.all()
    serializer_class = AlunoSerializer

class CursoViewSet(viewsets.ModelViewSet):
    """"Exibindo todos os cursos""""
    queryset = Curso.objects.all()
    serializer_class = CursoSerializer

```

* Configuração das rotas:
Anteriormente as urls do sistema estavam sendo direcionados para o 
retorno do ID do aluno de forma manual e fixa, no entanto precisamos ajustar as URLs para que sejam retornados todos os alunos do banco. De
forma que os viewsets criados sejam utilizados.

Vá em setup -> urls.py

```
from django.contrib import admin
from django.urls import path, include
from escola.views import AlunoViewSet, CursoViewSet
from rest_framework import routers

router = routers.DefaultRouter()
router.register('alunos',AlunosViewSet, basename='Alunos' )
router.register('cursos',CursosViewSet, basename='Cursos')

urlpatterns=[
    path('admin/',admin.site.urls),
    path('', include(router.urls))
]

```

## *Método GET e POST:*

* Pode ser criado um novo elemento somente com Django Rest no formato JSON.

* O POST é uma requisição da sua API para ser criado um novo elemento. Você está enviando dados para sua aplicação 

* O GET é o recebimento de dados de um app ou de um local auxiliar para 
sua API.

* Dica: Pode se usar o Postman para testar as APIs:

1- faça download do Postman no site: https://www.postman.com/downloads/
2- Instale o Postman no seu PC
3- No Dropdown ao lado do campo de URL selecione o método desejado.
4- insira a URL no campo "Enter request the URL" 
5- clique no botão SEND
6- a baixo verifique o retorno


## *Método PUT e PATCH:*

* O PUT e o PATCH são metódos utilizados para atualização no HTTP. 

* O PUT substitui todos os dados atuais de um recurso de destino pelos dados
passados na requisição. Exemplo: No caso de uma atualização completa de um 
determinado recurso.

* O PATCH permite que seja atualizado apenas um subconjunto de dados 
de um determinado recurso

* Testar o metodo PUT no Postman:

1- faça download do Postman no site: https://www.postman.com/downloads/
2- Instale o Postman no seu PC
3- No Dropdown ao lado do campo de URL selecione o método GET (será necessário trazer os dados desejados para que possamos alterá-los)
4- insira a URL no campo "Enter request the URL" 
5- clique no botão SEND
6- a baixo verifique o retorno
7- No primeiro quadro abaixo verifique se as configurações estão adequadas:
    7.1- O quadro deve esta na aba Body
    7.2- O radio button RAW deve estar selecionado
    7.3- O dropdown de tipo de documento a ser redigido deve marcar JSON
8- No segundo quadro abaixo verifique se as configurações estão adequadas:
    8.1- O quadro deve esta na aba Body
    8.2- O quadro também deve esta na aba Pretty
9- copie o JSON com a alteração desejada no primeiro quadro
3- No Dropdown ao lado do campo de URL selecione o método PUT (será necessário enviar a alteração realizada dos dados desejados)
4- insira a URL no campo "Enter request the URL" 
5- clique no botão SEND
6- a baixo verifique o retorno

## *Método DELETE:*

* Podemos deletar um aluno de nossa aplicação através de três caminhos diferentes:
1 - Pela aplicação de fato;
    1.1- Inicio -> Escola -> Alunos -> Determinado aluno -> DELETE;
2 - Pelo Django REST API;
    2.1- Utilizando a URL do aluno por exemplo http://localhost:8000/alunos/1/ logo em seguida apertando o botão DELETE;
3 - Pelo Postman;
    3.1- Utilizando a URL do aluno por exemplo http://localhost:8000/alunos/1/ logo em seguida enviando o método DELETE;

OBS: Com os métodos anteriormente vistos podemos fazer o CRUD, tendo em vista:
POST - Cria os elementos ( Create )
GET - Retornar os elementos ( Read )
PUT/PATCH - Atualiza os elementos ( Update )
DELETE - Deleta os elementos ( Delete )

## *Criar um relacionamento:*

* Vamos criar um relacionamento entre ALUNO e CURSO, um relacionamento chamado
matricula, pois assim saberemos os alunos que estão matriculados em cada curso.

* Vá em models.py e crie uma nova classe matricula. Exemplo:

```
class Matricula(models.Model):
    PERIODO = (
        ('M','Matutino'),
        ('V','Vespertino'),
        ('N','Noturno'),
    )
    aluno = models.ForeignKey(Aluno,on_delete=models.CASCADE)
    curso = models.ForeignKey(Curso,on_delete=models.CASCADE)
    periodo = models.CharField(max_length=1, choices=PERIODO, blank=False, null=False,default='M')

```

* Vá em admin.py e crie uma propriedade matrícula. Exemplo:

```
from escola.models import Aluno, Curso, Matricula

class Matriculas(admin.ModelAdmin):
    list_display = ('id', 'aluno', 'curso', 'periodo')
    list_display_links = ('id',)

admin.site.register(Matricula,Matriculas)

* Faça a migração do modelo para o banco de dados:

- No terminal com a venv ativa digite: python manage.py makemigrations
- Será criada a migração de Matriculas, agora digite: python manage.py migrate
- As aplicações serão aplicadas no banco, agora digite: python manage.py runserver
- O servidor deverá rodar para verificarmos a nova migração 

 

```

Recurso de Matricula -  continuar
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



