Zde jsou odpovědi na otázky z testu s upraveným formátováním LaTeX pro GitHub Markdown:

**1. Synchronní obvod pro rozpoznání lichých čísel**

*   **Popis:** Obvod má sériový vstup X (4 bity na číslo, LSB nejdříve) a výstup Y. Y=1 současně se čtvrtým bitem vstupujícího čísla, pokud bylo toto číslo liché, jinak Y=0. Počáteční stav je před LSB prvního čísla.
*   **Návrh:** Potřebujeme čítač bitů (0 až 3) a paměť pro LSB (první bit). Použijeme 3 D klopné obvody: Q1, Q0 pro čítání stavů (S0: 00, S1: 01, S2: 10, S3: 11) a QL pro uložení LSB.
    *   **Stavy čítače:** S0 (počáteční), S1 (po 1. bitu), S2 (po 2. bitu), S3 (po 3. bitu). Přechody: S0->S1, S1->S2, S2->S3, S3->S0.
        *   Logika přechodů (D vstupy pro Q1, Q0): $D_1 = Q_0$, $D_0 = \neg Q_1$
    *   **Uložení LSB:** Klopný obvod QL si pamatuje hodnotu vstupu X, když je čítač ve stavu S0. Jinak si drží hodnotu.
        *   Logika pro QL (D vstup): $D_L = (\neg Q_1 \land \neg Q_0 \land X) \lor ((Q_1 \lor Q_0) \land Q_L)$
    *   **Výstup Y:** Výstup Y má být 1, když přichází 4. bit (tj. jsme ve stavu S3 *před* hodinovým pulsem) a uložený LSB (v QL) je 1.
        *   Logika výstupu: $Y = Q_1 \land Q_0 \land Q_L$
*   **Graf přechodů:**
    *   4 hlavní stavy (S0-S3) pro čítání. Přechody mezi nimi řízené hodinovým signálem. Stav QL se mění jen při přechodu S0->S1.
    ```mermaid
    graph TD
        S0 -- X=0/QL=0 --> S1;
        S0 -- X=1/QL=1 --> S1;
        S1 -- X/ --> S2;
        S2 -- X/ --> S3;
        S3 -- X/Y=(QL=1?1:0) --> S0;
    ```
    *Poznámka: Mermaid diagram je zjednodušení, Y závisí na stavu (S3 a QL), ne na přechodu.*
*   **Tabulka přechodů a výstupů:**
    | Souč. stav (Q1 Q0 QL) | Vstup X | Příští stav (Q1+ Q0+ QL+) | Výstup Y |
    |-----------------------|---------|---------------------------|----------|
    | 0 0 0                 | 0       | 0 1 0                     | 0        |
    | 0 0 0                 | 1       | 0 1 1                     | 0        |
    | 0 0 1                 | 0       | 0 1 0                     | 0        |
    | 0 0 1                 | 1       | 0 1 1                     | 0        |
    | 0 1 0                 | 0       | 1 0 0                     | 0        |
    | 0 1 0                 | 1       | 1 0 0                     | 0        |
    | 0 1 1                 | 0       | 1 0 1                     | 0        |
    | 0 1 1                 | 1       | 1 0 1                     | 0        |
    | 1 0 0                 | 0       | 1 1 0                     | 0        |
    | 1 0 0                 | 1       | 1 1 0                     | 0        |
    | 1 0 1                 | 0       | 1 1 1                     | 0        |
    | 1 0 1                 | 1       | 1 1 1                     | 0        |
    | 1 1 0                 | 0       | 0 0 0                     | 0        |
    | 1 1 0                 | 1       | 0 0 0                     | 0        |
    | 1 1 1                 | 0       | 0 0 1                     | 1        |
    | 1 1 1                 | 1       | 0 0 1                     | 1        |
*   **Schéma:** Nakreslete 3 D-klopné obvody (pro Q1, Q0, QL). Přidejte kombinační logiku (brány AND, OR, NOT) pro výpočet $D_1, D_0, D_L$ a Y podle výše uvedených rovnic. Všechny klopné obvody sdílejí hodinový signál.
*   **Typ automatu:** Jedná se o **Mooreův** automat. Výstup Y závisí pouze na aktuálním stavu automatu (konkrétně na kombinaci stavů Q1, Q0 a QL), nikoli na aktuálním vstupu X. Hodnota $Y = Q_1 \land Q_0 \land Q_L$ je určena stavem *před* příchodem dalšího hodinového pulsu.

**2. Minimalizace funkce f(d,c,b,a)**

*   Funkce: $f(d,c,b,a) = \sum m(2, 3, 4, 9, 10, 11, 12, 14) + \sum d(1, 5, 13)$
*   Karnaughova mapa (d,c řádky; b,a sloupce):
    ```
       ba
    dc 00 01 11 10
    00  0  X  1  1
    01  1  X  0  0
    11  1  X  0  1
    10  0  1  1  1
    ```
*   **Esenciální prvoimplikanty (EPI):**
    *   EPI1: Pokrývá m3 => $\neg d \neg c b$ (pokrývá m2, m3)
    *   EPI2: Pokrývá m4 => $\neg d c \neg a$ (pokrývá m4, využívá d5)
*   **Zbývající mintermy k pokrytí:** 9, 10, 11, 12, 14.
*   **Další prvoimplikanty (PI):**
    *   PI3: $d c \neg b$ (pokrývá m12, využívá d13)
    *   PI4: $d c \neg a$ (pokrývá m12, m14)
    *   PI5: $d \neg c a$ (pokrývá m9, m11)
    *   PI6: $d \neg c b$ (pokrývá m10, m11)
    *   PI7: $b \neg a$ (pokrývá m2, m10, m14)
    *   PI8: $c a$ (pokrývá m9, využívá d1, d5, d13)
*   **Výběr PI pro pokrytí zbývajících mintermů:** Potřebujeme pokrýt 9, 10, 11, 12, 14. Všechny minimální řešení vyžadují 3 další PI k EPI1 a EPI2 (celkem 5 PI).
*   **Minimální normální disjunktivní formy (MNDF/SOP):** Existují 3 řešení s minimálním počtem literálů (14):
    1.  $f = \neg d \neg c b + \neg d c \neg a + b \neg a + d \neg c a + d c \neg b$
    2.  $f = \neg d \neg c b + \neg d c \neg a + b \neg a + d \neg c a + d c \neg a$
    3.  $f = \neg d \neg c b + \neg d c \neg a + c a + d c \neg a + d \neg c b$
    *(Poznámka: Další řešení $f = \neg d \neg c b + \neg d c \neg a + d \neg c a + d c \neg a + d \neg c b$ má 15 literálů a není tedy minimální.)*

**3. Obvod "půlsčítačka" (Half Adder)**

*   **Popis:** Půlsčítačka je kombinační logický obvod, který sčítá dva jednobitové vstupy.
*   **Vstupy:** A, B (dva sčítané bity).
*   **Výstupy:** S (Sum - součet), C (Carry - přenos do vyššího řádu).
*   **Pravdivostní tabulka:**
    | A | B | S | C |
    |---|---|---|---|
    | 0 | 0 | 0 | 0 |
    | 0 | 1 | 1 | 0 |
    | 1 | 0 | 1 | 0 |
    | 1 | 1 | 0 | 1 |
*   **Logické funkce:**
    *   $S = A \oplus B$ (Exkluzivní součet - XOR)
    *   $C = A \land B$ (Logický součin - AND)
*   **Schéma:** Nakreslete hradlo XOR a hradlo AND. Vstupy A a B přiveďte na vstupy obou hradel. Výstup hradla XOR je S, výstup hradla AND je C.

**4. Booleovská n-krychle (n=4)**

*   Předpokládáme Booleovskou 4-krychli ($n=4$).
*   **Kolik má tato krychle vrcholů?** $2^n = 2^4 = 16$ vrcholů.
*   **Kolik má booleovská funkce $f(x) : B^n \to B$, kde $B = \{0, 1\}$ proměnných?** Počet různých Booleovských funkcí n proměnných je $2^{(2^n)}$. Pro n=4 je to $2^{(2^4)} = 2^{16} = 65536$ funkcí.
*   **Kolik je literálů?** Literál je proměnná nebo její negace. Pro n proměnných je $2n$ literálů. Pro n=4 je $2 \times 4 = 8$ literálů (např. a, $\neg a$, b, $\neg b$, c, $\neg c$, d, $\neg d$).
*   **Kolik je všech logických formulí?** Pokud se myslí počet *různých Booleovských funkcí*, odpověď je 65536 (viz výše). Pokud se myslí počet *syntakticky různých zápisů* formulí, je jich nekonečně mnoho (např. $a \lor \neg a$, $a \lor \neg a \lor (b \land \neg b)$ atd. reprezentují stejnou funkci). Obvykle se míní počet různých funkcí.

**5. Von Neumannova vs. Harvardská architektura**

*   **Von Neumannova architektura:**
    *   Charakterizována **jedinou sdílenou pamětí** pro instrukce i data.
    *   Používá **jedinou sběrnici** (adresní, datovou, řídicí) pro přenos instrukcí i dat mezi CPU a pamětí.
    *   Instrukce a data se načítají sekvenčně přes stejnou sběrnici, což vede k tzv. **Von Neumannovu bottlenecku** (propustnost sběrnice omezuje výkon).
    *   Konstrukčně jednodušší.
    *   Převažuje u univerzálních počítačů.
*   **Harvardská architektura:**
    *   Používá **oddělené paměti** pro instrukce a data.
    *   Má **oddělené sběrnice** pro přístup k paměti instrukcí a paměti dat.
    *   Umožňuje **současné načítání instrukce a dat**, což může vést k vyššímu výkonu (paralelní zpracování).
    *   Konstrukčně složitější (více pamětí a sběrnic).
    *   Často se používá v signálových procesorech (DSP) a mikrokontrolérech, kde je klíčový výkon a rychlost zpracování specifických úloh.
*   **Rozdíl:** Hlavní rozdíl je ve **způsobu organizace paměti a sběrnic**. Von Neumann má sdílenou paměť a sběrnici, Harvard má oddělené paměti a sběrnice pro instrukce a data.

**6. Šestnáctkové sčítání**

*   Operandy: 7A, 97, C5, 8E (vše hex).

*   **a) Čísla jsou nezáporná (bez znaménka) - 8 bitů:**
    | Zadaná čísla | Výsledek v hexa | Přenos (Carry) ...1, 0 |
    |--------------|-----------------|------------------------|
    | 7A + 97      | 11              | 1                      |
    | C5 + 8E      | 53              | 1                      |
    *Výpočty:*
    *   7A + 97 = 111 (hex). Výsledek (8 bitů) = 11, Přenos C=1.
    *   C5 + 8E = 153 (hex). Výsledek (8 bitů) = 53, Přenos C=1.

*   **b) Čísla jsou v doplňkovém kódu - 8 bitů:**
    | Zadaná čísla | Výsledek | Interpretace operandů; a výsledku (obrazy) | Přetečení (Overflow) | znaménka: op, op, výsledek |
    |--------------|----------|--------------------------------------------|----------------------|----------------------------|
    | 7A + 97      | 11       | 7A=+122, 97=-105, 11=+17                   | 0                    | +, -, +                    |
    | C5 + 8E      | 53       | C5=-59, 8E=-114, 53=+83 (Chyba!)           | 1                    | -, -, +                    |
    *Výpočty:*
    *   7A (01111010) + 97 (10010111) = 00010001 (11 hex). Operandy +,-; výsledek +. Overflow $V = (A_7 \land B_7 \land \neg S_7) \lor (\neg A_7 \land \neg B_7 \land S_7) = (0 \land 1 \land \neg 0) \lor (\neg 0 \land \neg 1 \land 0) = 0$. Přetečení V=0.
    *   C5 (11000101) + 8E (10001110) = 01010011 (53 hex). Operandy -,-; výsledek +. Přetečení $V = (A_7 \land B_7 \land \neg S_7) \lor (\neg A_7 \land \neg B_7 \land S_7) = (1 \land 1 \land \neg 0) \lor (\neg 1 \land \neg 1 \land 0) = 1$. Přetečení V=1.

*   **c) Čísla jsou v přímém kódu - 7 bitů (+ znaménko = 8 bitů):**
    *   Předpoklad: MSB je znaménko (0+, 1-), zbylých 7 bitů je absolutní hodnota.
    *   7A = 0 1111010 => +, $Mag=7A=122$ (Pozor: 7A vyžaduje 7 bitů, 122 > 127, nelze zobrazit v 7 bitech. Pokud předpokládáme, že operandy *jsou* platné 7-bitové mag., pak 7A není platný vstup. Pokud je to 8-bitový přímý kód, pak Mag=7A.)
    *   97 = 1 0010111 => -, $Mag=17=23$
    *   C5 = 1 1000101 => -, $Mag=45=69$
    *   8E = 1 0001110 => -, $Mag=0E=14$
    *   *Pokud 7A je neplatný vstup, nelze řešit. Pokud předpokládáme 8-bitový přímý kód:*
        *   7A (+122) + 97 (-23): Různá znaménka. $Mag(A)-Mag(B) = 122-23 = 99$ (63h). Znaménko = +. Výsledek = 0 1100011 = 63h. Overflow=0.
        *   C5 (-69) + 8E (-14): Stejná znaménka. $Mag(A)+Mag(B) = 69+14 = 83$ (53h). Znaménko = -. Výsledek = 1 1010011 = D3h. Overflow=0 ($Mag 83 < 127$).
    | Zadaná čísla | Výsledek | Interpretace operandů; a výsledku (obrazy) | Přetečení (Overflow) | znaménka: op, op, výsledek |
    |--------------|----------|--------------------------------------------|----------------------|----------------------------|
    | 7A + 97      | 63       | 7A=(+,122), 97=(-,23), 63=(+,99)           | 0                    | +, -, +                    |
    | C5 + 8E      | D3       | C5=(-,69), 8E=(-,14), D3=(-,83)           | 0                    | -, -, -                    |

**7. Synchronní čítač M5**

*   **Popis:** Synchronní čítač modulo 5 (stavy 0, 1, 2, 3, 4). Řídicí vstup P (P=1 čítá, P=0 drží stav).
*   **Stavy:** 000, 001, 010, 011, 100. Potřeba 3 D-klopné obvody (Q2, Q1, Q0).
*   **Tabulka přechodů:**
    | P | Q2 Q1 Q0 | Q2+ Q1+ Q0+ |
    |---|----------|-------------|
    | 0 | x  x  x  | Q2  Q1  Q0  |
    | 1 | 0  0  0  | 0   0   1   |
    | 1 | 0  0  1  | 0   1   0   |
    | 1 | 0  1  0  | 0   1   1   |
    | 1 | 0  1  1  | 1   0   0   |
    | 1 | 1  0  0  | 0   0   0   |
    | 1 | 1  0  1  | x   x   x   | (Neurčený stav)
    | 1 | 1  1  0  | x   x   x   | (Neurčený stav)
    | 1 | 1  1  1  | x   x   x   | (Neurčený stav)
*   **Budící rovnice (pro D-FF):**
    *   $D_2 = Q_2^+ = \neg P Q_2 \lor P \neg Q_2 Q_1 Q_0$
    *   $D_1 = Q_1^+ = \neg P Q_1 \lor P \neg Q_2 (Q_1 \oplus Q_0)$
    *   $D_0 = Q_0^+ = \neg P Q_0 \lor P \neg Q_2 \neg Q_0$
*   **Schéma:** Nakreslete 3 D-FF (Q2, Q1, Q0). Implementujte budící rovnice D2, D1, D0 pomocí hradel NAND (nebo AND/OR/NOT) s vstupy P, Q2, Q1, Q0. Spojte hodinové vstupy FF. Výstupy Q2, Q1, Q0 reprezentují stav čítače.
*   **Graf přechodů:** 5 uzlů (000 až 100). Šipky pro P=1 (0->1->2->3->4->0). Smyčky pro P=0 u každého uzlu.
*   **Typ automatu:** **Mooreův** automat. Výstupy (hodnota čítače Q2, Q1, Q0) závisí pouze na aktuálním stavu klopných obvodů.

**8. Odvození logického výrazu z mapy (Moodle)**

*   Pro zodpovězení této otázky je potřeba vidět mapu (obrázek v Moodle).
*   **Postup:**
    1.  Identifikujte proměnné a jejich přiřazení osám mapy.
    2.  Zakreslete 1, 0 a X (neurčené stavy) do mapy podle zadání.
    3.  Najděte všechny prvoimplikanty (PI) - co největší možné skupiny $2^k$ jedniček a X.
    4.  Identifikujte esenciální prvoimplikanty (EPI) - ty, které pokrývají minterm (1) nepokrytý žádným jiným PI.
    5.  Vyberte minimální sadu zbývajících PI tak, aby pokryly všechny zbývající mintermy (1), které nebyly pokryty EPI.
    6.  Výsledná MNDF (SOP) je součtem (OR) všech EPI a vybraných PI.

**9. Realizace funkce hradly NAND (nebo AND-OR-NOT)**

*   Použijeme jednu z MNDF z otázky 2, např.: $f = \neg d \neg c b + \neg d c \neg a + b \neg a + d \neg c a + d c \neg b$
*   **Realizace pouze NAND:**
    1.  Vytvořte negace vstupů pomocí NAND: $\neg d = \text{NAND}(d, d)$, atd. (4 NAND hradla pro $\neg d, \neg c, \neg a, \neg b$).
    2.  Realizujte každý součinový term pomocí NAND (výstupem bude negace termu):
        *   $\text{NAND}(\neg d, \neg c, b)$
        *   $\text{NAND}(\neg d, c, \neg a)$
        *   $\text{NAND}(b, \neg a)$
        *   $\text{NAND}(d, \neg c, a)$
        *   $\text{NAND}(d, c, \neg b)$
        (Celkem 5 NAND hradel - 2x 2-vstupové, 3x 3-vstupové).
    3.  Spojte výstupy z kroku 2 na vstupy finálního NAND hradla (5-vstupové).
    *   **Celkový počet hradel:** 4 (negace) + 5 (termy) + 1 (součet) = 10 NAND hradel (za předpokladu dostupnosti vícevstupových hradel).
*   **Realizace AND-OR-NOT:**
    1.  4 NOT hradla pro negace ($\neg d, \neg c, \neg a, \neg b$).
    2.  5 AND hradel pro součinové termy (vstupy podle termů, např. $\neg d, \neg c, b$ na první AND).
    3.  1 OR hradlo (5-vstupové) pro součet výstupů AND hradel.
    *   **Celkový počet hradel:** 4 NOT + 5 AND + 1 OR = 10 hradel.
*   **Nejjednodušší realizace:** Obě varianty vyžadují stejný počet hradel (10). NAND realizace může být preferována z technologického hlediska.

**10. Přímý a podstatný implikant**

*   **Krychle/Podkrychle:** Booleovskou funkci n proměnných lze vizualizovat na n-rozměrné krychli. Každý vrchol odpovídá jednomu mintermu. Součinový term (implikant) odpovídá podkrychli (hrana, stěna, ...) obsahující vrcholy, kde je funkce 1 nebo X.
*   **Literál:** Proměnná nebo její negace (např. $a, \neg a$).
*   **Implikant:** Součinový term (AND term), který pokrývá pouze vrcholy (mintermy), kde je funkce rovna 1 nebo X (neurčený stav). Pokud je implikant roven 1, pak i funkce je rovna 1.
*   **Přímý implikant (Prime Implicant - PI):** Implikant, který nelze dále zjednodušit (vynecháním literálu) tak, aby stále pokrýval pouze 1 a X. Odpovídá podkrychli, kterou nelze zvětšit spojením se sousední podkrychlí tak, aby stále pokrývala pouze 1 a X. Obsahuje minimální počet literálů nutný k popisu dané skupiny mintermů.
*   **Podstatný implikant (Essential Prime Implicant - EPI):** Přímý implikant, který pokrývá alespoň jeden minterm (kde f=1), který není pokryt žádným jiným přímým implikantem. Tento implikant musí být součástí každé minimální formy funkce.
*   **Velikost/Počet literálů:** Podkrychle odpovídající PI s $k$ literály pokrývá $2^{n-k}$ vrcholů (kde n je celkový počet proměnných). Méně literálů znamená větší podkrychli.

**11. Schéma posuvného registru s volitelným směrem posuvu**

*   **Popis:** Posuvný registr (např. 4-bitový: Q3, Q2, Q1, Q0) s možností posuvu doprava nebo doleva, řízený signálem DIR (např. DIR=0: doprava, DIR=1: doleva).
*   **Komponenty:** 4 D-klopné obvody, 4 multiplexory 2-na-1 (MUX), řídicí signál DIR, sériový vstup pro posuv doprava (SIR), sériový vstup pro posuv doleva (SIL).
*   **Logika (vstupy D pro FF):** Každý MUX vybírá zdroj dat pro D vstup podle DIR.
    *   $D_0 = \text{MUX}(\text{IN}_0=Q_1, \text{IN}_1=\text{SIL}, \text{SEL}=\text{DIR}) = \neg \text{DIR} \cdot Q_1 + \text{DIR} \cdot \text{SIL}$
    *   $D_1 = \text{MUX}(\text{IN}_0=Q_2, \text{IN}_1=Q_0, \text{SEL}=\text{DIR}) = \neg \text{DIR} \cdot Q_2 + \text{DIR} \cdot Q_0$
    *   $D_2 = \text{MUX}(\text{IN}_0=Q_3, \text{IN}_1=Q_1, \text{SEL}=\text{DIR}) = \neg \text{DIR} \cdot Q_3 + \text{DIR} \cdot Q_1$
    *   $D_3 = \text{MUX}(\text{IN}_0=\text{SIR}, \text{IN}_1=Q_2, \text{SEL}=\text{DIR}) = \neg \text{DIR} \cdot \text{SIR} + \text{DIR} \cdot Q_2$
*   **Schéma:** Nakreslete 4 D-FF (Q3 až Q0). Ke vstupu D každého FF připojte výstup jednoho MUXu. Vstupy MUXů propojte podle rovnic výše (s Q výstupy sousedních FF a SIR/SIL). Řídicí vstup DIR připojte na SELECT vstupy všech MUXů. Spojte hodinové vstupy FF.
*   **Počet hradel/obvodů:** 4 D-FF, 4 MUX 2-na-1.

**12. Vyjádření a minimalizace funkce (lichá a > 6)**

*   **Funkce:** $f(a,b,c,d)$ čtyř proměnných (bity nezáporného binárního čísla N < 16, d=LSB). $f=1$ právě tehdy, když N je liché A N > 6.
*   **Podmínky:**
    *   Liché: $d=1$
    *   N > 6: $N \in \{7, 8, 9, 10, 11, 12, 13, 14, 15\}$
*   **Kombinace podmínek:** N musí být liché A (N > 6). Tedy $N \in \{7, 9, 11, 13, 15\}$.
*   **Mintermy:** $f = \sum m(7, 9, 11, 13, 15)$
*   **Algebraický výraz (kanonický SOP):** $f = \neg a b c d + a \neg b \neg c d + a \neg b c d + a b \neg c d + a b c d$
*   **Mapa:**
    ```
       cd
    ab 00 01 11 10
    00  0  0  0  0
    01  0  0  1  0  (m7)
    11  0  1  1  0  (m13, m15)
    10  0  1  1  0  (m9, m11)
    ```
*   **Minimalizace (MNDF):**
    *   Skupina 4: m9, m11, m13, m15 => $a d$ (EPI)
    *   Skupina 2: m7, m15 => $b c d$
    *   Výsledná funkce: $f = a d + b c d$

**13. Struktura multiplexoru**

*   **Multiplexor 4-na-1:**
    *   **Popis:** Kombinační obvod, který vybírá jeden ze čtyř datových vstupů a přivádí ho na výstup. Výběr je řízen dvěma selekčními vstupy.
    *   **Vstupy:** 4 datové vstupy ($I_0, I_1, I_2, I_3$), 2 selekční vstupy ($S_1, S_0$). Může mít i vstup Enable (E).
    *   **Výstup:** 1 datový výstup (Y).
    *   **Funkce:**
        *   $S_1S_0 = 00 \implies Y = I_0$
        *   $S_1S_0 = 01 \implies Y = I_1$
        *   $S_1S_0 = 10 \implies Y = I_2$
        *   $S_1S_0 = 11 \implies Y = I_3$
        (Pokud je Enable E=0, pak Y=0 nebo Y je ve stavu vysoké impedance).
    *   **Výstupní funkce v MNDF (SOP):** $Y = \neg S_1 \neg S_0 I_0 + \neg S_1 S_0 I_1 + S_1 \neg S_0 I_2 + S_1 S_0 I_3$ (Za předpokladu E=1). Toto je minimální forma vzhledem k vstupům S a I.
    *   **Struktura:** Nakreslete 4 AND hradla (každé se 3 vstupy: $I_k$, $S_1$ nebo $\neg S_1$, $S_0$ nebo $\neg S_0$). Výstupy AND hradel spojte na vstupy jednoho 4-vstupového OR hradla. Přidejte invertory pro $\neg S_1$ a $\neg S_0$.

**14. Dvoubitová binární násobička**

*   **Popis:** Kombinační obvod násobící dvě 2-bitová nezáporná čísla A($a_1, a_0$) a B($b_1, b_0$).
*   **Vstupy:** $a_1, a_0, b_1, b_0$.
*   **Výstup:** Produkt P($p_3, p_2, p_1, p_0$). Maximální hodnota $3 \times 3 = 9$, takže stačí 4 bity.
*   **Logika (školní násobení):**
    ```
          a1 a0
        x b1 b0
        -------
         (a1&b0)(a0&b0)  <- PP0
      (a1&b1)(a0&b1)     <- PP1 (posunuto)
      -----------------
      p3 p2 p1 p0
    ```
*   **Výpočet bitů P:**
    *   $p_0 = a_0 \land b_0$
    *   $p_1 = (a_1 \land b_0) \oplus (a_0 \land b_1)$
    *   $c_1 = (a_1 \land b_0) \land (a_0 \land b_1)$ (Přenos z $p_1$)
    *   $p_2 = (a_1 \land b_1) \oplus c_1$
    *   $c_2 = (a_1 \land b_1) \land c_1$ (Přenos z $p_2$)
    *   $p_3 = c_2$
*   **Struktura:**
    1.  4 AND hradla pro výpočet dílčích součinů: $a_0b_0, a_1b_0, a_0b_1, a_1b_1$.
    2.  1 Půlsčítačka (HA1): vstupy $a_1b_0, a_0b_1$; výstupy $S=p_1, C=c_1$.
    3.  1 Půlsčítačka (HA2): vstupy $a_1b_1, c_1$; výstupy $S=p_2, C=p_3$.
*   **Schéma:** Nakreslete 4 AND hradla a 2 půlsčítačky (HA) a propojte je podle výše uvedené logiky.

**15. Dvoubitová binární sčítačka**

*   **Popis:** Kombinační obvod sčítající dvě 2-bitová nezáporná čísla A($a_1, a_0$) a B($b_1, b_0$) bez vstupního přenosu ($p_0=0$).
*   **Vstupy:** $a_1, a_0, b_1, b_0$.
*   **Výstup:** Součet S($s_2, s_1, s_0$). Maximální hodnota $3 + 3 = 6$, takže stačí 3 bity.
*   **Logika:**
    *   **Bit 0 (LSB):** $s_0 = a_0 \oplus b_0$; $c_1 = a_0 \land b_0$ (přenos do dalšího řádu)
    *   **Bit 1 (MSB):** $s_1 = a_1 \oplus b_1 \oplus c_1$; $c_2 = (a_1 \land b_1) \lor (a_1 \land c_1) \lor (b_1 \land c_1)$ (celkový přenos ven)
    *   **Výstup:** $s_0, s_1, s_2=c_2$.
*   **Struktura:**
    1.  1 Půlsčítačka (HA) pro bit 0: vstupy $a_0, b_0$; výstupy $s_0, c_1$.
    2.  1 Úplná sčítačka (FA) pro bit 1: vstupy $a_1, b_1, c_1$; výstupy $s_1, c_2 (=s_2)$.
*   **Schéma:** Nakreslete 1 HA a 1 FA a propojte je podle logiky.

**16. Minimální počet klopných obvodů**

*   Pro realizaci sekvenčního obvodu s N vnitřními stavy je potřeba minimálně k klopných obvodů, kde k je nejmenší celé číslo splňující $2^k \ge N$.
*   Zde N = 18.
*   $2^4 = 16$ (nedostatečné)
*   $2^5 = 32$ (dostatečné)
*   Minimálně je potřeba **5** klopných obvodů.

**17. Počet tranzistorů v hradle NAND (CMOS)**

*   Standardní 2-vstupové hradlo NAND v technologii CMOS se skládá ze 4 tranzistorů:
    *   2 tranzistory NMOS zapojené v sérii (pull-down síť).
    *   2 tranzistory PMOS zapojené paralelně (pull-up síť).
*   Obecně, n-vstupové CMOS NAND hradlo má $2n$ tranzistorů.

**18. Minterm a literál**

*   **Literál:** Booleovská proměnná (např. A) nebo její negace (např. $\neg A$).
*   **Minterm:** Součinový term (AND term), který obsahuje právě jeden literál pro každou proměnnou funkce. Každý minterm odpovídá právě jedné kombinaci vstupních proměnných (jednomu řádku pravdivostní tabulky nebo jednomu vrcholu n-krychle). Minterm má hodnotu 1 pouze pro tuto jedinou vstupní kombinaci. Příklad pro f(a,b,c): minterm $m_3 = \neg a b c$ je 1 pouze pro vstup abc=011.

**19. Algebraický výraz pro M3(a,b,c)**

*   Minterm M3 (nebo $m_3$) odpovídá binární hodnotě 3, což pro proměnné (a,b,c) znamená $a=0, b=1, c=1$.
*   Algebraický výraz se tvoří tak, že proměnné s hodnotou 0 jsou negovány a proměnné s hodnotou 1 zůstávají.
*   $M_3(a,b,c) = \neg a \land b \land c$ (často psáno jako $\neg a b c$).

**20. Dekodér 4-bit na 1zN**

*   **Popis:** Dekodér ze 4 bitů na "1 z N" (4-to-N decoder). Jedná se o kombinační logický obvod.
*   **N:** Protože jsou 4 vstupní bity, existuje $2^4 = 16$ možných vstupních kombinací. Proto N=16. Je to dekodér 4-na-16.
*   **Vstupy:** 4 datové vstupy (např. $A_3, A_2, A_1, A_0$). Často má i vstup povolení (Enable - E).
*   **Výstupy:** 16 výstupů ($Y_0, Y_1, ..., Y_{15}$). Právě jeden výstup $Y_i$ je aktivní (např. v log. 1), pokud binární hodnota na vstupech A odpovídá indexu i. Ostatní výstupy jsou neaktivní (např. v log. 0).
*   **Kombinační/Sekvenční:** Je to **kombinační** obvod.
*   **Struktura:** Typicky se realizuje pomocí 16 AND hradel (každé se 4 vstupy) a 4 invertorů pro generování negovaných vstupů. Každé AND hradlo realizuje jeden minterm vstupních proměnných. Např. $Y_0 = \neg A_3 \neg A_2 \neg A_1 \neg A_0$, $Y_{15} = A_3 A_2 A_1 A_0$. Pokud je přítomen vstup E, je přiveden na vstup každého AND hradla.
*   **Schéma:** Nakreslete blokové schéma se 4 vstupy (+E) a 16 výstupy.

**21. Aritmetický posuv (8-bit, doplňkový kód)**

*   **Popis:** Obvod pro aritmetický posuv 8-bitového čísla v doplňkovém kódu o 2 bity vpravo (ASR, dělení 4) nebo vlevo (ASL, násobení 4).
*   **ASR o 2 bity:** Znaménkový bit (MSB, b7) se kopíruje na dvě nejvyšší uvolněné pozice. Bity se posunou doprava, LSB (b1, b0) se ztrácí.
    *   Výstup O: $O_7=Q_7, O_6=Q_7, O_5=Q_7, O_4=Q_6, O_3=Q_5, O_2=Q_4, O_1=Q_3, O_0=Q_2$
    *   Detekce ztráty přesnosti: $Loss = Q_1 \lor Q_0$ (Ztráta, pokud byl ztracen nenulový bit).
*   **ASL o 2 bity:** Bity se posunou doleva, na uvolněné pozice (LSB, b1, b0) se doplní nuly. MSB (b7, b6) se ztrácí.
    *   Výstup O: $O_7=Q_5, O_6=Q_4, O_5=Q_3, O_4=Q_2, O_3=Q_1, O_2=Q_0, O_1=0, O_0=0$
    *   Detekce přetečení (Overflow): K přetečení dojde, pokud se během posuvu změní znaménko (tj. bit posunutý na pozici znaménka se liší od původního znaménka). Pro posuv o 2: $V = (Q_7 \oplus Q_6) \lor (Q_6 \oplus Q_5)$.
*   **Schéma:** Nakreslete 8 vstupních linek (Q7-Q0) a 8 výstupních (O7-O0). Pro ASR a ASL nakreslete pevné propojení podle vzorců výše (lze použít MUX pro volbu směru). Přidejte logická hradla (OR, XOR) pro výpočet signálů Loss (pro ASR) a Overflow (pro ASL).

**22. Aritmetický posuv (8-bit, přímý kód)**

*   **Popis:** Obvod pro aritmetický posuv 8-bitového čísla v přímém kódu (MSB=znaménko, b6-b0=absolutní hodnota) o 2 bity vpravo/vlevo.
*   **ASR o 2 bity:** Znaménkový bit (b7) zůstává. Absolutní hodnota (b6-b0) se logicky posune doprava o 2 bity (doplní se nuly zleva). Bity b1, b0 se ztrácí.
    *   Výstup O: $O_7=Q_7, O_6=0, O_5=0, O_4=Q_6, O_3=Q_5, O_2=Q_4, O_1=Q_3, O_0=Q_2$
    *   Detekce ztráty přesnosti: $Loss = Q_1 \lor Q_0$
*   **ASL o 2 bity:** Znaménkový bit (b7) zůstává. Absolutní hodnota (b6-b0) se logicky posune doleva o 2 bity (doplní se nuly zprava). Bity b6, b5 se ztrácí.
    *   Výstup O: $O_7=Q_7, O_6=Q_4, O_5=Q_3, O_4=Q_2, O_3=Q_1, O_2=Q_0, O_1=0, O_0=0$
    *   Detekce přetečení (Overflow): K přetečení dojde, pokud byl z absolutní hodnoty vysunut nenulový bit. $V = Q_6 \lor Q_5$.
*   **Schéma:** Podobné jako u Q21, ale s jiným propojením a logikou detekce. Nakreslete vstupy, výstupy, pevné propojení pro ASR/ASL a hradla pro Loss/Overflow.

**23. Rozšíření řádové mřížky (4 na 8 bitů, doplňkový kód)**

*   **Popis:** Obvod pro znaménkové rozšíření (sign extension) 4-bitového čísla v doplňkovém kódu (B = $b_3b_2b_1b_0$) na 8-bitové číslo (W = $w_7w_6w_5w_4w_3w_2w_1w_0$).
*   **Logika:** Nižší 4 bity výstupu jsou stejné jako vstupní bity. Vyšší 4 bity výstupu jsou kopiemi znaménkového bitu původního čísla ($b_3$).
    *   $w_0 = b_0$
    *   $w_1 = b_1$
    *   $w_2 = b_2$
    *   $w_3 = b_3$
    *   $w_4 = b_3$
    *   $w_5 = b_3$
    *   $w_6 = b_3$
    *   $w_7 = b_3$
*   **Schéma:** Velmi jednoduché. Nakreslete 4 vstupní linky (b3-b0) a 8 výstupních (w7-w0). Propojte b0-b3 přímo na w0-w3. Propojte b3 také na w4, w5, w6, w7 (rozbočení signálu b3).

**24. Detekce přetečení při sčítání v doplňkovém kódu**

*   Při sčítání $A + B = S$ v n-bitovém doplňkovém kódu dochází k přetečení (nesprávný výsledek), pokud výsledek leží mimo reprezentovatelný rozsah $[ -2^{n-1}, 2^{n-1}-1 ]$.
*   **Výrazy pro detekci přetečení (V):**
    1.  **Pomocí znamének:** Přetečení nastane, pokud oba sčítance mají stejné znaménko a výsledek má znaménko opačné.
        $V = (A_{n-1} \land B_{n-1} \land \neg S_{n-1}) \lor (\neg A_{n-1} \land \neg B_{n-1} \land S_{n-1})$
        (kde $X_{n-1}$ je znaménkový bit čísla X)
    2.  **Pomocí přenosů:** Přetečení nastane, pokud přenos do znaménkového bitu ($C_{n-1}$) se liší od přenosu ze znaménkového bitu ($C_n$).
        $V = C_{n-1} \oplus C_n$

**25. Detekce přetečení při odčítání v doplňkovém kódu**

*   Odčítání $A - B$ se realizuje jako sčítání $A + (\neg B + 1)$.
*   **Výrazy pro detekci přetečení (V):**
    1.  **Použitím pravidel pro sčítání:** Aplikujte pravidla z Q24 na operaci $A + (\text{complement } B)$.
        *   $V = C_{n-1} \oplus C_n$ (kde $C_{n-1}, C_n$ jsou přenosy při sčítání $A$ a doplňku $B$).
        *   $V = (A_{n-1} \land (\neg B)_{n-1} \land \neg S_{n-1}) \lor (\neg A_{n-1} \land \neg (\neg B)_{n-1} \land S_{n-1})$
    2.  **Přímo pomocí znamének A, B, S:** Přetečení nastane, pokud A a B mají *různá* znaménka a výsledek S má stejné znaménko jako B.
        $V = (\neg A_{n-1} \land B_{n-1} \land \neg S_{n-1}) \lor (A_{n-1} \land \neg B_{n-1} \land S_{n-1})$

**26. Detekce nesprávného výsledku při sčítání nezáporných čísel**

*   Při sčítání n-bitových nezáporných čísel $A + B = S$ dochází k nesprávnému výsledku (přetečení), pokud součet $S$ je větší než maximální hodnota reprezentovatelná n bity ($2^n - 1$).
*   **Výraz pro detekci:** K tomuto dochází právě tehdy, když vznikne přenos z nejvyššího řádu (z pozice n-1).
    *   Přetečení = $C_n$ (kde $C_n$ je přenos z MSB pozice).

**27. Rozdíl mezi přenosem (carry) a přetečením (overflow)**

*   **Přenos (Carry - C, $C_n$):**
    *   Je to bit přenosu generovaný při sčítání nejvyšších bitů (MSB) operandů.
    *   Je relevantní primárně pro **nezáporná (unsigned)** čísla. Signalizuje, že výsledek operace (typicky sčítání) překročil maximální hodnotu zobrazitelnou daným počtem bitů ($2^n - 1$).
    *   Používá se také při aritmetice s vyšší přesností (sčítání vícebytových čísel).
*   **Přetečení (Overflow - V):**
    *   Signalizuje, že výsledek aritmetické operace (sčítání, odčítání) se **znaménkovými (signed)** čísly leží mimo rozsah reprezentovatelný daným formátem (např. doplňkovým kódem, tj. mimo $[ -2^{n-1}, 2^{n-1}-1 ]$).
    *   Jeho detekce závisí na znaménkách operandů a výsledku, nebo na přenosech do a z znaménkového bitu ($V = C_{n-1} \oplus C_n$). **Není** to totéž co bit $C_n$.
*   **Rozdíl:** Carry indikuje překročení rozsahu u *nezáporné* interpretace. Overflow indikuje překročení rozsahu u *znaménkové* interpretace.
*   **Relevance pro kód se znaménkem:** Koncept **přetečení (Overflow - V)** je zásadní pro kódy čísel se znaménkem, zejména pro **doplňkový kód**, kde přímo signalizuje neplatný výsledek v rámci tohoto kódu. U přímého kódu se přetečení týká spíše překročení rozsahu absolutní hodnoty.
