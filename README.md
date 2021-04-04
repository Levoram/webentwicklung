# Moderne Webentwicklung

## Beschreibung:

Dieses Repository dient der Ideenfindung für die Präsentation zur modernen Webentwicklung am Donnerstag den 08.04.2021.

Verschiedene Fähigkeiten, Programmiersprachen, Frameworks und Technologien werden derzeit in der Webentwicklung eingesetzt. Beginnen wollen wir jedoch am Anfang.

## Was ist eine Webseite

Eine Webseite ist in der Regel ein einfaches html Dokument. Bei html handelt es sich um eine Programmiersprache im herkömmlichen Sinne. Es ist im grunde lediglich eine Auszeichnungssprache für einfache Textdokumente. Das heißt man kann eine einfache html oder Webseite in jedem Texteditor schreiben und es ist hierzu keine Entwicklungsumgebung von Nöten.

Eine einfache html 5 Webseite ohne Inhalt

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="style.css" />
    <script>
      document.getElementById("demo").innerHTML = "Hello JavaScript!";
    </script>
  </head>
  <body>
  <div id="demo"></div>
  </body>
</html>
```

## Git initializieren

```
$git init
```

Author: Nico Küchler
