# Práctica 3-5. Despliegue de una aplicación Flask

## Instalación de prerrequisitos

Para instalar los paquetes necesarios para esta práctica hay que ejecutar: 

```console
sudo apt install nginx
sudo apt install python3-pip pipenv
```

De esta manera, tendremos instalados:

- Servidor Nginx.
- El gestor para paquetes de Python, pip.
- El gestor de entornos virtuales de Python, pipenv.

Podemos comprobar la correcta instalación de estas herramientas ejecutando:

```console
sudo nginx -v
pip -V
pipenv --version
pip list	# Mostrará todos los paquetes de Python instalados.
```

## Despliegue de una aplicación de prueba

Creamos un directorio para almacenar el contenido de nuestro proyecto en 
/var/www, hacemos dueño a nuestro usuario y le damos los permisos necesarios 
para que pueda ser leído por cualquier usuario: 

```console
sudo mkdir /var/www/app
sudo chown -R alberto:www-data /var/www/app
sudo chmod -R 775 /var/www/app
```

Creamos un archivo '.env' en el directorio del proyecto con el siguiente 
contenido:

```console
FLASK_APP=wsgi.py
FLASK_ENV=production
```

Iniciamos el entorno virtual e instalamos en él las dependencias de Flask y 
Gunicorn: 

```console
pipenv shell
pipenv install flask gunicorn
```

Creamos ahora una aplicación de prueba 'app.py':

```py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
	return '<h1>Aplicación desplegada</h1>
```

Y también un archivo 'wsgi.py', que será el encargado de ejecutar la aplicación 
principal:

```py
from app import app

if __name__ == '__main__':
	app.run(debug=False)
```

Podemos despliegar la aplicación ejecutando: 

```console
flask run --host '0.0.0.0'
```

Y comprobamos que está en funcionamiento:


