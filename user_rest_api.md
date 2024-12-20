
# Benutzer-Manager REST API – Architektur & Konzept

---

## 1. Zweck der REST API im Benutzer-Manager

Die **Benutzer-Manager REST API** ermöglicht es, Benutzer zu verwalten, indem sie Funktionen wie **Erstellen, Lesen, Aktualisieren und Löschen (CRUD)** bereitstellt. Sie dient als Schnittstelle zwischen dem Backend der Anwendung und Clients wie dem Frontend. Benutzer können registriert, ihre Daten abgerufen und aktualisiert werden. Zudem bietet die API Authentifizierungsfunktionen, um Benutzersitzungen zu verwalten. Durch die Nutzung von standardisierten **HTTP-Methoden** (GET, POST, PUT, DELETE) wird eine flexible und effiziente Datenkommunikation im **JSON-Format** gewährleistet.

---

## 2. API-Endpunkte

### 2.1 Benutzer-Endpunkte

| **HTTP-Methode** | **Endpunkt**           | **Beschreibung**                             | **Parameter**                              |
|------------------|------------------------|----------------------------------------------|--------------------------------------------|
| GET              | /api/users            | Alle Benutzer abrufen                        | -                                          |
| GET              | /api/users/{id}       | Benutzer anhand der ID abrufen              | id (Pfadvariable)                          |
| POST             | /api/users            | Neuen Benutzer erstellen                     | Benutzer-Objekt im Request-Body            |
| PUT              | /api/users/{id}       | Bestehenden Benutzer aktualisieren          | id (Pfadvariable), Benutzer-Objekt         |
| DELETE           | /api/users/{id}       | Benutzer löschen                             | id (Pfadvariable)                          |
| POST             | /api/users/login      | Benutzer einloggen                           | username, password (Request-Body)          |
| POST             | /api/users/logout     | Benutzer ausloggen                           | -                                          |

---

## 3. Aufbau der Klassen für Benutzer

### 3.1 User (Model-Klasse)

**Beschreibung:**
Diese Klasse repräsentiert einen Benutzer.

**Attribute:**
- id: Eindeutige ID des Benutzers (Typ: Long).
- username: Benutzername des Benutzers (Typ: String).
- password: Passwort des Benutzers (Typ: String).
- email: E-Mail-Adresse des Benutzers (Typ: String).
- roles: Rollen des Benutzers (Typ: Liste von Strings, z. B. "ADMIN", "USER").

### 3.2 UserRepository (Speicherschicht)

**Beschreibung:**
Diese Klasse speichert und verwaltet alle Benutzer mithilfe einer **HashMap**.

**Attribute:**
- users: Eine **HashMap**, die alle Benutzer speichert (Schlüssel: ID, Wert: Benutzer).
- currentId: Ein Zähler zur automatischen Generierung neuer IDs.

**Methoden:**
- findAll(): Gibt eine Liste aller gespeicherten Benutzer zurück.
- findById(Long id): Findet einen Benutzer anhand seiner ID.
- save(User user): Speichert einen neuen Benutzer in der HashMap.
- update(Long id, User updatedUser): Aktualisiert einen vorhandenen Benutzer.
- delete(Long id): Löscht einen Benutzer anhand der ID.

### 3.3 UserService (Geschäftslogik)

**Beschreibung:**
Diese Klasse enthält die Geschäftslogik für die Verwaltung von Benutzern.

**Methoden:**
- getAllUsers(): Gibt eine Liste aller Benutzer zurück.
- getUserById(Long id): Ruft einen Benutzer basierend auf seiner ID ab.
- createUser(User user): Erstellt einen neuen Benutzer und speichert ihn.
- updateUser(Long id, User updatedUser): Aktualisiert einen bestehenden Benutzer.
- deleteUser(Long id): Löscht einen Benutzer basierend auf seiner ID.
- loginUser(String username, String password): Überprüft die Anmeldedaten und erstellt eine Sitzung.
- logoutUser(Long id): Beendet die Sitzung eines Benutzers.

### 3.4 UserController (REST Controller)

**Beschreibung:**
Diese Klasse definiert die **REST API-Endpunkte** für die Benutzerverwaltung.

**Endpunkte und zugehörige Methoden:**
- **GET /api/users**: Ruft alle Benutzer ab.
- **GET /api/users/{id}**: Ruft die Details eines bestimmten Benutzers anhand seiner ID ab.
- **POST /api/users**: Erstellt einen neuen Benutzer.
- **PUT /api/users/{id}**: Aktualisiert einen bestehenden Benutzer.
- **DELETE /api/users/{id}**: Löscht einen Benutzer basierend auf seiner ID.
- **POST /api/users/login**: Meldet einen Benutzer an.
- **POST /api/users/logout**: Meldet einen Benutzer ab.

---

## 4. HTTP-Statuscodes Übersicht

| **Statuscode**   | **Bedeutung**                     |
|------------------|-----------------------------------|
| 200 OK           | Anfrage war erfolgreich.          |
| 201 Created      | Neue Ressource wurde erstellt.    |
| 204 No Content   | Erfolgreich, aber keine Rückgabe. |
| 400 Bad Request  | Ungültige Eingabe.                |
| 401 Unauthorized | Authentifizierung fehlgeschlagen. |
| 404 Not Found    | Ressource nicht gefunden.         |
| 500 Internal Server Error | Serverfehler aufgetreten. |

---

## 5. Erweiterungen und Überlegungen

1. **Sicherheit:** Passwörter sollten niemals im Klartext gespeichert werden; stattdessen sollte ein Hashing-Algorithmus wie bcrypt verwendet werden.
2. **Session-Management:** Authentifizierung sollte über sichere Tokens (z. B. JWT) abgewickelt werden.
3. **Skalierbarkeit:** Die API sollte so entworfen werden, dass sie leicht in eine Microservice-Architektur integriert werden kann.
4. **Internationalisierung:** Fehler- und Erfolgsmeldungen könnten mehrsprachig gestaltet werden.

---
