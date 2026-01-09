# Experiment: Time-to-First-Byte (TTFB) – Origin vs. CDN

**Experiment: Time-to-First-Byte (TTFB) – Origin vs. CDN**

In diesem Experiment soll untersucht werden, welchen Einfluss ein Content Delivery Network (CDN) auf die Antwortzeit beim Abruf einer Videodatei hat.

**Konkret wird die Time-to-First-Byte (TTFB) verglichen:**
- einmal beim direkten Abruf der Datei aus dem STACKIT Object Storage (Origin)
- einmal beim Abruf derselben Datei über das Fastly CDN

Die TTFB beschreibt die Zeit, die vergeht, bis das erste Datenbyte beim Client ankommt, nachdem die Anfrage abgeschickt wurde.
Sie ist ein wichtiger Indikator für die wahrgenommene Ladegeschwindigkeit von Medieninhalten.

## Schritt-für-Schritt-Anleitung

### Schritt 1: Voraussetzungen prüfen

- **Die Videodatei testvideo_1080p.mp4 liegt im STACKIT Object Storage**

- **Die Datei ist öffentlich über Fastly erreichbar, z. B.:**

```bash
https://<username>.global.ssl.fastly.net/testvideo_1080p.mp4
```
### Schritt 2: TTFB direkt vom Origin messen

Öffnen Sie ein Terminal (Linux/macOS) oder die PowerShell unter Windows
und führen Sie folgenden Befehl aus:

```bash
curl.exe -w "TTFB Origin: %{time_starttransfer}`n" -o NUL -s `
https://object.storage.eu01.onstackit.cloud/leonueberholz-4567/testvideo_1080p.mp4

```

**Dieser misst die Zeit des Origin downloads:**

**Notieren Sie sich bitte diese Zeit!**

**Nun öffnen Sie bitte eine weitere CMD und geben dort folgendes ein:**

```bash
curl.exe -w "TTFB CDN: %{time_starttransfer}`n" -o NUL -s `
https://leonueberholz.global.ssl.fastly.net/testvideo_1080p.mp4
```

<div style="
  border: 2px solid #ffffff;
  padding: 14px;
  border-radius: 6px;
  margin: 14px 0;
">
  <span style="color:cyan; font-weight:bold; font-size:1.2em;">
    Aufgabe:
  </span><br>
  <ul>
    <li>Messen Sie die Time-to-First-Byte (TTFB) für jede transcodierte Version
        (<code>1080p</code>, <code>720p</code>, <code>480p</code>) über das CDN.</li>
    <li>Führen Sie jede Messung mindestens drei Mal durch.</li>
    <li>Berechnen Sie für jede Auflösung den Median der gemessenen TTFB-Werte.</li>
    <li>Vergleichen Sie die Mediane der verschiedenen Auflösungen.</li>
  </ul>
</div>

**Das sollte dann so aussehen:**

![ObjectSTorage](ttfb.jpg)

## Lernergebnisse

In diesem Versuch wurde praxisnah nachvollzogen, wie die Auslieferung von Videoinhalten über ein Content Delivery Network (CDN) funktioniert und welchen Einfluss ein CDN auf Performance und Skalierbarkeit hat.

Zunächst wurde ein vollständiger Video-on-Demand-Workflow aufgebaut, bei dem transcodierte Videodateien im **STACKIT Object Storage** als Origin abgelegt und anschließend über das **Fastly CDN** öffentlich ausgeliefert wurden. Dabei wurde deutlich, dass Origin und CDN klar getrennte Rollen einnehmen: Der Object Storage dient als zuverlässiger, zentraler Speicher, während das CDN für die performante Verteilung an Endnutzer verantwortlich ist.

Ein zentrales Lernergebnis ist das Verständnis des **Caching-Prinzips**. Durch wiederholte Abrufe derselben Datei konnte beobachtet werden, dass Fastly Inhalte nach dem ersten Zugriff auf Edge-Servern zwischenspeichert. Dies ließ sich eindeutig anhand der HTTP-Header (`X-Cache: MISS` → `HIT`, steigender `Age`-Wert) nachvollziehen. Dadurch wurde klar, dass CDNs nicht nur Anfragen weiterleiten, sondern aktiv zur Reduzierung von Latenzen und Origin-Last beitragen.

Mit dem Experiment zur **Time-to-First-Byte (TTFB)** wurde der Performancegewinn messbar gemacht. Der Vergleich zwischen direktem Zugriff auf den Object Storage und dem Abruf über das CDN zeigte, dass das CDN in der Regel eine deutlich kürzere TTFB aufweist. Gleichzeitig wurde gelernt, warum mehrere Messungen notwendig sind und weshalb der Median besser geeignet ist als Einzelwerte, um realistische Aussagen über die Performance zu treffen.

Zusätzlich wurde der Umgang mit **HTTP-Headern und DNS-Auflösung** vertieft. Durch die Analyse von `curl`-Ausgaben und DNS-Abfragen wurde sichtbar, welcher Edge-Server die Inhalte ausliefert und wie CDNs je nach Standort unterschiedliche Server auswählen.

Insgesamt vermittelt der Versuch ein praxisnahes Verständnis dafür, warum CDNs ein unverzichtbarer Bestandteil moderner Medienverteilung sind. Die erlernten Konzepte – Object Storage als Origin, CDN-Caching, Edge-Auslieferung, Performance-Messung und DNS-Mechanismen – treten in der Medientechnik und in realen Streaming- und Video-on-Demand-Systemen regelmäßig auf und bilden eine wichtige Grundlage für weiterführende Experimente und Projekte.
