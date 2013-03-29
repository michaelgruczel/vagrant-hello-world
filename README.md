# Hello World mit Vagrant

## Einleitung

Dies ist ein sehr simples Beispiel wie man Vagrant 
in Kombination mit "Virtual Box" verwenden kann.
Um es sehr einfach zu sagen:
"Virtual Box" kann man lokal virtuelle machines lokal aufsetzen und managen.
Vagant bietet die Möglichkeit dies vollständig zu automatisieren.  
Diese Kombination ist sehr mächtig, denn sie bietet Entwicklern einige Vorteile:

* Die Ausführung von Installationen ist automatisierbar
* Die Ausführung von Installationen kann auf einem leeren Basissystem erfolgen
* Installationen können auf beliebigen Zielbetriebssystemen ausgeführt werden.
* Installationen können lokal identisch mit dem Endsystem erfolgen
* Installationen können unabhängig voneinander ausgeführt werden 
* Installationen können mit dem Code getestet und versioniert werden 


Oder anders formuliert, folgendes sollte nicht mehr passieren:
* Jeder Entwickler folgt einer Wiki um sich seine Software zu installieren
* Jede Installation ist irgendwie anders
* Installationen haben Nebenwirkungen auf andere Installation (z.B. Vers. Versionen der gleichen Software)
* Bugs weil die Software sich auf dem Ziel-OS anders verhält als lokal
* Installationsanweisungen welche nur bei manchen funktionieren
* ...


## Virtual Box

Oracle's Virtual Box ist für die meisten gängigen Betriebssysteme frei erhältlich
(https://www.virtualbox.org). Es bietet die Möglichkeit Betriebssysteme in einer 
lokalen Box zu installieren. Hierzu bietet es eine Grafische Oberfläche mit
der auch gemeinsame Ordner oder Port-Forwarding definiert werden können.
Zunächst muss also Virtual Box (mit Guest Additions) installiert werden 

## Vagrant

Vagrant ist für die meisten gängigen Betriebssysteme frei erhältlich 
(http://www.vagrantup.com). Vagrant macht das Händling von virtual machines
automatisierbar. Ich werde hier nicht auf die volle Leistungsfähigkeit von Vagant
eingehen. Ich werde nur erwähnen nur so viel wie für diese Beispiel nötig ist.
Erwähnt sei nur noch additiv, dass Vagrant versucht zunehmend mehr Cloud-Lösungen anzubinden. 

Wir starten mit unserem Projekt (Nachdem wir Vagant installiert haben) 
indem wir in einem beliebigen Ordner folgende aufrufen:

*"vagrant init"*

Nur sollte ein "Vagrantfile" erzeugt worden sein.
Dies sollte man sich einmal anschauen um ein besseres Verständnis zu gewinnen.

### base box, port forwarding, shared folder

In diesem Beispiel entferne ich den gesamten Inhalt des Vagrantfiles.
Wir brauchen zunächst lediglich ein Basisimage.
Hierzu empfehle ich eine Basebox aus http://www.vagrantbox.es zu wählen.
Diese sind in der Regel klein und haben bereits wichtige Elemente installiert.
Die wichtigen Einträge sind in deVagrantfile:

* config.vm.box = "<ein beliebiger Name für die Box>"
* config.vm.box_url = "<eine box>"

Zudem wollen wir einen Ordner zwischen der Box und unserem eigentlichen System sharen.
In diesem Fall soll der relativer Ordner "workspaces" identisch sein mit dem absoluten Ordner /vagrant_data auf der box.

* config.vm.synced_folder "./workspaces", "/vagrant_data"

Weil ich später auf services der box zugreifen möchte muss ich die Ports weiterleiten#

* config.vm.network :forwarded_port, guest: 8080, host: 4567
* config.vm.network :forwarded_port, guest: 80, host: 4568

Wir haben jetzt also eine vollständige Installation eines Betriebsystems inklusive
Port-Forwarding und einem "shared folder". Wenn wir wollen
können wir dem Vorgang in Virtual Box zuschauen (vb.gui-flag).
Der root user lautet in der Regel "vagrant" mit dem Passwort "vagrant",
ich verzichte allerdings daraus, weshalb folgenden Vagrantfile bleibt. 

```html

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu12041-CP-Vortrag"
  config.vm.box_url = "http://dl.dropbox.com/u/4031118/Vagrant/ubuntu-12.04.1-server-i686-virtual.box"
  config.vm.network :forwarded_port, guest: 8080, host: 4567
  config.vm.network :forwarded_port, guest: 80, host: 4568
  config.vm.synced_folder "./workspaces", "/vagrant_data"
  config.vm.provider :virtualbox do |vb|
    vb.gui = false
  end
end
```
Das ganze lässt sich nun mit "vagrant up" starten.
Mit "vagrant ssh" kann man sich gegen die box verbinden (bei windows zum Beispiel mit putty).

### Shell Provisioner

TODO

### Chef Provisioner

TODO

## Dieses Beispiel

TODO

### vagrant init

TODO

### java

TODO

### Ergebnis

TODO

