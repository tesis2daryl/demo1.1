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

2. Configuraciòn de vue

- instalamos globalmente la herramienta CLI de Vue:

        $ npm install -g @vue/cli4.5.11

- dentro del la carpeta "flask-vue" ejecutams el siguiente comando para inciar el proyecto de Vue llamado 'client':

        $ vue create client

Seleccionamos las funciones manualmente: 

- Manually select features,
- luego seleccionamos "Choose vue version, Babel, Router y Linter/Formatter", luego presionamos enter
- Luego seleccionamos "2.x" para la version de Vue
- Luego le damos "Y" para el modo historial del enrutador y enter
- Luego "Eslint with error prevention only" ye enter
- Luego "Lint on save" y enter
- Luego "In package.json" y enter
- Luego "Lint on save" y enter.
y Listo se va a instalar toda la configuracion.


Se va a instalar muchas carpetas y archivos pero nos concentarremos en la carpeta "src".

Dentro de "client/src", tenemos los siqguientes archivos:

- `main.js`: Punto de entrada de la aplicacion, que carga e inicializa Vue junto con el componente raiz.
- `App.vue`: Componente raìz, que es el punto de partida desde el que se renderiza todos los demàs componentes.
- `carpeta Componentes`: donde se almacenan los componentes de la interfz de usuario.
- `router/index.js`: donde la Url´s se definen y se asignan a los componentes.
- `views`: almacen los componentes de la intrefaz de usuario vinculados al enrutador.
- `assets`: donde se almacenan los archivos estaticos, como imagenes y fuentes.

Ahora os centramos en el archivo `client/src/components/HelloWorld.vue`, es un componente de archivo unico, que se divide en 3 partes:

1) template: para el HTML
2) script: implementa la logica del componente a traves de JS.
3) styles: para stilos CSS.

Ahora toca correr el servidor.

        $ cd client
        $ npm run serve

nos dirigimos ´http://localhost:80808´ en el navegador.

----

Eliminamos las vistas "client/src/views". Luego agregamos un nuevo componente a la carpeta components "client/src/components" llamado Ping.vue

        <template>
                <div>
                  <p>{{ msg }}</p>
                </div>
        </template>

        <script>
        export default {
                name: 'Ping',
                data() {
                  return {
                    msg: 'Hello!',
        };
        },
        };
        </script>

ahora actualizamos el "client/router/index,js" para mapear al Componente `Ping`:

        import Vue from 'vue'
        import VueRouter from 'vue-router'

        import Ping from '../components/Ping.vue'

        Vue.use(VueRouter)

        const routes = [
        {
        path: '/ping',
        name: 'Ping',
        component: Ping,
        },
        
        ]

        const router = new VueRouter({
        mode: 'history',
        base: process.env.BASE_URL,
        routes
        })

        export default router

Finalmnete dentro de "client/src/App.vue", elimine la navegacion junto con los stilos, debe quedar asi:

        <template>
                <div id="app">
                <router-view/>
                </div>
        </template

Levantamos el servidor en caso no los este "npm run serve", y nos dirigimos a "http://localhost:8080/ping" y tendremos como resultado un "Hello!".

Ahora toca la parte mas importante dentro de la conexion Flask con Vue. 
Vamos a usar la biblioteca Axios pra enviar solicitudes Ajax.

        $ npm install axios@0.21.1 --save

Ahora actualizamos el script del componente Ping.vue, qued asì:

        <script>
        import axios from 'axios';

        export default {
        name: 'Ping',
        data() {
        return {
        msg: '',
        };
        },
        methods: {
        getMessage() {
        const path = 'http://localhost:5000/ping';
        axios.get(path)
                .then((res) => {
                this.msg = res.data;
                })
                .catch((error) => {
                // eslint-disable-next-line
                console.error(error);
                });
        },
        },
        created() {
        this.getMessage();
        },
        };
        </script>


Inicie la aplicación Flask en una nueva ventana de terminal. Debería verlo pong!en el navegador. Esencialmente, cuando se devuelve una respuesta desde el back-end, establecemos msgel valor de datadesde el objeto de respuesta.

