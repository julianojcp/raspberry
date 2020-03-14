# Raspberry

Instalação do Raspberry Pi 2 com Raspbian

## Raspberry PI2 V3 2019-atual

Foi necessário comprar um novo SdCard, desta vez 16gb, e instalar o Raspbian Buster.
Guias: https://www.raspberrypi.org/documentation/

## Criando o SDCard

Baixar de:

https://downloads.raspberrypi.org/raspbian_lite_latest.torrent
https://downloads.raspberrypi.org/raspbian_lite_latest

https://sourceforge.net/projects/win32diskimager/

Usando a versão Lite, baixei a imagem de 400mb, e apliquei a img no sdcard usando o Win32DiskImager.
No SDCard, criar o arquivo "SSH" sem extensão na raiz da partição Fat32 para iniciar em modo SSH

## Primeira inicialização

No primeiro boot, o raspi expande a imagem para o tamanho total do SDCard e faz várias pré configurações.
As vezes até realiza um check disk, então pode demorar alguns minutos pra iniciar.

1. Encontrar o IP do RPI via DHCP
2. Conectar via SSH 22 usando usuario pi e senha raspberry
3. Trocar a senha do usuario pi para SENHAPI
4. Ativador SSH via `sudo raspi-config` no menu Interfacing / SSH / Yes / Ok e senha SENHAPI / Finish
5. Alterar porta SSH de 22 para XXXXX / Reboot via `sudo nano /etc/ssh/sshd_config`

## Instalação padrão

```
sudo su
apt-get update && apt-get upgrade
apt-get install screen vim ntpdate wakeonlan nmap lynx
```
### DNSMasq

Instalando DSN Server pelo comando `sudo apt install dnsmasq`.
Restaurar arquivos: hosts e dnsmasq.d
Reiniciar `sudo service dnsmasq restart`

### NoIp 2

```
sudo wget http://www.no-ip.com/client/linux/noip-duc-linux.tar.gz
sudo tar xzvf noip-duc-linux.tar.gz
cd noip-2.1.9-1/
sudo make
sudo make install
```
E incluir no crontab:
```
crontab -e
0	*/6	*	*   *  root	/usr/local/bin/noip2
```

### Node-Red

Copiando aqui o processo do Get Started de nodered.org

Para instalar, usar o comando:
`bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)`

Depois, executar `node-red-start` para validar se está tudo ok

Para inserir credenciais de segurança, executar:

`nano /home/pi/.node-red/settings.js`

E inserir as linhas abaixo no local do exemplo
```
adminAuth: {
    type: "credentials",
    users: [{
        username: "admin",
        password: "$2a$08$o5rtBPstCMpw1sfzxyEdjdHco5rtBpxADP75krc3UxFpw1sfo5rtBoy",
        permissions: "*"
    }]
},
```

A password acima é gerada pelo comando:
```
cd /usr/lib/node_modules/node-red/
node -e "console.log(require('bcryptjs').hashSync(process.argv[1], 8));" suasenha
```

E voi lá!
Execute um `node-red-restart` para assumir a credencial de acesso.
