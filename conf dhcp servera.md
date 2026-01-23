Настройка DHCP-servera


`# apt install isc-dhcp-server -y`   **устанавливаем серверное программное обеспечение** <br>
<img width="710" height="203" alt="Screenshot_1" src="https://github.com/user-attachments/assets/4674e0ff-d49b-40e9-995f-3ad557a8e349" />

`#nano /etc/network/interfaces`      **переходим в конфигурационный файл интерфейсов** <br>
<img width="404" height="50" alt="Screenshot_2" src="https://github.com/user-attachments/assets/a6ad9e7a-1469-431b-9581-a76f1b9e642e" />

в файле <br>
`auto eth1`                           **обозначаем интерфейс на котором будем server**<br>
`iface eth1 inet static`              **указываем что адрес будем статическим**<br>
	`address 192.168.5.2`         **адрес самого сервера**        <br>
	`netmask 255.255.255.0`       **маска сети**<br>
	`gateway 192.168.5.1`         **шлюз по умолчанию**<br>
	`dns-nameservers 8.8.8.8`<br>
<img width="1272" height="351" alt="Screenshot_3" src="https://github.com/user-attachments/assets/6cd731e8-3562-4f7d-91e2-f51c53466a96" />

`#nano /etc/dhcp/dhcpd.conf`          **переходим в конфигурационный файл dhcp**<br>
<img width="751" height="782" alt="Screenshot_4" src="https://github.com/user-attachments/assets/476df455-56b9-4b29-a348-1f4d82a5242b" />

`# dhcpd.conf`<br>
**тут задаем `pool` адресов , обязательно должен быть `pool` c адресом сервера**<br>
`subnet 192.168.5.0 netmask 255.255.255.0 {`<br>
  `range 192.168.5.3 192.168.5.30;`<br>
  `option domain-name-servers ns1.internal.example.org;`<br>
  `option domain-name "internal.example.org";`<br>
  `option routers 192.168.5.1;`<br>
  `default-lease-time 600;`<br>
  `max-lease-time 7200;`<br>
`}`<br>
   **`добавляем необходимое колличество pool`**<br>
`subnet 192.168.2.0 netmask 255.255.255.0 {`       **`сеть которую будем раздавать`**<br>
  `range 192.168.2.3 192.168.2.30;`                 **`диапазон адресов из которых будем раздавать`**<br>
  `option domain-name-servers ns1.internal.example.org;`<br>
  `option domain-name "internal.example.org";`<br>
  `option routers 192.168.2.1;`                   **шлюз по умолчанию для этой сети**<br>
  `default-lease-time 600;`                        **время аренды адреса**<br>
  `max-lease-time 7200;`                           **максимальное время аренды адреса**<br>
`}`<br>
 
`subnet 192.168.3.0 netmask 255.255.255.0 {`<br>
  `range 192.168.3.3 192.168.3.30;`<br>
  `option domain-name-servers ns1.internal.example.org;`<br>
  `option domain-name "internal.example.org";`<br>
  `option routers 192.168.3.1;`<br>
  `default-lease-time 600;`<br>
  `max-lease-time 7200;`<br>
`}`<br>

`#nano /etc/default/isc-dhcp-servers`     **тут необходимо указать интерфейс с которого сервер будет слушать DHCP запросы**<br>
<img width="476" height="72" alt="Screenshot_6" src="https://github.com/user-attachments/assets/1cd72a17-bcdb-492e-a1f6-0c8a17016b7c" />

 в файле <br>
`INTERFACESv4="eth1"`<br>
`INTERFACESv6=""`<br>

`#systemctl restart networking`    **перезагружаем систему**<br>
`#systemctl start isc-dhcp-server`  **запускаем сервер**<br>
`#systemctl enable isc-dhcp-server`  **эта команда включает сервер при загрузке, без нее после перезагрузки сервер необходимо законо включать**<br>
`#systemctl status isc-dhcp-server`  **статус сервера**<br>
<img width="886" height="270" alt="Screenshot_8" src="https://github.com/user-attachments/assets/bd286085-c58e-4ceb-a292-7e5c83fe2d5e" />

`● isc-dhcp-server.service`<br>
     `Loaded: not-found (Reason: Unit isc-dhcp-server.service not found.)`<br>
     `Active: active (running) since Fri 2026-01-23 21:51:00 MSK; 14min ago`<br>
 `Invocation: 7661811046b4437fa76df8ca3bd98e5c`<br>
        `CPU: 67ms`<br>
     `CGroup: /system.slice/isc-dhcp-server.service`<br>
             `└─6037 /usr/sbin/dhcpd -4 -q -cf /etc/dhcp/dhcpd.conf eth1`<br>

`янв 23 21:50:58 vbox systemd[1]: Starting isc-dhcp-server.service - LSB: DHCP server...`<br>
`янв 23 21:50:58 vbox isc-dhcp-server[6024]: Launching IPv4 server only.`<br>
`янв 23 21:50:58 vbox dhcpd[6037]: Wrote 0 leases to leases file.`<br>
`янв 23 21:50:58 vbox dhcpd[6037]: Server starting service.`
`янв 23 21:51:00 vbox isc-dhcp-server[6024]: Starting ISC DHCPv4 server: dhcpd.`
`янв 23 21:51:00 vbox systemd[1]: Started isc-dhcp-server.service - LSB: DHCP server`
