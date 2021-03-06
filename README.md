# Moderne Webentwicklung

## Beschreibung:

Dieses Repository dient der Ideenfindung für die Präsentation zur modernen Webentwicklung am Donnerstag den 08.04.2021.

Verschiedene Fähigkeiten, Programmiersprachen, Frameworks und Technologien werden derzeit in der Webentwicklung eingesetzt. Beginnen wollen wir jedoch am Anfang.

## Was ist eine Webseite

Eine Webseite ist in der Regel ein html Dokument welche durch einen Webserver ausgeliefert und in einem Browser angezeigt wird. Bei html handelt es sich nicht um eine Programmiersprache.Im Grunde ist HTML eine Auszeichnungssprache und kann in jedem Texteditor geschrieben werden. Anders ausgedrückt kann jeder ein HTML Dokument oder eine Webseite in jedem Texteditor der zur Verfügung steht schreiben. Hierzu ist keine hochspezialisierte Entwicklungsumgebung nötig.

Dies ist ein einfaches HTML 5 Dokument:

```
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="style.css" />
    <script>
      document.getElementById("demo").innerHTML = "Hallo JavaScript!";
    </script>
  </head>
  <body>
    <div id="demo"></div>
  </body>
</html>
```

(Beispiele/beispiel1.html)

Wird diese Datei mit einem beliebigen Browser aus dem lokalen Dateisystem geöffnet, kann Sie ohne sichtbare Probleme geöffnet werden und sollte ungefähr so angezeigt werden.

![Bild 1](/images/beispiel1.png)"beispiele/beispiel1.html"

Wer jedoch die Chrome Developer Tools mit F12 öffnet wird direkt von diesen Fehlermeldungen begrüßt.

![Bild 2](/images/beispiel2.png)"beispiele/beispiel2.html"

Woher diese beiden Fehler kommen ist leicht zu verstehen.  
Der erste Fehler zeigt an das die Datei style.css nicht gefunden werden kann.
Da wir diese Datei bisher noch nicht bereitgestellt haben kann sie natürlich auch nicht gefunden werden.
Der zweite Fehler ist etwas schwerer zu verstehen und auch immer mal wieder Gegenstand von Diskussionen unter Webentwicklern.

Wir haben das "</script/>" tag in den head Bereich des HTML Dokuments geschrieben und der JavaScript Code der darin verwendet wird ist auch korrekt... Warum bekommen wir also einen Fehler?

Antwort:  
Der Javascript Code wird direkt an Ort und Stelle ausgeführt noch bevor der Browser Zeit hatte das gesammte Dokument zu lesen und zu verarbeiten. Mit anderen Worten, wir versuchen im head auf ein Element zuzugreifen, dass erst später vom Browser gelesen und erzeugt wird. Daher sagt uns die Fehlermeldung: Cannot set property "innerHTML" of null, wobei null ein nicht vorhandenes Objekt im DOM (Document Objekt Model) des Browsers ist.

Es gibt zwei Möglichkeiten diesen zweiten Fehler zu beheben.

1. Man schreibt das script tag als letztes vor das schießende body tag

```
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="style.css" />

  </head>
  <body>
    <div id="demo"></div>
    <script>
      document.getElementById("demo").innerHTML = "Hallo JavaScript!";
    </script>
  </body>
</html>
```

(Beispiele/beispiel1.html - modifiziert)
Hierdurch stellen wir sicher das der Browser die erforderlichen Zeilen im Dokument bereits gelesen und das DIV mit der id=demo bereits erzeugt hat.

2. Oder wir stellen im JavaScript Code sicher das der Code erst ausgeführt wird nachdem der Browser den DOM erzeugt hat.

```
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="style.css" />
     <script>
     document.addEventListener("DOMContentLoaded", function(event) {
        document.getElementById("demo").innerHTML = "Hallo JavaScript!";
      });
    </script>
  </head>
  <body>
    <div id="demo"></div>
  </body>
</html>
```

(Beispiele/beispiel2.html)

Wird nun im gleichen Ordner in dem sich die beispiel2.hmtl Datei befindt noch eine leere style.css Datei erzeugt, so sind unsere Fehlermeldungen behoben und die Seite wird nun wirklich fehlerfrei angezeigt wie man in der folgenden Abbildung sehen kann.

![Bild 3](/images/beispiel3.png)"Beispiele/beispiel3.html"

In Grunde haben wir jetzt schon die meisten Themen angerissen und alle Typen von Dateien angelegt die benötigt werden, um eine kleine Webseite zu erstellen und sie im Browser anzeigen zu können. Vom Thema dieses Tutorials sind wir allerdings noch sehr weit entfernt. Diese kleinen Beispiele sollten lediglich aufzeigen, wie einfach es im Grunde sein kann eine kleine Webseite oder auch eine kleine Anwendung zu schreiben, ohne sich mit Source-Code-Verwaltung, Javascript Frameworks, Bundlern, CI & CD Pipelines, Codeanalyse, Testframeworks, Integrierten Entwicklungsumgebungen und anderen Werkzeugen herumschlagen zu müssen.  
Die gute Nachricht ist allerdings, dass am Ende aller Schritte die im folgenden besprochen werden, ähnliche Dateien in einem Ordner der sich meistens dist nennt auf uns warten werden, um auf einen Webserver kopiert zu werden, damit sich die Welt an unseren tollen Anwendungen erfreuen kann.

Info:
Alle Dateien, Codebeispiele, IDE Abbildungen wurden mit Visual Studio Code Version: 1.55.0 unter Windows 10 Pro erstellt.
Für alle Abbildungen und Beispiele wurde als Webbrowser ein aktueller Chrome Browser der Version 89.0.4389.114 (Offizieller Build) (64-Bit) verwendet.

# Source-Code-Verwaltung

Eigentlich jedes Projekt in der modernen Webentwicklung braucht eine Source-Code-Verwaltung. Wir werden uns [github.com](https://github.com/) und [git-scm.com](https://git-scm.com/) ein wenig ansehen.
Auf die Installation der Anwendungen werde ich an dieser Stelle nicht weiter eingehen.

Nachdem wir auf [git-scm.com](https://git-scm.com/) git for windows heruntergeladen und installiert haben, können wir ein Bash Terminal öffnen und verwenden. Dies geht zum einen direkt in Visual Studio Code (strg+ö) als integiertes Terminal, oder über den Fileexplorer in unserem Projektordner über rechte Maustaste GIT BASH here. Nachdem wir sichergestellt haben das wir uns im richtigen Ordner befinden können wir als erstes den folgenden Befehl eingeben.

```
$git init
```

Mit diesem Befehl wird ein lokales Repository (versteckter Ordner .git) auf unserem Rechner im Projektordner angelegt und wir können mit der Source-Code-Verwaltung beginnen. Alle zukünftigen Änderungen werden zuerst hier abgelegt und gesichert.  
Hier ein Cheet Sheet der gebräuchlichsten Git Befehle für die tägliche Arbeit:

<div style="border-left:solid teal 10px; box-sizing: border-box;color: black; font-size: 16px; margin-left:15px;
padding:15px; background-color:white; box-shadow: 10px 5px 5px teal;">
<h2 style="font-weight:bold; width: 100%;background-color:teal; ">Git Cheet Sheet:</h2>
Git installieren
GitHub bietet Desktop-Clients an, die eine grafische Benutzeroberfläche für die häufigsten Aktionen auf Repositories beinhalten, sowie eine automatisch aktualisierte Kommandozeilen-Version von Git für erweiterte Szenarien.

GitHub für Windows
[windows.github.com](https://windows.github.com/)

GitHub für Mac
[mac.github.com](https://mac.github.com)

Git-Distributionen für Linux- und POSIX-Systeme sind auf der offiziellen Git SCM-Webseite verfügbar.

Git für alle Plattformen
[git-scm.com](https://git-scm.com/)

Werkzeuge konfigurieren
Konfigurieren von Benutzerinformationen für alle lokalen Repositories

`$ git config --global user.name "[name]"`

Setzt den Benutzernamen, den du an deine Commit-Transaktionen hängen willst

`$ git config --global user.email "[email address]"`

Setzt die Emailadresse, die du an deine Commit-Transaktionen hängen willst

Repositories anlegen
Ein neues Repository anlegen, oder eines von einer bestehenden URL herunterladen

`$ git init [project-name]`

Legt ein neues lokales Repository mit dem angegebenen Namen an

`$ git clone [url]`

Klont ein Projekt und lädt seine gesamte Versionshistorie herunter

Änderungen vornehmen
Änderungen überprüfen und eine Commit-Transaktion anfertigen

`$ git status`

Listet alle zum Commit bereiten neuen oder geänderten Dateien auf

`$ git diff`

Zeigt noch nicht indizierte Dateiänderungen an

`$ git add [file]`

Indiziert den derzeitigen Stand der Datei für die Versionierung

`$ git diff --staged`

Zeigt die Unterschiede zwischen dem Index (“staging area”) und der aktuellen Dateiversion

`$ git reset [file]`

Nimmt die Datei vom Index, erhält jedoch ihren Inhalt

`$ git commit -m"[descriptive message]"`

Nimmt alle derzeit indizierten Dateien permanent in die Versionshistorie auf

Änderungen gruppieren
Benennen von Commit-Serien und Zusammenfassen erledigter Tasks

`$ git branch`

Listet alle lokalen Branches im aktuellen Repository auf

`$ git branch [branch-name]`

Erzeugt einen neuen Branch

`$ git checkout [branch-name]`

Wechselt auf den angegebenen Branch und aktualisiert das Arbeitsverzeichnis

`$ git merge [branch-name]`

Fasst die Historie des angegeben Branches mit der des aktuell ausgecheckten Branches zusammen

`$ git branch -d [branch-name]`

Löscht den angegebenen Branch

Dateinamen refaktorisieren
Verschieben und Löschen versionierter Dateien

`$ git rm [file]`

Löscht die Datei aus dem Arbeitsverzeichnis und wird im Index zur Löschung markiert, dadurch wird die Datei beim nächsten commit aus der Versionskontrolle gelöscht

`$ git rm --cached [file]`

Entfernt die Datei aus der Versionskontrolle, behält sie jedoch lokal

`$ git mv [file-original] [file-renamed]`

Ändert den Namen der Datei und bereitet diese für den Commit vor

Tracking unterdrücken
Temporäre Dateien und Pfade ausschließen

_.log
build/
temp-_
Eine Textdatei namens <em>.gitignore</em> verhindert das versehentliche Committen von Dateien und Pfaden mit den spezifizierten Patterns

`$ git ls-files --others --ignored --exclude-standard`

Listet alle innerhalb dieses Projekts ignorierten Dateien auf

Fragmente speichern
Aufschieben und Wiederherstellen unvollständiger Änderungen

`$ git stash`

Speichert temporär alle getrackten Dateien mit Änderungen

`$ git stash pop`

Stellt die zuletzt zwischengespeicherten Dateien wieder her

`$ git stash list`

Listet alle zwischengespeicherten Änderungen auf

`$ git stash drop`

Verwirft die zuletzt zwischengespeicherten Änderungen

Historie überprüfen
Durchsuchen und Inspizieren der Entwicklung von Projektdateien

`$ git log`

Listet die Versionshistorie für den aktuellen Branch auf

`$ git log --follow [file]`

Listet die Versionshistorie für die aktuelle Datei auf, inklusive Umbenennungen

`$ git diff [first-branch]...[second-branch]`

Zeigt die inhaltlichen Unterschiede zwischen zwei Branches

`$ git show [commit]`

Gibt die Änderungen an Inhalt und Metadaten durch den angegebenen Commit aus

Commits wiederholen
Fehler beseitigen und die Historie bereinigen

`$ git reset [commit]`

Macht alle Commits nach [commit] rückgängig, erhält die Änderungen aber lokal

`$ git reset --hard [commit]`

Verwirft die Historie und Änderungen seit dem angegebenen Commit

Änderungen synchronisieren
Registrieren eines externen Repositories (URL) und Tauschen der Repository-Historie

`$ git fetch [remote]`

Lädt die gesamte Historie eines externen Repositories herunter

`$ git merge [remote]/[branch]`

Integriert den externen Branch in den aktuell lokal ausgecheckten Branch

`$ git push [remote] [branch]`

Pusht alle Commits auf dem lokalen Branch zu GitHub

`$ git pull`

Pullt die Historie vom externen Repository und integriert die Änderungen  
[Ouelle: training.github.com (Stand: 07.04.21)](https://training.github.com/downloads/de/github-git-cheat-sheet/)

</div>
</br>
Am Schluss noch ein Hinweis zu GIT im Allgemeinen. Es ist nicht immer ersichtlich woher git die aktuelle Konfiguration bezieht. Hierfür gibt es einen Befehl der im offiziellen Cheet Sheet nicht aufgeführt ist.

`$ git config --list --show-origin`

Dieser Befehl listet alle git bekannten Konfigurationen auf und zeigt aus welcher Datei die Konfigurationen jeweils bezogen werden.

Wichtig für jedes neue Projekt ist es eine .gitignore file im Rootverzeichnis des Repositories anzulegen.  
Diese Datei verhindert, dass nicht benötigte Dateien oder Ordner versehendlich in ein öffentliches Repository im Internet gelangen können.

Hier eine simple .gitignore file für den Anfang:

```
# See https://help.github.com/articles/ignoring-files/ for more about ignoring files.

# dependencies
/node_modules
/.pnp
.pnp.js

# testing
/coverage

# production
/build

# misc
.DS_Store
.env.local
.env.development.local
.env.test.local
.env.production.local

debug.log*
npm-debug.log*
yarn-debug.log*
yarn-error.log*
```

Eine package-lock.json oder eine yarn.lock datei gehören hier allerdings nicht auf die Liste. Diese Dateien stellen sicher das die korrekten Abhängikeiten bei einer Installation bezogen werden.

# Webbrowser, NODE.js

Jedes mir derzeit bekannte Javascript und Typescript Framework benötigt auf dem Entwicklungsrechner eine Installation von Node.js.

> "Node.js® ist eine JavaScript-Laufzeitumgebung, die auf Chromes V8 JavaScript-Engine basiert."
> [Quelle:Node.js® (Stand:07.04.21)](https://nodejs.org/de/)

Die V8 JavaScript-Engine wird derzeit in den zwei relevantesten Browsern Google Chrome und Microsoft Edge verwendet.
Im Grunde gibt es derzeit nicht mehr allzuviele Browser Engines auf dem Markt. Hier die wichtigsten Engines:

<table style= background-color:white;color:black>

<thead><tr>
<th class="headerSort" tabindex="0" role="columnheader button" title="Sort ascending">Engine
</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="Sort ascending">Status
</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="Sort ascending">Steward
</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="Sort ascending"><a href="/wiki/Software_license" title="Software license">License</a>
</th>
<th class="headerSort" tabindex="0" role="columnheader button" title="Sort ascending">Embedded in
</th></tr></thead><tbody>
<tr>
<th style="background: #ececec; color: black; font-weight: bold; vertical-align: middle; text-align: left;" class="table-rh"><a href="/wiki/WebKit" title="WebKit">WebKit</a>
</th>
<td bgcolor="#9F9">Active
</td>
<td><a href="/wiki/Apple_Inc." title="Apple Inc.">Apple</a>
</td>
<td style="background: #ddfd; color: black; vertical-align: middle; text-align: center;" class="free table-free"><a href="/wiki/GNU_Lesser_General_Public_License" title="GNU Lesser General Public License">GNU LGPL</a>, <a href="/wiki/BSD_licenses" title="BSD licenses">BSD-style</a>
</td>
<td><a href="/wiki/Safari_(web_browser)" title="Safari (web browser)">Safari</a> browser, <a href="/wiki/GNOME_Web" title="GNOME Web">Gnome Web</a>, plus all browsers hosted on the <a href="/wiki/IOS" title="IOS">iOS</a> <a href="/wiki/App_Store_(iOS)" class="mw-redirect" title="App Store (iOS)">App Store</a>
</td></tr>
<tr>
<th style="background: #ececec; color: black; font-weight: bold; vertical-align: middle; text-align: left;" class="table-rh"><a href="/wiki/Blink_(browser_engine)" title="Blink (browser engine)">Blink</a>
</th>
<td bgcolor="#9F9">Active
</td>
<td><a href="/wiki/Google" title="Google">Google</a>
</td>
<td style="background: #ddfd; color: black; vertical-align: middle; text-align: center;" class="free table-free"><a href="/wiki/GNU_Lesser_General_Public_License" title="GNU Lesser General Public License">GNU LGPL</a>, <a href="/wiki/BSD_licenses" title="BSD licenses">BSD-style</a>
</td>
<td><a href="/wiki/Google_Chrome" title="Google Chrome">Google Chrome</a> and all other <a href="/wiki/Chromium_(web_browser)" title="Chromium (web browser)">Chromium</a>-based browsers such as <a href="/wiki/Microsoft_Edge" title="Microsoft Edge">Microsoft Edge</a>, <a href="/wiki/Brave_(web_browser)" title="Brave (web browser)">Brave</a> and <a href="/wiki/Opera_(web_browser)" title="Opera (web browser)">Opera</a>
</td></tr>
<tr>
<th style="background: #ececec; color: black; font-weight: bold; vertical-align: middle; text-align: left;" class="table-rh"><a href="/wiki/Gecko_(software)" title="Gecko (software)">Gecko</a>
</th>
<td bgcolor="#9F9">Active
</td>
<td><a href="/wiki/Mozilla" title="Mozilla">Mozilla</a>
</td>
<td style="background: #ddfd; color: black; vertical-align: middle; text-align: center;" class="free table-free"><a href="/wiki/Mozilla_Public_License" title="Mozilla Public License">Mozilla Public</a>
</td>
<td><a href="/wiki/Firefox" title="Firefox">Firefox</a> browser and <a href="/wiki/Mozilla_Thunderbird" title="Mozilla Thunderbird">Thunderbird</a> email client, plus <a href="/wiki/Fork_(software_development)" title="Fork (software development)">forks</a> such as <a href="/wiki/SeaMonkey" title="SeaMonkey">SeaMonkey</a> and <a href="/wiki/Waterfox" title="Waterfox">Waterfox</a>
</td></tr>
<tr>
<th style="background: #ececec; color: black; font-weight: bold; vertical-align: middle; text-align: left;" class="table-rh"><a href="/wiki/Servo_(software)" title="Servo (software)">Servo</a>
</th>
<td bgcolor="#9F9">Active
</td>
<td><a href="/wiki/Linux_Foundation" title="Linux Foundation">Linux Foundation</a>
</td>
<td style="background: #ddfd; color: black; vertical-align: middle; text-align: center;" class="free table-free"><a href="/wiki/Mozilla_Public_License" title="Mozilla Public License">Mozilla Public</a>
</td>
<td>experimental browser
</td></tr>
<tr>
<th style="background: #ececec; color: black; font-weight: bold; vertical-align: middle; text-align: left;" class="table-rh"><a href="/wiki/Goanna_(software)" title="Goanna (software)">Goanna</a>
</th>
<td bgcolor="#9F9">Active
</td>
<td>M. C. Straver<sup id="cite_ref-4" class="reference"><a href="#cite_note-4">[4]</a></sup>
</td>
<td style="background: #ddfd; color: black; vertical-align: middle; text-align: center;" class="free table-free"><a href="/wiki/Mozilla_Public_License" title="Mozilla Public License">Mozilla Public</a>
</td>
<td><a href="/wiki/Pale_Moon_(web_browser)" title="Pale Moon (web browser)">Pale Moon</a> and <a href="/wiki/Basilisk_(web_browser)" title="Basilisk (web browser)">Basilisk</a> browsers
</td></tr>
<tr>
<th style="background: #ececec; color: black; font-weight: bold; vertical-align: middle; text-align: left;" class="table-rh"><a href="/wiki/NetSurf" title="NetSurf">NetSurf</a>
</th>
<td bgcolor="#9F9">Active
</td>
<td>hobbyists<sup id="cite_ref-5" class="reference"><a href="#cite_note-5">[5]</a></sup>
</td>
<td style="background: #ddfd; color: black; vertical-align: middle; text-align: center;" class="free table-free"><a href="/wiki/GNU_General_Public_License#Version_2" title="GNU General Public License">GNU GPLv2</a>
</td>
<td>NetSurf browser<sup id="cite_ref-6" class="reference"><a href="#cite_note-6">[6]</a></sup>
</td></tr>
<tr>
<th style="background: #ececec; color: black; font-weight: bold; vertical-align: middle; text-align: left;" class="table-rh"><a href="/wiki/KHTML" title="KHTML">KHTML</a>
</th>
<td bgcolor="#9F9">Active
</td>
<td><a href="/wiki/KDE" title="KDE">KDE</a>
</td>
<td style="background: #ddfd; color: black; vertical-align: middle; text-align: center;" class="free table-free"><a href="/wiki/GNU_Lesser_General_Public_License" title="GNU Lesser General Public License">GNU LGPL</a>
</td>
<td><a href="/wiki/Konqueror" title="Konqueror">Konqueror</a> browser
</td></tr>
<tr>
<th style="background: #ececec; color: black; font-weight: bold; vertical-align: middle; text-align: left;" class="table-rh"><a href="/wiki/EdgeHTML" title="EdgeHTML">EdgeHTML</a>
</th>
<td bgcolor="#FFB">Maintenance&nbsp;only
</td>
<td><a href="/wiki/Microsoft" title="Microsoft">Microsoft</a>
</td>
<td style="background: #ddf; vertical-align: middle; text-align: center;" class="table-proprietary"><a href="/wiki/Proprietary_software" title="Proprietary software">Proprietary</a>
</td>
<td><a href="/wiki/Universal_Windows_Platform" title="Universal Windows Platform">Universal Windows Platform</a> apps; formerly in the Edge browser<sup id="cite_ref-7" class="reference"><a href="#cite_note-7">[7]</a></sup>
</td></tr>
<tr>
<th style="background: #ececec; color: black; font-weight: bold; vertical-align: middle; text-align: left;" class="table-rh"><a href="/wiki/Trident_(software)" title="Trident (software)">Trident</a>
</th>
<td bgcolor="#F99">Discontinued
</td>
<td><a href="/wiki/Microsoft" title="Microsoft">Microsoft</a>
</td>
<td style="background: #ddf; vertical-align: middle; text-align: center;" class="table-proprietary"><a href="/wiki/Proprietary_software" title="Proprietary software">Proprietary</a>
</td>
<td><a href="/wiki/Internet_Explorer" title="Internet Explorer">Internet Explorer</a> browser and <a href="/wiki/Microsoft_Outlook" title="Microsoft Outlook">Microsoft Outlook</a> email client
</td></tr>
<tr>
<th style="background: #ececec; color: black; font-weight: bold; vertical-align: middle; text-align: left;" class="table-rh"><a href="/wiki/Presto_(browser_engine)" title="Presto (browser engine)">Presto</a>
</th>
<td bgcolor="#F99">Discontinued
</td>
<td><a href="/wiki/Opera_Software" class="mw-redirect" title="Opera Software">Opera Software</a>
</td>
<td style="background: #ddf; vertical-align: middle; text-align: center;" class="table-proprietary"><a href="/wiki/Proprietary_software" title="Proprietary software">Proprietary</a>
</td>
<td>formerly in the <a href="/wiki/Opera_(web_browser)" title="Opera (web browser)">Opera</a> browser
</td></tr>
</tbody><tfoot></tfoot>
</table>

[Quelle:wikipedia.org® (Stand:07.04.21)](https://en.wikipedia.org/wiki/Comparison_of_browser_engines)

Wie man in der Tabelle sehen kann sind im Grunde eigentlich nur noch 7 Engines für die Webentwicklung der nächsten Jahre wirklich relevant und von diesen 7 Engines sind maximal 4 für einen Privatnutzer ohne IT-Hintergrund relevant.  
Bei den drei meistverwendeten Engines WebKit, Blink und Geko handelt es sich um die sogenannten EVERGREEN Browser.
Mit anderen Worten wenn eine neue Technologie auf den Markt kommt wird sie von diesen Engines bereits unterstüzt.  
Das sind gute Neuigkeiten, denn so muss man sich in den nächsten Jahren nicht mehr mit der unterstützung veralteter Engines aufhalten.

Node.js bietet Javascript Entwicklern die Möglichkeit Browser unabhängige Anwendungen zu entwickeln, Prozesse zu automatisieren, Abhängigkeiten zu verwalten und Tests zu schreiben.

Hier ein Beispiel für einen einfachen http-server in Node.js ohne externe Bibliotheken.

```
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

(Beispiele/helloworldserver.js)

Als asynchrone Event getriebene JavaScript Laufzeitumgebung wurde Node.js entwickelt um scalierbare Netzwerkanwendungen zu schreiben. Der oben gezeigte Server ist in der Lage viele Verbindungen parallel zu versorgen. Jede neue Verbindung löst direkt eine Callback Funktion aus. Ist jedoch keine Arbeit zu verrichten legt sich der gesammte Prozess bis zur nächsten Anfrage schlafen und verbraucht nahezu keine Resourcen.  
Diese Eigenschaft wird auch bei den sogenannten LAMDA Funktionen in der serverlosen Cloudentwicklung immer häufiger eingesetzt.

Um den Server zu starten reicht es Node.js und git bash installiert zu haben.
Im geöffneten Terminalfenster einfach den folgenden Befehl eingeben und node startet unseren mini api server.

`node Beispiele/helloworldserver.js`

Mit dieser Methode lässt sich relativ schnell eine kleine lokale REST API für die Frontend-Entwicklung erstellen.  
Gerade für diesen Anwendungsfall gibt es im Node.js Ökosystem eine vielzahl an hilfreichen Bibliotheken von denen ich hier nur meine Favoriten nennen möchte.

- [express](http://expressjs.com/)
- [json-server](https://www.npmjs.com/package/json-server)

# Lokaler Webserver für die Entwicklung

Für die meisten Arbeitenen in der Webentwicklung wird eine lokalen Webserver benötigt. Einige Features von modernen Browsern erfordern es, dass ein HTML-Dokument oder auch andere Dateitypen von einem Webserver und nicht von der lokalen Festplatte eingelesen werden. Es gibt viele Projekte die sich mit diesem Problem befassen und eines der bekanntesten möchte ich hier kurz Vorstellen.

[http-server](https://www.npmjs.com/package/http-server) ist einer der leichtgewichtigsten Webserver und steht praktischerweise als NPM-Paket zur Verfügung. Dieser Server läst sich sowohl global, lokal wie auch On-Demand installieren und verwenden.

## Lokale Installation

Für eine lokale Installation wird im Projektordner eine Konsole geöffnet und die folgenden zwei Befehle nacheinander ausgeführt:

```
npm init -y
npm install http-server
```

Das erste Kommando initialisiert ein neues Projekt und legt eine package.json-Datei im aktuellen Verzeichnis an.  
Der zweite Befehl installiert das npm-Packet http-server im aktuellen Verzeichnis im Unterordner node_modules.
Sind beide Befehle erfolgreich, kann im Anschluss mit dem Befehl `node_modules/.bin/http-server .` einen Webserver gestartet werden, der den Inhalt des aktuellen Verzeichnisses auf allen verfügbaren Netzwerkschnittstellen bereitstellt.

## Sagen wir der Welt Hallo

Da wir nun wissen, wie wir einen Webserver lokal betreiben können, folgt nun ein kurzes Beispiel wie wir Ihn nutzen können, um eine kleine React Anwendung lokal zu entwickeln ohne React selbst installieren zu müssen.

Hierzu wechseln wir mit der IDE unserer Wahl in das Unterverzeichniss ./Beispiele/react-mit-cdn und betrachten dieses Verzeichnis für den Moment als unseren basis Projekt Ordner. Wir initialisieren ein neues npm Projekt und installieren den http-server lokal wie oben beschrieben.

Im nächsten Schritt erstellen wir im Ordner eine index.html Datei und füllen sie mit folgendem Inhalt:

```
<!DOCTYPE html>
<html lang="de">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hallo Welt</title>
    <script
      src="https://unpkg.com/react@16/umd/react.development.js"
      crossorigin
    ></script>
    <script
      src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"
      crossorigin
    ></script>
    <script
      src="https://unpkg.com/babel-standalone@6/babel.min.js"
      crossorigin
    ></script>
    <script src="index.js" type="text/babel"></script>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

(Beispiele/react-mit-cdn/index.html)

Diese Datei ist der spätere Einstiegspunkt unserer ersten Anwendung. Um eine Webanwendung mit React programieren zu können, benötigen wir mindestens die folgenden zwei Bibliotheken react und react-dom.  
Da wir React nicht lokal im Projekt installiert haben, müssen wir diese beiden Bibliotheken irgendwo anders her bekommen. Das tun wir über ein sogenanntes Content Delivery Networks kurz CDN. Die obersten Beiden script Tags binden in userem Beispiel die notwendigen Pakete über das bekannte CDN unpkg.com ein. Damit wir wie gewohnt jsx schreiben können laden wir mit dem dritten Script Tag nun ebenfalls [Babel](https://babeljs.io/) herunter. Nach diesem Setup könne wir nun unsere eigene index.js Datei einbinden, die unseren ersten React Anwendungscode enthält. Wichtig ist bei diesem letzten script tag die Angabe des `type="text/babel"`. Dieses Tag veranlasst Babel zur Laufzeit dazu unseren Source Code zu transpilieren ( übersetzen ), damit der Browser unser Programm überhaupt versteht.

```
const Hallo = ({ name }) => <h1>Hallo, {name}!</h1>;

ReactDOM.render(<Hallo name="Welt" />, document.getElementById("root"));
```

(Beispiele/react-mit-cdn/index.js)

Nachdem alles an seinem Platz ist müssen wir jetzt lediglich http-server dazu benutzen unser Programm zu hosten. Wir können dies mit dem bereits bekannten Befehl `node_modules/.bin/http-serer .` erreichen, oder wir modifizieren die neu erstellte package.json Datei indem wir in der scripts section ein neues `"start":"http.server .",` Script hinzufügen.

```
 {
  "dependencies": {
    "http-server": "^0.12.3"
  },
  "name": "react-mit-cdn",
  "version": "1.0.0",
  "main": "index.js",
  "devDependencies": {},
  "scripts": {
    "start": "http-server .",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "Nico Küchler",
  "license": "ISC",
  "description": "react with CDN Example"
}
```

Durch das hinzugefügte Script können wir jetzt ebenfalls den Befehl `npm start` im Terminal verwenden und unsere Anwendung wird gestartet. Im Terminal sollte die folgende Ausgabe erscheinen.

```
$ npm start

> react-mit-cdn@1.0.0 start
> http-server .

Starting up http-server, serving .
Available on:
  http://192.168.178.20:8080
  http://127.0.0.1:8080
  http://172.25.144.1:8080
Hit CTRL-C to stop the server
```

Wir können uns das Ergebnis unter einer der angegebenen Links ansehen oder den Server wie angegeben mit STRG-C stoppen.  
<br/>

# Die Welt der Frameworks und Bibliotheken

Bevor man mit der Entwicklung einer Webanwendung oder Webseite beginnt, muss man sich der Frage stelle, welche Bibliothek oder welches Framework man für seine Anwendung benutzen möchte. Hiervon hängt einiges Ab, da man mit dieser Entscheidung oft Jahrelang leben muss. Gerade in der Webentwicklung sind die möglichkeiten schier überwältigend. Gibt man einen Begriff wie bestes Javascript Framework 2021 in die Suchleiste ein, bekommt man Titel wie "10 Best JavaScript Frameworks to Use in 2021" oder ähnliche zu hauf. Das Internet erschlägt einen Quasi mit Antworten und nächste Woche ist die Antwort eine andere.  
Ich arbeite täglich mit unterschiedlichen Frameworks wie [Angular.js](https://angularjs.org/), [Vue.js](https://vuejs.org/), [React](https://reactjs.org/) oder auch ganz ohne Frameworks mit Vanilla Javascript. Doch auch [Javascript](https://www.w3schools.com/Js/js_versions.asp#:~:text=JavaScript%20Versions.%20JavaScript.%20Versions.%20JavaScript%20was%20invented%20by,abbreviated%20to%20ES1%2C%20ES2%2C%20ES3%2C%20ES5%2C%20and%20ES6.) ist nicht gleich Javascript.  
Es gibt ES1-ES6 sowie die neueren Versionen ECMAScript 2016 - 2018 und [TypeScript](https://www.typescriptlang.org/) verschiedene Arten von [JavaScript Modul Definitionen](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) umd, amd, commonjs. All dies ist ein Haufen zu lernen und zu berücksichtigen wenn man eine Anwendung schreiben will.
<br>  
Im Grunde gibt es bei der Entscheidung für das eine oder andere kein Richtig oder Falsch, sondern nur die folgenden Fragen.

- Wie lange gibt es das schon?
- Wie warscheinlich ist es das es weiter unterstützt wird?
- Bietet es Lösungen für meine Problemstellungen?
- Wo finde ich Informationen, wie es zu benutzen ist?

Ich persönlich bevorzuge React, da man mit React im Grunde standartkonformes Javascript oder Typescript schreiben kann, ohne sich mit zuviel Framework specifischem Code auseinandersetzen zu müssen.  
<br>
Wir betrachten kurz die Gemeinsamkeiten vom Angular (Framework / Erstveröffentlichung:2009), React( Bibliothek/Erstveröffentlichung:2011) und Vue (Framework / Erstveröffentlichung:2014) weil ich denke diese drei Frameworks sind die relevantesten:

- Eigenes Command-Line-Interface (CLI)
- Benutzen ein virtuelles DOM (React & Vue)
- Benutzen schnelle und zusammenstellbare view components
- Besitzen eine Kern Bibliothek (React & Vue)
- Bieten Möglichkeiten für SPA routing und globales Status Management ob integriert oder durch zusätzliche Bibliotheken
- Haben eigene Developer Tools für die Installation im Browser

Wer einen detailierteren Vergleich sucht der wird [hier](https://vuejs.org/v2/guide/comparison.html) fündig werden.  
Ein [3rd Party Benchmark](https://stefankrause.net/js-frameworks-benchmark8/table.html) zum Vergleich der Renderleistungen kann man [hier](https://stefankrause.net/js-frameworks-benchmark8/table.html) finden.  
<br/>

Die Konfiguration eines neues Projektes ist immer ein großer Aufwand, daher werden wir das ganze von hinten betrachten und uns im nächsten Schritt von Create React App eine neue Anwendung generieren lassen. Es ist einfacher und schneller die Funktion von Webpack, Babel, es-lint, jest und den anderen tollen Tools auf diesem Weg zu erklären.

# Create React App

Wir wechseln in den Ordner Beispiele/create-react-app-example
Hier erzeugen wir mit dem Befehl  
`npx create-react-app my-app --template typescript`  
eine neue React Anwendung.
Der Output sollte das folgende oder ein ähnliches Ergebnis liefern und mit  
"Happy hacking!" abschließen.

```
$ npx create-react-app my-app --template typescript
Need to install the following packages:
  create-react-app
Ok to proceed? (y) y

Creating a new React app in D:\modernwebdevelopment\Beispiele\create-react-app-example\my-app.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts with cra-template-typescript...

yarn add v1.22.5
[1/4] Resolving packages...
[2/4] Fetching packages...
info fsevents@1.2.13: The platform "win32" is incompatible with this module.
info "fsevents@1.2.13" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@2.3.2: The platform "win32" is incompatible with this module.
info "fsevents@2.3.2" is an optional dependency and failed compatibility check. Excluding it from installation.
[3/4] Linking dependencies...
warning "react-scripts > @typescript-eslint/eslint-plugin > tsutils@3.20.0" has unmet peer dependency "typescript@>=2.8.0 || >= 3.2.0-dev || >= 3.3.0-dev || >= 3.4.0-dev || >= 3.5.0-dev || >= 3.6.0-dev || >= 3.6.0-beta || >= 3.7.0-dev || >= 3.7.0-beta".
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 7 new dependencies.
info Direct dependencies
├─ cra-template-typescript@1.1.2
├─ react-dom@17.0.2
├─ react-scripts@4.0.3
└─ react@17.0.2
info All dependencies
├─ cra-template-typescript@1.1.2
├─ immer@8.0.1
├─ react-dev-utils@11.0.4
├─ react-dom@17.0.2
├─ react-scripts@4.0.3
├─ react@17.0.2
└─ scheduler@0.20.2
Done in 47.71s.

Installing template dependencies using yarnpkg...
yarn add v1.22.5
[1/4] Resolving packages...
[2/4] Fetching packages...
info fsevents@2.3.2: The platform "win32" is incompatible with this module.
info "fsevents@2.3.2" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@1.2.13: The platform "win32" is incompatible with this module.
info "fsevents@1.2.13" is an optional dependency and failed compatibility check. Excluding it from installation.
[3/4] Linking dependencies...
warning " > @testing-library/user-event@12.8.3" has unmet peer dependency "@testing-library/dom@>=7.21.4".
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 24 new dependencies.
info Direct dependencies
├─ @testing-library/jest-dom@5.11.10
├─ @testing-library/react@11.2.6
├─ @testing-library/user-event@12.8.3
├─ @types/jest@26.0.22
├─ @types/node@12.20.7
├─ @types/react-dom@17.0.3
├─ @types/react@17.0.3
├─ react-dom@17.0.2
├─ react@17.0.2
├─ typescript@4.2.3
└─ web-vitals@1.1.1
info All dependencies
├─ @testing-library/dom@7.30.3
├─ @testing-library/jest-dom@5.11.10
├─ @testing-library/react@11.2.6
├─ @testing-library/user-event@12.8.3
├─ @types/aria-query@4.2.1
├─ @types/jest@26.0.22
├─ @types/node@12.20.7
├─ @types/prop-types@15.7.3
├─ @types/react-dom@17.0.3
├─ @types/react@17.0.3
├─ @types/scheduler@0.16.1
├─ @types/testing-library__jest-dom@5.9.5
├─ css.escape@1.5.1
├─ css@3.0.0
├─ csstype@3.0.7
├─ dom-accessibility-api@0.5.4
├─ lz-string@1.4.4
├─ min-indent@1.0.1
├─ react-dom@17.0.2
├─ react@17.0.2
├─ redent@3.0.0
├─ strip-indent@3.0.0
├─ typescript@4.2.3
└─ web-vitals@1.1.1
Done in 15.56s.

We detected TypeScript in your project (src\App.test.tsx) and created a tsconfig.json file for you.

Your tsconfig.json has been populated with default values.

Removing template package using yarnpkg...

yarn remove v1.22.5
[1/2] Removing module cra-template-typescript...
[2/2] Regenerating lockfile and installing missing dependencies...
info fsevents@2.3.2: The platform "win32" is incompatible with this module.
info "fsevents@2.3.2" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@1.2.13: The platform "win32" is incompatible with this module.
info "fsevents@1.2.13" is an optional dependency and failed compatibility check. Excluding it from installation.
warning " > @testing-library/user-event@12.8.3" has unmet peer dependency "@testing-library/dom@>=7.21.4".
success Uninstalled packages.
Done in 13.34s.

Success! Created my-app at D:\modernwebdevelopment\Beispiele\create-react-app-example\my-app
Inside that directory, you can run several commands:

  yarn start
    Starts the development server.

  yarn build
    Bundles the app into static files for production.

  yarn test
    Starts the test runner.

  yarn eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd my-app
  yarn start

Happy hacking!
```

Im Anschluss können wir genau tun was uns gesagt wird und in den Ordner my-app wechseln und dort den Befehl yarn start ausführen. Das Ergebnis ist eine funktionierende React Anwendung die uns im Browser angezeigt wird.

Author: Nico Küchler
