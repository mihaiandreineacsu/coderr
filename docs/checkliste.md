# Checkliste Django/DRF-Projekte

Quelle: https://docs.google.com/document/d/1-gUz-skb24UTLAiY5Y-wYDB6GEYI4H9vnxATo-2QsOM/edit?tab=t.0#heading=h.oeodsmlvkdaa

Bitte erfuelle alle Punkte auf dieser Liste, bevor du das Projekt einreichst.

## Definition of Done

### 1. Allgemeines

#### Endpoints

- [ ] Alle Endpoints sind nach Dokumentation erstellt.
- [ ] Das Projekt erreicht bei Abgabe durch die PM-Tests eine Test-Coverage von mindestens 95%.

#### Clean Code/Dokumentation

- [ ] Eine Funktion/Methode hat maximal eine Aufgabe und maximal 14 Zeilen.
- [ ] Kein auskommentierter Code oder `print()`-Befehle verbleiben im Projekt.
- [ ] Der Code ist PEP8-konform.
- [ ] Der Code ist dokumentiert/kommentiert.

#### GitHub Repository

- [ ] Es existiert eine aussagekraeftige `README.md`, die mindestens alles zum Starten des Projektes beinhaltet.
- [ ] Saemtliche Besonderheiten sind in der `README.md` aufgefuehrt.
- [ ] Die `README.md` ist zwingend auf Englisch verfasst.
- [ ] Das Backend ist in einem eigenen Repository ohne Frontend hochgeladen.
- [ ] Es existiert eine vollstaendige `requirements.txt`.
- [ ] Die Datenbank wird niemals auf GitHub geladen.

### 2. Conventions

#### Projekt- & App-Struktur

- [ ] Das Projekt wird beim Starten `core` genannt.
- [ ] Der Ordner mit `settings.py`, `urls.py`, `wsgi.py` usw. heisst dadurch `core` und ist klar von den Apps unterscheidbar.
- [ ] Alle Apps erhalten ein sprechendes Praefix oder Suffix, z. B. `auth_app`, `kanban_app`.
- [ ] Jede App enthaelt zusaetzlich einen `api/`-Ordner, in dem sich `serializers.py`, `views.py`, `urls.py`, `permissions.py` usw. befinden.
- [ ] Die Admin-Umgebung ist nutzbar.

#### Models

- [ ] Models verwenden sprechende Klassennamen im PascalCase, z. B. `UserProfile`.
- [ ] Felder verwenden `snake_case`, z. B. `first_name`, `is_active`.
- [ ] Models verwenden fuer eine sinnvolle Darstellung die `__str__`-Methode.
- [ ] Bei Bedarf sind in den `Meta`-Optionen `verbose_name`, `verbose_name_plural` und `ordering` gesetzt.
- [ ] Es befindet sich keine Logik in Modellen.
- [ ] Model-Beziehungen sind sauber mit `related_name` und `on_delete` definiert.

Beispiel:

```python
user = models.ForeignKey(
    User,
    on_delete=models.CASCADE,
    related_name="projects",
)
```

#### Serializer

- [ ] Fuer CRUD-Serialisierungen wird `ModelSerializer` genutzt.
- [ ] Felder werden explizit angegeben, z. B. `fields = ["id", "title"]`, nicht mit `__all__`.
- [ ] Felder sind in der gewuenschten Reihenfolge benannt.
- [ ] Fuer extra Validierung eines Feldes wird z. B. `validate_title(self, value)` genutzt.
- [ ] Fuer zusammenhaengende Validierung wird `validate(self, attrs)` genutzt.

#### Views

- [ ] Fuer CRUD wird `ModelViewSet` verwendet.
- [ ] Fuer individuelle Endpunkte werden `APIView` oder `GenericAPIView` verwendet.
- [ ] `queryset` und `serializer_class` sind als Properties in der Klasse definiert.
- [ ] Fuer dynamische Querysets wird `get_queryset()` verwendet, z. B. User-spezifisch.
- [ ] Permissions sind klar mit `permission_classes = [...]` deklariert.

#### URLs

- [ ] API-Routen sind ressourcenorientiert und nicht aktionsbasiert.
- [ ] Jede App hat ihre eigene URL-Datei.
- [ ] Das Hauptprojekt `core` hat ein zentrales Routing, in dem alle URLs included werden.

Beispiel:

```text
/api/boards/42/
```

statt:

```text
/api/getProjectById/
```

#### Permissions & Auth

- [ ] Jede App hat eine eigene `permissions.py`, sofern noetig.
- [ ] Permissions werden logisch kombiniert, z. B. `IsAuthenticated & IsOwner`.
- [ ] Es gibt keine offenen Endpunkte ohne expliziten Grund oder Vorgabe.

### 3. Best Practices

#### Imports

- [ ] Importe sind gruppiert und sortiert.

Beispiel:

```python
# 1. Standardbibliothek
import os
from datetime import datetime

# 2. Drittanbieter (Third-party)
from django.db import models
from rest_framework import serializers

# 3. Lokale Importe (eigene Module)
from .models import Project
from .services.project_logic import create_project_with_tasks
```

#### Verantwortlichkeitsprinzip

- [ ] Models enthalten die Datenstruktur.
- [ ] Serializers uebernehmen Validierung und Transformation.
- [ ] Views enthalten API-Logik und Routing.
- [ ] Permissions regeln Zugriffskontrolle.

Beispiel-User-Story:

> Als Nutzer moechte ich mich registrieren koennen, damit ich ein persoenliches Konto erstellen kann.

#### HTTP-Statuscodes

- [ ] HTTP-Statuscodes werden korrekt verwendet.
- [ ] DRF-Standardverhalten wird nicht unnoetig ueberschrieben.
- [ ] Statuscodes sind getestet beziehungsweise werden beim Testen beachtet.

| Zweck | Statuscode |
| --- | --- |
| Objekt erfolgreich erstellt | `201 CREATED` |
| Kein Inhalt zurueckgegeben | `204 NO CONTENT` |
| Validierungsfehler | `400 BAD REQUEST` |
| Berechtigung fehlt | `403 FORBIDDEN` |
| Objekt nicht gefunden | `404 NOT FOUND` |

