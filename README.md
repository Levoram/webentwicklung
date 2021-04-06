# Moderne Webentwicklung mit React

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

## Source-Code-Verwaltung

Eigentlich jedes Projekt in der modernen Webentwicklung braucht eine Source-Code-Verwaltung. Wir werden uns [github.com](https://github.com/) und [git-scm.com](https://git-scm.com/) ein wenig ansehen.
Auf die Installation der Anwendungen werde ich an dieser Stelle nicht weiter eingehen.

Nachdem wir auf [git-scm.com](https://git-scm.com/) git for windows heruntergeladen und installiert haben, können wir ein Bash Terminal öffnen und verwenden. Dies geht zum einen direkt in Visual Studio Code (strg+ö) als integiertes Terminal, oder über den Fileexplorer in unserem Projektordner über rechte Maustaste GIT BASH here. Nachdem wir sichergestellt haben das wir uns im richtigen Ordner befinden können wir als erstes den folgenden Befehl eingeben.

```
$git init
```

Mit diesem Befehl haben wir ein lokales Repository auf unserem Rechner angelegt und können mit der Source-Code-Verwaltung beginnen.
Author: Nico Küchler
