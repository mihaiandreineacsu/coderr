# Coderr API Endpoint Dokumentation

Quelle: https://cdn.developerakademie.com/courses/Backend/EndpointDoku/index.html?name=coderr

## Endpoint-Checkliste

## Authentication

Login und Registrierung

### POST /api/registration/

- [ ] `POST /api/registration/` implementiert
- Beschreibung: Erstellt einen neuen Benutzer. Dieser Benutzer kann entweder ein Customer- oder Business-User sein.
- Permissions: Keine speziellen Permissions angegeben.
```json
{
    "username":  "exampleUsername",
    "email":  "example@mail.de",
    "password":  "examplePassword",
    "repeated_password":  "examplePassword",
    "type":  "customer"
}
```


#### Response
Erfolgreicher Erstellung gibt dies ein Token sowie die Benutzerinformationen zurueck, inklusive die einzigartige Nutzer-ID.
```json
{
    "token":  "83bf098723b08f7b23429u0fv8274",
    "username":  "exampleUsername",
    "email":  "example@mail.de",
    "user_id":  123
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `201` | Der Benutzer wurde erfolgreich erstellt. |
| `400` | Ungueltige Anfragedaten. |
| `500` | Interner Serverfehler. |

### POST /api/login/

- [ ] `POST /api/login/` implementiert
- Beschreibung: Authentifiziert einen Benutzer und liefert ein Authentifizierungs-Token zurueck, das fuer weitere API-Anfragen genutzt wird.
- Permissions: Keine speziellen Permissions angegeben.
```json
{
    "username":  "exampleUsername",
    "password":  "examplePassword"
}
```


#### Response
Erfolgreiche Authentifizierung gibt ein Token sowie Benutzerinformationen zurueck.
```json
{
    "token":  "83bf098723b08f7b23429u0fv8274",
    "username":  "exampleUsername",
    "email":  "example@mail.de",
    "user_id":  123
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Erfolgreiche Anmeldung. |
| `400` | Ungueltige Anfragedaten. |
| `500` | Interner Serverfehler. |

## Profile

Alles an CRUD das fuer das Frontend eine Rolle spielt

### GET /api/profile/{pk}/

- [ ] `GET /api/profile/{pk}/` implementiert
- Beschreibung: Ruft die detaillierten Informationen eines Benutzerprofils ab (sowohl fuer Kunden- als auch fuer Geschaeftsnutzer). Ermoeglicht auch das Bearbeiten der Profildaten (PATCH).
- Permissions: Der Benutzer muss authentifiziert sein.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `pk` | - | Die ID des Benutzers, dessen Profil abgerufen oder bearbeitet wird. |

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Die Antwort enthaelt die vollstaendigen Profildaten eines spezifischen Benutzers. Die Felder first_name, last_name, location, tel, description und working_hours duerfen im Response nicht null sein, sondern muessen, falls keine Werte vorhanden sind, mit einem leeren String ('' '') belegt werden.
```json
{
    "user":  1,
    "username":  "max_mustermann",
    "first_name":  "Max",
    "last_name":  "Mustermann",
    "file":  "profile_picture.jpg",
    "location":  "Berlin",
    "tel":  "123456789",
    "description":  "Business description",
    "working_hours":  "9-17",
    "type":  "business",
    "email":  "max@business.de",
    "created_at":  "2023-01-01T12:00:00Z"
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Die Profildaten wurden erfolgreich abgerufen. |
| `401` | Benutzer ist nicht authentifiziert. |
| `404` | Das Benutzerprofil wurde nicht gefunden. |
| `500` | Interner Serverfehler. |

### PATCH /api/profile/{pk}/

- [ ] `PATCH /api/profile/{pk}/` implementiert
- Beschreibung: Ermoeglicht es einem Benutzer, bestimmte Profilinformationen zu aktualisieren.
- Permissions: Der Benutzer kann NUR sein eigenes Profil bearbeiten.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `pk` | - | Die ID des Benutzers, dessen Profil bearbeitet wird. |

#### Request

Im Anfrage-Body koennen bestimmte Profilinformationen aktualisiert werden. Nicht alle Felder sind erforderlich.
```json
{
    "first_name":  "Max",
    "last_name":  "Mustermann",
    "location":  "Berlin",
    "tel":  "987654321",
    "description":  "Updated business description",
    "working_hours":  "10-18",
    "email":  "new_email@business.de"
}
```


#### Response
Die Antwort enthaelt das aktualisierte Profil des Benutzers. Die Felder first_name, last_name, location, tel, description und working_hours duerfen im Response nicht null sein, sondern muessen, falls keine Werte vorhanden sind, mit einem leeren String ('' '') belegt werden.
```json
{
    "user":  1,
    "username":  "max_mustermann",
    "first_name":  "Max",
    "last_name":  "Mustermann",
    "file":  "profile_picture.jpg",
    "location":  "Berlin",
    "tel":  "987654321",
    "description":  "Updated business description",
    "working_hours":  "10-18",
    "type":  "business",
    "email":  "new_email@business.de",
    "created_at":  "2023-01-01T12:00:00Z"
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Das Profil wurde erfolgreich aktualisiert. |
| `401` | Benutzer ist nicht authentifiziert |
| `403` | Authentifizierter Benutzer ist nicht der Eigentuemer Profils |
| `404` | Das Benutzerprofil wurde nicht gefunden. |
| `500` | Interner Serverfehler. |

### GET /api/profiles/business/

- [ ] `GET /api/profiles/business/` implementiert
- Beschreibung: Gibt eine Liste aller Geschaeftsnutzer auf der Plattform zurueck.
- Permissions: Der Benutzer muss authentifiziert sein.

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Die Antwort enthaelt eine Liste aller Geschaeftsnutzer mit ihren Profilinformationen. Die Felder first_name, last_name, location, tel, description und working_hours duerfen im Response nicht null sein, sondern muessen, falls keine Werte vorhanden sind, mit einem leeren String ('' '') belegt werden.
```json
{
    "user":  1,
    "username":  "max_business",
    "first_name":  "Max",
    "last_name":  "Mustermann",
    "file":  "profile_picture.jpg",
    "location":  "Berlin",
    "tel":  "123456789",
    "description":  "Business description",
    "working_hours":  "9-17",
    "type":  "business"
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Erfolgreiche Antwort mit der Liste der Geschaeftsnutzer. |
| `401` | Benutzer ist nicht authentifiziert. |
| `500` | Interner Serverfehler. |

### GET /api/profiles/customer/

- [ ] `GET /api/profiles/customer/` implementiert
- Beschreibung: Gibt eine Liste aller Kundenprofile auf der Plattform zurueck.
- Permissions: Der Benutzer muss authentifiziert sein.

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Die Antwort enthaelt eine Liste aller Kunden mit ihren Profilinformationen. Die Felder first_name, last_name, location, tel, description und working_hours duerfen im Response nicht null sein, sondern muessen, falls keine Werte vorhanden sind, mit einem leeren String ('' '') belegt werden.
```json
{
    "user":  2,
    "username":  "customer_jane",
    "first_name":  "Jane",
    "last_name":  "Doe",
    "file":  "profile_picture_customer.jpg",
    "uploaded_at":  "2023-09-15T09:00:00",
    "type":  "customer"
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Erfolgreiche Antwort mit der Liste der Kundenprofile. |
| `401` | Benutzer ist nicht authentifiziert. |
| `500` | Interner Serverfehler. |

## Angebote (offers)

Alles an CRUD das fuer das Frontend eine Rolle spielt

### GET /api/offers/

- [ ] `GET /api/offers/` implementiert
- Beschreibung: Dieser Endpunkt gibt eine Liste von Angeboten zurueck. Jedes Angebot enthaelt eine Uebersicht der Angebotsdetails, den minimalen Preis und die kuerzeste Lieferzeit.
- Permissions: Keine speziellen Permissions angegeben.
- Extra Information: Die Antwort verwendet PageNumberPagination.

#### Query Parameters

| Name | Type | Description |
| --- | --- | --- |
| `creator_id` | integer | Filtert die Angebote nach dem Benutzer, der sie erstellt hat. |
| `min_price` | float | Filtert Angebote mit einem Mindestpreis. |
| `max_delivery_time` | integer | Filtert Angebote, deren Lieferzeit kuerzer oder gleich dem angegebenen Wert ist. |
| `ordering` | string | Sortiert die Angebote nach den Feldern 'updated_at' oder 'min_price'. |
| `search` | string | Durchsucht die Felder 'title' und 'description' nach Uebereinstimmungen. |
| `page_size` | integer | Gibt an, wie viele Ergebnisse pro Seite zurueckgegeben werden sollen. Dies sollte mit dem Frontend abgestimmt sein. |

#### Request

Es ist keine Anfrage mit einem Body erforderlich.

#### Response
Die Antwort ist eine paginierte Liste von Angeboten mit den zugehoerigen Details.
```json
{
    "count":  1,
    "next":  "http://127.0.0.1:8000/api/offers/?page=2",
    "previous":  null,
    "results":  [
                    {
                        "id":  1,
                        "user":  1,
                        "title":  "Website Design",
                        "image":  null,
                        "description":  "Professionelles Website-Design...",
                        "created_at":  "2024-09-25T10:00:00Z",
                        "updated_at":  "2024-09-28T12:00:00Z",
                        "details":  [
                                        {
                                            "id":  1,
                                            "url":  "/offerdetails/1/"
                                        },
                                        {
                                            "id":  2,
                                            "url":  "/offerdetails/2/"
                                        },
                                        {
                                            "id":  3,
                                            "url":  "/offerdetails/3/"
                                        }
                                    ],
                        "min_price":  100.00,
                        "min_delivery_time":  7,
                        "user_details":  {
                                             "first_name":  "John",
                                             "last_name":  "Doe",
                                             "username":  "jdoe"
                                         }
                    }
                ]
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Die Anfrage war erfolgreich und eine Liste von Angeboten wurde zurueckgegeben. |
| `400` | Ungueltige Anfrageparameter. |
| `500` | Interner Serverfehler. |

### POST /api/offers/

- [ ] `POST /api/offers/` implementiert
- Beschreibung: Dieser Endpunkt ermoeglicht es, ein neues Angebot (Offer) zu erstellen. Ein Offer muss 3 Details enthalten!
- Permissions: Nur User vom type 'business' duerfen Angebote erstellen

#### Request

Beim Erstellen eines Angebots muessen genau drei Details angegeben werden, jeweils mit dem Typ basic, standard und premium. 'revisions' sind Integer, wobei -1 fuer unendlich steht. 'delivery_time_in_days' muss ein positiver Integer sein. 'price' kann entweder ein Float oder ein String sein, welcher einen Float darstellt.
```json
{
    "title":  "Grafikdesign-Paket",
    "image":  null,
    "description":  "Ein umfassendes Grafikdesign-Paket fuer Unternehmen.",
    "details":  [
                    {
                        "title":  "Basic Design",
                        "revisions":  2,
                        "delivery_time_in_days":  5,
                        "price":  100.0,
                        "features":  [
                                         "Logo Design",
                                         "Visitenkarte"
                                     ],
                        "offer_type":  "basic"
                    },
                    {
                        "title":  "Standard Design",
                        "revisions":  5,
                        "delivery_time_in_days":  7,
                        "price":  200.0,
                        "features":  [
                                         "Logo Design",
                                         "Visitenkarte",
                                         "Briefpapier"
                                     ],
                        "offer_type":  "standard"
                    },
                    {
                        "title":  "Premium Design",
                        "revisions":  10,
                        "delivery_time_in_days":  10,
                        "price":  500.0,
                        "features":  [
                                         "Logo Design",
                                         "Visitenkarte",
                                         "Briefpapier",
                                         "Flyer"
                                     ],
                        "offer_type":  "premium"
                    }
                ]
}
```


#### Response
Bei erfolgreicher Erstellung wird das Angebot mit den zugehoerigen Details zurueckgegeben, einschliesslich IDs fuer das Angebot und jedes Detail.
```json
{
    "id":  1,
    "title":  "Grafikdesign-Paket",
    "image":  null,
    "description":  "Ein umfassendes Grafikdesign-Paket fuer Unternehmen.",
    "details":  [
                    {
                        "id":  1,
                        "title":  "Basic Design",
                        "revisions":  2,
                        "delivery_time_in_days":  5,
                        "price":  100.0,
                        "features":  [
                                         "Logo Design",
                                         "Visitenkarte"
                                     ],
                        "offer_type":  "basic"
                    },
                    {
                        "id":  2,
                        "title":  "Standard Design",
                        "revisions":  5,
                        "delivery_time_in_days":  7,
                        "price":  200.0,
                        "features":  [
                                         "Logo Design",
                                         "Visitenkarte",
                                         "Briefpapier"
                                     ],
                        "offer_type":  "standard"
                    },
                    {
                        "id":  3,
                        "title":  "Premium Design",
                        "revisions":  10,
                        "delivery_time_in_days":  10,
                        "price":  500.0,
                        "features":  [
                                         "Logo Design",
                                         "Visitenkarte",
                                         "Briefpapier",
                                         "Flyer"
                                     ],
                        "offer_type":  "premium"
                    }
                ]
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `201` | Das Angebot wurde erfolgreich erstellt. |
| `400` | Ungueltige Anfragedaten oder unvollstaendige Details. |
| `401` | Benutzer ist nicht authentifiziert. |
| `403` | Authentifizierter Benutzer ist kein 'business' Profil |
| `500` | Interner Serverfehler. |

### GET /api/offers/{id}/

- [ ] `GET /api/offers/{id}/` implementiert
- Beschreibung: Dieser Endpunkt gibt die Details eines spezifischen Angebots anhand der angegebenen ID zurueck.
- Permissions: Der Benutzer muss authentifiziert sein
- Extra Information: Die Angebotsdetails enthalten die URLs zu den einzelnen Angebotsdetail-Objekten.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `id` | integer | Die ID des gewuenschten Angebots. |

#### Request

Es ist keine Anfrage mit einem Body erforderlich.

#### Response
Gibt die Details eines spezifischen Angebots, Angebotsdetails und Metadaten zurueck. 'user' ist hier die ID des User der dieses Angebot erstellt hat.
```json
{
    "id":  66,
    "user":  114,
    "title":  "Grafikdesign-Paket",
    "image":  null,
    "description":  "Ein umfassendes Grafikdesign-Paket fuer Unternehmen.",
    "created_at":  "2025-01-23T07:44:15.365773Z",
    "updated_at":  "2025-01-23T07:44:15.365773Z",
    "details":  [
                    {
                        "id":  199,
                        "url":  "http://127.0.0.1:8000/api/offerdetails/199/"
                    },
                    {
                        "id":  200,
                        "url":  "http://127.0.0.1:8000/api/offerdetails/200/"
                    },
                    {
                        "id":  201,
                        "url":  "http://127.0.0.1:8000/api/offerdetails/201/"
                    }
                ],
    "min_price":  50.0,
    "min_delivery_time":  5
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Die Anfrage war erfolgreich, die Angebotsdetails wurden zurueckgegeben. |
| `401` | Benutzer ist nicht authentifiziert |
| `404` | Das Angebot mit der angegebenen ID wurde nicht gefunden. |
| `500` | Interner Serverfehler. |

### PATCH /api/offers/{id}/

- [ ] `PATCH /api/offers/{id}/` implementiert
- Beschreibung: Aktualisiert ein spezifisches Angebot. Ein PATCH ueberschreibt nur die angegebenen Felder. Es muessen nicht alle Felder angegeben werden, nur die, die aktualisiert werden sollen.
- Permissions: Nur Ersteller des Angebotes koennen dies veraendern.
- Extra Information: Nur die angegebenen Felder werden aktualisiert. Alle nicht angegebenen Felder bleiben unveraendert. Details koennen einzeln aktualisiert werden, wobei ihre IDs unveraendert bleiben muessen. Desweiteren sollte der Typ (offer_type) immer mitgegeben werden, um das Detail eindeutig zu identifizieren.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `id` | integer | Die ID des zu aktualisierenden Angebots. |

#### Request

Die Anfrage kann jedes Feld enthalten, das aktualisiert werden soll. Felder, die nicht angegeben werden, bleiben unveraendert. Wenn ein Detail veraendert werden soll, wird immer der type mitgegeben zur Identifizierung.
```json
{
    "title":  "Updated Grafikdesign-Paket",
    "details":  [
                    {
                        "title":  "Basic Design Updated",
                        "revisions":  3,
                        "delivery_time_in_days":  6,
                        "price":  120.0,
                        "features":  [
                                         "Logo Design",
                                         "Flyer"
                                     ],
                        "offer_type":  "basic"
                    }
                ]
}
```


#### Response
Gibt das aktualisierte Angebot mit allen Feldern zurueck, unabhaengig davon, welche Felder in der Anfrage angegeben wurden.
```json
{
    "id":  66,
    "title":  "Updated Grafikdesign-Paket",
    "image":  null,
    "description":  "Ein umfassendes Grafikdesign-Paket fuer Unternehmen.",
    "details":  [
                    {
                        "id":  199,
                        "title":  "Basic Design Updated",
                        "revisions":  3,
                        "delivery_time_in_days":  6,
                        "price":  120.0,
                        "features":  [
                                         "Logo Design",
                                         "Flyer"
                                     ],
                        "offer_type":  "basic"
                    },
                    {
                        "id":  200,
                        "title":  "Standard Design",
                        "revisions":  5,
                        "delivery_time_in_days":  10,
                        "price":  120.0,
                        "features":  [
                                         "Logo Design",
                                         "Visitenkarte",
                                         "Briefpapier"
                                     ],
                        "offer_type":  "standard"
                    },
                    {
                        "id":  201,
                        "title":  "Premium Design",
                        "revisions":  10,
                        "delivery_time_in_days":  10,
                        "price":  150.0,
                        "features":  [
                                         "Logo Design",
                                         "Visitenkarte",
                                         "Briefpapier",
                                         "Flyer"
                                     ],
                        "offer_type":  "premium"
                    }
                ]
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Das Angebot wurde erfolgreich aktualisiert. |
| `400` | Ungueltige Anfragedaten oder unvollstaendige Details. |
| `401` | Benutzer ist nicht authentifiziert |
| `403` | Authentifizierter Benutzer ist nicht der Eigentuemer des Angebots |
| `404` | Das Angebot mit der angegebenen ID wurde nicht gefunden. |
| `500` | Interner Serverfehler. |

### DELETE /api/offers/{id}/

- [ ] `DELETE /api/offers/{id}/` implementiert
- Beschreibung: Loescht ein spezifisches Angebot anhand der angegebenen ID.
- Permissions: Nur Ersteller des Angebotes koennen dies loeschen.
- Extra Information: Dieser Endpunkt gibt im Erfolgsfall keinen Antwortinhalt zurueck, sondern nur den HTTP-Statuscode 204.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `id` | integer | Die ID des zu loeschenden Angebots. |

#### Request

Es ist keine Anfrage mit einem Body erforderlich.

#### Response
Bei Erfolg wird ein HTTP-Statuscode 204 No Content zurueckgegeben, ohne Inhalt in der Antwort.
#### Status Codes

| Code | Description |
| --- | --- |
| `204` | Das Angebot wurde erfolgreich geloescht. |
| `401` | Benutzer ist nicht authentifiziert |
| `403` | Authentifizierter Benutzer ist nicht der Eigentuemer des Angebots |
| `404` | Das Angebot mit der angegebenen ID wurde nicht gefunden. |
| `500` | Interner Serverfehler. |

### GET /api/offerdetails/{id}/

- [ ] `GET /api/offerdetails/{id}/` implementiert
- Beschreibung: Ruft die Details eines spezifischen Angebotsdetails ab.
- Permissions: Der Benutzer muss authentifiziert sein.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `id` | integer | Die ID des Angebotsdetails, das abgerufen werden soll. |

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Gibt die vollstaendigen Details des Angebotsdetails zurueck, einschliesslich Titel, Preis, Lieferzeit, Features und Angebotstyp.
```json
{
    "id":  1,
    "title":  "Basic Design",
    "revisions":  2,
    "delivery_time_in_days":  5,
    "price":  100.0,
    "features":  [
                     "Logo Design",
                     "Visitenkarte"
                 ],
    "offer_type":  "basic"
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Das Angebotsdetail wurde erfolgreich abgerufen. |
| `401` | Benutzer ist nicht authentifiziert |
| `404` | Das Angebotsdetail mit der angegebenen ID wurde nicht gefunden. |
| `500` | Interner Serverfehler. |

## Bestellungen (orders)

Alles an CRUD das fuer das Frontend eine Rolle spielt

### GET /api/orders/

- [ ] `GET /api/orders/` implementiert
- Beschreibung: Gibt eine Liste der Bestellungen zurueck, die entweder vom angemeldeten Benutzer als Kunde oder als Geschaeftspartner erstellt wurden.
- Permissions: Der Benutzer muss authentifiziert sein.
- Extra Information: Dieser Endpunkt gibt nur Bestellungen zurueck, die mit dem angemeldeten Benutzer entweder als Kunde oder als Geschaeftspartner verbunden sind.

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Eine Liste von Bestellungen, einschliesslich Details wie Kunde, Geschaeftspartner, Titel, Status und Erstellungsdatum.
```json
{
    "id":  1,
    "customer_user":  1,
    "business_user":  2,
    "title":  "Logo Design",
    "revisions":  3,
    "delivery_time_in_days":  5,
    "price":  150.0,
    "features":  [
                     "Logo Design",
                     "Visitenkarten"
                 ],
    "offer_type":  "basic",
    "status":  "in_progress",
    "created_at":  "2024-09-29T10:00:00Z",
    "updated_at":  "2024-09-30T12:00:00Z"
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Die Liste der Bestellungen wurde erfolgreich abgerufen. |
| `401` | Benutzer ist nicht authentifiziert. |
| `500` | Interner Serverfehler. |

### POST /api/orders/

- [ ] `POST /api/orders/` implementiert
- Beschreibung: Erstellt eine neue Bestellung basierend auf den Details eines Angebots (OfferDetail).
- Permissions: Der Benutzer muss authentifiziert sein und vom typ 'customer' sein.
- Extra Information: Nur Benutzer vom typ 'customer' koennen Bestellungen erstellen. Der Benutzer gibt eine OfferDetail ID an, und die Bestellung wird auf Grundlage dieses Angebots erstellt. Beachte, dass das Angebot sowohl den Anbieter als auch den Kunden beinhalten muss. Diese Informationen koennen aus der Authentifizierung und der Offer entnommen werden.

#### Request

Die Anfrage muss die ID eines Angebotsdetails enthalten, auf dem die Bestellung basieren soll.
```json
{
    "offer_detail_id":  1
}
```


#### Response
Die erstellte Bestellung wird zurueckgegeben, einschliesslich Details wie ID, Kunde, Geschaeftspartner, Titel, Preis und Status.
```json
{
    "id":  1,
    "customer_user":  1,
    "business_user":  2,
    "title":  "Logo Design",
    "revisions":  3,
    "delivery_time_in_days":  5,
    "price":  150.0,
    "features":  [
                     "Logo Design",
                     "Visitenkarten"
                 ],
    "offer_type":  "basic",
    "status":  "in_progress",
    "created_at":  "2024-09-29T10:00:00Z",
    "updated_at":  "2024-09-30T12:00:00Z"
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `201` | Die Bestellung wurde erfolgreich erstellt. |
| `400` | Ungueltige Anfragedaten (z. B. wenn 'offer_detail_id' fehlt oder ungueltig ist). |
| `401` | Benutzer ist nicht authentifiziert. |
| `403` | Benutzer hat keine Berechtigung, z.B. weil nicht vom typ 'customer'. |
| `404` | Das angegebene Angebotsdetail wurde nicht gefunden. |
| `500` | Interner Serverfehler. |

### PATCH /api/orders/{id}/

- [ ] `PATCH /api/orders/{id}/` implementiert
- Beschreibung: Aktualisiert den Status einer spezifischen Bestellung. Moegliche Statuswerte sind z.B. 'in_progress', 'completed', oder 'cancelled'.
- Permissions: Nur ein Benutzer vom typ 'business' kann den Status einer Bestellung aktualisieren.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `id` | integer | Die eindeutige ID der Bestellung, die aktualisiert werden soll. |

#### Request

Nur der Status der Bestellung wird aktualisiert. Andere Felder bleiben unveraendert.
```json
{
    "status":  "completed"
}
```


#### Response
Die aktualisierten Details der Bestellung werden zurueckgegeben, einschliesslich des neuen Status und aktualisierter Timestamps.
```json
{
    "id":  1,
    "customer_user":  1,
    "business_user":  2,
    "title":  "Logo Design",
    "revisions":  3,
    "delivery_time_in_days":  5,
    "price":  150.0,
    "features":  [
                     "Logo Design",
                     "Visitenkarten"
                 ],
    "offer_type":  "basic",
    "status":  "completed",
    "created_at":  "2024-09-29T10:00:00Z",
    "updated_at":  "2024-09-30T15:00:00Z"
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Der Status der Bestellung wurde erfolgreich aktualisiert. |
| `400` | Ungueltiger Status oder unzulaessige Felder in der Anfrage. |
| `401` | Benutzer ist nicht authentifiziert. |
| `403` | Benutzer hat keine Berechtigung, diese Bestellung zu aktualisieren. |
| `404` | Die angegebene Bestellung wurde nicht gefunden. |
| `500` | Interner Serverfehler. |

### DELETE /api/orders/{id}/

- [ ] `DELETE /api/orders/{id}/` implementiert
- Beschreibung: Loescht eine spezifische Bestellung. Diese Aktion ist auf Admin-Benutzer (Staff) beschraenkt.
- Permissions: Nur Admin-Benutzer (Staff) duerfen Bestellungen loeschen.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `id` | integer | Die eindeutige ID der zu loeschenden Bestellung. |

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Die Antwort enthaelt keinen Inhalt und zeigt an, dass die Bestellung erfolgreich geloescht wurde.
#### Status Codes

| Code | Description |
| --- | --- |
| `204` | Die Bestellung wurde erfolgreich geloescht. Keine weiteren Inhalte in der Antwort. |
| `401` | Benutzer ist nicht authentifiziert. |
| `403` | Benutzer hat keine Berechtigung, die Bestellung zu loeschen. |
| `404` | Die angegebene Bestellung wurde nicht gefunden. |
| `500` | Interner Serverfehler. |

### GET /api/order-count/{business_user_id}/

- [ ] `GET /api/order-count/{business_user_id}/` implementiert
- Beschreibung: Dieser Endpunkt gibt die Anzahl der laufenden Bestellungen eines bestimmten Geschaeftsnutzers (Business User) zurueck. Laufende Bestellungen sind solche mit dem Status 'in_progress'.
- Permissions: Der Benutzer muss authentifiziert sein.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `business_user_id` | integer | Die eindeutige ID des Geschaeftsnutzers, dessen laufende Bestellungen gezaehlt werden sollen. |

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Die Antwort enthaelt die Anzahl der laufenden Bestellungen fuer den angegebenen Geschaeftsnutzer.
```json
{
    "order_count":  5
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Die Anzahl der laufenden Bestellungen wurde erfolgreich abgerufen. |
| `401` | Benutzer ist nicht authentifiziert. |
| `404` | Kein Geschaeftsnutzer mit der angegebenen ID gefunden. |
| `500` | Interner Serverfehler. |

### GET /api/completed-order-count/{business_user_id}/

- [ ] `GET /api/completed-order-count/{business_user_id}/` implementiert
- Beschreibung: Gibt die Anzahl der abgeschlossenen Bestellungen eines bestimmten Geschaeftsnutzers zurueck. Abgeschlossene Bestellungen haben den Status 'completed'.
- Permissions: Der Benutzer muss authentifiziert sein.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `business_user_id` | integer | Die eindeutige ID des Geschaeftsnutzers, dessen abgeschlossene Bestellungen gezaehlt werden sollen. |

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Die Antwort enthaelt die Anzahl der abgeschlossenen Bestellungen fuer den angegebenen Geschaeftsnutzer.
```json
{
    "completed_order_count":  10
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Die Anzahl der abgeschlossenen Bestellungen wurde erfolgreich abgerufen. |
| `401` | Benutzer ist nicht authentifiziert. |
| `404` | Kein Geschaeftsnutzer mit der angegebenen ID gefunden. |
| `500` | Interner Serverfehler. |

## Bewertungen (reviews)

Alles an CRUD das fuer das Frontend eine Rolle spielt

### GET /api/reviews/

- [ ] `GET /api/reviews/` implementiert
- Beschreibung: Listet alle verfuegbaren Bewertungen auf. Die Bewertungen koennen nach 'updated_at' oder 'rating' geordnet werden. Es koennen auch Filter-Parameter wie 'business_user_id' und 'reviewer_id' verwendet werden.
- Permissions: Jeder authentifizierte Benutzer kann Bewertungen lesen.

#### Query Parameters

| Name | Type | Description |
| --- | --- | --- |
| `business_user_id` | integer | Die ID des Geschaeftsbenutzers, fuer den Bewertungen gefiltert werden sollen. |
| `reviewer_id` | integer | Die ID des Benutzers, der die Bewertungen erstellt hat. |
| `ordering` | string | Die Sortierreihenfolge der Bewertungen. Moegliche Werte: 'updated_at' oder 'rating'. |

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Die Antwort enthaelt eine Liste aller Bewertungen, die gefiltert und geordnet werden koennen.
```json
[
    {
        "id":  1,
        "business_user":  2,
        "reviewer":  3,
        "rating":  4,
        "description":  "Sehr professioneller Service.",
        "created_at":  "2023-10-30T10:00:00Z",
        "updated_at":  "2023-10-31T10:00:00Z"
    },
    {
        "id":  2,
        "business_user":  5,
        "reviewer":  3,
        "rating":  5,
        "description":  "Top Qualitaet und schnelle Lieferung!",
        "created_at":  "2023-09-20T10:00:00Z",
        "updated_at":  "2023-09-20T12:00:00Z"
    }
]
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Erfolgreiche Antwort mit der Liste der Bewertungen. |
| `401` | Unauthorized. Der Benutzer muss authentifiziert sein. |
| `500` | Interner Serverfehler. |

### POST /api/reviews/

- [ ] `POST /api/reviews/` implementiert
- Beschreibung: Erstellt eine neue Bewertung fuer einen Geschaeftsbenutzer. Nur authentifizierte Benutzer mit einem Kundenprofil duerfen Bewertungen erstellen. Ein Benutzer kann pro Geschaeftsprofil nur eine Bewertung abgeben.
- Permissions: Nur authentifizierte Benutzer mit einem Kundenprofil duerfen Bewertungen erstellen. Jeder authentifizierte Benutzer kann Bewertungen lesen.
- Extra Information: Dieser Endpunkt erlaubt es Kunden, eine Bewertung fuer einen Geschaeftsbenutzer zu hinterlassen. Eine Bewertung kann nur einmal pro Geschaeftsbenutzer abgegeben werden.

#### Request

Der Anfrage-Body muss die Bewertung und Beschreibung enthalten, ausserdem die ID des Geschaeftsbenutzers.
```json
{
    "business_user":  2,
    "rating":  4,
    "description":  "Alles war toll!"
}
```


#### Response
Erfolgreiche Antwort, die die Details der neu erstellten Bewertung zurueckgibt.
```json
{
    "id":  3,
    "business_user":  2,
    "reviewer":  3,
    "rating":  4,
    "description":  "Alles war toll!",
    "created_at":  "2023-10-30T15:30:00Z",
    "updated_at":  "2023-10-30T15:30:00Z"
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `201` | Erfolgreich erstellt. |
| `400` | Fehlerhafte Anfrage. Der Benutzer hat moeglicherweise bereits eine Bewertung fuer das gleiche Geschaeftsprofil abgegeben. |
| `401` | Unauthorized. Der Benutzer muss authentifiziert sein und ein Kundenprofil besitzen. |
| `403` | Forbidden. Ein Benutzer kann nur eine Bewertung pro Geschaeftsprofil abgeben. |
| `500` | Interner Serverfehler. |

### PATCH /api/reviews/{id}/

- [ ] `PATCH /api/reviews/{id}/` implementiert
- Beschreibung: Aktualisiert ausgewaehlte Felder einer bestehenden Bewertung (nur 'rating' und 'description' sind editierbar). Der Endpunkt erlaubt es dem Ersteller der Bewertung, die Bewertung zu bearbeiten.
- Permissions: Nur der Ersteller der Bewertung darf diese Aktion durchfuehren.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `id` | integer | Die ID der spezifischen Bewertung, die aktualisiert werden soll. |

#### Request

Der Anfrage-Body muss die Felder 'rating' und/oder 'description' enthalten, die aktualisiert werden sollen. Nur der Ersteller der Bewertung oder ein Admin kann diesen Endpunkt nutzen.
```json
{
    "rating":  5,
    "description":  "Noch besser als erwartet!"
}
```


#### Response
Die Antwort enthaelt die aktualisierten Details der Bewertung.
```json
{
    "id":  1,
    "business_user":  2,
    "reviewer":  3,
    "rating":  5,
    "description":  "Noch besser als erwartet!",
    "created_at":  "2023-10-30T10:00:00Z",
    "updated_at":  "2023-11-01T08:00:00Z"
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Erfolgreich aktualisiert. Die aktualisierte Bewertung wird zurueckgegeben. |
| `400` | Bad Request. Der Anfrage-Body enthaelt ungueltige Daten. |
| `401` | Unauthorized. Der Benutzer muss authentifiziert sein. |
| `403` | Forbidden. Der Benutzer ist nicht berechtigt, diese Bewertung zu bearbeiten. |
| `404` | Nicht gefunden. Es wurde keine Bewertung mit der angegebenen ID gefunden. |

### DELETE /api/reviews/{id}/

- [ ] `DELETE /api/reviews/{id}/` implementiert
- Beschreibung: Loescht eine spezifische Bewertung. Nur der Ersteller der Bewertung koennen diese Aktion ausfuehren.
- Permissions: Nur der Ersteller der Bewertung darf diese Aktion durchfuehren.

#### URL Parameters

| Name | Type | Description |
| --- | --- | --- |
| `id` | integer | Die ID der spezifischen Bewertung, die geloescht werden soll. |

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Die Antwort bestaetigt, dass die Bewertung erfolgreich geloescht wurde.
#### Status Codes

| Code | Description |
| --- | --- |
| `204` | Erfolgreich geloescht. Es wird kein Inhalt zurueckgegeben. |
| `401` | Unauthorized. Der Benutzer muss authentifiziert sein. |
| `403` | Forbidden. Der Benutzer ist nicht berechtigt, diese Bewertung zu loeschen. |
| `404` | Nicht gefunden. Es wurde keine Bewertung mit der angegebenen ID gefunden. |

## Uebergreifende Endpoints

Alle Endpoints die uebergreifende z.B. aggregierende Funktionene haben

### GET /api/base-info/

- [ ] `GET /api/base-info/` implementiert
- Beschreibung: Ruft allgemeine Basisinformationen zur Plattform ab, einschliesslich der Anzahl der Bewertungen, des durchschnittlichen Bewertungsergebnisses, der Anzahl der Geschaeftsnutzer und der Anzahl der Angebote.
- Permissions: Keine speziellen Permissions angegeben.
- Extra Information: Die durchschnittliche Bewertung ('average_rating') basiert auf allen abgegebenen Bewertungen und ist auf eine Dezimalstelle gerundet

#### Request

Es wird kein Anfrage-Body benoetigt.

#### Response
Die Antwort enthaelt statistische Informationen ueber die Plattform.
```json
{
    "review_count":  10,
    "average_rating":  4.6,
    "business_profile_count":  45,
    "offer_count":  150
}
```

#### Status Codes

| Code | Description |
| --- | --- |
| `200` | Die Basisinformationen wurden erfolgreich abgerufen. |
| `500` | Interner Serverfehler. |

