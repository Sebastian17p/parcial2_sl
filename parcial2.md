# parcial2
# Juan Sebastián Valencia Charry		2205254
# Andrés David Luna Peña 						2184984_sl
#PUNTO 1
Se abre el vagrant file
Se abre el panel de wifi, se entra en propiedades y se copia la ip de IPV4
En el vagrantfile de agregan las siguientes dos lineas:
servidor.vm.network :public_network, bridge: "ethp", ip: "11.11.8.14"
servidor.vm.network :public_network, bridge: "ethp", :dhcp => true
Se inician las maquinas en el cmd
(parte del servidor)
cd /etc/ufw
Se abre vim before.rules
Se edita las siguientes lineas: 
# NAT table rules
*nat
:PREROUTING ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
# Forward traffic from port 80 to port 80.
-A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.50.2:80  (puerto 80 para las paginas)
# Forward traffic from port 21 to port 21. (puerto 21 para hacer la comunicacion es decir subir archivos)
-A PREROUTING -i eth3 -p tcp --dport 21 -j DNAT --to-destination 192.168.50.2:21
-A PREROUTING -i eth3 -p tcp --dport 40000:50000 -j DNAT --to-destination 192.168.50.2 (puerto 40000 y 50000 para comunicar la ip publica con el servidor y cliente)
# Don't masquerade local traffic.
-A POSTROUTING -s 192.168.50.0/24 -j MASQUERADE
-A POSTROUTING -s 11.11.18.0/24 -j MASQUERADE (indica que se modifica con la mascara desde 1 a 255)
# Abrir los puertos 20,21 y 40000 a 50000
-A INPUT -p tcp --dport 20 -j ACCEPT
-A INPUT -p tcp --dport 21 -j ACCEPT
-A INPUT -p tcp --dport 40000:50000 -j ACCEPT
Se da ufw status (para ver los permisos otorgados)
systemctl restart ufw
(parte del cliente)
sudo apt update 
sudo apt install vsftpd
service vsftpd status (para ver que este activo)
ufw allow openssh (para la certificación)
ls /etc/ssl/private  (para ver si el vsftpd.pem esta)
vim /etc/vsftpd.com (mirar lo que hay adentro)
crear un usuario con: sudo adduser ftpuser
sudo vim /etc/ssh/sshd_confid
agregamos Denyusersftpuserjuan (para denegar el cliente)
ssh ftpuser@11.11.8.143 (prueba para ver que deniega)
#Punto 2
vim /etc/systemd/resolved.conf
vim /etc/networkmanager/conf.d/10-dns.systemctl-resolved (para que no se vayan a cambiar las ip de los dns colocados anteriormente)
systemctl enable systemd - resolved 
systemctl status systemd - resolved
wyresharck
meterse a wiffi
utilizar tcp.port==853
Desde el servidor dar nslookup youtube.com
visualizar wyresharck

