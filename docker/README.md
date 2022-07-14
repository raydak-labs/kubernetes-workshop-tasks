# Docker - Aufgaben

## Aufgabe 1

Starte einen Docker Container mit dem image `hello-world`

## Aufgabe 2

Starte einen Container mit dem Image `nginx` (tag: `alpine`) und expose den Container-Port `80` auf dem Host-Port `8080`.

Teste folgendes:
- Container muss laufen
- Öffne den Browser und prüfe, ob die Website erreichbar ist http://localhost:8080


## Aufgabe 3

Führe den Container mit dem Image `blackdark93/workshop3` und dem Tag `1.0` im interaktiven Terminal-Modus aus (Hinweis default shell = sh).
Löse die Aufgabe.

## Aufgabe 4

Baue einen eigenen Container (Basis `nginx:alpine`) und verändere die Standard HTML Seite mit dem Matrix Screen.
Hinweis: Lese die Dokumentation zum nginx Container

Test:
- Rufe die nginx website auf
- Anstelle des Standard Fensters, muss ein Matrix Bild zu sehen sein

## Aufgabe 5

Baue ein `ubuntu` Image, das als Startausführung dieses Script ausführt:

```bash
base64 -d <<<"H4sIAJM2MVYAA1NQgAEDIIhHBsgCBmgAU8TAQJsL2SgU41AFiDALYRhUF8I0NAEUCbBZUB7MBGRrUXXg8DC6CagORwkYtDCDcw3IMwDdOBL1IyRRwpBI7cihTlSYkRRNUHcRnUZgXIQGIlOoOQC/4ufk0gIAAA==" | gunzip
```

Test:
- Baue das Container Image
- führe es aus und sieh dir den Output an
