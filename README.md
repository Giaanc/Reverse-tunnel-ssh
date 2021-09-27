# Reverse-tunnel-ssh
tunnel reverse with raspberry pi

# Raspberry pi 4
Activar SSH (colocar una contraseña bastante fuerte, estamos expuestos ante internet.)

- apt-get update
- apt-get upgrade
- apt-get install stunnel4 

# crear stunnel.conf 


###### output = /var/log/stunnel4/stunnel.log
###### cert=/etc/stunnel/stunnel.pem
###### key=/etc/stunnel/stunnel.pem
###### pid=/var/run/stunnel4/stunnel.pid
###### client=no
###### sslVersion = all
###### options = NO_SSLv2
###### options = NO_SSLv3
###### options = NO_TLSv1
###### options = NO_TLSv1.1
###### socket = l:TCP_NODELAY=1
###### socket = r:TCP_NODELAY=1
###### [ssh]
###### accept = ip-raspberry:443
###### connect = 127.0.0.1:22

# Abrir puertos Router (Foward Port) (si su proveedor utiliza cgnat, pida su ip publica)

Puertos 22 (es mejor cambiar el puerto por seguridad), 443

# crear private key

###### openssl genrsa -out key.pem 2048

# generar certificados auto firmados.

###### openssl req -new -x509 -key key.pem -out cert.pem -days 365

insertar datos requeridos (puede quedar todo en blanco) excepto el common name (todavia no estoy seguro si esto es reelevante).

# coloque ambos certificados al archivo que apuntaremos en nuestro archivo de configuración de stunnel

###### cat key.pem cert.pem >> /etc/stunnel/stunnel.pem

# comenzar stunnel

###### /etc/init.d/stunnel4 start
o
###### sudo su -> stunnel4
(no usar stunnel (sin el 4) he descubierto que solo genera una conneccion con udpgw segun netstat, stunnel4 genera mas conecciones, lo que es = a mejor rendimiento)

# abrir firewall para SSL

###### iptables -A INPUT -p tcp --dport 443 -j ACCEPT


# Parte 2, instalar BADVPN-UDPGW especifico para redirigir todo puerto udp via tcp (en caso de utilizar Transmision de video/Llamadas)

instalar cmake, apunten la ultima version al instalar (el tutorial apunta a una version mas vieja).

###### https://cmake.org/install/
###### https://trendoceans.com/how-to-install-cmake-on-debian-10-11/

BADVPN: https://github.com/ambrop72/badvpn

tutorial: https://youtu.be/-Ie9c0A6a0U?t=165

###### wget https://github.com/abrop72/badvpn/archive/master.zip

###### unzip master.zip

###### cd badvpn-master

###### mkdir build

###### cd build

###### cmake .. -DBUILD_NOTHING_BY_DEFAULT=1 -DBUILD_UDPGW=1

###### sudo make install

finalmente ejecutar (dirigirse al directorio donde este badvpn-udpgw)

###### badvpn-udpgw --listen-addr 127.0.0.1:7300 --max-clients 1000 --max-connections-for-client 999



