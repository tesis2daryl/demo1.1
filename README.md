DEPENDENCIAS PRINCIPALES

Vue v2.6.11
Vue CLI 4.5.11
Node v14.16.0
npm v6.14.11
flask v.1.1.2 (instalado dentro de cada entorno)
Python v3.8.5

NOTA: Todas las dependencias mencionadas lìneas arriba son las que estan instaladas en mi maquina (Linux) lo menciono para que cuando lo quieras correr en Win no tengas problemas de version.

1. Crearemos el servidor Flask

En mi caso estoy trabajando en Linux (me es mas facil hacerlo todo por comandos XD), Linux acepta tiene por defecto python 2.x, asi que instale python 3.x (mas adelante se detalla la version)

1.1 Necesitamos crear un entorno virtual y activarlo

- creamos el entorno virtual, con el siguiente comando (tienes que estar instalado python)


        python3 -m venv env

- activamos el entorno

        source env/bin/activate

saldrà como resultado

    (env) noa@noa-desktop:~/Escritorio/conexion-flask-vue$ 

significa que el entorno esta activado

1.2 instalaremos Flask y CORS

    (env)$ pip install Flask==1.1.2 Flask-Cors=3.0.10

en ambos casos estamos instalamos versiones especificas, tanto de Flask como de la librearia CORS.

Flask: libreria que permite utilizar el framework flask.

Cors: libreria que permite intercambiar recursos de origen cruzados; CORS define una forma en la que un navegador y un servidor pueden interactuar para determinar si es seguro permitir la solicitud de origen cruzado

- Agregue un archivo app.py al directorio "servidor" recién creado:

        from flask import Flask, jsonify
        from flask_cors import CORS

        # configuracion 
        DEBUG=True

        # instanciando la aplicacion
        app = Flask(__name__)
        app.config.from_object(__name__)

        # enable CORS
        CORS(app, resources = {r'/*': {'origins': '*'}})

        # sanity check route
        @app.route('/ping', methods=['GET'])
        def ping_pong():
            return jsonify('pong!, XD')

        if __name__ == '__main__':
            app.run()


- Ejecutamos la aplicacion, no olvidar que tenemos que estar dentro de la carpeta "servidor"

        (env) python app.py

Para probar, apunte su navegador a http://localhost:5000/ping . Deberías ver:

        pong! xd

Para matar al servidor hacer "Ctrl + c"