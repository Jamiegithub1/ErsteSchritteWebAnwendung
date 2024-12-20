
# User-Manager REST API – Architektur & Konzept

---

## 1. Zweck der REST API im User-Manager

Die **User-Manager REST API** ermöglicht es, User zu verwalten, indem sie Funktionen wie **Erstellen, Lesen, Aktualisieren und Löschen (CRUD)** bereitstellt. Sie dient als Schnittstelle zwischen dem Backend der Anwendung und Clients wie dem Frontend. User können registriert, ihre Daten abgerufen und aktualisiert werden. Zudem bietet die API Authentifizierungsfunktionen, um Usersitzungen zu verwalten. Durch die Nutzung von standardisierten **HTTP-Methoden** (GET, POST, PUT, DELETE) wird eine flexible und ...

---

## 2. API-Endpunkte

### 2.1 User-Endpunkte

| **HTTP-Methode** | **Endpunkt**           | **Beschreibung**                             | **Parameter**                              |
|------------------|------------------------|----------------------------------------------|--------------------------------------------|
| GET              | /api/users            | Alle User abrufen                        | -                                          |
| GET              | /api/users/{id}       | User anhand der ID abrufen              | id (Pfadvariable)                          |
| POST             | /api/users            | Neuen User erstellen                     | User-Objekt im Request-Body            |
| PUT              | /api/users/{id}       | Bestehenden User aktualisieren          | id (Pfadvariable), User-Objekt         |
| DELETE           | /api/users/{id}       | User löschen                             | id (Pfadvariable)                          |
| POST             | /api/users/login      | User einloggen                           | username, password (Request-Body)          |
| POST             | /api/users/logout     | User ausloggen                           | -                                          |

---

## 3. Aufbau der Klassen für User

### 3.1 User (Model-Klasse)

**Beschreibung:**
Diese Klasse repräsentiert einen User.

**Attribute:**
- id: Eindeutige ID des Users (Typ: Long).
- username: Benutzername des Users (Typ: String).
- password: Passwort des Users (Typ: String).
- email: E-Mail-Adresse des Users (Typ: String).
- roles: Rollen des Users (Typ: Liste von Strings, z. B. "ADMIN", "USER").

### 3.2 UserRepository (Speicherschicht)

**Beschreibung:**
Diese Klasse speichert und verwaltet alle User mithilfe einer **HashMap**.

**Attribute:**
- users: Eine **HashMap**, die alle User speichert (Schlüssel: ID, Wert: User).
- currentId: Ein Zähler zur automatischen Generierung neuer IDs.

**Methoden:**
- findAll(): Gibt eine Liste aller gespeicherten User zurück.
- findById(Long id): Findet einen User anhand seiner ID.
- save(User user): Speichert einen neuen User in der HashMap.
- update(Long id, User updatedUser): Aktualisiert einen vorhandenen User.
- delete(Long id): Löscht einen User anhand der ID.

### 3.3 UserService (Geschäftslogik)

**Beschreibung:**
Diese Klasse enthält die Geschäftslogik für die Verwaltung von Usern.

**Methoden:**
- getAllUsers(): Gibt eine Liste aller User zurück.
- getUserById(Long id): Ruft einen User basierend auf seiner ID ab.
- createUser(User user): Erstellt einen neuen User und speichert ihn.
- updateUser(Long id, User updatedUser): Aktualisiert einen bestehenden User.
- deleteUser(Long id): Löscht einen User basierend auf seiner ID.
- loginUser(String username, String password): Überprüft die Anmeldedaten und erstellt eine Sitzung.
- logoutUser(Long id): Beendet die Sitzung eines Users.

### 3.4 UserController (REST Controller)

**Beschreibung:**
Diese Klasse definiert die **REST API-Endpunkte** für die Userverwaltung.

**Endpunkte und zugehörige Methoden:**
- **GET /api/users**: Ruft alle User ab.
- **GET /api/users/{id}**: Ruft die Details eines bestimmten Users anhand seiner ID ab.
- **POST /api/users**: Erstellt einen neuen User.
- **PUT /api/users/{id}**: Aktualisiert einen bestehenden User.
- **DELETE /api/users/{id}**: Löscht einen User basierend auf seiner ID.
- **POST /api/users/login**: Meldet einen User an.
- **POST /api/users/logout**: Meldet einen User ab.

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
