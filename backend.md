# Backend: Lorem Ipsum

_Version 0.5.0_

## Changelog

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
* Status der Basis (`baseStatus`) (`PROTECTED` / `UNPROTECTED`)
* Geofancing (`latitude`, `longitude`, `radius`)

Zudem hält das Backend auch alle Daten für die Internationialiserung/I18N bereit.

Eine Authentifizierung der Benutzer und deren Request/Aktionen erfolgt für den Prototypen nicht.

## API

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



### `GET /users?unprotected=1`

Gibt einen Benutzer zurück, dessen Basis momentan ungeschützt ist

```json
{
    "id": "27591d77-5d72-4800-9695-53c939fae73e",
    "username": "Yellow Elephant",
    "score": 800,
    "rank": 3
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

Gibt Liste von *n* (20 oder so) Benutzern absteigend sortiert nach Score zurück.

#### Beispiel

##### Response

```json
{
    "content": [
        {
            "username": "alex",
            "score": 1250,
            "countryCode": "DE"
        },
        // ...
  ]  
}
```

### `GET /i18n/:countryCode`

Gibt Sprachdefintion zu `countryCode` zurück

#### Beispiel

##### Response

```json
{
    "welcome": "Hallo :username:",
    "save": "Speichern",
    // ....
}
```
