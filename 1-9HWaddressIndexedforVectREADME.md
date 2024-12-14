### **Explicație pas cu pas**

#### **A. Inițializarea segmentelor**
```asm
mov ax, @data
mov ds, ax
```
- Configurăm **DS** (Data Segment) să indice segmentul unde se află vectorul nostru definit în secțiunea `data`.

---

#### **B. Inițializarea parametrilor**
```asm
mov si, offset vector ; SI indică adresa de început a vectorului
mov cx, 5             ; CX = numărul de elemente din vector
mov bl, 10h           ; BL = valoarea constantă pe care o adăugăm fiecărui element
```
- **SI (Source Index)**: Este folosit pentru adresarea indexată a vectorului.
- **CX**: Este folosit ca un contor pentru bucla care parcurge elementele vectorului.
- **BL**: Stochează valoarea constantă de adăugat la fiecare element.

---

#### **C. Modificarea vectorului**
```asm
modify_loop:
    mov al, [si]       ; Încarcă elementul curent în AL
    add al, bl         ; Adaugă valoarea constantă din BL
    mov [si], al       ; Stochează elementul modificat în vector
    inc si             ; Trecem la următorul element (incrementăm SI)
    loop modify_loop   ; Decrementează CX și repetă dacă CX > 0
```
- **Bucla pas cu pas**:
  1. **`mov al, [si]`**: Accesează elementul curent al vectorului, folosind adresarea indexată, și îl încarcă în registrul **AL**.
  2. **`add al, bl`**: Adaugă valoarea constantă din registrul **BL** la elementul curent.
  3. **`mov [si], al`**: Scrie valoarea modificată înapoi în poziția curentă a vectorului.
  4. **`inc si`**: Incrementăm **SI** pentru a trece la următorul element din vector.
  5. **`loop modify_loop`**: Decrementează **CX** și repetă bucla dacă mai sunt elemente.

---

#### **D. Terminarea programului**
```asm
mov ah, 4Ch
int 21h
```
- Setăm **AH** la `4Ch`, care este codul DOS pentru terminarea programului, și apelăm întreruperea `int 21h`.

---

#### **E. Definiția vectorului**
```asm
data segment
vector db 10h, 20h, 30h, 40h, 50h ; Vectorul de test (5 elemente)
data ends
```
- Definim un vector cu 5 elemente (`10h, 20h, 30h, 40h, 50h`).
- Valoarea constantă `10h` (16 în zecimal) va fi adăugată fiecărui element.

---

### **Rezultatul final**

După execuția programului, vectorul inițial:
```
10h, 20h, 30h, 40h, 50h
```
va deveni:
```
20h, 30h, 40h, 50h, 60h
```

---

### **Noțiuni fundamentale utilizate**

1. **Adresarea indexată**:
   - Adresarea indexată accesează un element dintr-un vector folosind un registru index (în acest caz, **SI**).
   - **`[si]`** accesează conținutul de la adresa indicată de **SI**.

2. **Instrucțiuni cheie**:
   - **`MOV AL, [SI]`**: Încarcă octetul de la adresa [SI] în registrul **AL**.
   - **`ADD AL, BL`**: Adună valoarea constantă din **BL** la octetul din **AL**.
   - **`MOV [SI], AL`**: Stochează valoarea modificată în vector, la adresa indicată de **SI**.
   - **`INC SI`**: Incrementează **SI** pentru a accesa următorul element.

3. **Bucla cu `LOOP`**:
   - **`LOOP`** decrementează registrul **CX** și verifică dacă mai sunt elemente de procesat.
   - Dacă **CX > 0**, se revine la eticheta specificată (`modify_loop`).

---

### **Extensii posibile**

1. **Modificați valoarea constantă**:
   - Puteți schimba valoarea din **BL** la altceva (de exemplu, `BL = 5h` pentru a adăuga 5).

2. **Vector cu lungime variabilă**:
   - Înlocuiți valoarea fixă a lui **CX** (5) cu o variabilă care să fie calculată din lungimea reală a vectorului.

3. **Afișarea rezultatelor**:
   - Adăugați un cod suplimentar pentru a afișa vectorul rezultat utilizând funcții DOS.

