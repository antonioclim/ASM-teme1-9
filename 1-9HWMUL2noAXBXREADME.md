### **Explicație pas cu pas**

#### **1. Noțiuni fundamentale despre `MUL`**

Instrucțiunea **`MUL`** efectuează înmulțirea nesemnată între două numere. Dimensiunea operandului determină modul în care sunt utilizate registrele:
- **8 biți**: Înmulțește valoarea din **AL** cu operandul (rezultatul este în **AX**).
- **16 biți**: Înmulțește valoarea din **AX** cu operandul (rezultatul este în **DX:AX**, unde:
  - **AX** conține partea inferioară (mai puțin semnificativă) a rezultatului.
  - **DX** conține partea superioară (mai semnificativă) a rezultatului.

În acest exemplu, folosim operandul pe **16 biți**.

---

#### **2. Segmentul de date**
```asm
mov ax, @data
mov ds, ax
```
- Inițializăm segmentul de date. **DS** este setat să indice segmentul unde stocăm rezultatul.

---

#### **3. Înmulțirea numerelor**
```asm
mov ax, 5      ; Primul număr în AX (5 zecimal)
mov bx, 10     ; Al doilea număr în BX (10 zecimal)
mul bx         ; Înmulțește AX cu BX: DX:AX = AX * BX
```
- **`MUL BX`** calculează produsul celor două numere:
  \[
  DX:AX = AX \times BX
  \]
  - Partea inferioară a rezultatului este stocată în **AX**.
  - Partea superioară a rezultatului (dacă există depășire de 16 biți) este stocată în **DX**.

---

#### **4. Stocarea rezultatului**
```asm
mov result_low, ax ; Salvăm partea inferioară a rezultatului
mov result_high, dx ; Salvăm partea superioară a rezultatului
```
- **AX** (partea inferioară a rezultatului) este stocat în variabila `result_low`.
- **DX** (partea superioară a rezultatului) este stocat în variabila `result_high`.

---

#### **5. Terminarea programului**
```asm
mov ah, 4Ch
int 21h
```
- Terminăm programul folosind funcția DOS **4Ch**.

---

#### **6. Datele**
```asm
data segment
result_low dw ?         ; Rezervăm spațiu pentru partea inferioară a rezultatului
result_high dw ?        ; Rezervăm spațiu pentru partea superioară a rezultatului
data ends
```
- **`result_low`** și **`result_high`** sunt utilizate pentru a stoca rezultatul final. **DW (?)** rezervă spațiu fără a inițializa o valoare.

---

### **Cum funcționează programul**

1. **Inițializare**:
   - Programul setează segmentele și registrele pentru a accesa memoria.

2. **Înmulțirea**:
   - **AX = 5**, **BX = 10**. Operația de înmulțire calculează:
     \[
     5 \times 10 = 50
     \]
   - Rezultatul este stocat astfel:
     - **AX** = 50 (partea inferioară, pentru că rezultatul încape pe 16 biți).
     - **DX** = 0 (partea superioară, deoarece rezultatul nu depășește 16 biți).

3. **Stocarea rezultatului**:
   - Valoarea 50 este stocată în `result_low`.

4. **Terminarea**:
   - Programul se încheie, iar rezultatul rămâne stocat în memorie.

---

### **Rezultatul final**

După execuție:
- **`result_low`** = 50 (`0x0032` în hexazecimal).
- **`result_high`** = 0 (`0x0000`).

---

### **Extensii posibile**

1. **Operații pe alte valori**:
   - Schimbați valorile din **AX** și **BX** pentru a testa alte produse.

2. **Rezultate mari**:
   - Testați cu valori mai mari care generează rezultate ce depășesc 16 biți. De exemplu:
     ```asm
     mov ax, 1000h ; 4096
     mov bx, 1000h ; 4096
     mul bx        ; Rezultatul va fi stocat în DX:AX (16 biți superiori și inferiori).
     ```

3. **Afișarea rezultatului**:
   - Adăugați cod pentru a afișa rezultatul stocat în `result_low` și `result_high`.
