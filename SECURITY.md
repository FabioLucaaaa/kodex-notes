# Security Policy — KODEX

Die Sicherheit, Integrität und Vertraulichkeit von Nutzerdaten haben bei KODEX höchste Priorität. KODEX verfolgt ein striktes **Local-First- und Zero-Knowledge-Prinzip**: Daten werden clientseitig verarbeitet, bei Aktivierung mittels Web Crypto API (AES-256-GCM, PBKDF2) verschlüsselt und standardmäßig offline gehalten.

Sicherheitsrelevante Schwachstellen — insbesondere in Bezug auf kryptografische Implementierungen, Eingabebereinigung (XSS) oder die WebSocket-/Relay-Synchronisation — nehmen wir sehr ernst.

---

## 1. Unterstützte Versionen

Sicherheitsupdates und Patches werden primär für den aktuellen Release-Zweig bereitgestellt.

| Version | Unterstützt | Status |
| :--- | :---: | :--- |
| `1.x.x` (Main / Latest) | :white_check_mark: | Aktiv gepflegt & unterstützt |
| `< 1.0.0` (Vorabversionen) | :x: | Nicht mehr unterstützt |

---

## 2. Sicherheitslücke melden (Responsible Disclosure)

**Bitte eröffne unter keinen Umständen ein öffentliches GitHub-Issue für Sicherheitslücken oder Exploits.**

Um Schwachstellen diskret und sicher zu melden, nutze bitte einen der folgenden Wege:

1. **GitHub Private Vulnerability Reporting (Bevorzugt):**  
   Navigiere im Repository auf den Reiter **Security** → **Advisories** → **Report a vulnerability**, um einen privaten Bericht direkt über GitHub zu übermitteln.
2. **Direkter Kontakt per E-Mail:**  
   Sende deinen Bericht an:  
   📧 **FabioLucaa@pm.me**  
   *(Betreff-Präfix: `[SECURITY] KODEX - <Kurzbeschreibung>`)*

### Was der Bericht enthalten sollte
Damit die Meldung schnell analysiert und verifiziert werden kann, füge bitte nach Möglichkeit Folgendes bei:
* **Beschreibung der Schwachstelle:** Wo liegt das Problem und welche Komponenten sind betroffen (z. B. Kryptografie/Key-Derivation, DOM-Injection/XSS, Sync-Protokoll, Datenbereinigung)?
* **Reproduktionsschritte (PoC):** Eine nachvollziehbare Schritt-für-Schritt-Anleitung oder ein minimaler Exploit/Code-Ausschnitt.
* **Auswirkung (Impact):** Welches Risiko besteht für Endnutzer (z. B. Entschlüsselung ohne Passwort, Ausführen von Schadcode, Datenverlust bei Merge)?
* **Betroffene Umgebung:** Getestete Browser (Chrome, Firefox, Safari), Betriebssystem und Versionsstand der HTML-Datei bzw. des App-Pakets.

---

## 3. Reaktionszeiten & Prozess

* **Eingangsbestätigung:** Innerhalb von **48 Stunden** nach Erhalt der Meldung bestätigen wir den Eingang deines Berichts.
* **Analyse & Einstufung:** Wir prüfen die Reproduzierbarkeit und bestimmen den Schweregrad (nach CVSS-Kriterien).
* **Behebung:** Wir entwickeln zeitnah einen Fix im privaten Entwicklungszweig.
* **Koordiniertes Release:** Nach Bereitstellung des Patches wird die Sicherheitslücke im Rahmen eines GitHub Security Advisories transparent dokumentiert und der Finder (auf Wunsch) namentlich gewürdigt.

Wir bitten darum, bis zum offiziellen Release des Patches keine Details zur Schwachstelle öffentlich zu teilen.

---

## 4. Sicherheitsarchitektur & Relevante Schwerpunkte

KODEX implementiert spezifische Schutzmechanismen, die bei Sicherheitsanalysen besonders beachtet werden sollten:

1. **Clientseitige Kryptografie:**  
   * Schlüsselableitung mittels **PBKDF2** (SHA-256, 150.000 Runden, kryptografisch zufälliger Salt).
   * Verschlüsselung über **AES-256-GCM** mit eindeutigen IVs (Initialisierungsvektoren) pro Speichervorgang.
   * Passwörter und abgeleitete Schlüssel werden niemals im Klartext persistiert, sondern verbleiben flüchtig im Arbeitsspeicher.
2. **Eingabebereinigung & XSS-Schutz:**  
   * Block-Inhalte werden als unformatierter Text verarbeitet und beim Rendern bereinigt (`strip()` / `esc()`).
   * HTML-Injection in custom DOM-Elementen oder Formelauswertungen (Safe Parser statt `eval()`) ist strikt untersagt.
3. **Synchronisation & Relais:**  
   * Verschlüsselte Tresor-Seiten dürfen unter keinen Umständen über BroadcastChannel oder WebSocket-Verbindungen übertragen werden.
   * Das Zusammenführen von Zuständen (`mergeStates`) muss deterministisch und manipulationssicher gegen unberechtigte Überschreibungen sein.

---

## 5. Abgrenzung (Out of Scope)

Die folgenden Szenarien gelten **nicht** als Sicherheitslücken von KODEX:
* **Kompromittierte Endgeräte:** Angriffe, die auf Malware, Keyloggern oder physischem Zugriff auf ein bereits entsperrtes Gerät basieren.
* **Vergessene Passwörter:** Das Unvermögen, Daten ohne den passenden Schlüssel wiederherzustellen (dies ist das beabsichtigte Verhalten unseres Zero-Knowledge-Ansatzes).
* **Schwache Passwörter:** Brute-Force-Erfolge gegen bewusst trivial gewählte Nutzerpasswörter (wobei die PBKDF2-Parametrisierung als Schutzfaktor dient).
* **Social Engineering:** Phishing- oder Täuschungsversuche gegenüber Nutzern.

---

Vielen Dank, dass du dazu beiträgst, KODEX für alle sicher und vertrauenswürdig zu halten!
