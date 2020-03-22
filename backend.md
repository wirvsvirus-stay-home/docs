# Backend: Lorem Ipsum

_Version 0.9.0_

## Changelog

### 0.9.0

* `GET /users/:id/history` hinzugefügt 

### 0.8.0

* `POST /actions/check-in` gibt bei verfrühten Check-In nun Anzahl der Sekunden zum nächsten möglichen Check-In zurück

### 0.7.0

* `POST /actions/attack` hinzugefügt
* `POST /actions/check-in` hinzugefügt
* `GET /users?unprotected=1` entfernt
* `tickets` zu User-Model hinzugefügt

### 0.6.1

* `baseStatus` von Benutzter um möglichen Status `ABANDONED` erweitert

### 0.6.0

* Beschreibung von `GET /leaders` aktualisert
* Invalides JSON in Beispielen korrigiert

### 0.5.0

* Beispiel-Request von `PUT /users/:id` aktualisiert
* Beispiel-Response von `PUT /users/:id` aktualisiert

### 0.4.0

* Beispiel-Response von `GET /users/:id` aktualisiert

### 0.3.0

* `deviceId` aus Benutzer entfernt
* `id` von Benutzer ist nun UUIDv4

### 0.2.0

* Beispiel-Request von `POST /users` aktualisert
* Beispiel-Response von `POST /users` aktualisert

### 0.1.0

* Username wird nun vom Backend bei `POST /users` automatisch generiert un zurückgegeben

## Einleitung

Für den Prototypen beschränken wir uns auf folgende zu erfassende Benutzerdaten

* Benutzername (`username`)
* Land (`country`)
* Score (`score`)
* Rank (`rank`)
* Status der Basis (`baseStatus`) (`PROTECTED`, `ABANDONED` / `UNPROTECTED`)
* Geofancing (`latitude`, `longitude`, `radius`)

Zudem hält das Backend auch alle Daten für die Internationialiserung/I18N bereit.

Eine Authentifizierung der Benutzer und deren Request/Aktionen erfolgt für den Prototypen nicht.

## API

### `POST /actions/attack`

Führt eine Attacke aus. 

#### Beispiel

##### Request

Header muss `X-User-Id` mit der Benutzer-ID muss vorhanden sein.

```sh
curl -X "POST" "http://localhost:3000/actions/attack" \
     -H 'X-User-Id: 3f6ffbc8-2648-4b93-a132-667ef48a1138' \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{}'
```

##### Response

```json
{
  "status": 201,
  "message": "CREATED",
  "user": {
    "username": "Yellow Cat"
  },
  "action": {
    "id": "620cc1ee-b79d-4a5d-8853-5a787190ab3b",
    "type": "ATTACK",
    "amount": 100,
    "userId": "3f6ffbc8-2648-4b93-a132-667ef48a1138",
    "updatedAt": "2020-03-22T15:20:45.899Z",
    "createdAt": "2020-03-22T15:20:45.899Z"
  }
}

```

Ist kein Benutzer mit einer `UNPROTECTED` Base vorhanden, dann ist `user=null` und `action.amount` verdoppelt.

```json
{
  "status": 201,
  "message": "CREATED",
  "user": null,
  "action": {
    "id": "629f898c-e0b3-4646-9f3f-5337f47cd2fa",
    "type": "ATTACK",
    "amount": 200,
    "userId": "3f6ffbc8-2648-4b93-a132-667ef48a1138",
    "updatedAt": "2020-03-22T15:22:29.708Z",
    "createdAt": "2020-03-22T15:22:29.708Z"
  }
}
```

### `POST /actions/check-in`

Führt einen Check-In aus. 

#### Beispiel

##### Request

Header muss `X-User-Id` mit der Benutzer-ID muss vorhanden sein.

```sh
curl -X "POST" "httpw://www.cguard.de/api/v1/actions/check-in" \
     -H 'X-User-Id: 3f6ffbc8-2648-4b93-a132-667ef48a1138' \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{}'
```

##### Response

```json
{
  "status": 201,
  "message": "Created",
  "action": {
    "id": "4fcb8ce6-6fdc-4f2f-9939-06d11c474fe4",
    "userId": "3f6ffbc8-2648-4b93-a132-667ef48a1138",
    "type": "CHECK_IN",
    "amount": 100,
    "updatedAt": "2020-03-22T14:54:33.184Z",
    "createdAt": "2020-03-22T14:54:33.184Z"
  }
}
```

Wenn Check-In noch nicht erlaubt

```json
{
  "status": 423,
  "message": "Locked",
  "secondsToWait": 3156
}
```

### `POST /users`

Erstellt einen Benutzer.

* Username wird automatisch generiert

#### Beispiel

##### Request

```json
{
    "country": "DE",
    "latitude": 37.285951,
    "longitude": -121.936650,
    "radius": 100
}
```

##### Response

```json
{
    "status": 201,
    "message": "Created",
    "user": {
        "id": "27591d77-5d72-4800-9695-53c939fae73e",
        "username": "Yellow Elephant",
        "country": "DE",
        "score": 0,
        "rank": 1,
        "baseStatus": "PROTECTED",
        "latitude": 37.285951,
        "longitude": -121.936650,
        "radius": 100,        
        "createdAt": "2020-03-21T21:50:59.000Z",
        "updatedAt": "2020-03-21T21:50:59.000Z"        
    }
}
```

### `GET /users/:id`

Gibt Daten zu Benutzer mit der ID `id` zurück

#### Beispiel

##### Response

```json
{
      "id": "d1053640-225d-44fd-9d16-572889052979",
      "username": "Pink Mouse",
      "country": "DE",
      "score": 750,
      "rank": 2,
      "baseStatus": "UNPROTECTED",
      "latitude": "37.285951",
      "longitude": "-121.93665",
      "radius": 100,
      "createdAt": "2020-03-21T23:26:05.000Z",
      "updatedAt": "2020-03-21T23:26:05.000Z"
}
```

### `GET /users/:id/history`

Gibt Score-History zu Benutzer mit der ID `id` zurück

#### Beispiel

##### Response

```json
{
  "content": [
    {
      "id": "32445d8d-842b-49b3-ae90-528285474b4d",
      "type": "ATTACK",
      "amount": 200,
      "createdAt": "2020-03-22T17:15:23.000Z"
    },
    { "...": "..." }
  ]
}
```

### `PUT /users/:id`

Aktualisiert einen Benutzer.

Folgende Werte können aktualisiert werden, sind aber alle optional

* `score`
* `rank`
* `baseStatus` 

#### Beispiel

##### Request

```json
{    
    "rank": 2,
    "score": 850    
}
```

##### Response

```json
{
    "status": 200,
    "message": "OK"    
}
```



### `GET /leaders`

Gibt Liste von 10 Benutzern absteigend sortiert nach Rank und Score zurück.

#### Beispiel

##### Response

```json
{
    "content": [
        {
            "username": "alex",
            "score": 1250,
            "rank": 1250,
            "country": "DE"
        },
        { "...": "..." },
        { "...": "..." },
  ]  
}
```

### `GET /i18n/:country`

Gibt Sprachdefintion zu `country` zurück

#### Beispiel

##### Response

```json
{
    "welcome": "Hallo :username:",
    "save": "Speichern",
    "...": "..."
}
```
