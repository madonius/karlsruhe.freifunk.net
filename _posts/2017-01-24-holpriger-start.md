---
layout: post
title:  "Ein holpriger Start ins neue Jahr"
date:   2017-01-24 00:00:00
categories: community
---

Nach verschiedenen technischen Schwierigkeiten innerhalb der letzten Wochen, die uns leider auch sehr unglücklich gemacht haben, läuft Freifunk Karlsruhe nun wieder stabil. Wir möchten euch daher einen Einblick in einige der Probleme geben, die uns seit Weihnachten beschäftigt haben.

<!--*-->

#### Ausfall zweier Supernodes bei FFRL
Zwei unserer Supernodes werden uns freundlicherweise vom Freifunk Rheinland zur Verfügung gestellt. Aufgrund abgelaufener Zertifikate haben wir leider den Zugang zur Managementschnittstelle verloren. Dadurch ist es uns derzeit nicht möglich, die beiden vServer nach Ausfällen (an Weihnachten und Mitte Januar) wieder zu starten.

#### Ausfall zweier weiterer Supernodes
Wir haben zwei unserer Supernodes auf gemieteten vServern betrieben. Da beide Supernodes "überdurchschnittlich CPU- und I/O-Ressourcen in Anspruch [genommen]" haben (weiter so!), hat der Hoster zwischen Weihnachten und Neujahr beide vServer gesperrt. Leider traf uns dies mit nur zwei Stunden Vorwarnzeit während des [33. Chaos Communication Congress](https://events.ccc.de/congress/2016/wiki/Main_Page).
Wir haben sofort einen der beiden Server auf einen leistungsstärkeren dedizierten Server umgezogen und planen in Kürze mehrere weitere Supernodes auf dedizierter Hardware in Betrieb zu nehmen (Details folgen).

#### Umzug unserer Nameserver
Für unsere Domains betreiben wir mehrfach redundante Nameserver.
Regulär wird diese Aufgabe von unseren Supernodes übernommen.
Da aufgrund der ungünstigerweise zeitgleichen Ausfälle die Anzahl noch aktiver Supernodes nicht mehr ausreichend war, um die Erreichbarkeit der Website und der Supernodes zu gewährleisten, haben wir unsere Domains als vorübergehende Lösung auf freundlicherweise vom Entropia e.V. bereitgestellte Nameserver umgezogen.

#### VM-Host-Crashes aufgrund von Kernel-Bugs
Die Migration eines Supernodes auf eines unserer dedizierten VM-Hostsysteme hat leider reproduzierbar zu Kernel-Crashes des Hostsystems innerhalb von Minuten nach Start der VM auch unter geringer Last geführt. Wir konnten das Problem letzten Endes durch Upgrade auf einen neueren Kernel (4.8) als den von Debian stable paketierten (3.16) lösen.

#### Bug im Segmentation Offloading
Der Transport der Pakete zwischen unseren Supernodes und unserem Upstream-Provider, dem Freifunk Rheinland e.V., erfolgt über das Tunnelprotokoll GRE. Nach der Migration unserer Supernodes konnten über GRE transportierte TCP-Verbindungen nur noch etwa 500 kBit/s erreicht werden. Grund dafür war Reordering (umgekehrte Wagenreihung) und Duplikation von Paketen (das Gegenteil von Zugausfall, aber nicht besser). Dies konnte schließlich auf das [Segmentation Offloading](https://www.kernel.org/doc/Documentation/networking/segmentation-offloads.txt) Feature von Linux zurückgeführt werden. Nachdem GSO/TSO/GRO auf dem Hostsystem für das physische Netzwerkinterface deaktiviert wurden, konnten wieder Geschwindigkeiten im Bereich bis zu 1 GBit/s erreicht werden.

#### Load-Balancing Problem fastd
Zur Kommunikation zwischen Nodes und Supernodes wird die Tunnelsoftware "fastd" verwendet. fastd ist hauptsächlich CPU-bound. Leider ist fastd single threaded und kann daher anfallende Last nicht selbsttätig auf mehrere CPU-Cores verteilen.
Als mittelfristige Lösung verwenden wir daher Destination NAT, um eingehende Verbindungen anhand ihrer Source-IP auf mehrere fastd-Prozesse zu verteilen.

#### Firmware Update für neue Supernodes
Weitere Supernodes befinden sich bereits im Testbetrieb. Dank der Arbeit mit Ansible im letzten Jahr, konnten wir die Inbetriebnahme wesentlich beschleunigen. Sie werden in Kürze mit einem Firmware-Update für alle Nodes nutzbar gemacht.


#### Finanzierung
Leider erhöht die Migration der Supernodes von preiswerten vServern auf dedizierte Server unsere monatlichen Betriebskosten, die wir aus eigener Tasche bezahlen, erheblich.
Unter anderem um eine langfristig stabile Finanzierung des Projekts zu ermöglichen, sind wir derzeit dabei, einen Verein zu gründen. Details dazu folgen demnächst.
Bis dahin würden wir uns über finanzielle Unterstützung in Form von [Spenden an den Entropia e.V.](https://entropia.de/Bankverbindung) freuen.


Viele Grüße
Eure FFKA-Admins
