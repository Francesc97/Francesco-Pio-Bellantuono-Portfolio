# Sistema di Gestione Inventario - VendiCose SpA 🏪

Un sistema completo di database per la gestione dell'inventario multi-magazzino di una catena di negozi, con monitoraggio automatico delle giacenze e alert di restock.

## 🎯 Obiettivo del Progetto

Sviluppare un database relazionale per VendiCose SpA che gestisca:
- Inventario distribuito su più magazzini
- Vendite in tempo reale dei negozi collegati
- Monitoraggio automatico delle soglie di restock
- Aggiornamento dinamico delle giacenze

## 🏗️ Architettura del Database

### Schema ER
Il database è progettato seguendo i principi di normalizzazione per minimizzare la ridondanza:

**Entità Principali:**
- **Categoria**: Classificazione prodotti con soglie restock
- **Prodotto**: Catalogo articoli con prezzi
- **Magazzino**: Depositi distribuiti geograficamente
- **Negozio**: Punti vendita collegati ai magazzini
- **Giacenza**: Quantità prodotti per magazzino
- **Vendite**: Transazioni dei negozi
- **Dettaglio_Vendite**: Articoli venduti per transazione

### Relazioni Chiave
- Un magazzino serve più negozi (1:N)
- Un prodotto può essere in più magazzini (N:M via Giacenza)
- Una vendita può contenere più prodotti (1:N via Dettaglio_Vendite)

## ⚡ Funzionalità Implementate

### 1. Aggiornamento Automatico Inventario
```sql
-- Aggiorna giacenze dopo ogni vendita
UPDATE GIACENZA G
JOIN (SELECT * FROM ID) AS SUB
ON g.ProdottoID = sub.ProdottoID AND g.MagazzinoID = sub.MagazzinoID
SET g.Quantità_magazzino = g.Quantità_magazzino - sub.QuantitàVenduta;
```

### 2. Monitoraggio Giacenze per Magazzino
```sql
-- Vista completa giacenze prodotti
CREATE VIEW Anagrafica_prodotto AS (
    SELECT g.prodottoid, p.nomeprodotto, g.quantità_magazzino AS Giacenza_prodotto, 
           g.magazzinoid, m.nomemagazzino
    FROM giacenza g
    JOIN prodotto p ON g.prodottoid = p.prodottoid
    JOIN magazzino m ON g.magazzinoid = m.magazzinoid 
    ORDER BY prodottoid ASC
);
```

### 3. Sistema di Alert Restock
```sql
-- Monitoraggio soglie critiche
CREATE VIEW Restock AS (
    SELECT g.prodottoid, p.nomeprodotto, g.magazzinoid, m.nomemagazzino, 
           g.quantità_magazzino, c.sogliarestock, 
           IF((g.quantità_magazzino - c.sogliarestock) > 0, 'OK', 'RESTOCK') AS Alert_Restock
    FROM giacenza g
    JOIN prodotto p ON g.prodottoid = p.prodottoid          
    JOIN magazzino m ON g.magazzinoid = m.magazzinoid
    JOIN categoria c ON p.categoriaid = c.categoriaid
    ORDER BY Alert_Restock DESC
);
```

## 🛠️ Tecnologie Utilizzate

- **MySQL** - Database relazionale
- **SQL DDL/DML** - Definizione e manipolazione dati  
- **Views** - Viste dinamiche per reporting
- **Transactions** - Gestione consistenza dati
- **Stored Procedures Logic** - Automazione processi

## 📊 Dati di Test

Il database include dati realistici per:
- **5 categorie** prodotto (Action Figures, Puzzle, Giochi Educativi, Bambole, Costruzioni)
- **9 prodotti** con prezzi variabili
- **3 magazzini** distribuiti (Milano, Napoli, Firenze)
- **3 negozi** collegati ai magazzini
- **Transazioni di esempio** con aggiornamenti automatici

## 🚀 Come Utilizzare

1. **Setup Database**:
   ```sql
   CREATE DATABASE vendicosespa;
   USE vendicosespa;
   ```

2. **Eseguire lo script** `Database VendicoseSPA.sql` per:
   - Creare le tabelle
   - Popolare con dati di test
   - Configurare le views

3. **Testare le funzionalità**:
   - Inserire nuove vendite
   - Verificare aggiornamenti giacenze
   - Controllare alert restock

## 💡 Caratteristiche Avanzate

- **Gestione Transazioni**: Operazioni atomiche per vendite
- **Subquery Dinamiche**: Recupero automatico ultimo ID transazione
- **Views Responsive**: Aggiornamento in tempo reale dei dati
- **Business Logic**: Integrazione logiche aziendali nel database
- **Scalabilità**: Struttura estendibile per nuovi magazzini/negozi

## 🎓 Competenze Dimostrate

- Database Design & Normalization
- SQL Query Optimization  
- Business Process Automation
- Real-time Data Management
- Transaction Management
- View-based Reporting

## 📝 Possibili Estensioni

- Dashboard web per visualizzazione giacenze
- Sistema di notifiche automatiche per restock
- Integrazione API per e-commerce
- Analytics predittive per demand forecasting
- Mobile app per gestori magazzino

---

*Progetto sviluppato durante la Build Week 2 - Dimostrazione pratica di competenze in database design e gestione inventario aziendale.*
