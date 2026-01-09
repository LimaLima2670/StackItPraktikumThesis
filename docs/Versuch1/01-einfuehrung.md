# Einführung

In diesem Versuch werden zunächst die grundlegenden Begriffe und Konzepte eines cloudbasierten Transcodierungs-Workflows erläutert. Anschließend wird anhand einer Schritt-für-Schritt-Anleitung ein erster Transcodierauftrag erstellt und ausgeführt. Ziel ist es, nach Abschluss von Versuch 1 ein grundlegendes Verständnis für Cloud-Transcoder zu entwickeln und einen einfachen Video-on-Demand-Workflow in einer europäischen Cloud-Umgebung selbstständig umsetzen zu können.

Der Versuch basiert auf der Cloud-Plattform STACKIT als Infrastruktur-Anbieter sowie dem Content Delivery Network Fastly zur Auslieferung der Medieninhalte.

## Grundbegriffe

### Cloud-Speicher (*Object-Storage*)

![STACKIT Object Storage](../assets/Versuch1/object-storage.jpg)


Cloud-Speicher funktioniert im Grundprinzip ähnlich wie bekannte Dienste wie Dropbox oder Google Drive: Dateien werden zentral gespeichert und können bei Bedarf wieder abgerufen werden. In professionellen Cloud-Umgebungen kommt dafür jedoch meist Objektspeicher (Object Storage) zum Einsatz.

Im Gegensatz zu klassischen Ordnerstrukturen werden die Daten hier als einzelne Objekte gespeichert. Jedes Objekt liegt in einem sogenannten Bucket und besitzt eine eindeutige Kennung. Die interne Organisation übernimmt der Cloud-Anbieter, sodass sich Anwender nicht um Verzeichnisstrukturen kümmern müssen.

Für diesen Praktikumsversuch wird STACKIT Object Storage verwendet. Über die Weboberfläche des STACKIT Control Centers oder über standardisierte Schnittstellen (S3-kompatible API) können Dateien hochgeladen und verwaltet werden. In diesem Versuch dient der Objektspeicher als Ablageort für die hochgeladenen Videodateien sowie für die später erzeugten, transcodierten Versionen.

⚠️ Achtung – Kosten
Für gespeicherte Daten fallen laufende Kosten an, außerdem können Kosten für den Datenabruf entstehen. Bei den im Rahmen dieses Versuchs verwendeten kleinen Datenmengen sind diese Kosten sehr gering. Bei größeren Projekten mit vielen oder sehr großen Dateien können die Speicher- und Übertragungskosten jedoch deutlich steigen.
### Transcodierer (*AWS Elemental MediaConvert*)


STACKIT stellt keinen eigenen, spezialisierten Transcoding-Dienst bereit. Stattdessen erfolgt die Verarbeitung von Medieninhalten über virtuelle Maschinen, die mit dem Produkt STACKIT Compute Engine bereitgestellt werden. Eine virtuelle Maschine kann dabei wie ein normaler Server betrachtet werden, auf dem eigene Software ausgeführt wird.

In diesem Praktikumsversuch wird auf einer solchen virtuellen Maschine eine Transcoding-Software (z. B. FFmpeg) installiert. Die VM greift zunächst auf die im STACKIT Object Storage abgelegten Videodateien zu und lädt diese zur Verarbeitung herunter. Anschließend werden die Videodateien in verschiedene Zielformate und Auflösungen umgewandelt, um eine spätere Wiedergabe auf unterschiedlichen Endgeräten zu ermöglichen.

Nach Abschluss der Transcodierung werden die erzeugten Ausgabedateien wieder im Object Storage gespeichert. Von dort aus können sie in einem weiteren Schritt über ein Content Delivery Network (CDN) an die Endnutzer ausgeliefert werden. Die virtuelle Maschine übernimmt somit ausschließlich die Aufgabe der Medienverarbeitung und ist nicht direkt an der Auslieferung beteiligt.

Dieses Vorgehen entspricht einem typischen cloudbasierten Video-on-Demand-Workflow und verdeutlicht, wie Rechenressourcen, Speicher und Auslieferung in der Cloud getrennt voneinander eingesetzt werden.

![FFMPEG Encoding](ffmpeg-coding.png)

## AWS WebGUI

### Log-in

Für diesen Praktikumsversuch erhalten die Studierenden die benötigten Zugangsdaten (Credentials) direkt vom Dozenten. Eine eigene Registrierung ist nicht erforderlich.

Zur Anmeldung auf StackIT gelangt man über diesen Link: https://portal.stackit.cloud/


![Login User](login-user.jpg)

**Hier kann der bereitgestellte Username aus der E-Mail entnommen werden**

![Login password](login-pw.jpg)

**Hier kann das bereitgestellte Password aus der E-Mail entnommen werden**

![Login mfa](mfa-login.jpg)

**Hier besteht die Möglichkeit eine 2 Faktor Authentifizierung einzugeben. Dies kann wahlweise mit der Uni-Mail oder mit einer App erfolgen. Als App für das Handy empfiehlt sich hier Microsoft Authenticator https://support.microsoft.com/de-de/account-billing/microsoft-authenticator-herunterladen-351498fc-850a-45da-b7b6-27e523b8702a**

![Landigpage region](Landingpage-region.jpg)

**Auf der Landingpage sollte in der oberen Leiste das richtige Projekt und die region EU01 - (Deutschland Süd) ausgewählt werden**

![AWS WebGUI](../assets/versuch1/aws_dashboard.png)

