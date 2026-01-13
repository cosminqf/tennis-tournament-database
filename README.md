# Tennis Tournament Management System (Oracle SQL)

Sistem complex de baze de date pentru gestionarea logisticii, statisticilor și ticketing-ului în turneele de tenis.

<img width="1079" height="966" alt="GestiuneTurneuTenis drawio" src="https://github.com/user-attachments/assets/93ca9b31-d1d6-48cd-9c68-17fa54213fad" />

<img width="1257" height="1131" alt="FINAL" src="https://github.com/user-attachments/assets/41bddaa3-9b05-42b8-9180-b00b008921f2" />

## Descriere Proiect

Acest proiect reprezintă o soluție completă de **backend**, implementată în **Oracle PL/SQL**, pentru digitalizarea unui turneu de tenis. Sistemul gestionează volume mari de date interconectate, acoperind:

- **Logistică**: locații, terenuri, programarea meciurilor  
- **Resurse umane**: jucători, arbitri, staff, voluntari  
- **Comercial**: bilete, zone, sponsori  

Proiectul a fost dezvoltat în cadrul cursului de **Sisteme de Gestiune a Bazelor de Date**, demonstrând utilizarea conceptelor avansate Oracle:
- pachete PL/SQL
- triggere
- colecții (VARRAY, NESTED TABLE)
- cursoare (simple și Master–Detail)

## Funcționalități Cheie

### 1. Logistică & Programare (Match Scheduling)

- **Managementul terenurilor**  
  - Funcția `Verifica_Incarcare_Teren` limitează la **maximum 5 meciuri/zi/teren**

- **Order of Play**  
  - Generare automată a programului zilnic  
  - Format TV-friendly:  
    ```
    Djokovic N. vs Nadal R.
    ```

- **Mutări urgente de meciuri**  
  - Procedură stocată pentru mutarea meciurilor (ploaie, întârzieri)  
  - Validare în timp real a disponibilității terenului  


### 2. Statistici Avansate (Analytics)

- **Analiza serviciului**  
  - Clasificarea jucătorilor după numărul de ași  
  - Implementare cu **VARRAY** și **NESTED TABLE**

- **Fișa jucătorului**  
  - Calcul automat pentru:
    - victorii / înfrângeri  
    - seturi și game-uri câștigate/pierdute  
  - Include cazuri speciale (retrageri medicale)


### 3. Ticketing & Financiar

- **Rapoarte de vânzări**  
  - Cursoare complexe **Master–Detail**  
  - Încasări detaliate pe zone:
    - VIP  
    - Tribună  
    - Peluză  

- **Integritate financiară**  
  - Constrângeri **UNIQUE**  
  - Previne vânzarea aceluiași loc de mai multe ori  


### 4. Securitate & Integritate (Triggers)

- **Protecția istoricului**  
  - Trigger `BEFORE DELETE` care blochează ștergerea meciurilor

- **Validare logică**  
  - Trigger `BEFORE UPDATE`  
  - Interzice retragerea unui jucător dintr-un meci în care nu este programat

- **Protecția schemei**  
  - Trigger **DDL** care blochează:
    ```sql
    DROP TABLE
    ```


## Structura Bazei de Date

Baza de date conține **18 tabele relaționale**, normalizate.

### Infrastructură
- `TARI`
- `ORASE`
- `LOCATII`
- `TURNEE`

### Participanți
- `JUCATORI`
- `ARBITRI`
- `STAFF_ORGANIZARE`
- `VOLUNTARI`

### Competiție
- `ZILE_TURNEU`
- `MECIURI`
- `SETURI`

### Comercial
- `SPONSORI`
- `BILETE`
- `ZONE_BILETE`


## Implementare Tehnică (Code Highlights)

### Pachete PL/SQL — `PKG_LOGISTICA_TURNEU`

- Încapsulează logica de organizare a turneului
- Exemplu flux mutare meci:
  1. Verifică încărcarea terenului (`Verifica_Incarcare_Teren`)
  2. Execută `UPDATE`
  3. Altfel → `RAISE_APPLICATION_ERROR`


### Statistici cu Colecții

- `INDEX BY TABLE`
- `VARRAY`
- Procesare **in-memory**
- Fără tabele temporare → performanță crescută


### Cursoare Parametrizate

- Rapoarte financiare eficiente
- Iterare prin:
  - turnee  
  - zone de bilete  
- Structură **Master–Detail**


## Cum se Rulează Proiectul

### Cerințe
- Oracle Database (XE sau Enterprise)
- Oracle SQL Developer

### Setup

1. Deschide fișierul:
   **all.sql**
2. Rulează secțiunile în ordine:
- **CREATE TABLES** (DDL)
- **INSERT DATA** (DML)
3. Execută:
```sql
COMMIT;

