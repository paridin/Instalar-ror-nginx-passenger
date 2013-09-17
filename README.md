# Daemon Nginx uWSGI
==================

The daemon were written in python, it was developed on OSX

*Nginx is compiled on ``/usr/local/`` and uWSGI service installed on ``virtualenv``*

You have to change some paths in the script to execute it, also to find Nginx and uWSGI services in your system unless you have the same paths.

***Note: psutil must be installed, if you don't have it execute the command***

```bash
sudo easy_install psutil
```


# How to use it:

You must put ``start_server.py`` in your root project ``/path/your/projectName/``

# Instalación Ruby + Rails + OpenSSL + Nginx
==================

***Nota: El ambiente no debe contener ninguna instalación de Ruby en caso de tenerla eliminar todo

ubicarnos en la ruta donde descargaremos todo el contenido en mi caso ``/usr/local/src``

```
cd /usr/local/src
```

Descargamos la ultima versión de ruby 

```
sudo curl -O "https://ftp.ruby-lang.org/pub/ruby/2.0/ruby-2.0.0-p247.tar.gz"
sudo tar -xvzf ruby-2.0.0-p247.tar.gz
cd ruby-2.0.0-p247
sudo ./configure
sudo make
sudo make install
```

Instalar RVM (Ruby Vitual Machine)
```
\curl -L https://get.rvm.io | bash -s stable
```

Corrije soporte de OpenSSL
```
rvm pkg install openssl
rvm reinstall 2.0.0 --with-openssl-dir=$rvm_path/usr
```

Instando rails vía gems
```
gem update
gem install rails
```


Instalando paquetería faltante para correr el servidor de prueba

```
bundle install
```

Compilando Nginx con soporte de passenger

```
curl -O "http://nginx.org/download/nginx-1.5.2.tar.gz"
tar -xvzf nginx-1.5.2.tar.gz

rvmsudo passenger-install-nginx-module

a) enter
b) opción 2 

**El path del source para compilar** ``/usr/local/src/nginx-1.5.2/``
**El path para configurar nginx** ``/usr/local/``

```
editar ``nginx.conf``

```
sudo vi /usr/local/conf/nginx.conf 
```

```
#Ejemplo de rutas
  http {
      ...
      passenger_root /Users/$USER/.rvm/gems/ruby-2.0.0-p247/gems/passenger-4.0.14;
      passenger_ruby /Users/$USER/.rvm/wrappers/ruby-2.0.0-p247/ruby;
      
      include sites-enabled/*.conf;
  }
```

Crear un bloque server para servir a nuestra aplicación Ruby on Rails

```
sudo vi /usr/local/conf/sites-enabled/demo_ruby_on_rails.conf
```

```
  server {
     listen 8080;
     server_name railsapp.local;
     root /Users/$USER/$RUTA_PROYECTO/railsapp/public;
     passenger_enabled on;
     rack_env development;
  }
```

