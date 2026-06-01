# 🍻 FantAmici

> Il fantagioco che trasforma le serate tra amici in un campionato.

FantAmici è una web app ispirata al FantaSanremo: crei una squadra di amici, l'host assegna **bonus** e **malus** in base a quanto ognuno si lancia nelle sfide della serata, e due classifiche (giocatori e squadre) decretano chi è il più *accollativo* del gruppo.

🔗 **Demo live:** _aggiungi qui il link dopo il deploy (es. https://fantamici.netlify.app)_

![Stato](https://img.shields.io/badge/stato-pronto-22B473) ![Licenza](https://img.shields.io/badge/licenza-Proprietaria-FF5A6E)

---

## ✨ Funzionalità

- **Leghe con codice di invito** — l'host crea la lega e condivide un codice a 6 cifre (o un link diretto).
- **Creazione squadra** — ogni giocatore sceglie il proprio personaggio e compone una rosa di 5 amici con un budget di 12 punti, senza poter scegliere sé stesso.
- **40 bonus & malus situazionali** — pensati per le serate vere, con regole speciali (un bonus a serata, condizioni su "offre da bere", ritardi a fasce).
- **Due classifiche** — singolo giocatore e squadra, con dettaglio cliccabile (storico punti per persona, componenti per squadra).
- **Aggiornamenti in tempo reale** — le classifiche si aggiornano da sole quando l'host assegna i punti (Supabase Realtime, con fallback automatico a polling).
- **Recensioni & segnalazioni** — gli utenti lasciano feedback e segnalano bug direttamente dalla home.
- **Modalità chiara/scura** — tema "villaggio turistico" di giorno, "tramonto tropicale" di notte.
- **Monetizzazione** — versione *Plus* con regole personalizzate (verifica del pagamento via licenza), supporto con un caffè e contatto per collaborazioni con i locali.
- **URL puliti e condivisibili** — SPA con routing reale (`/lega/ABC123`), back/forward e refresh funzionanti.

---

## 🛠️ Stack tecnico

- **Frontend:** HTML, CSS e JavaScript vanilla, senza framework né build step — un singolo `index.html` autosufficiente.
- **Routing:** Single Page App basata su History API, con fallback automatico ad hash routing per gli ambienti senza server.
- **Persistenza:** `localStorage` per la modalità offline, **Supabase** (PostgreSQL + Realtime) per la modalità online multi-dispositivo.
- **Pagamenti Plus:** verifica licenza lato client tramite Gumroad (nessun server richiesto).
- **Deploy:** sito statico, ottimizzato per Netlify (incluse configurazione SPA e header).

> Scelta progettuale: zero dipendenze di build. Tutto gira aprendo il file nel browser; il cloud è opzionale e si attiva inserendo le chiavi in cima allo script.

---

## 🚀 Come provarlo in locale

Nessuna installazione. Apri direttamente `index.html` nel browser, oppure servi la cartella con un server statico:

```bash
# con Python
python3 -m http.server 8000
# poi apri http://localhost:8000
```

In locale i dati restano nel browser. Per il funzionamento online e multi-dispositivo, segui la guida.

---

## 📦 Messa online

La guida passo-passo è in **[SETUP.md](./SETUP.md)** e copre:

1. Personalizzazione (PayPal, email, prezzo).
2. Database online con **Supabase** (script in [`supabase-schema.sql`](./supabase-schema.sql)).
3. *Plus* a pagamento con verifica reale tramite **Gumroad**.
4. Pubblicazione su **Netlify** con URL puliti.

---

## 📁 Struttura del repository

```
.
├── index.html            # L'intera applicazione (frontend + logica)
├── supabase-schema.sql   # Schema del database + permessi + realtime
├── netlify.toml          # Config Netlify (SPA + header)
├── _redirects            # Fallback SPA per Netlify
├── SETUP.md              # Guida completa alla configurazione e al deploy
└── README.md             # Questo file
```

---

## 🔒 Note sulla sicurezza

FantAmici è pensato per gruppi di amici: la protezione di una lega è il suo codice segreto e la password dell'host. È perfetto per il divertimento, ma non è pensato per dati sensibili. Le chiavi inserite nel codice sono la *anon public key* di Supabase (pensata per essere pubblica) e l'ID prodotto Gumroad: entrambe sono progettate per stare in un client pubblico. Il valore difendibile del progetto — gli incassi del Plus, il conto PayPal e il database — resta nei tuoi account privati, mai nel codice.

---

## 📄 Licenza

**Tutti i diritti riservati.** Questo progetto è di proprietà esclusiva di **Giuseppe Valguarnera**. Il codice è pubblicamente visibile solo a scopo di portfolio e dimostrazione: non è concesso copiarlo, riutilizzarlo, modificarlo o ridistribuirlo senza permesso scritto. Vedi il file [LICENSE](./LICENSE) per i dettagli.

---

Fatto con 🍻 da Giuseppe Valguarnera per rendere leggendaria ogni serata tra amici.
