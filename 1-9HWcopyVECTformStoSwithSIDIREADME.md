### **Explicație pas cu pas**

Pentru a înțelege acest program, este important să înțelegem mai întâi câteva noțiuni fundamentale despre arhitectura **8086** și cum funcționează asamblarea:

---

### **1. Noțiuni fundamentale**

- **Memoria segmentată**:
  - Procesorul 8086 folosește o arhitectură segmentată pentru memorie. Memoria este împărțită în segmente: **CS** (Code Segment), **DS** (Data Segment), **ES** (Extra Segment), și **SS** (Stack Segment).
  - **DS** este folosit pentru a accesa datele din segmentul sursă, iar **ES** pentru datele din segmentul destinație.

- **Registre importante**:
  - **AX, BX, CX, DX** sunt registre generale (pe 16 biți).
  - **SI** (Source Index) și **DI** (Destination Index) sunt registre folosite pentru a accesa datele din memorie.
  - **CX** este folosit ca un contor, de obicei pentru bucle.

- **Instrucțiuni cheie**:
  - `MOV` copiază date între registre și memorie.
  - `LODSB` încarcă un octet din vectorul sursă în registrul **AL** și incrementează automat **SI**.
  - `STOSB` stochează octetul din registrul **AL** în vectorul destinație și incrementează automat **DI**.
  - `LOOP` decrementează **CX** și continuă execuția buclei dacă **CX > 0**.

---

### **2. Ce face acest program**

Acest program copiază un vector (o listă de 5 valori) dintr-un segment de memorie în alt segment. **Vectorul sursă** este `10h, 20h, 30h, 40h, 50h`, iar programul îl copiază într-un **vector destinație**. Fiecare element este procesat pe rând, iar la final, vectorul destinație conține exact aceleași valori ca vectorul sursă.

---

### **3. Explicație pas cu pas**

#### **A. Inițializarea segmentelor**
```asm
mov ax, data   ; Setăm segmentul de date (adresa bazei segmentului de date)
mov ds, ax     ; DS = ax (acum DS indică segmentul sursă)
mov ax, 0x7000 ; Segmentul destinație este la adresa 0x7000
mov es, ax     ; ES = ax (acum ES indică segmentul destinație)
```
- **Ce facem aici**:
  - Setăm registrul **DS** să indice segmentul de date în care se află `vector_sursa`.
  - Setăm registrul **ES** să indice segmentul de memorie la adresa `0x7000`, unde vom copia datele (vectorul destinație).

---

#### **B. Inițializarea vectorilor**
```asm
mov si, offset vector_sursa ; SI indică începutul vectorului sursă
mov di, offset vector_dest  ; DI indică începutul vectorului destinație
mov cx, 5                   ; CX = 5 (numărul de elemente din vector)
```
- **Ce facem aici**:
  - **SI (Source Index)** este setat să indice începutul vectorului sursă (`vector_sursa`).
  - **DI (Destination Index)** este setat să indice începutul vectorului destinație (`vector_dest`).
  - **CX (Contor)** este setat la `5`, deoarece vectorul conține 5 elemente.

---

#### **C. Copierea vectorului**
```asm
copy_loop:
lodsb      ; Încarcă un octet din [DS:SI] în AL și incrementează SI
stosb      ; Stochează octetul din AL în [ES:DI] și incrementează DI
loop copy_loop ; Decrementează CX și repetă bucla dacă CX > 0
```
- **Pas cu pas**:
  1. **`LODSB`**: 
     - Încarcă elementul curent din vectorul sursă (adresa [DS:SI]) în registrul **AL**.
     - Incrementează automat **SI** pentru a trece la următorul element.
  2. **`STOSB`**:
     - Stochează valoarea din **AL** în vectorul destinație (adresa [ES:DI]).
     - Incrementează automat **DI** pentru a trece la următorul element.
  3. **`LOOP`**:
     - Decrementează **CX** (contorul). Dacă **CX > 0**, controlul revine la eticheta `copy_loop`.

- **Ce facem aici**:
  - Copiem fiecare element din vectorul sursă în vectorul destinație, element cu element, folosind registrele **SI** și **DI**.

---

#### **D. Terminarea programului**
```asm
mov ah, 4Ch    ; Funcția DOS pentru terminare
int 21h        ; Terminare program
```
- **Ce facem aici**:
  - Setăm **AH** la `4Ch`, care este codul DOS pentru "terminare program".
  - Apoi apelăm întreruperea `int 21h` pentru a încheia execuția programului.

---

#### **E. Segmentul de date**
```asm
data segment
vector_sursa db 10h, 20h, 30h, 40h, 50h ; Vector sursă (5 octeți)
vector_dest  db 5 dup (?)               ; Vector destinație (spațiu rezervat)
data ends
```
- **Ce facem aici**:
  - Definim vectorul sursă, care conține 5 valori (`10h, 20h, 30h, 40h, 50h`).
  - Rezervăm spațiu pentru vectorul destinație (`5 dup (?)`), dar inițial nu punem nicio valoare acolo (`?` înseamnă "necunoscut").

---

### **4. Rezultatul final**

După execuția programului:
- Vectorul destinație (`vector_dest`) va conține aceleași valori ca vectorul sursă:
  ```
  vector_dest = 10h, 20h, 30h, 40h, 50h
  ```

---

### **5. Recapitulare - Noțiuni de bază folosite în program**

1. **Segmentele de memorie**:
   - **DS** este segmentul pentru datele sursă.
   - **ES** este segmentul pentru datele destinație.

2. **Registrele SI și DI**:
   - **SI** (Source Index) indică poziția curentă în vectorul sursă.
   - **DI** (Destination Index) indică poziția curentă în vectorul destinație.

3. **Instrucțiuni utile**:
   - **LODSB**: Încarcă un octet din vectorul sursă în registrul **AL** și incrementează **SI**.
   - **STOSB**: Stochează octetul din registrul **AL** în vectorul destinație și incrementează **DI**.
   - **LOOP**: Controlează bucla și verifică dacă mai sunt elemente de procesat.

---

### **6. Întrebări frecvente**
- **Ce se întâmplă dacă vectorul sursă are mai mult de 5 elemente?**
  Programul nu va procesa elemente suplimentare, deoarece **CX** este setat la `5`, limitând numărul de iterații.

- **Cum schimbăm dimensiunea vectorului?**
  Modificați valoarea din instrucțiunea `mov cx, 5` și asigurați-vă că **vector_sursa** și **vector_dest** au suficient spațiu alocat.

