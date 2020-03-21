# Backend: Lorem Ipsum

_Version 0.3.0_

## Changelog

### 0.3.0

* `deviceId` aus Benutzer entfernt
* `id` von Benutzer ist nun UUIDv4

### 0.2.0

* `POST /users` aktualisert

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
    "username": "Yellow Elephant",
    "rank": 2,
    "score": 950,
    "countryCode": "DE",
    "latitude": 37.285951,
    "longitude": -121.936650,
    "radius": 100,
    "baseStatus": "PROTECTED"
}
```

### `PUT /users/:id`

Aktualisiert einen Benutzer

#### Beispiel

##### Request

```json
{    
    "countryCode": "IT",
    "rank": 2,
    "score": 850,
    "baseStatus": "PROTECTED"
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
