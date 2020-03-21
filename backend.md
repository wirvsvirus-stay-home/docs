# Backend: Lorem Ipsum

_Version 0.1.0_

## Changelog

### 0.1.0

* Username wird nun vom Backend bei `POST /users` automatisch generiert un zurückgegeben

## Einleitung

Für den Prototypen beschränken wir uns auf folgende zu erfassende Benutzerdaten

- Device-ID (`deviceId`)
- Benutzername (`username`)
- Land (`country`)
- Score (`score`)
- Rank (`rank`)
- Status der Basis (`baseStatus`) (`PROTECTED` / `UNPROTECTED`)
- Geofancing (`latitude`, `longitude`, `radius`)

Zudem hält das Backend auch alle Daten für die Internationialiserung/I18N bereit.

Eine Authentifizierung der Benutzer und deren Request/Aktionen erfolgt für den Prototypen nicht.

## API

### `POST /users`

Erstellt einen Benutzer

#### Beispiel

##### Request

```json
{
    "deviceId": "1234-5678-9012",
    "countryCode": "DE",
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
        "deviceId": "1234-5678-9012",
        "username": "Yellow Elephant",
        "countryCode": "DE",
        "latitude": 37.285951,
        "longitude": -121.936650,
        "radius": 100        
    }
}
```



### `GET /users?unprotected=1`

Gibt einen Benutzer zurück, dessen Basis momentan ungeschützt ist

```json
{
    "deviceId": "1234-5678-6451",
    "username": "Yellow Elephant",
    "score": 800,
    "rank": 3
}
```



### `GET /users/:deviceId`

Gibt Daten zu Benutzer mit Device-ID `deviceId` zurück

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

### `PUT /users/:deviceId`

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
