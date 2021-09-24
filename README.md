# Reverse-tunnel-ssh
tunnel reverse with raspberry pi

# Raspberry pi 4
Activar SSH (colocar una contraseña bastante fuerte, estamos expuestos ante internet.)

- apt-get update
- apt-get upgrade
- apt-get install stunnel4 

#crear stunnel.conf (copiar sin comillas)


#output = /var/log/stunnel4/stunnel.log
#cert=/etc/stunnel/stunnel.pem
#key=/etc/stunnel/stunnel.pem
#pid=/var/run/stunnel4/stunnel.pid
#client=no
#sslVersion = all
#options = NO_SSLv2
#options = NO_SSLv3
#socket = l:TCP_NODELAY=1
#socket = r:TCP_NODELAY=1
#[ssh]
#accept = ip-raspberry:443
#connect = 127.0.0.1:22

