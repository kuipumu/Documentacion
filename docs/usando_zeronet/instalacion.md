# Instalando ZeroNet

* Descarga el empaque ZeroBundle: [Microsoft Windows](https://github.com/HelloZeroNet/ZeroNet-win/archive/dist/ZeroNet-win.zip), [Apple macOS](https://github.com/HelloZeroNet/ZeroNet-mac/archive/dist/ZeroNet-mac.zip), [Linux 64bit](https://github.com/HelloZeroNet/ZeroBundle/raw/master/dist/ZeroBundle-linux64.tar.gz), [Linux 32bit](https://github.com/HelloZeroNet/ZeroBundle/raw/master/dist/ZeroBundle-linux32.tar.gz)
* Descomprimelo donde quieras
* Ejecuta `ZeroNet.exe` (win), `ZeroNet(.app)` (macOS), `ZeroNet.sh` (linux)

#### Instalación manual para Debian Linux

* `sudo apt-get update`
* `sudo apt-get install msgpack-python python-gevent`
* `wget https://github.com/HelloZeroNet/ZeroNet/archive/master.tar.gz`
* `tar xvpfz master.tar.gz`
* `cd ZeroNet-master`
* Inicia con `python zeronet.py`
* Abre http://127.0.0.1:43110/ en tu explorador

### [Vagrant](https://www.vagrantup.com/)

* `vagrant up`
* Accede al VM con `vagrant ssh`
* `cd /vagrant`
* Corre `python zeronet.py --ui_ip 0.0.0.0`
* Abre http://127.0.0.1:43110/ en tu explorador

### [Docker](https://www.docker.com/)
* `docker run -d -v <local_data_folder>:/root/data -p 15441:15441 -p 127.0.0.1:43110:43110 nofish/zeronet`
* Esta imagen de Docker incluye el proxy Tor, el cual es desabilitado por defecto. Esta alerta de que algunos provedores de alojamiento pueden no permitir que corras Tor en sus servidores. Si quieres habilitarlo, establece `ENABLE_TOR` la variable de a `true` (Por defecto: `false`). Ej.:

 `docker run -d -e "ENABLE_TOR=true" -v <local_data_folder>:/root/data -p 15441:15441 -p 127.0.0.1:43110:43110 nofish/zeronet`
* Abrir http://127.0.0.1:43110/ en tu explorador


### [Virtualenv](https://virtualenv.readthedocs.org/en/latest/)

* `virtualenv env`
* `source env/bin/activate`
* `pip install msgpack-python gevent`
* `python zeronet.py`
* Abrir http://127.0.0.1:43110/ en tu explorador


