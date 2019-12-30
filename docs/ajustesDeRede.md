
### Configuração de IP Estatico

Para visualizar o nome dos adaptadores de rede use o comando `ifconfig`, geralmente eth0 para o adptador ethernet e wlan0 para o adpatador wi-fi

```sh
ifconfig

#### Saida do comando ifconfig: ####

eth0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether b8:27:eb:eb:57:2b  txqueuelen 1000  (Ethernet)
        RX packets 25  bytes 2357 (2.3 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 27  bytes 3488 (3.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 9  bytes 524 (524.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 9  bytes 524 (524.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.15.52  netmask 255.255.255.0  broadcast 192.168.15.255
        inet6 2804:7f2:2892:3141:1f68:438c:63e9:a181  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::ba6e:ce29:1b8:63df  prefixlen 64  scopeid 0x20<link>
        ether b8:27:eb:be:02:7e  txqueuelen 1000  (Ethernet)
        RX packets 181271  bytes 258188050 (246.2 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 84538  bytes 9167009 (8.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

Arquivo dhcpcd.conf para configurações de IP.

```sh
 sudo nano /etc/dhcpcd.conf 
```

Modelo para configuração de IP Estatico, adicionar ao final do arquivo.

```sh
#static IP configuration

interface eth0
static ip_address=192.168.1.15/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1

#### Minhas Configuracoes ####

# IP Estatico - Raspberry Guilherme
# Ethernet
interface eth0
static ip_address=10.0.0.52/24

# Wi-Fi
interface wlan0
static ip_address=192.168.15.52/24
static routers=192.168.15.1
static domain_name_servers=8.8.8.8
```

Após salvar o arquivo é necessario reiniciar a raspberry com `sudo reboot` para as alterações surtirem efeito.

### Conectando WiFi via linha de comando

Arquivo wpa_supplicant.conf para configurações de redes Wireless

```sh
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf 
```

Conteudo do arquivo para conectar automaticamente em uma rede WiFi.

```sh
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=BR

network={
        ssid="VIVO-0FE0"
        psk="C662220FE0"
}
```

Após salvar as alterações, basta executar o comando abaixo para o adaptador conectar a rede.

```sh
wpa_cli -i wlan0 reconfigure
```


#### Referencias:
[[Solved] Raspbian Stretch: Setting a static IP - Raspberry Pi Forums](https://www.raspberrypi.org/forums/viewtopic.php?t=191140)

[Setting WiFi up via the command line - Raspberry Pi Documentation](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)
