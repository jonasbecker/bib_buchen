# 📋 Projektplan: Uni-Bib Platzbuchungssystem

**Ziel:** Entwicklung einer Webanwendung zum Anzeigen und Buchen von Bibliotheksplätzen.
**Technologie:** Spring Boot (Java), Thymeleaf (Frontend), H2 (Datenbank), CSS.

---

## 🏗 Phase 1: Setup & Konfiguration
*Das Fundament legen, bevor der erste Code geschrieben wird.*

- [ ] **Projekt erstellen**
    - [ ] Type: **Maven**
    - [ ] JDK/Java: **17** oder **21**
    - [ ] Dependencies auswählen:
        - `Spring Web` (Server)
        - `Thymeleaf` (Template Engine für HTML)
        - `Spring Data JPA` (Datenbank-Interaktion)
        - `H2 Database` (In-Memory Datenbank)
        - `Spring Boot DevTools` (Automatischer Reload)
- [ ] **Verzeichnisstruktur prüfen**
    - Sicherstellen, dass `src/main/resources/templates` (für HTML) und `src/main/resources/static` (für CSS/Bilder) existieren.
- [ ] **Datenbank-Konsole aktivieren**
    - In der Datei `src/main/resources/application.properties` die Zeile einfügen: `spring.h2.console.enabled=true`.
    - (Damit kannst du später unter `/h2-console` im Browser deine Tabellen prüfen).

---

## 🗂 Phase 2: Das Datenmodell (Model)
*Definieren, wie ein "Platz" in der Datenbank aussieht.*

- [ ] **Entity-Klasse erstellen (`Seat.java`)**
    - [ ] Klasse im Haupt-Package (oder Unterordner `model`) anlegen.
    - [ ] Mit `@Entity` annotieren.
    - [ ] Attribute hinzufügen:
        - `Long id` (mit `@Id` und `@GeneratedValue`)
        - `String seatNumber` (z.B. "A1", "B4")
        - `boolean isOccupied` (frei/besetzt)
    - [ ] Getter und Setter Methoden generieren.

---

## 💾 Phase 3: Der Datenbank-Zugriff (Repository)
*Die Schnittstelle zur Datenbank schaffen.*

- [ ] **Repository-Interface erstellen (`SeatRepository.java`)**
    - [ ] Interface anlegen (z.B. im Unterordner `repository`).
    - [ ] Das Interface muss `JpaRepository<Seat, Long>` extenden.
    - [ ] *Kein weiterer Code nötig – Spring übernimmt die SQL-Befehle automatisch.*

---

## ⚙️ Phase 4: Die Geschäftslogik (Service)
*Die Logik zwischen Datenbank und Webseite.*

- [ ] **Service-Klasse erstellen (`SeatService.java`)**
    - [ ] Klasse anlegen und mit `@Service` annotieren.
    - [ ] Das `SeatRepository` per Constructor-Injection einbinden.
- [ ] **Methoden implementieren**
    - [ ] `findAllSeats()`: Gibt eine Liste aller Plätze zurück.
    - [ ] `toggleSeatStatus(Long id)`: Lädt einen Platz per ID, kehrt den `isOccupied` Wert um und speichert ihn wieder mit `save()`.

---

## 🚦 Phase 5: Der Web-Controller (Controller)
*Die Steuerung der Anfragen (Requests).*

- [ ] **Controller-Klasse erstellen (`SeatController.java`)**
    - [ ] Klasse anlegen und mit `@Controller` annotieren.
    - [ ] Den `SeatService` einbinden.
- [ ] **GET-Route für die Startseite (`/`)**
    - [ ] Methode mit `@GetMapping("/")` erstellen.
    - [ ] Liste der Plätze vom Service holen.
    - [ ] Liste dem `Model` hinzufügen (`model.addAttribute("seats", liste)`).
    - [ ] Rückgabewert: `"index"` (Name der HTML-Datei).
- [ ] **POST-Route zum Buchen (`/book/{id}`)**
    - [ ] Methode mit `@PostMapping("/book/{id}")` erstellen.
    - [ ] ID aus dem Pfad auslesen (`@PathVariable`).
    - [ ] Service-Methode zum Ändern des Status aufrufen.
    - [ ] Rückgabewert: `"redirect:/"` (Seite neu laden).

---

## 🖥 Phase 6: Das Frontend (HTML & Thymeleaf)
*Die Benutzeroberfläche bauen.*

- [ ] **HTML-Datei erstellen (`index.html`)**
    - [ ] Datei in `src/main/resources/templates/` anlegen.
    - [ ] Thymeleaf Namespace im `<html>` Tag hinzufügen (`xmlns:th="http://www.thymeleaf.org"`).
- [ ] **Liste der Plätze anzeigen**
    - [ ] Einen Container (z.B. `<div>` oder `<ul>`) erstellen.
    - [ ] `th:each` nutzen, um über die "seats" aus dem Model zu iterieren.
- [ ] **Logik für die Anzeige**
    - [ ] `th:text`: Platznummer anzeigen.
    - [ ] `th:classappend`: CSS-Klasse dynamisch setzen (z.B. "occupied" wenn besetzt).
- [ ] **Buchen-Funktion**
    - [ ] Kleines `<form>` Element für jeden Platz erstellen.
    - [ ] `th:action` nutzen, um an `/book/{id}` zu senden.
    - [ ] Button beschriften.

---

## 🎨 Phase 7: Design (CSS)
*Das Ganze hübsch machen.*

- [ ] **CSS-Datei erstellen (`style.css`)**
    - [ ] Datei in `src/main/resources/static/css/` anlegen.
- [ ] **CSS einbinden**
    - [ ] Im `<head>` der `index.html` verlinken.
- [ ] **Layout gestalten**
    - [ ] Flexbox oder CSS Grid nutzen, um die Plätze quadratisch anzuordnen.
    - [ ] Farben definieren: Grün für frei, Rot für besetzt.

---

## 🚀 Phase 8: Testdaten & Start
*Damit die Bib nicht leer ist.*

- [ ] **DataLoader erstellen (Optional)**
    - [ ] Klasse erstellen, die `CommandLineRunner` implementiert.
    - [ ] Mit `@Component` annotieren.
    - [ ] In der `run`-Methode Dummy-Plätze erstellen und speichern.
- [ ] **Anwendung starten**
    - [ ] `BibliothekApplication` (Main-Klasse) ausführen.
    - [ ] Browser öffnen: `http://localhost:8080`.
yooo