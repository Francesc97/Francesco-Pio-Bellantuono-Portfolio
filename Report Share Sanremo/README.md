# Analisi Storica dello Share del Festival di Sanremo (1987–2023)

📊 Questo progetto analizza e visualizza l’andamento dello share televisivo del Festival di Sanremo dal 1987 al 2023, utilizzando tecniche di pulizia, arricchimento e visualizzazione dei dati.

## Link al Report
[Visualizza il Report in Looker Studio](https://lookerstudio.google.com/reporting/f598a957-c44a-475f-bf00-87e0293a3793)

## Dataset
- **Fonte primaria:** dataset di partenza con informazioni sul Festival di Sanremo  
- **Fonte secondaria:** dati di share Auditel estratti tramite web scraping dalla pagina Wikipedia del Festival di Sanremo  
- **Periodo analizzato:** 1987–2023  
- **Formato dati:** normalizzati e unificati in formato tabellare, con campi `Data`, `Share (%)` e `Anno`  

## Metodologia

### Analisi e Pulizia dei Dati
- Utilizzo di Python in JupyterLab per l’EDA (Exploratory Data Analysis) e normalizzazione dei dati  
- Standardizzazione delle date e gestione di valori mancanti o formati incoerenti  
- Inclusione nel repository di uno snippet di codice Python utilizzato per la lavorazione  

### Arricchimento del Dataset
- Merge con i dati di share Auditel estratti tramite web scraping  
- Elaborazione in Excel e Power Query:
  - Selezione delle sole colonne rilevanti  
  - Formattazione dei dati (date, percentuali)  

### Visualizzazione
- Caricamento del dataset finale in Looker Studio  
- Creazione di serie storiche tramite un campo calcolato (`YEAR(data)`) per raggruppare i dati per anno  
- Realizzazione di dashboard interattive per analizzare trend e variazioni dello share  

## Risultati e Insight
- Analisi storica dettagliata dello share televisivo di Sanremo negli ultimi 35+ anni  
- Individuazione di picchi e cali di audience in relazione a edizioni specifiche  
- Possibilità di esplorazione interattiva tramite Looker Studio  

## Tecnologie Utilizzate
- Python (JupyterLab)  
- Pandas, NumPy per EDA e trasformazioni  
- Excel / Power Query per pulizia e formattazione  
- Looker Studio per dashboard e visualizzazioni  
- Web Scraping per estrazione dati Wikipedia
