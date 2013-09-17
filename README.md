# Instalación 

**Ruby + Rails + OpenSSL + Nginx**


***Nota: El ambiente no debe contener ninguna instalación de Ruby en caso de tenerla eliminar todo***

Ubicarnos en la ruta donde descargaremos todo el contenido en mi caso ``/usr/local/src``

```
cd /usr/local/src
```

**Instalar RVM (Ruby Vitual Machine)**
```
\curl -L https://get.rvm.io | bash -s stable
```

**Corrije soporte de OpenSSL**
```
rvm pkg install openssl
rvm reinstall 2.0.0 --with-openssl-dir=$rvm_path/usr
```

**Instando rails vía gems**
```
gem update
gem install rails
```

**Instalando Passenger**
```
gem install passenger
```

**Instalando paquetería faltante para correr el servidor de prueba**

```
bundle install
```

**Compilando Nginx con soporte de passenger**

```
curl -O "http://nginx.org/download/nginx-1.5.2.tar.gz"
tar -xvzf nginx-1.5.2.tar.gz

rvmsudo passenger-install-nginx-module

a) enter
b) opción 2 

**El path del source para compilar** ``/usr/local/src/nginx-1.5.2/``
**El path para configurar nginx** ``/usr/local/``

```
**editar** ``nginx.conf``

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

**Crear un bloque server para servir a nuestra aplicación Ruby on Rails**

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

