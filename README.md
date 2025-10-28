# 🔋 Video Battery Manager

Sistema di gestione batterie video con QR code - Traccia lo stato di carica delle tue batterie tramite scansione QR

![Battery Manager](https://img.shields.io/badge/versione-1.0-blue)
![Lingua](https://img.shields.io/badge/lingua-italiano-green)

## 📱 Cosa fa questo progetto?

Questo sistema ti permette di:
- 📊 Monitorare la percentuale di carica delle tue batterie
- 🔍 Scansionare QR codes per vedere istantaneamente lo stato
- 📝 Aggiornare facilmente i dati tramite Google Sheets
- 🖨️ Stampare QR codes da applicare sulle batterie
- 📱 Visualizzare tutto da smartphone o tablet

## 🚀 Setup Veloce (5 minuti)

### Passo 1: Attiva GitHub Pages

1. Vai su **Settings** del repository
2. Clicca su **Pages** nel menu laterale
3. In "Source" seleziona **main** branch
4. Clicca **Save**
5. Aspetta 2-3 minuti e il tuo sito sarà disponibile su:
   ```
   https://eskar147.github.io/video-battery-manager
   ```

### Passo 2: Crea il Google Sheet

1. Vai su [Google Sheets](https://sheets.google.com)
2. Crea un nuovo foglio chiamato "Batterie Video"
3. Nella prima riga inserisci le intestazioni:
   ```
   | id       | name            | percentage |
   |----------|-----------------|------------|
   | BATT001  | Batteria Sony 1 | 85         |
   | BATT002  | Batteria Sony 2 | 60         |
   | BATT003  | V-Mount Grande  | 100        |
   | BATT004  | NP-F970 A       | 45         |
   | BATT005  | NP-F970 B       | 20         |
   ```
4. Vai su **Estensioni** → **Apps Script**
5. Cancella tutto e incolla questo codice:

```javascript
function doGet() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Foglio1');
  const data = sheet.getDataRange().getValues();
  const headers = data[0];
  const rows = data.slice(1);
  
  const json = rows.map(row => {
    let obj = {};
    headers.forEach((header, index) => {
      obj[header] = row[index];
    });
    return obj;
  });
  
  return ContentService.createTextOutput(JSON.stringify(json))
    .setMimeType(ContentService.MimeType.JSON);
}
```

6. Clicca su **Deploy** → **New deployment**
7. Tipo: **Web app**
8. Execute as: **Me**
9. Who has access: **Anyone**
10. Clicca **Deploy**
11. Copia l'URL che ti viene dato (tipo: `https://script.google.com/macros/s/ABC123.../exec`)

### Passo 3: Configura il sito

1. Apri il file `index.html` in questo repository
2. Clicca sulla matita per modificarlo
3. Trova questa riga (circa linea 163):
   ```javascript
   const SHEET_URL = 'YOUR_GOOGLE_SHEETS_URL_HERE';
   ```
4. Sostituisci con il tuo URL:
   ```javascript
   const SHEET_URL = 'https://script.google.com/macros/s/TUO_URL_QUI/exec';
   ```
5. Clicca **Commit changes**

### Passo 4: Genera i QR Codes

1. Apri nel browser: `https://eskar147.github.io/video-battery-manager/qr-generator.html`
2. Inserisci: `eskar147.github.io/video-battery-manager`
3. Numero batterie: `5` (o quante ne hai)
4. Clicca **Genera QR Codes**
5. Clicca **Stampa QR Codes**
6. Stampa e attacca i QR codes sulle batterie

## 🎯 Come usare

1. **Scansiona** il QR code sulla batteria con qualsiasi app di scansione
2. Si aprirà automaticamente la pagina con lo stato della batteria
3. Vedrai:
   - 🔋 Nome batteria
   - 📊 Percentuale di carica
   - 🎨 Barra colorata (verde/arancione/rosso)
   - ✅ Stato (carica/da ricaricare/urgente)

## 📝 Aggiornare le percentuali

1. Apri il tuo Google Sheet
2. Modifica la colonna **percentage** con il nuovo valore
3. Salva
4. La prossima scansione mostrerà il valore aggiornato!

## ➕ Aggiungere nuove batterie

### Nel Google Sheet:
Aggiungi una nuova riga:
```
| BATT006 | Nuova Batteria | 100 |
```

### Genera nuovo QR code:
1. Vai su `qr-generator.html`
2. Aumenta il numero di batterie
3. Rigenera e stampa

## 🔧 Struttura del progetto

```
video-battery-manager/
├── index.html           # Pagina principale visualizzazione batterie
├── qr-generator.html    # Generatore QR codes
└── README.md           # Questa guida
```

## 🎨 Personalizzazioni

### Cambiare i colori della barra
Modifica in `index.html`:
```css
.fill-high { background: linear-gradient(90deg, #10b981, #059669); }    /* Verde */
.fill-medium { background: linear-gradient(90deg, #f59e0b, #d97706); }  /* Arancione */
.fill-low { background: linear-gradient(90deg, #ef4444, #dc2626); }     /* Rosso */
```

### Cambiare le soglie percentuale
Modifica in `index.html`:
```javascript
function getBatteryLevel(percentage) {
    if (percentage >= 70) return 'high';      // Verde sopra 70%
    if (percentage >= 30) return 'medium';    // Arancione 30-70%
    return 'low';                             // Rosso sotto 30%
}
```

## ❓ Risoluzione Problemi

### Il sito non si carica
- Aspetta 5 minuti dopo aver attivato GitHub Pages
- Controlla che l'URL sia corretto: `https://eskar147.github.io/video-battery-manager`

### "Configurazione non completata"
- Hai inserito l'URL di Google Sheets in `index.html`?
- Hai fatto il commit delle modifiche?

### "Batteria non trovata"
- L'ID nel QR code corrisponde a quello nel Google Sheet?
- L'ID è scritto correttamente? (es: BATT001, non BATT1)

### I dati non si aggiornano
- Hai salvato il Google Sheet?
- Prova a ricaricare la pagina (F5)
- Controlla che lo script Apps Script sia deployato correttamente

## 📄 Licenza

MIT License - Usa liberamente!

## 🤝 Contributi

Miglioramenti e suggerimenti sono benvenuti! Apri una Issue o Pull Request.

---

Made with ❤️ for video professionals

**Versione:** 1.0  
**Ultimo aggiornamento:** Gennaio 2025
