Autor: Erismar Oliveira<br>
Instagram ErismarDev: https://www.instagram.com/erismardev<br>
YouTube ErismarDev: https://www.youtube.com/erismardev<br>
LinkedIn Erismar Oliveira: https://www.linkedin.com/in/erismar-oliveira/<br>
Github Erismar Oliveira: https://github.com/erismaroliveira<br>
Data de criação: 13/08/2024<br>
Data de atualização: 13/08/2024<br>
Versão: 0.1<br>
Testado e homologado no GNU/Linux Ubuntu Server 22.04.x LTS

Release Ubuntu Server 22.04.4: https://fridge.ubuntu.com/2024/02/22/ubuntu-22-04-4-lts-released/<br>
Release Ubuntu Server 22.04.3: https://fridge.ubuntu.com/2023/08/11/ubuntu-22-04-3-lts-released/<br>
Release Ubuntu Server 22.04.2: https://fridge.ubuntu.com/2023/02/24/ubuntu-22-04-2-lts-released/<br>
Release Ubuntu Server 22.04.1: https://fridge.ubuntu.com/2022/08/12/ubuntu-22-04-1-lts-released/<br>
Release Ubuntu Server 22.04: https://fridge.ubuntu.com/2022/04/01/ubuntu-22-04-jammy-jellyfish-final-beta-released/

Release Notes Ubuntu Server 22.04.x: https://discourse.ubuntu.com/t/jammy-jellyfish-release-notes/24668<br>
Ubuntu Advantage for Infrastructure: https://ubuntu.com/advantage<br>
Ciclo de Lançamento do Ubuntu Server: https://ubuntu.com/about/release-cycle<br>
Releases All Ubuntu Server: https://wiki.ubuntu.com/Releases

#01\_ Desligando e reinicializando o servidor com halt no Ubuntu Server<br>

```bash
#opção do comando halt: -p (poweroff)
sudo halt -p
sudo halt --reboot
```

#02\_ Desligando e reinicializando o servidor com poweroff no Ubuntu Server<br>

```bash
#opção do comando poweroff: --reboot (reboot host)
sudo poweroff
sudo poweroff --reboot
```

#03\_ Desligando e reinicializando o servidor com init no Ubuntu Server<br>

```bash
#OBSERVAÇÃO: init é o primeiro processo iniciado durante a inicialização do sistema
#de computador. O init é um processo daemon que continua executando até o sistema
#ser desligado. o init trabalha no conceito de Runlevel (níveis de execução), no
#GNU/Linux temos basicamente 08 (oito) tipos de Runlevels: init 0 - Shutdown, init
#1 - Single user mode or emergency mode, init 2 - No network, init 3 - Network is
#present, init 4 It is similar to runlevel 3, init 5 - Network is present, init 6
#This runlevel is defined to system restart, init s - Tells the init command to
#enter the maintenance mode, init S - Same as init s, init m - Same as init s and
#init S e init M - Same as init s or init S or init m.

#opção do comando init: 0 (halt), 6 (reboot)
sudo init 0
sudo init 6
```

#04\_ Desligando e reinicializando o servidor com reboot no Ubuntu Server<br>

```bash
#opção do comando reboot: --halt (shutdown host)
sudo reboot --halt
sudo reboot
```

#05\_ Desligando e reinicializando o servidor com shutdown no Ubuntu Server<br>

```bash
#opção do comando shutdown: -P (poweroff), -h (halt 60 second), -r (reboot), -c (cancel)
#now (Shutdown immediately), 19:50 (Shutdown at 19:50 pm), +20 (Shutdown in 20 minutes)
sudo shutdown -P
sudo shutdown -h
sudo shutdown -r
sudo shutdown -h now
sudo shutdown -r now

#agendando o desligamento ou o reboot do servidor
sudo date
sudo shutdown -r 19:50 Servidor será reinicializado às 19:50hs
sudo shutdown -r +20 Servidor será reinicializado em 20 minutos
sudo shutdown -c
```
