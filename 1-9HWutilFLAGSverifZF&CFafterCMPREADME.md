### **Explicație pas cu pas**

#### **1. Noțiuni fundamentale despre registrul FLAGS**

- Registrul **FLAGS** în procesorul **8086** stochează diferite informații despre rezultatele operațiilor. În acest exemplu, ne concentrăm pe:
  - **ZF (Zero Flag)**:
    - Setat (1) dacă rezultatul unei operații este 0 (egalitate între valori).
    - Resetat (0) dacă rezultatul nu este 0.
  - **CF (Carry Flag)**:
    - Setat (1) dacă apare un împrumut în cazul unei scăderi (când prima valoare este mai mică decât a doua, adică AL < BL).
    - Resetat (0) dacă nu apare un împrumut (AL >= BL).

#### **2. Segmentul de date**
```asm
mov ax, @data
mov ds, ax
```
- Setăm segmentul de date, astfel încât să putem accesa mesajele definite în secțiunea `data`.

---

#### **3. Operația de comparație**
```asm
mov al, 10h    ; Încarcă valoarea 10h în registrul AL
mov bl, 20h    ; Încarcă valoarea 20h în registrul BL
cmp al, bl     ; Compară AL cu BL (AL - BL)
```
- **`CMP AL, BL`** efectuează operația:
  \[
  AL - BL
  \]
  - Modifică indicatorii din registrul **FLAGS**:
    - **ZF** este setat dacă **AL == BL**.
    - **CF** este setat dacă **AL < BL**.

---

#### **4. Verificarea indicatorilor FLAGS**

##### **A. Verificarea lui ZF (Zero Flag)**
```asm
je equal        ; Săritura se face la "equal" dacă ZF = 1 (AL == BL)
jne not_equal   ; Săritura se face la "not_equal" dacă ZF = 0 (AL != BL)
```
- **`JE` (Jump if Equal)**: Sare la eticheta `equal` dacă **ZF = 1**.
- **`JNE` (Jump if Not Equal)**: Sare la eticheta `not_equal` dacă **ZF = 0**.

##### **B. Verificarea lui CF (Carry Flag)**
```asm
jc less_than    ; Săritura se face la "less_than" dacă CF = 1 (AL < BL)
jnc greater_than; Săritura se face la "greater_than" dacă CF = 0 (AL >= BL)
```
- **`JC` (Jump if Carry)**: Sare la eticheta `less_than` dacă **CF = 1**.
- **`JNC` (Jump if Not Carry)**: Sare la eticheta `greater_than` dacă **CF = 0**.

---

#### **5. Etichetele pentru rezultat**

Fiecare etichetă afișează un mesaj specific folosind funcția **DOS 09h** pentru afișare.

##### Exemple:
- **Dacă ZF = 1**, controlul sare la eticheta `equal`:
```asm
equal:
mov ah, 09h         ; Funcția DOS pentru afișare
lea dx, msg_equal   ; Adresa mesajului "Egal"
int 21h             ; Afișare
jmp end_program
```
- **Dacă CF = 1**, controlul sare la eticheta `less_than`:
```asm
less_than:
mov ah, 09h         ; Funcția DOS pentru afișare
lea dx, msg_less    ; Adresa mesajului "Mai mic"
int 21h             ; Afișare
jmp end_program
```

---

#### **6. Terminarea programului**
```asm
end_program:
mov ah, 4Ch
int 21h
```
- Terminăm programul folosind funcția DOS **4Ch**.

---

#### **7. Mesajele**
```asm
data segment
msg_equal db 'Egal$', 0
msg_not_equal db 'Nu sunt egale$', 0
msg_less db 'Mai mic$', 0
msg_greater db 'Mai mare$', 0
data ends
```
- **`$`** marchează sfârșitul mesajului, deoarece funcția **09h** folosește această convenție.

---

### **Rezultatul final**

Programul determină relația dintre **AL** și **BL** și afișează unul dintre următoarele mesaje pe ecran:
1. **Egal** (dacă AL == BL).
2. **Nu sunt egale** (dacă AL != BL).
3. **Mai mic** (dacă AL < BL).
4. **Mai mare** (dacă AL > BL).

---

### **Cum să modificați codul**

1. **Testați alte valori**:
   - Modificați valorile inițiale pentru **AL** și **BL**:
     ```asm
     mov al, 30h
     mov bl, 30h
     ```
     pentru a vedea comportamentul programului.

2. **Afișați valorile FLAGS**:
   - Adăugați cod suplimentar pentru a afișa starea indicatorilor FLAGS.

3. **Integrați alte operații**:
   - Schimbați operația de comparație pentru a explora cum afectează alte instrucțiuni registrul FLAGS.

