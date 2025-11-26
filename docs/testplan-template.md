# Testiplaani mall

# Testiplaan – Task 2: Projekti seadistus ja avalikud API-d

**Projekti nimi:** API kvaliteedikontroll   
**Autorid ja kuupäev:** Daria, 24.11.2025  
**Versioon:** 1.0

---

## 1. Sissejuhatus
**Eesmärk** – testida FastAPI backendit, mis ühendab JSONPlaceholderi ja Rick & Morty avalike API-de andmed üheks ennustatavaks skeemiks.  
**Testimise kontekst:** veenduda, et `/api/koond` tagastab korrektse JSONi ja töötleb korrektselt väliste API-de vigu (HTTP 502).

---

## 2. Ulatus
**Kaasatud komponendid:**
- Backend FastAPI (`backend/main.py`)
- Avalikud API-d: JSONPlaceholder ja Rick & Morty
- Endpoints: `/status` ja `/api/koond`
- Logimine ja error handling

---

## 3. Nõuded ja aktsepteerimiskriteeriumid
| Nõue | Testimeetod | Oodatav tulemus |
|-------|-------------|----------------|
| `/status` tagastab `200 OK` | GET /status | {"olek": "aktiivne", "allikas": "Kvaliteedijälg API"} |
| `/api/koond` koondab mõlema API andmed | GET /api/koond | JSON sisaldab `postitus`, `tegelane`, `allikad`, `paastikuAeg` |
| Logimine | vaata serveri logisid | INFO logid väliste API päringute ja koondamise kohta |
| Viga välises API-s | muuda URL katkise või eksisteerimata | HTTP 502 koos `detail`: sonum, siht, pohjus |

---

## 4. Riskid ja maandus
| Risk | Mõju | Tõenäosus | Maandus |
|------|------|------------|---------|
| Välise API kättesaamatus | Kõik koondatud andmed puuduvad | Keskmine | Tagada error handling 502 |
| Vale JSON skeem | Testid läbivad ebaõigesti | Madal | Kontrollida kõiki väljastatud välju |

---

## 5. Meetodid ja tööriistad
- Ühikutestid: FastAPI endpoints
- Integratsioonitestid: `/api/koond` väliste API-dega
- Automaatika: Python `pytest`, HTTP päringud käsitsi `curl` või `http`

---

## 6. Testkeskkonnad ja andmed
- OS: Windows 11
- Python: 3.13
- Testandmed: avalikud API-d (`JSONPlaceholder`, `Rick & Morty`)
- Mock-serverid võivad kasutada vigade simuleerimiseks katkiseid URL-e

---

## 7. Ajajoon ja vastutajad
| Tegevus | Vastutaja | Aeg |
|----------|-----------|-----|
| Venv loomine ja sõltuvuste paigaldus | Daria | 5 min |
| Serveri käivitamine ja `/status` testimine | Daria | 2 min |
| `/api/koond` korrektse skeemi test | Daria | 2 min |
| Vigade käsitlemise test | Daria | 10 min |

---

## 8. Raporteerimine
- Testitulemused salvestatakse: `docs/results/task2.md` ja `backend/README.md`
- Edukriteerium: kõik kriitilised testid (`/api/koond` ja 502 error handling) läbivad


# Testplaan – GA4 

## 1. Sissejuhatus
**Projekti nimi:** Kvaliteedijälg – A/B katse  

**Eesmärk:** Kontrollige, et A/B-testi sündmused (`variant_vaade` ja `variant_vahetus`) saadetakse korrektselt Google Analyticsisse.

---

## 2. Ulatus
**Kaasatud moodulid:**  
- Frontend (`index.html`, `app.js`, `ab.js`)  
- Backend (`GET /api/koond`)  
- GA4 teenus ja DebugView  

---

## 3. Nõuded ja aktsepteerimiskriteeriumid

**Funktsionaalsed nõuded:**  
1. GA4 skript on lisatud faili `index.html` ja kasutab identifikaatorit `G-XXXXTEST`.    
2. Sündmused `variant_vaade` ja `variant_vahetus` kutsutakse välja iga kord, kui kuvatakse paigutus ja vahetatakse varianti.   
3. Network-paneelis on näha päringud GA4 serverile (`collect`).   


| Kontrollpunkt | Kriteerium | 
|----------------|------------|
| GA4 skript on olemas | `index.html` sisaldab `<script src="https://www.googletagmanager.com/gtag/js?...">` |
| `variant_vaade` sündmus | Event saadetakse Network → collect, sisaldab `variant`, `sessioonId`, `katsestaadium` |
| `variant_vahetus` sündmus | Event saadetakse pärast nuppu "Vaheta varianti" vajutamist | ✅ |
| Ekraanipildid | Võrgupäringute ja console.log’i kuvatud sündmused on salvestatud `docs/results/analytics/` |

---

## 4. Riskid ja maandus
| Kirjeldus | Mõju | Tõenäosus | Maandus |
|-----------|-------|------------|---------|
| GA4 päringud ei lähe läbi | Testi ebaõnnestumine | Keskmine | Kasutada debug_mode ja mock GA4 tunnust |
| Backend `/api/koond` ei tööta | Variantide info ei laaditu | Kõrge | Kontrolli backendi käivitust enne testi |
| Brauseri CORS / cookie hoiatused | Mõned sündmused ei lähe läbi | Keskmine | Testida localhost, vajadusel lubada CORS |

---

## 5. Meetodid ja tööriistad
- **Testimise liigid:** A/B testide sündmuste kontroll, integratsioon frontend+backend, võrgu päringud  
- **Automaatika vahendid:** DevTools, console.log, Python HTTP server (`python -m http.server`), GitHub Actions (CI)  

---

## 6. Testkeskkonnad ja andmed
**Konfiguratsioonid:**  
- Python: 3.10+ (HTTP server)  
- Node.js / npm: vajadusel frontend build  
- Browser: Chrome / Firefox (DevTools Network)  

**Testandmete allikad:**  
- Mock GA4 tunnus: `G-XXXXTEST`  
- API sandbox: `GET /api/koond`  

---

## 7. Ajajoon ja vastutajad
| Tegevus | Vastutaja | Aeg |
|----------|---------------|-------|
| Sündmuste testimine (`variant_vaade`, `variant_vahetus`) | Daria | 10 мин |
| Network-paneeli ja DebugView kontrollimine | Daria | 5 мин |
| Ekraanipiltide kogumine ja aruande koostamine | Daria | 5 мин |

---

## 8. Raporteerimine
- **Aruannete formaadid:** `docs/results/analytics/`
- **Edukriteeriumid:**  
  - Kõik sündmused (`variant_vaade` ja `variant_vahetus`) ilmuvad Network → collect  
  - Ekraanipildid on salvestatud kausta `docs/results/analytics/`  
  - Console.log näitab õiged event objektid  

---

