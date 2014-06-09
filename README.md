# Buildout for OpenERP with PostgreSQL
OpenERP 7.0, PostgreSQL 9.3.4 and Supervisord 3.0
- Buildout create cron for starting Supervisord after machine reboot
- Supervisor run PostgreSQL, more http://supervisord.org/
- PostgreSQL compile and run under user (not need root login), and build enabled "trust" authentication for local connections,
 more http://www.postgresql.org/docs/9.3/static/auth-methods.html

# Usage
```
$ cd virtualenv projects dir (cd $WORKON_HOME)
$ git clone https://github.com/kybi/buildout-openerp-postgres openerp
$ mkvirtualenv openerp
$ cdvirtualenv
$ mkdir eggs
$ python bootstrap.py
$ buildout
$ supervisord             # start supervisor deamon
$ supervisorctl status    # check if running postgres
$ psql -d postgres -c 'CREATE DATABASE ...'  # copy this line from shell after run buildout
$ start_openerp
$ http://localhost:8069
  login: admin
  pass:  admin
```

# Settings
defaults in buildout.cfg

```
openerp_version = nightly 7.0 latest
openerp_xmlrpc_port = 8069
openerp_xmlrpcs_port = 8071

postgres_version = 9.3.0
postgres_host = 127.0.0.1
postgres_db_name = openerp
postgres_port = 5434
postgres_maxconn = 100

supervisor_port = 9002
supervisor_url = http://127.0.0.1
```
## Configure OpenERP
config file: etc/openerp.cfg, if you want to change more options in openerp.cfg, don't edit this file,
please add section [openerp] to buildout.cfg
and set options.'add_option' = value, where 'add_option' is from openerp.cfg and run buildout again.

Example: change logging level for OpenERP
```
'buildout.cfg'
...
[openerp]
options.log_handler = [':ERROR']
...
```

if you want to run more then one instance of OpenERP, or another user is running same buildout on the same machine,
please change ports:
```
openerp_xmlrpc_port = 8069  (8069 default openerp)
openerp_xmlrpcs_port = 8071 (8071 default openerp)
supervisor_port = 9002      (9001 default supervisord)
postgres_port = 5434        (5432 default postgres)
```

# TODO
- move Usage to Fabric, more http://fabfile.org/
- generate Apache and Nginx config for virualhost with Buildout

# Contributors

## Creators

Rastislav Kober, http://www.kybi.sk




# Buildout base para proyectos con OpenERP y PostgreSQL
OpenERP master en el base, PostgreSQL 9.3.4 y Supervisord 3.0
- Buildout crea cron para iniciar Supervisord después de reiniciar (esto no lo he probado)
- Supervisor ejecuta PostgreSQL, más info http://supervisord.org/
- También ejecuta la instancia de PostgreSQL
- Si existe un archivo dump.sql, el sistema generará la base de datos con ese dump
- Si existe  un archivo frozen.cfg es el que se debeía usar ya que contiene las revisiones aprobadas
- PostgreSQL se compila y corre bajo el usuario user (no es necesario loguearse como root), se habilita al autentificación "trust" para conexiones locales. Más info en more http://www.postgresql.org/docs/9.3/static/auth-methods.html
- Existen plantillas para los archivo de configuración de Postgres que se pueden modificar para cada proyecto.
 

# Uso (adaptado)
En caso de no haberse hecho antes en la máquina en la que se vaya a realizar, instalar las dependencias que mar Anybox
- Añadir el repo a /etc/apt/sources.list:
```
$ deb http://apt.anybox.fr/openerp common main
```
- Si se quiere añadir la firma. Esta a veces tarda mucho tiempo o incluso da time out. Es opcional meterlo
```
$ sudo apt-key adv --keyserver hkp://subkeys.pgp.net --recv-keys 0xE38CEB07
```
- Actualizar e instalar
```
$ sudo apt-get update
$ sudo apt-get install openerp-server-system-build-deps
```
- Para poder compilar e instalar postgres (debemos valorar si queremos hacerlo siempre), es necesario instalar el siguiente paquete (no e sla solución ideal, debería poder hacerlo el propio buildout, pero de momento queda así)
```
$ sudo apt-get install libreadline-dev
```
- Descargar el  repositorio de buildouts :
```
$ git clone https://github.com/Pexego/Buildouts.git
```
- [EN REVISIÓN] Hacer checkout de la rama deseada según proyecto
```
$ git checkout <rama>
```
- Crear un virtualenv dentro de la carpeta del respositorio. Esto podría ser opcional, obligatorio para desarrollo o servidor de pruebas, tal vez podríamos no hacerlo para un despliegue en producción. Si no está instalado, instalar el paquete de virtualenv
```
$ sudo apt-get install python-virtualenv
$ virtualenv sandbox --no-setuptools
```
- Crear la carpeta eggs (no se crea al vuelo, ¿debería?
```
$ mkdir eggs
```
- Ahora procedemos a ehecutar el buildout en nuestro entorno virtual
```
$ sandbox/bin/python bootstrap.py
```
- Y por último
```
$ bin/buildout
```



## Configurar OpenERP
Archivo de configuración: etc/openerp.cfg, si sequieren cambiar opciones en  openerp.cfg, no se debe editar el fichero,
si no añadirlas a la sección [openerp] deñ buildout.cfg
y establecer esas opciones .'add_option' = value, donde 'add_option'  y ejecutar buildout otra vez.

Por ejmplo: cambiar el nivel de logging de OpenERP
```
'buildout.cfg'
...
[openerp]
options.log_handler = [':ERROR']
...
```

Si se quiere jeecutar más de una instancia de OpenERP, se deben cambiar los puertos,
please change ports:
```
openerp_xmlrpc_port = 8069  (8069 default openerp)
openerp_xmlrpcs_port = 8071 (8071 default openerp)
supervisor_port = 9002      (9001 default supervisord)
postgres_port = 5434        (5432 default postgres)
```

# TODO
- Generar Apache and Nginx config for virualhost with Buildout

# Contributors

Rastislav Kober, http://www.kybi.sk
Santi Argüeso https://github.com/santiarg
## 
