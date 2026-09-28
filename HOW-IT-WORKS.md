# Come funziona la "special repository" del profilo GitHub

## 1. La regola del nome

GitHub ha una convenzione: se crei un repository **con lo stesso nome del tuo
username**, e ci metti dentro un `README.md`, quel file viene mostrato
automaticamente in cima alla tua pagina profilo.

- Username: `NotJenova`
- Repo: `NotJenova/NotJenova` ✅ (è la "special repo")
- File magico: `README.md` nella **root**, sul branch **default** (`main`)

Se il repo avesse un altro nome (es. `my-profile`) non funzionerebbe.
Se il `README.md` fosse in una sottocartella, non funzionerebbe.

## 2. Come si aggiorna

Non c'è nessun pannello "modifica profilo" da compilare: **modifichi il file e
fai push**.

```bash
cd NotJenova
# modifica README.md
git add README.md
git commit -m "docs: personalize profile readme"
git push
```

Dopo qualche secondo la pagina https://github.com/NotJenova si aggiorna.
Se non lo fa, è **cache**: aspetta 1–2 minuti o fai un hard refresh (Ctrl+F5).

## 3. Cosa può contenere

Il README è **Markdown**, quindi tutto quello che GitHub supporta:

| Elemento | Sintassi | Note |
|---|---|---|
| Titoli | `#`, `##` | standard |
| Badge | `![alt](https://img.shields.io/...)` | [shields.io](https://shields.io) genera l'URL per te |
| Card statistiche | `![...](https://github-readme-stats.vercel.app/api?...&username=NotJenova)` | servizio esterno gratuito ([repo](https://github.com/anuraghazra/github-readme-stats)) |
| Link | `[testo](url)` | |
| Emoji | `:rocket:` o 🚀 | |
| HTML | `<p align="center">`, `<img>` | GitHub sanifica: niente JS/CSS |

> ⚠️ Limite importante: **non puoi usare JavaScript o CSS custom**.
> Non puoi nemmeno caricare video/audio. Le "card animate" che vedi in giro
> sono **immagini** generate da servizi esterni via URL (come
> github-readme-stats o shields.io).

## 4. La parte che quasi tutti sbagliano: le immagini da repo privati

Se metti il `README.md` in un repo **pubblico**, va tutto liscio.
Ma i servizi esterni (stats, badges dinamici) leggono solo dati **pubblici**:
se i tuoi repo sono privati, le card mostreranno numeri bassi o vuoti.

Per contare anche i commit privati, con github-readme-stats si aggiunge il
parametro `&count_private=true` — ma resta comunque il fatto che il *servizio*
vede solo ciò che l'API pubblica espone.

## 5. Alternativa senza servizi esterni

Se non vuoi dipendere da `github-readme-stats.vercel.app` (che a volte è lento
o va in rate-limit), puoi:

- usare solo badge shields.io **statici** (nessuna chiamata alle API GitHub);
- generare le card con una **GitHub Action** schedulata che scrive i numeri
  dentro il README (es. `README.md` + cron giornaliero);
- fare tutto a mano: testo + emoji, zero dipendenze. Spesso è la scelta più
  elegante e la più veloce da caricare.

## 6. Cosa ho personalizzato in questa repo

1. `README.md` — riscritto con:
   - header e tagline
   - sezione "Tech I work with" con badge shields.io
   - due card statistiche di github-readme-stats
   - sezione "What I'm up to" + contatti
2. Questo file `HOW-IT-WORKS.md` — non appare sul profilo, è solo documentazione
   per te (GitHub mostra **solo** il `README.md`).

## 7. Prossimi passi possibili

- Sostituire il link generico con i tuoi profili reali (LinkedIn, X, email)
- Cambiare il tema delle card (`theme=tokyonight` → `dark`, `radical`, `gruvbox`...)
- Aggiungere una GitHub Action che aggiorna automaticamente una sezione
  "latest projects" pescando dai repo pinnati
- Aggiungere un `banner` in alto (immagine in `/assets` nella repo)
