## Python - Downloads e Instalação
https://www.python.org/downloads/

Windows: https://www.python.org/ftp/python/3.11.3/python-3.11.3-amd64.exe

MacOS: https://www.python.org/ftp/python/3.11.3/python-3.11.3-macos11.pkg

Linux: 

-- Procurar no GOOGLE, o repositório do python, pela distribuição. --
Fedora: `sudo dnf install python3 python3-devel`

Ubuntu: `sudo apt-get install python3 python3-dev`

### VENV - ambiente virtual e ativar
Criar o ambiente virtual
`python -m venv <venv>`

Ativar
- Linux: `source <venv>/bin/activate`
- Windows: `<venv>\Scripts\activate.bat`
PS.: Sempre que for criar um projeto lembre-se de verificar se a venv está ativa.

## DJANGO
`pip install django`

### Criar projeto django
`django-admin startproject <nome_do_repositorio> .`

#### Configurar o setting.py
-> templates
```python
TEMPLATES = [
	{
		'BACKEND': 'django.template.backends.django.DjangoTemplates',
		'DIRS': [os.path.join(BASE_DIR, 'core/templates')], #Adicione a linha
		'APP_DIRS': True,
		'OPTIONS': { ...
```

-> configurar na urls.py os estaticos e medias
```python
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
STATICFILES_DIRS = [
	os.path.join(BASE_DIR, 'core/static')
]

MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL = '/media/'
```

-> Linguagem e timezone
```Python
LANGUAGE_CODE = 'pt-br'
TIME_ZONE = 'America/Recife'
```

#### Configurar urls.py
```python 
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

### Apps - agora iniciamos com os apps : )
`python manage.py startapp <nomedoapp>`

1º adicionar `<nomedoapp>` ao INSTALLED_APPS, no settings.py ;
``` Python
from django.urls import path, include

urlpatterns = [
	path('', include('<nomedoapp>.urls'))
]
```
2º criar um arquivo `urls.py`, no app criado, e inicar com a variável `urlpatterns`;

### Modelar classes
Os principais tipo:  -- tem outros : ) 
- CharField(max_length=255) 
- TextField()
- IntegerField()
- DateField()
- FileField()
- ImageField()

OBS.: Ao utilizar o ImageField, lembrar de instalar a biblioteca Pillow:
`pip install Pillow`

Recomendação:
Caso desejem visualizar o banco de dados, utilize o SqliteView é uma extensão disponível no vscode.

-> Sempre que atulizar o modelo ou criar um ou delete:
```Python
python manage.py makemigrations
python manage.py migrate
```

### Templates
 - `{% load static %}` -- permitir o uso de arquivo estáticos, como imagem, js, css, deve ser adicionado no topo do arquivo
 - `{% static 'nome do estatico' %}`
 - `{% url 'nome da rota' %}`
 - `{% for i in 'nome da variavel do contexto' %}`
 - `{% if condicao %}`
 - `{% csrf_token %}` -- utilizar ao enviar um formulário, é referente a segurança do django
 - `{{ qs }}` -- adicionar um dado no html, que veio no contexto

Em `form` que irão enviar algum tipo de media - img, .mkv, jpeg, js, pdf, ... - utilizar na **tag form** `<form enctype="multipart/form-data"></form>`

### Views
-> Comando para obter dados de input, forms...
```Python
request.POST['<nomedoinput>']
request.POST.get('<nomedoinput>')

request.GET['<nomedoinput>']
request.GET.get('<nomedoinput>')

request.FILES['<nomedoinput>']
request.FILES.get('<nomedoinput>')
```

-> Comando para consulta -Obter dados do banco de dados-.
```Python
<model>.objects.all() # Obter todos os objetos do modelo, que estão no banco
<model>.objects.filter(<atributo>=<valor>) # Obter os objetos filtrados
<model>.objects.get(<atributo>=<valor>) # Obter um único objetos, caso tenha mais de um, é disparado um erro
```

-> Comando para inserir/savar, um objetos/dados no banco de dados.
```Python
<model>(
	atributo1 = valor1,
	atributo2 = valor2,
	.
	.
	.
	atributo9..9 = valor9..9
).save() # Save, salva os dados no banco de dados

# Exemplo
bebida = Bebida(
	formula='H2O',
	aparencia='Transparente'
)
bebida.save()
```

-> Deletar um objeto do banco
```Python
var1 = <model>.objects.get(id=1) # Obter um objeto qualquer
var1.delele() # deleta o objeto existente no banco
```

#### Paginação - Views
```python
from django.core.paginator import Paginator, EmptyPage, PageNotAnInteger

page = request.GET.get('page', 1)
paginator = Paginator(<qs>,10) # Onde tem qs, é o conjunto de dados, a lista em sí - queryset -

try:
	result = paginator.page(page)
except PageNotAnInteger:
	result = paginator.page(1)
except EmptyPage:
	result = paginator.page(paginator.num_pages)

context = {
	'result': result
}
```

```django
{% for i in result.paginator.page_range %}
<li>
	<a href="?page={{ i }}">
		{{ i }}
	</a>
</li>
{% endfor %}
```

#### GITHUB
PS.: Crie um arquivo `.gitignore`, nele adicione a palavra venv -- ou qualquer nome da venv que você tenha criado

### DEPLOYYYY
Para ser mais simples é so seguir o tutorial disponível no DjangoGirls

```
pip3.8 install --user pythonanywhere
```

```
pa_autoconfigure_django.py --nuke --python=3.8 https://github.com/<your-github-username>/<nome-do-repositorio>.git
```
OBS1.: quando você tiver feito algo utilizando o pythonanywhere e esse comando acima, --nuke vai 'limpar' e reinstalar algumas coisas
