# Einleitung

## Motivation

Soziale Netzwerke haben sich in den letzten Jahren zu einem alltäglichen Instrument der Kommunikation und Informationsbeschaffung entwickelt. Menschen können mithilfe solcher Netzwerke sehr einfach miteinander in Kontakt treten, sich vernetzen und organisieren. 
Auch das Nachrichtenwesen befindet sich zur Zeit im Wandel, denn Informationen verbreiten sich auf Twitter oder Facebook meist schneller, als über klassische Medien wie TV, Radio oder Print. Die oftmals als "viral" bezeichnete Ausbreitung von Beiträgen ist ein Phänomen das man sehr häufig in sozialen Netzwerken beobachten kann und von dem viele Personen, Organisationen und Unternehmen zu profitieren versuchen, besonders wenn es um die Reichweite von Informationen und Kommunikation geht.

Ein soziales Netzwerk, in welchem sich Menschen miteinander vernetzen können, ob als Extranet einer Organisation oder als öffentliches Netzwerk, wird heutzutage von einer Vielzahl von Faktoren bestimmt und beeinflusst.

Ein vor allem entscheidener Faktor in einem Netzwerk ist die Orientierung und Ausrichtung an einem bestimmten Thema oder Themenbereich. Und immer häufiger entstehen auf solche sehr beschränkte Themenbereiche spezialisierte Plattformen, die unter Umständen zwar weniger Personengruppen ansprechen können, aber in der grundlegenden Funktionsweise das Ziel dieses Netzwerks besser unterstützen. (Beispiele: dribbble.com, stackoverflow.com, github.com).

Auch die mobile Internetnutzung muss als weiterer wichtiger Faktor genannt werden, dem immer größere Bedeutung zugesprochen wird. Facebook liefert beispielsweise interessante Statistiken dazu. So haben ca. 73% der monatlich aktiven Nutzer, das Netzwerk auch von einem mobilen Endgerät genutzt. Zusätzlich nutzten ca. 21% der Nutzer das Netzwerk ausschliesslich über ein mobiles Gerät. Ein Grund für die ausschliessliche mobile Nutzung kann das Fehlen der entsprechenden Infrastruktur für einen festen Internetanschluss sein, insbesondere in Entwicklungsländern.

    Facebook Nutzerzahlen (http://allfacebook.de/wp-content/uploads/2013/11/FB_Q313InvestorDeck.pdf)
    Q3 monthly active Users 1189M
    Q3 monthly active mobile Users 874M (73%)
    Q3 monthly active mobile-only Users 254M (21%)
 
Zusätzlich sind Sicherheit, Datenschutz und Privatsphäre gerade zur heutigen Zeit, nicht zuletzt wegen der NSA-Affäre und dem Whistleblower Edward Snowden, immer wieder Bestandteil von Diskussionen wenn es um die Übertragung von Kommunikation und Informationen über das Internet geht und ebenfalls ein Faktor für moderne soziale Netze. OpenSource Software gewinnt so natürlich immer mehr an Bedeutung, da offener Quellcode von jedem Menschen eingesehen werden kann und die Wahrscheinlichkeit reduziert, dass eine Applikation seine Nutzer aktiv ausspioniert.

## Aufgabenstellung im Detail

Die rasant fortschreitende Entwicklung im Bereich der Webtechnologien, welche auch immer stärker für die Verwendung auf mobilen Endgeräten wie Smartphones oder Tablets ausgelegt sind, ermöglicht webbasierten Applikationen eine größere Leistungsfähigkeit. Nicht nur durch schnellere Hardware, sondern auch durch bessere Webbrowser und deren JavaScript Interpreter hat ein Webentwickler heutzutage größere Chancen auf verschiedensten Endgeräten, ob Mobil oder Desktop, die gleichen technischen Vorraussetzungen vorzufinden und so Applikationen mit einer einheitlichen Codebasis zu schreiben, die auf allen Geräten ohne größere Unterschiede funktionieren.

Diese Arbeit widmet sich dem Ziel, unter Beachtung der zuvor genannten Faktoren, eine experimentelle Basisplattform für sogenannte Social Extranets zu entwickeln. Es soll untersucht werden ob die erarbeitete Lösung für einen Einsatz unter realen Bedingungen geeignet ist und welche Anforderungen erfüllt werden müssen um einen sinnvollen Einsatz der Plattform, auch im Hinblick auf die Erweiterbarkeit und Weiterentwicklung durch andere Entwickler, zu gewährleisten.

Im Kern entstehen so zwei Anforderungen die im Laufe dieser Arbeit bewältigt werden sollen. Die Applikation soll zum einen mit mobilen Geräten benutzbar sein und zum anderen auch noch ohne aktive Internetverbindung (eingeschränkt) funktionieren.

Als wichtigste Komponente für dieses Vorhaben, steht die Datenbank im Mittelpunkt. Im Allgemeinen hat man heutzutage meist noch sehr begrenzte Möglichkeiten um in einem Webbrowser große Mengen an Daten ohne den Einsatz externer Datenbanksoftware direkt im Browser zu speichern. Um die Applikation grundsätzlich unabhängig von externen Einflüssen und darüber hinaus auch offline funktionsfähig zu machen, muss die Datenbank also innerhalb der Laufzeitumgebung (PhoneGap/Webview) bereitgestellt werden.

Die Offline Funktionalität stellt eine weitere Herausforderung da, denn es muss dafür Sorge getragen werden, dass Änderungen welche auf einem Gerät ohne Internetverbindung getätigt wurden, auch auf andere Geräte übertragen werden können, sobald dieses Gerät wieder eine Internetverbindung hat und im Umkehrschluss müssen Änderungen anderer Geräte auch empfangen werden. Das Netzwerk muss also in der Lage sein Datenbanken in alle Richtungen zu replizieren.

## Umfeld

Diese Arbeit entstand im Innovation Lab der Pixelpark AG am Standort Köln. Das Innovation Lab ist eine Forschungsabteilung die aktiv mit neuen und teilweise experimentellen Technologien verschiedenste Projekte umsetzt. Als Teilnehmer des "Future Internet" Programms im Bereich "FIcontent2" der Europäischen Union übernimmt Pixelpark die Verantwortung zur Erstellung eines sogenannten "Social Network Enablers".

Das Social Network Enabler Projekt ist im Bereich der Smart City Services angesiedelt und steht als offene Plattform für Entwickler zur Verfügung, um darauf aufbauend eigene Ideen umzusetzen.

Die in dieser Arbeit erreichten Ergebnisse werden als Kernkomponente in den Social Network Enabler eingebunden.

## Planung
**Wie wurde vorgegangen um das zuvor beschriebene Ziel zu erreichen?**

