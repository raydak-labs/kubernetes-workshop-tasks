# Kubernetes - Aufgaben

## Aufgabe 1

Starte ein Kubernetes Cluster mit Docker Desktop.

Teste
- Verbindung zum Kubernetes Cluster ist erfolgreich
- `default` namespace enthält keine pods
- es ist 1 Node verfügbar
- Zeige die Kubernetes Version des Servers und Clients an

## Aufgabe 2

Deploye einen `nginx` pod und gebe die Logs aus

Teste folgendes:
- Der pod ist verfügbar
- Der state des pods ist `Running`

## Aufgabe 3

Erstelle ein komplettes Deployment mit Service, um auf die Anwendung zuzugreifen.

Folgende Konfiguration soll verwendet werden:
- Image `nginx`
- NodePort `8080`
- Anzahl Replicas `2`
- namespace `task3`

Teste:
- 2 Pods sind aktiv und im Running state
- Service ist verfügbar
- Zugriff über http://localhost:8080 funktioniert

## Aufgabe 4

Versuche folgenden Pod zu deployen

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: default
  labels:
    app: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: ngiinx:alpine
          ports:
            - containerPort: 80
```

## Aufgabe 5

Versuche folgenden Pod zu deployen:


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: default
  labels:
    app: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:alpine
          ports:
            - containerPort: 80
          env:
            - name: SPECIAL_ENV
              valueFrom:
                configMapKeyRef:
                  name: special-config
                  key: game.properties
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-config
  namespace: default
data:
  game.properties: |
    enemies=aliens
    lives=3
    enemies.cheat=true
    enemies.cheat.level=noGoodRotten
    secret.code.passphrase=UUDDLRLRBABAS
    secret.code.allowed=true
    secret.code.lives=30
  ui.properties: |
    color.good=purple
    color.bad=yellow
    allow.textmode=true
    how.nice.to.look=fairlyNice
```
