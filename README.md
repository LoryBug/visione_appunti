# 📚 Visione Artificiale - Appunti Corso Magistrale

Appunti interattivi e ben strutturati per il corso di **Visione Artificiale** della Magistrale in Informatica, Università di Bologna.

**Docente:** Prof. Annalisa Franco
**Anno Accademico:** 2024-2025

---

## 🚀 Guida Rapida

### Installazione Locale (Jekyll)

**Prerequisiti:**
- Ruby 2.7+
- Bundler

**Installazione:**

```bash
# Clona il repository
git clone https://github.com/username/visione-artificiale.git
cd visione-artificiale

# Installa le dipendenze
bundle install

# Esegui il server locale
bundle exec jekyll serve
```

Visita `http://localhost:4000/visione-artificiale` nel browser.

---

## 📁 Struttura del Progetto

```
visione-artificiale/
├── docs/
│   ├── index.md                    ← Homepage
│   ├── lezioni/
│   │   ├── index.md               ← Index lezioni
│   │   ├── 00-introduzione/
│   │   │   └── index.md           ← Lezione 00
│   │   ├── 01-sistemi-visione/
│   │   │   └── index.md           ← Lezione 01
│   │   └── ... (altre lezioni)
│   └── risorse/
│       └── index.md               ← Risorse e riferimenti
├── _config.yml                     ← Configurazione Jekyll
├── Gemfile                         ← Dipendenze Ruby
├── .gitignore
└── README.md                       ← Questo file
```

---

## 📝 Formato Lezioni

Ogni lezione è un file Markdown con front matter Jekyll:

```markdown
---
layout: default
title: Lezione 00 - Introduzione
parent: Lezioni
nav_order: 1
has_children: false
description: "Introduzione al corso..."
---

# 📚 Titolo Lezione

Contenuto della lezione...
```

### Supportati:
- ✅ Markdown standard
- ✅ Diagrammi Mermaid (con jekyll-mermaid)
- ✅ Tabelle GFM
- ✅ Code highlighting
- ✅ Formule LaTeX (con kramdown)

---

## 🌐 Deploy su GitHub Pages

### Passaggio 1: Setup Iniziale

```bash
# Inizializza git
git init
git add .
git commit -m "Initial commit: appunti Visione Artificiale"

# Aggiungi remote
git remote add origin https://github.com/username/visione-artificiale.git
git branch -M main
git push -u origin main
```

### Passaggio 2: Abilita GitHub Pages

1. Vai su **Settings** → **Pages**
2. Sotto **Source**, seleziona `main` branch
3. Sotto **Folder**, seleziona `/ (root)` oppure `/docs` (dipende dalla struttura)
4. Salva

### Passaggio 3: Accedi al Sito

Il sito sarà disponibile a:
```
https://username.github.io/visione-artificiale/
```

---

## 🎨 Tema: Just the Docs

Il sito usa il tema **[Just the Docs](https://just-the-docs.github.io/)** che offre:

- ✅ Dark mode di default
- ✅ Sidebar navigazione automatica
- ✅ Search integrato
- ✅ Responsive design
- ✅ Tabella dei contenuti per pagina
- ✅ Supporto Mermaid diagrams

### Personalizzazione

Modifica `_config.yml` per:
- Cambiare colore tema: `color_scheme: light` / `dark`
- Aggiungere link header
- Configurare search

---

## 📖 Come Contribuire

### Aggiungere una Nuova Lezione

1. Crea una cartella: `docs/lezioni/NN-titolo/`
2. Crea il file: `docs/lezioni/NN-titolo/index.md`
3. Aggiungi il front matter:

```markdown
---
layout: default
title: Lezione NN - Titolo
parent: Lezioni
nav_order: N
has_children: false
description: "Descrizione della lezione"
---
```

4. Scrivi il contenuto in Markdown
5. Commit e push

---

## 🔧 Comandi Utili

```bash
# Build locale
bundle exec jekyll build

# Serve locale (con auto-reload)
bundle exec jekyll serve

# Clean cache
bundle exec jekyll clean

# Aggiornare gemme
bundle update
```

---

## 📚 Risorse Utili

- [Jekyll Documentation](https://jekyllrb.com/)
- [Just the Docs Guide](https://just-the-docs.github.io/)
- [Markdown Guide](https://www.markdownguide.org/)
- [Mermaid Diagrams](https://mermaid.js.org/)

---

## 📋 Checklist Lezione

Quando aggiungi una nuova lezione, assicurati di includere:

- [ ] Front matter YAML corretto
- [ ] Titolo e numero lezione
- [ ] Overview dei concetti
- [ ] Almeno 1-2 diagrammi Mermaid
- [ ] Almeno 1 tabella o lista
- [ ] Glossario o flashcard in forma di tabella
- [ ] Almeno 3-5 domande Q&A tipiche d'esame
- [ ] Riferimenti bibliografici
- [ ] Link a lezioni precedenti/successive

---

## 📄 Licenza

Questi appunti sono forniti per scopi educativi all'interno del corso universitario.

---

## 👨‍💼 Contatti

- **Docente:** Prof. Annalisa Franco
- **Università:** Università di Bologna
- **Dipartimento:** Informatica
- **Sito BIOLAB:** http://biolab.csr.unibo.it/

---

**Last updated:** Marzo 2025

**Buono studio!** 🎓
