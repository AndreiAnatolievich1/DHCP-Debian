Настройка DHCP-servera


# apt install isc-dhcp-server -y   **устанавливаем серверное программное обеспечение, **
#nano /etc/network/interfaces      **переходим в конфигурационный файл интерфейсов**
в файле 
auto eth1                           **обозначаем интерфейс на котором будем server**
iface eth1 inet static              **указываем что адрес будем статическим**
	address 192.168.5.2         **адрес самого сервера**        
	netmask 255.255.255.0       **маска сети**
	gateway 192.168.5.1         **шлюз по умолчанию**
	dns-nameservers 8.8.8.8
#nano /etc/dhcp/dhcpd.conf          **переходим в конфигурационный файл dhcp**
# dhcpd.conf
#
**тут задаем pool адресов , обязательно должен быть pool  в котором состоит адрес сервер**
subnet 192.168.5.0 netmask 255.255.255.0 {
  range 192.168.5.3 192.168.5.30;
  option domain-name-servers ns1.internal.example.org;
  option domain-name "internal.example.org";
  option routers 192.168.5.1;
  default-lease-time 600;
  max-lease-time 7200;
}
 **добавляем необходимое колличество pool**
subnet 192.168.2.0 netmask 255.255.255.0 {       **сеть которую будем раздавать**
  range 192.168.2.3 192.168.2.30;                 **диапазон адресов из которыз будем раздавать**
  option domain-name-servers ns1.internal.example.org;
  option domain-name "internal.example.org";
  option routers 192.168.2.1;                   **шлюз по умолчанию для этой сети**
  default-lease-time 600;                        **время аренды адреса**
  max-lease-time 7200;                           **максимальное время аренды адреса**
}
 
subnet 192.168.3.0 netmask 255.255.255.0 {
  range 192.168.3.3 192.168.3.30;
  option domain-name-servers ns1.internal.example.org;
  option domain-name "internal.example.org";
  option routers 192.168.3.1;
  default-lease-time 600;
  max-lease-time 7200;
}

#nano /etc/default/isc-dhcp-servers     **тут необходимо указать интерфейс с которого сервер будет слушать DHCP запросы**
 в файле 
INTERFACESv4="eth1"
INTERFACESv6=""


#systemctl restart networking    **перезагружаем систему**
#systemctl start isc-dhcp-server  **запускаем сервер**
#systemctl enable isc-dhcp-server  **эта команда включает сервер при загрузке, без нее после перезагрузки сервер необходимо законо включать**
#systemctl status isc-dhcp-server  **статус сервера**
● isc-dhcp-server.service
     Loaded: not-found (Reason: Unit isc-dhcp-server.service not found.)
     Active: active (running) since Fri 2026-01-23 21:51:00 MSK; 14min ago
 Invocation: 7661811046b4437fa76df8ca3bd98e5c
        CPU: 67ms
     CGroup: /system.slice/isc-dhcp-server.service
             └─6037 /usr/sbin/dhcpd -4 -q -cf /etc/dhcp/dhcpd.conf eth1

янв 23 21:50:58 vbox systemd[1]: Starting isc-dhcp-server.service - LSB: DHCP server...
янв 23 21:50:58 vbox isc-dhcp-server[6024]: Launching IPv4 server only.
янв 23 21:50:58 vbox dhcpd[6037]: Wrote 0 leases to leases file.
янв 23 21:50:58 vbox dhcpd[6037]: Server starting service.
янв 23 21:51:00 vbox isc-dhcp-server[6024]: Starting ISC DHCPv4 server: dhcpd.
янв 23 21:51:00 vbox systemd[1]: Started isc-dhcp-server.service - LSB: DHCP server