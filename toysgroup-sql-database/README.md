# ToysGroup Database - Sistema Distributivo Giocattoli 🧸

Un progetto completo di database design e implementazione per un'azienda di distribuzione giocattoli multi-regionale, con analisi avanzate delle vendite e performance business.

## 🎯 Scenario Business

**ToysGroup** è un distributore di articoli per bambini che opera in diverse aree geografiche del mondo. Il sistema gestisce:
- Prodotti classificati per categoria
- Mercati organizzati per regioni di vendita
- Vendite distribuite geograficamente
- Analisi performance e trend di mercato

## 🏗️ Architettura del Database

### Modello Concettuale
**Entità Principali:**
- `Product` - Catalogo prodotti con categorizzazione
- `Category` - Classificazione merceologica
- `Region` - Aree geografiche di vendita
- `Country` - Stati appartenenti alle regioni
- `Sales` - Transazioni di vendita

### Modello Logico
```
Category (1) ----< (N) Product (1) ----< (N) Sales (N) >---- (1) Country (N) >---- (1) Region
```

### Relazioni e Cardinalità
- **Product → Category**: N:1 (molti prodotti per categoria)
- **Sales → Product**: N:1 (molte vendite per prodotto)
- **Sales → Country**: N:1 (molte vendite per paese)
- **Country → Region**: N:1 (molti paesi per regione)

## 📊 Schema Fisico

### Struttura Tabelle
```sql
-- Tabella Categorie
CREATE TABLE Category (
    CategoryID INT AUTO_INCREMENT PRIMARY KEY,
    CategoryName VARCHAR(50) NOT NULL
);

-- Tabella Regioni
CREATE TABLE Region (
    RegionID INT AUTO_INCREMENT PRIMARY KEY,
    RegionName VARCHAR(50) NOT NULL
);

-- Tabella Prodotti
CREATE TABLE Product (
    ProductID INT AUTO_INCREMENT PRIMARY KEY,
    ProductName VARCHAR(50) NOT NULL,
    CategoryID INT NOT NULL,
    FOREIGN KEY (CategoryID) REFERENCES Category(CategoryID)
);

-- Tabella Paesi
CREATE TABLE Country (
    CountryID INT AUTO_INCREMENT PRIMARY KEY,
    CountryName VARCHAR(50) NOT NULL,
    RegionID INT NOT NULL,
    FOREIGN KEY (RegionID) REFERENCES Region(RegionID)
);

-- Tabella Vendite
CREATE TABLE Sales (
    SONumber INT AUTO_INCREMENT PRIMARY KEY,
    DateOrder DATE NOT NULL,
    DateShip DATE NOT NULL,
    UnitSold DECIMAL(10,2) NOT NULL,
    UnitCost DECIMAL(10,2) NOT NULL,
    TotalProductCost DECIMAL(10,2) NOT NULL,
    SalesAmount DECIMAL(10,2) NOT NULL,
    ProductID INT NOT NULL,
    CountryID INT NOT NULL,
    FOREIGN KEY (ProductID) REFERENCES Product(ProductID),
    FOREIGN KEY (CountryID) REFERENCES Country(CountryID)
);
```

## 🔍 Query di Business Intelligence

### 1. Validazione Integrità Dati
```sql
-- Verifica univocità chiavi primarie
SELECT ProductID, COUNT(*) 
FROM Product 
GROUP BY ProductID 
HAVING COUNT(*) > 1;
```

### 2. Analisi Performance Temporali
```sql
-- Transazioni con indicatore aging (>180 giorni)
SELECT s.sonumber, s.dateorder, p.productname, 
       DATEDIFF(s.dateship, s.dateorder) > 180 AS IsOld
FROM sales s 
JOIN product p ON s.productid = p.productid;
```

### 3. Analisi Prodotti Top Performer
```sql
-- Prodotti sopra la media delle vendite annue
SELECT productID, SUM(UnitSold) AS TotaleSold
FROM Sales
WHERE YEAR(DateOrder) = (SELECT MAX(YEAR(DateOrder)) FROM Sales)
GROUP BY ProductID
HAVING SUM(UnitSold) > (
    SELECT AVG(SommaVendite)
    FROM (SELECT SUM(UnitSold) AS SommaVendite
          FROM Sales
          WHERE YEAR(DateOrder) = (SELECT MAX(YEAR(DateOrder)) FROM Sales)
          GROUP BY ProductID) AS SubVendite
);
```

### 4. Analisi Geografica Fatturato
```sql
-- Fatturato per stato e anno
SELECT YEAR(DateOrder) AS Anno, 
       SUM(SalesAmount) AS Fatturato, 
       CountryName
FROM Sales s
JOIN Country c ON s.CountryID = c.CountryID
GROUP BY CountryName, YEAR(DateOrder)
ORDER BY YEAR(DateOrder), Fatturato DESC;
```

### 5. Market Analysis
```sql
-- Categoria più richiesta dal mercato
SELECT cat.CategoryName, SUM(s.UnitSold) AS PezziVenduti
FROM Sales s 
JOIN Product p ON s.ProductID = p.ProductID
JOIN Category cat ON p.CategoryID = cat.CategoryID
GROUP BY cat.CategoryName
ORDER BY PezziVenduti DESC
LIMIT 1;
```

## 📈 Views per Reporting

### Vista Prodotti Denormalizzata
```sql
CREATE VIEW VersioneDenormalizzata AS (
    SELECT ProductID AS CodiceProdotto, 
           ProductName AS NomeProdotto,
           (SELECT CategoryName FROM Category cat 
            WHERE p.CategoryID = cat.CategoryID) AS NomeCategoria
    FROM Product p
);
```

### Vista Informazioni Geografiche
```sql
CREATE VIEW InfoGeografiche AS (
    SELECT c.CountryID AS CodiceStato, 
           c.CountryName AS Stato, 
           r.RegionName AS Regione
    FROM Country c 
    JOIN Region r ON c.RegionID = r.RegionID
);
```

## 🛠️ Tecnologie Utilizzate

- **MySQL** - Database relazionale
- **SQL DDL** - Definizione strutture dati
- **SQL DML** - Manipolazione e interrogazione dati
- **Advanced SQL** - Subquery complesse, CTE, funzioni analitiche
- **Views** - Viste materializzate per reporting
- **Business Intelligence** - Query di analisi e KPI

## 📊 Dataset di Test

Il database contiene dati realistici per:
- **5 categorie** prodotto (Costruzioni, Bambole, Veicoli, Puzzle, Educativi)
- **8 prodotti** con dettagli completi
- **3 regioni** (NorthEurope, SouthEurope, WestEurope)
- **6 paesi** distribuiti nelle regioni
- **8 transazioni** di vendita con metriche complete

## 🚀 Funzionalità Avanzate

### Analisi Multi-dimensionale
- Performance prodotti per categoria
- Trend geografici delle vendite
- Analisi temporale delle performance
- Identificazione prodotti invenduti

### Data Quality Assurance
- Validazione integrità referenziale
- Controllo univocità chiavi primarie
- Verifica consistenza dati

### Approcci Multipli
- Due metodologie per identificare prodotti invenduti
- Query ottimizzate con diversi approcci (JOIN vs Subquery)

## 📝 Competenze Dimostrate

- **Database Design** - Modellazione concettuale e logica
- **SQL Mastery** - Query complesse e ottimizzazione
- **Business Intelligence** - Analisi dati e KPI
- **Data Modeling** - Normalizzazione e integrità
- **Performance Analysis** - Trend e comparative analytics
- **Documentation** - Progettazione strutturata e completa

## 🎓 Valore Business

Questo progetto dimostra la capacità di:
- Tradurre requirements business in modelli dati
- Implementare sistemi di analisi performance
- Sviluppare query per decision making
- Garantire data quality e consistenza
- Creare reporting views per stakeholder

---

*Progetto sviluppato come esame completo SQL - Dimostrazione di competenze avanzate in database design, business intelligence e analisi dati per settore retail.*
