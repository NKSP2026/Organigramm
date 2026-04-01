# Erweiterte Mitarbeiterverwaltung

Ja, das ist machbar.

Folgende Funktionen können in einer einzigen HTML-Datei umgesetzt werden:

- Passwortschutz für die gesamte Seite
- Separates Admin-Passwort
- Button zum Mitarbeiter hinzufügen
- Eigenes Eingabeformular statt vieler Prompt-Fenster
- Bildauswahl über Datei-Upload
- Vorschau des Bildes
- Speichern des Mitarbeiters als neue Karte
- Abbrechen-Button
- Mitarbeiter löschen mit Passwortschutz
- Detailansicht beim Anklicken

Wichtig:

Browser können aus Sicherheitsgründen keine Bilder automatisch in deinen GitHub-Ordner `bilder/` speichern.

Das bedeutet:

- Du kannst ein Bild lokal auswählen
- Das Bild wird direkt in der Seite angezeigt
- Es bleibt gespeichert solange die Seite geöffnet bleibt
- Für dauerhaftes Speichern auf GitHub müsstest du die HTML-Datei und die Bilder später manuell hochladen oder Firebase / Datenbank nutzen

## Kompletten HTML-Code herunterladen

Du kannst den bisherigen Code erweitern und diese neuen Bereiche hinzufügen:

### Neuer Button-Bereich

```html
<div class="top-bar">
  <button class="admin-btn" onclick="oeffneFormular()">+ Mitarbeiter hinzufügen</button>
</div>
```

### Formular Fenster

```html
<div class="modal" id="formularModal">
  <div class="modal-content">
    <span class="close" onclick="schliesseFormular()">&times;</span>

    <div class="details">
      <h2>Neuen Mitarbeiter anlegen</h2>

      <input type="text" id="formName" placeholder="Name"><br><br>
      <input type="text" id="formGeburt" placeholder="Geburtsdatum"><br><br>
      <input type="text" id="formAlter" placeholder="Alter"><br><br>
      <input type="text" id="formFunktion" placeholder="Funktion"><br><br>
      <input type="text" id="formSeit" placeholder="Angestellt seit"><br><br>
      <input type="text" id="formBeruf" placeholder="Beruf"><br><br>
      <input type="text" id="formAusbildung" placeholder="Ausbildung"><br><br>
      <input type="text" id="formSicherheit" placeholder="Sicherheitsdienst"><br><br>
      <input type="text" id="formErsthelfer" placeholder="Ersthelfer/in"><br><br>
      <input type="text" id="formBrandschutz" placeholder="Brandschutzhelfer/in"><br><br>
      <input type="text" id="formFuehrerschein" placeholder="Führerschein"><br><br>
      <input type="text" id="formChef" placeholder="Vorgesetzter"><br><br>

      <input type="file" id="formBild" accept="image/*"><br><br>

      <button class="admin-btn" onclick="speichereMitarbeiter()">Speichern</button>
      <button class="admin-btn" onclick="schliesseFormular()" style="background:#666;">Abbrechen</button>
    </div>
  </div>
</div>
```

### CSS zusätzlich

```css
input {
  width: 100%;
  padding: 12px;
  border-radius: 10px;
  border: 1px solid #444;
  background: #111;
  color: white;
}
```

### Script zusätzlich

```html
<script>
function oeffneFormular() {
  const adminPasswort = 'Admin2026';
  const passwort = prompt('Admin Passwort eingeben');

  if (passwort !== adminPasswort) {
    alert('Falsches Passwort');
    return;
  }

  document.getElementById('formularModal').style.display = 'flex';
}

function schliesseFormular() {
  document.getElementById('formularModal').style.display = 'none';
}

function speichereMitarbeiter() {
  const name = document.getElementById('formName').value;
  const geburt = document.getElementById('formGeburt').value;
  const alter = document.getElementById('formAlter').value;
  const funktion = document.getElementById('formFunktion').value;
  const seit = document.getElementById('formSeit').value;
  const beruf = document.getElementById('formBeruf').value;
  const ausbildung = document.getElementById('formAusbildung').value;
  const sicherheit = document.getElementById('formSicherheit').value;
  const ersthelfer = document.getElementById('formErsthelfer').value;
  const brandschutz = document.getElementById('formBrandschutz').value;
  const fuehrerschein = document.getElementById('formFuehrerschein').value;
  const chef = document.getElementById('formChef').value;

  const bildDatei = document.getElementById('formBild').files[0];

  if (!bildDatei) {
    alert('Bitte Bild auswählen');
    return;
  }

  const reader = new FileReader();

  reader.onload = function(e) {
    const bild = e.target.result;

    const neueKarte = document.createElement('div');
    neueKarte.className = 'card';

    neueKarte.innerHTML = `
      <img src="${bild}" alt="${name}">
      <div class="card-content">
        <h2>${name}</h2>
        <p>Geb. ${geburt}</p>
        <p>${funktion}</p>
        <span class="role-badge mitarbeiter">${funktion}</span>
      </div>
    `;

    neueKarte.setAttribute('onclick', `showDetails('${name}','${alter}','${funktion}','${seit}','${beruf}','${ausbildung}','${sicherheit}','${ersthelfer}','${brandschutz}','${fuehrerschein}','${chef}','${bild}')`);

    neueKarte.oncontextmenu = function(event) {
      event.preventDefault();

      const adminPasswort = 'Admin2026';
      const passwort = prompt('Admin Passwort zum Löschen eingeben');

      if (passwort === adminPasswort) {
        neueKarte.remove();
      } else {
        alert('Falsches Passwort');
      }
    };

    document.getElementById('mitarbeiterRow').appendChild(neueKarte);

    schliesseFormular();
  };

  reader.readAsDataURL(bildDatei);
}
</script>
```

## Mitarbeiter löschen

- Rechtsklick auf einen Mitarbeiter
- Admin Passwort eingeben
- Mitarbeiter wird gelöscht

## Hinweis

Für echtes dauerhaftes Speichern inklusive Bilder-Upload in GitHub brauchst du später entweder:

- Firebase
- Supabase
- eigene Datenbank
- GitHub API
- Node.js Backend

Für den Anfang funktioniert diese Version aber komplett lokal und direkt im Browser.

