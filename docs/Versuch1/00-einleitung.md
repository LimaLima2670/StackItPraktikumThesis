# Einleitung

Video-on-Demand-Systeme (VoD) sind aus dem heutigen Medienalltag nicht mehr wegzudenken. Plattformen wie YouTube, Netflix oder die Mediatheken von ARD und ZDF zeigen, wie selbstverständlich audiovisuelle Inhalte jederzeit und auf unterschiedlichsten Endgeräten verfügbar sind. Hinter dieser scheinbaren Selbstverständlichkeit verbirgt sich jedoch ein komplexer technischer Workflow aus Speicherung, Verarbeitung und Verteilung von Medieninhalten.

Der Aufbau eines VoD-Systems in der Cloud ist in den letzten Jahren deutlich vereinfacht worden. Moderne Cloud-Plattformen stellen skalierbare Infrastrukturen bereit, mit denen große Mediendaten effizient ingestiert, gespeichert, transcodiert und ausgeliefert werden können. In diesem Praktikum wird dieser Workflow praxisnah nachvollzogen – jedoch bewusst nicht mit proprietären Komplettlösungen eines einzelnen Hyperscalers.

Stattdessen liegt der Fokus auf einem herstellerneutralen Ansatz unter Verwendung der europäischen Cloud-Plattform **STACKIT** in Kombination mit dem Content Delivery Network **Fastly**. Dadurch wird nicht nur ein realistischer und übertragbarer VoD-Workflow vermittelt, sondern auch ein Bewusstsein für Themen wie digitale Souveränität, Datenhoheit und den Einsatz europäischer Cloud-Infrastrukturen geschaffen.

## Einfaches Beispiel

Ein Video-on-Demand-Workflow lässt sich gut mit einem modernen Paketversand vergleichen. Zunächst wird ein hochwertiges Originalvideo in das System hochgeladen und sicher gespeichert – vergleichbar mit der Abgabe eines Pakets in einem zentralen Lager. Dieses Original dient als Ausgangspunkt für alle weiteren Verarbeitungsschritte.

Da unterschiedliche Endgeräte unterschiedliche Anforderungen haben, wird das Video anschließend in mehrere optimierte Versionen umgewandelt. Ähnlich wie ein Paket je nach Empfänger neu verpackt wird, entstehen so verschiedene Videoformate für Smartphones, Tablets oder große Bildschirme. Dadurch kann sichergestellt werden, dass Inhalte unabhängig von Gerät und Netzwerkbedingungen effizient und in gleichbleibender Qualität ausgeliefert werden.

Um eine schnelle und zuverlässige Bereitstellung für viele Nutzer gleichzeitig zu ermöglichen, werden diese Videoversionen schließlich über ein Content Delivery Network möglichst nah an die Nutzer verteilt. Dies reduziert Ladezeiten, entlastet zentrale Server und verbessert die Nutzererfahrung. Der Nutzer selbst erhält das Video über einen Browser oder Mediaplayer, ohne die zugrunde liegende Infrastruktur wahrzunehmen – für ihn zählt lediglich, dass die Wiedergabe schnell, stabil und ohne Unterbrechungen startet.

![STACKIT Beispiel Architektur](./assets/versuch1/stackit-beispiel.jpg)




## Fastly als Content-Delivery-Network

Fastly wird in diesem Versuch als Content Delivery Network (CDN) eingesetzt und übernimmt die performante Auslieferung der transcodierten Medieninhalte an die Endnutzer. Ein CDN besteht aus einer Vielzahl weltweit verteilter Server, die Inhalte möglichst nah am jeweiligen Nutzer bereitstellen. Dadurch werden Ladezeiten reduziert und eine gleichmäßige Auslieferung auch bei hoher Zugriffszahl ermöglicht.

Im vorliegenden Workflow dient der STACKIT Object Storage als sogenannte Origin-Quelle. Fastly greift bei der ersten Anfrage auf die dort abgelegten Medieninhalte zu und speichert diese temporär in seinem Cache. Nachfolgende Anfragen können direkt von einem nahegelegenen Edge-Server bedient werden, ohne dass erneut auf den Origin-Speicher zugegriffen werden muss.

Dieser Ansatz entlastet den Ursprungsspeicher, verbessert die Skalierbarkeit des Systems und sorgt für eine stabile Nutzererfahrung. Für den Anwender bleibt dieser Prozess transparent – aus seiner Sicht startet die Wiedergabe schnell und zuverlässig, unabhängig von Standort oder Anzahl gleichzeitiger Zugriffe.





## Workflow

Unabhängig vom eingesetzten Cloud-Anbieter folgt der Distributionsweg von Video-on-Demand-Systemen in der Regel einem ähnlichen Grundprinzip. Zunächst werden hochwertige Quelldateien in das System ingestiert und in einem nicht öffentlich zugänglichen Speicher abgelegt. Abhängig vom vorgesehenen Distributionsweg werden diese Quelldateien anschließend in verschiedene Zielformate transcodiert, um eine optimale Wiedergabe auf unterschiedlichen Endgeräten und Netzwerkbedingungen zu ermöglichen.

Die erzeugten Distributionsformate können optional mit weiteren Mechanismen wie Zugriffsbeschränkungen oder Kopierschutzmaßnahmen ergänzt werden. Nach Abschluss der Transcodierung werden die Medieninhalte über ein Content Delivery Network (CDN) verteilt, das eine performante und skalierbare Auslieferung an eine große Anzahl von Nutzern ermöglicht.

Der Zugriff auf die Inhalte erfolgt schließlich über eine Weboberfläche oder einen Mediaplayer, der die im CDN bereitgestellten Medien abruft. Im Rahmen dieses Versuchs wird dieser Workflow Schritt für Schritt nachgebildet und anhand konkreter Konfigurationen auf STACKIT und Fastly nachvollzogen.


In der Praxis wird dieser simple Ablauf noch mit verschiedenen Steuerungs- und Monitoriung-Werkzeugen ergänzt, um Arbeitsschritte zu automatisieren oder beispielsweise dem zuständigen Mitarbeiter Benachrichtigungen zu senden, sobald eine Datei transcodiert wurde.

## Cloud-Anbieter

Cloud-Anbieter stellen Rechen-, Speicher- und Netzwerkressourcen über das Internet bereit. Anstatt eigene Hardware zu betreiben, nutzt der Anwender diese Ressourcen bedarfsgerecht und zeitlich begrenzt. Dadurch lassen sich Systeme flexibel skalieren und nur für tatsächlich benötigte Zeiträume betreiben.

Die angebotenen Dienste lassen sich grob in drei Kategorien einteilen:

Infrastructure as a Service (IaaS)
IaaS stellt grundlegende IT-Infrastruktur wie virtuelle Server, Netzwerke oder Speicher bereit. Der Nutzer ist für das Betriebssystem sowie die Konfiguration der darauf laufenden Systeme selbst verantwortlich.

Platform as a Service (PaaS)
PaaS abstrahiert die Infrastruktur weiter und stellt Plattformdienste wie Objektspeicher oder Container-Orchestrierung (z. B. Kubernetes-Cluster) bereit. Der Betrieb der zugrunde liegenden Infrastruktur wird dabei vom Cloud-Anbieter übernommen.

Software as a Service (SaaS)
SaaS stellt vollständig betriebene Anwendungen bereit, beispielsweise Transcodierungs- oder Monitoring-Dienste. Der Nutzer interagiert ausschließlich mit der Anwendung, ohne sich um Infrastruktur oder Plattform kümmern zu müssen[^1].

Beispiele für Cloud-Anbieter sind unter anderem STACKIT, Microsoft Azure, AWS oder Linode.
Für die globale Auslieferung von Inhalten kommen spezialisierte Anbieter für Content-Delivery-Networks (CDN) wie Fastly zum Einsatz.

!!! info
Für die folgenden Versuche werden Dienste der STACKIT-Cloud für Speicherung, Verarbeitung und Monitoring sowie das Content-Delivery-Network (CDN) von Fastly zur performanten Auslieferung der Videoinhalte verwendet.

!!! danger "Kosten"
Die genutzten Cloud- und CDN-Dienste sind nutzungsabhängig kostenpflichtig. Insbesondere dauerhaft laufende Ressourcen oder hohe ausgehende Datenmengen können zu unerwarteten Kosten führen. Nach Abschluss eines Versuchs müssen alle nicht mehr benötigten Ressourcen überprüft und gegebenenfalls deaktiviert oder gelöscht werden.
### Weboberfläche

Cloud-Produkte lassen sich in der Regel über eine grafische Weboberfläche einrichten und verwalten. Diese ermöglicht eine übersichtliche Bedienung und einen schnellen Einstieg, ist jedoch nur eingeschränkt für automatisierte Abläufe geeignet. Über die Weboberfläche können mit Hilfe der persönlichen Zugangsdaten Cloud-Ressourcen mit wenigen Klicks erstellt, konfiguriert oder wieder entfernt werden.

Bei STACKIT erfolgt der Zugriff über das STACKIT Portal. Dieses dient als zentrale Verwaltungsoberfläche zur Konfiguration und Überwachung von Cloud-Ressourcen wie Object Storage, virtuellen Maschinen oder Kubernetes-Clustern. Das Portal bietet unter anderem Funktionen zur Ressourcenverwaltung, Zugriffssteuerung und Kostenübersicht.


![STACKIT Weboberfläche](./assets/versuch1/StackIT-GUI.jpg)


### Kommandozeile

Neben der grafischen Oberfläche können Cloud-Ressourcen auch mithilfe von Kommandozeilen-Anwendungen (CLI) sowie Skripten bestellt und bedient werden. Zwar ist dies nicht so übersichtlich für Anfänger, jedoch können fortgeschrittene Nutzer schneller und automatisierter Ressourcen buchen und Workflows erstellen.


[^1]: Der Begriff dient außerdem zur Beschreibung von Geschäftsmodellen, die Software per Abonnement zur Verfügung stellen. Diese Software muss nicht zwingend in der Cloud betrieben werden, hat aber oft Verknüpfungen zu beispielsweise Cloud-Speicher Diensten (z.B. Adobe Creative Cloud).
