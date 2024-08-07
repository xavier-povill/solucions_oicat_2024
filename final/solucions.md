# Solucions Final OIcat 2024

## Taula de continguts
* [Problema C1. Localitat més llunyana](#C1)
* [Problema C2. Quadrilàters](#C2)
* [Problema Q1. Suma de cubs (1)](#Q1)
* [Problema G1. Llonganisses](#G1)
* [Problema C3. Collaret de perles (1)](#C3)
* [Problema G2. Vestits](#G2)
* [Problema Q2. Superprimers](#Q2)
* [Problema C4. Més o menys?](#C4)
* [Problema Q3. Pocs palíndroms](#Q3)
* [Problema G3. Pinball](#G3)
* [Problema C5. Quadradets](#C5)
* [Problema Q4. Suma de cubs (2)](#Q4)
* [Problema C6. Laberint](#C6)
* [Problema C7. Collaret de perles (2)](#C7)
* [Problema C8. Arbres Red-Black](#C8)


## [Problema C1. Localitat més llunyana](https://jutge.org/problems/P37104_ca) <a name="C1"></a>

En aquest problema es donen dues ciutats, amb la seva distància en milles o quilòmetres, i s'ha de dir quina de les dues és la més llunyana (suposant que una milla equival exactament a 1609 metres). Conceptualment, aquest problema és molt senzill, però alguns participants van tenir errors per haver manipulat malament els nombres amb decimals. Per evitar-ho, la solució més senzilla consisteix en passar totes les quantitats a metres. Així, les quantitats en quilòmetres s'han de multiplicar per 1000 i les quantitats en milles s'han de multiplicar per 1609, i no fa falta utilitzar nombres amb decimals en cap moment.  

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
  string nom1, nom2;
  int dist1, dist2;
  string unitat1, unitat2;
  cin >> nom1 >> dist1 >> unitat1;
  cin >> nom2 >> dist2 >> unitat2;
  
  if(unitat1 == "milles") 
    dist1 *= 1609;
  else 
    dist1 *= 1000;

  if(unitat2 == "milles") 
    dist2 *= 1609;
  else 
    dist2 *= 1000;

  if(dist1 > dist2) 
    cout << nom1 << endl;
  else 
    cout << nom2 << endl;
}
```
</details>

<details>
  <summary><b>Codi (Python 3)</b></summary>

```py
from easyinput import read

def Convert(a):
  if a[2] == "milles":
    a[1] *= 1609
  else:
    a[1] *= 1000


a = read(str, int, str)
b = read(str, int, str)
Convert(a)
Convert(b)
if a[1] > b[1]:
  print(a[0])
else:
  print(b[0])
```
</details>

## [Problema C2. Quadrilàters](https://jutge.org/problems/P67380_ca) <a name="C2"/>

En aquest problema, donades les longituds de 4 segments, ens demanen si podem construir un quadrilàter amb ells. La clau del problema és adonar-se'n que es pot construir un quadrilàter si, i només si, el segment més llarg és més curt que la suma de la resta.

En el cas d'un triangle, la mateixa condició continua sent vàlida, i es coneix amb el nom de [desigualtat triangular](https://ca.wikipedia.org/wiki/Desigualtat_triangular). Així doncs, podrem construir un triangle amb costats $a \leq b \leq c$ sempre que es compleixi que $c < a + b$. 
<details>
  <summary><b>Demostració del cas del triangle (no del tot rigurosa)</b></summary>
Per veure-ho, observem que si $c \geq a + b$, amb els segments de longituds $a$ i $b$ no podem arribar a "tancar" el de longitud $c$ (en el cas $c = a + b$ sí que podríem, però la figura resultant tindria àrea zero, i no considerem que sigui un triangle). En canvi, si $c < a + b$, col·loquem el segment $c$ horitzontal amb els segments $a$ i $b$ verticals un a cada extrem, i anem rotant els segments $a$ i $b$ de manera que la part superior dels dos segments estigui a la mateixa altura. No és difícil convèncer-se que la condició $c < a + b$ implica que els extrems lliures dels dos segments s'acabaran trobant, tancant el triangle.
</details>

Un cop ho hem resolt pel triangle, podem utilitzar aquesta mateixa idea pel cas del quadrilàter:
<details>
  <summary><b>Demostració del cas del quadrilàter (no del tot rigurosa)</b></summary>
  Suposem que tenim quatre segments amb longituds $a \leq b \leq c \leq d$. Si $d \geq a + b + c$, igual que en el cas del triangle és fàcil veure que no es pot aconseguir un quadrilàter d'àrea no zero. Suposem doncs que $d < a + b + c$. Si $a = b = c = d$, podem construir un quadrat. Així doncs, podem assumir que no tots els segments són igual de llargs. Això implica que podem ajuntar els dos segments més petits i construir un triangle amb costats de longitud $d$, $c$ i $a + b$ (en efecte, es continua complint que $d < c + a + b$, i també que $a + b < c + d$, ja que $a \leq c$ i $b \leq d$, i la igualtat només es podria donar si els quatre costats fossin iguals). Un cop construït el triangle amb un costat de longitud $a + b$, podem ajuntar lleugerament els dos extrems del costat $a + b$, i subdividir-lo en dos costats de longitud $a$ i $b$, que ja no seran paral·lels. 

  Òbviament, aquesta no és una demostració totalment rigurosa, però esperem que serveixi per donar una idea intuïtiva de perquè la observació és certa.
</details>

La implementació és senzilla, ja sigui trobant el costat màxim i la suma, i comprovant que `suma - maxim > maxim`, o ordenant els quatre costats de petit a gran i comprovant la condició directament.

<details>
  <summary><b>Codi (C++)</b></summary>
  
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
  int n;
  cin >> n;
  while(n--) {
    int a, b, c, d;
    cin >> a >> b >> c >> d;
    int suma = a + b + c + d;
    int maxim = max(a, max(b, max(c, d)));
    if (suma - maxim > maxim)
      cout << "si" << endl;
    else
      cout << "no" << endl;
  }
}
```
</details>

<details>
  <summary><b>Codi (Python 3)</b></summary>

```py
from easyinput import read

n = read(int)
for _ in range(n):
  a = [read(int) for _ in range(4)]
  a.sort()
  if a[0] + a[1] + a[2] > a[3]:
    print("si")
  else:
    print("no")
```
</details>

## [Problema Q1. Suma de cubs (1)](https://jutge.org/problems/P43901_ca) <a name="Q1"/>

En aquest problema hem de trobar el nombre més petit que sigui la suma de 8 cubs consecutius i que sigui divisible per 2024 (a part del 2024 mateix). Per resoldre-ho, calculem totes les possibles sumes de cubs consecutius i comprovem per cadascuna si és divisible per 2024. 

<details>
  <summary><b>Codi (C++)</b></summary>
  
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
  for(int n = 1; n <= 100; ++n) {
    int suma_cubs = 0;
    for(int i = 0; i < 8; ++i) {
      suma_cubs += (n+i) * (n+i) * (n+i);
    }
    if(suma_cubs % 2024 == 0) {
      cout << suma_cubs << endl;
    }
  }
}
```
</details>

<details>
  <summary><b>Codi (Python 3)</b></summary>

```py
for n in range(100):
  suma_cubs = 0
  for i in range(8):
    suma_cubs += (n + i)**3
  if suma_cubs % 2024 == 0:
    print(suma_cubs)
```
</details>


## [Problema G1. Llonganisses](https://jutge.org/problems/P55377_ca) <a name="G1"/>

En aquest problema s'ha de crear una imatge representant una fusta de la qual pengen una sèrie de llonganisses de diferents mides. La única dificultat de la solució és implementar les instruccions donades sense equivocar-se.

Recordeu que podeu trobar una xuleta pels problemes gràfics a [https://lliçons.jutge.org/python/grafics/pil.html](https://lliçons.jutge.org/python/grafics/pil.html), amb exemples d'ús de les funcions més habituals.

<details>
  <summary><b>Codi (Python 3)</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read

# Dibuixa un rectangle:
#    nw = cantonada superior esquerra
#    se = cantonada inferior dreta
def DibuixaRectangle(nw, se, color):
  dib.polygon([nw, (nw[0], se[1]), se, (se[0], nw[1])], color)

n = read(int)
a = [read(int) for _ in range(n)]

alcada = 10 + 40 + max(a) + 20
llargada = 20 + 20*n + 10*(n-1) + 20
img = Image.new('RGB', (llargada, alcada), 'MidnightBlue')
dib = ImageDraw.Draw(img)

DibuixaRectangle((0,0), (llargada-1, 10 - 1), "Sienna")
x = 20
for i in range(n):
  DibuixaRectangle((x + 9, 10), (x + 10, 49), "White")
  dib.ellipse([(x, 50), (x + 19, 69)], "Grey")
  DibuixaRectangle((x, 60), (x + 19, a[i] + 39), "Grey")
  dib.ellipse([(x, a[i] + 30), (x + 19, a[i] + 49)], "Firebrick")
  x += 30

img.save('output.png')
```
</details>

## [Problema C3. Collaret de perles (1)](https://jutge.org/problems/P38607_ca) <a name="C3"/>

En aquest problema ens donen un collaret amb perles blanques i negres, i hem de comprovar si el collaret és vàlid (diem que un collaret és vàlid si no té 4 o més perles blanques seguides i no té 2 o més perles negres seguides). La dificultat principal del problema és comprovar que els extrems del collaret encaixin bé. Això es pot fer amb un bucle a part, o utilitzant mòduls en els índexos, tal i com es fa al codi a continuació. No obstant, si es fa d'aquesta última manera, s'ha d'anar en compte que el codi tracti bé els casos com `BBB`, que no té 4 perles blanques consecutives però sí que compleix que `a[i] = a[(i+1)%n] = a[(i+2)%n] = a[(i+3)%n] = 'B'`.

<details>
  <summary><b>Codi (C++)</b></summary>
  
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
  string s;
  while(cin >> s) {
    int n = s.size();
    bool valid = true;
    for(int i = 0; i < n; ++i) {
      if(s[i] == 'N' and s[(i+1)%n] == 'N') {
        valid = false;
        break;
      }
      if(n >= 4 and s[i] == 'B' and s[(i+1)%n] == 'B' and s[(i+2)%n] == 'B' and s[(i+3)%n] == 'B') {
        valid = false;
        break;
      }
    }
    cout << (valid ? "si" : "no") << endl;
  }
}
```
</details>

## [Problema G2. Vestits](https://jutge.org/problems/P15490_ca) <a name="G2"/>

En aquest problema ens donen $n$ colors de faldilles i $m$ colors de samarretes i hem de dibuixar una graella amb totes les possibles combinacions de colors. Novament, la dificultat principal és la implementació.

<details>
  <summary><b>Codi (Python3)</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read

# Dibuixa un rectangle:
#    nw = cantonada superior esquerra
#    se = cantonada inferior dreta
def DibuixaRectangle(nw, se, color):
  dib.polygon([nw, (nw[0], se[1]), se, (se[0], nw[1])], color)

n = read(int)
color_f = [read(str) for _ in range(n)]
m = read(int)
color_s = [read(str) for _ in range(m)]

img = Image.new('RGB', (100*m, 150*n), 'Grey')
dib = ImageDraw.Draw(img)

for i in range(n):
  for j in range(m):
    x = 100*j
    y = 150*i
    dib.ellipse([(x + 35, y + 5), (x + 64, y + 34)], "AntiqueWhite")
    DibuixaRectangle((x + 5, y + 40), (x + 94, y + 54), color_s[j])
    DibuixaRectangle((x + 30, y + 55), (x + 69, y + 94), color_s[j])
    dib.polygon([(x + 30, y + 100), (x + 69, y + 100), (x + 79, y + 144), (x + 20, y + 144)], color_f[i])

img.save('output.png')
```
</details>

## [Problema Q2. Superprimers](https://jutge.org/problems/P99345_ca) <a name="Q2"/>

En aquest problema ens demanen trobar el nombre més gran tal que qualsevol seqüència de dígits consecutius formi un nombre primer (suposem que l'1 és un nombre primer). Així doncs, el 137 seria un nombre vàlid, ja que totes les seqüències de dígits consecutius són nombres primers: '1', '3', '7', '13', '37', '137'.

Hi ha diverses maneres de resoldre'l. Si suposem d'un principi que la resposta no és massa gran, podem iterar pels nombres de 1 fins a $10^5$ (o un nombre gran similar), i comprovar per cadascun si és una resposta vàlida. Observem que si tinguéssim un nombre vàlid de $n$ dígits, traient-li l'últim dígit n'obtindríem un de $n-1$ dígits que també és vàlid. Per tant, com que no en trobem cap de 5 dígits, sabem que no n'hi haurà cap amb més de 5 dígits tampoc.

Una solució alternativa i més eficient consisteix en començar amb una llista amb tots els nombres vàlids de 1 dígit (1, 2, 3, 5 i 7), i anar construint iterativament els nombres vàlids de $n$ dígits a partir dels de $n-1$ dígits. Per fer-ho, podem iterar per totes les parelles de nombres vàlids de $n-1$ i de 1 dígit, i comprovar si el resultat de concatenar-los és primer.   

<details>
  <summary><b>Solució 1 (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

bool IsPrime(int x) {
  for(int i = 2; i*i <= x; ++i) {
    if(x%i == 0) return false;
  }
  return true;
}

bool IsSuperprime(int x) {
  string s = to_string(x);
  int k = s.size();
  for(int i = 0; i < k; ++i) {
    for(int j = i; j < k; ++j) {
      if(not IsPrime(stoi(s.substr(i, j-i+1))))
        return false;
    }
  }
  return true;
}

int main() {
  int N = 1e6;
  for(int i = 1; i < N; ++i) {
    if(IsSuperprime(i)) cout << i << endl;
  }
}
```
</details>

<details>
  <summary><b>Solució 2 (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

bool IsPrime(int x) {
  for(int i = 2; i*i <= x; ++i) {
    if(x%i == 0) return false;
  }
  return true;
}

int main() {
  vector<int> v = {1, 2, 3, 5, 7};
  set<int> superprimers;
  queue<int> q;
  for(int x : v) {
    superprimers.insert(x);
    q.push(x);
  }
  while(not q.empty()) {
    int x = q.front();
    q.pop();
    for(int i : v) {
      int y = 10*x + i;
      string s = to_string(y);
      reverse(s.begin(), s.end());
      s.pop_back();
      reverse(s.begin(), s.end());
      if(IsPrime(y) and superprimers.count(stoi(s))) {
        q.push(y);
        superprimers.insert(y);
      }
    }
  }
  for(int x : superprimers) {
    cout << x << endl;
  }
}
```
</details>

## [Problema C4. Més o menys?](https://jutge.org/problems/P17499_ca) <a name="C4"/>

En aquest problema, dues persones estan jugant a un joc d'endevinar un nombre de $1$ a $n$. Per cada intent de la primera persona, la segona persona li diu si el nombre secret és més gran, més petit, o si l'ha endevinat exactament. Al final, ens demanen dir si la segona persona ha respost de manera coherent (és a dir, si existeix un nombre coherent amb totes les respostes), i en cas que n'hi hagi un de sol, dir quin és.

Observem que, si la primera persona diu el nombre $x$ i la segona persona respon 'més', això elimina tots els nombres des de $1$ fins a $x$ com a possibles solucions. Analogament, si la segona persona respon 'menys', això elimina tots els nombres des de $x$ fins a $n$. Així doncs, ens podem guardar el nombre més petit i el nombre més gran que encara no s'han eliminat, i amb cada resposta 'més' fem `petit = max(petit, x+1)`, i amb cada resposta 'menys' fem `gran = min(gran, x-1)`. Al final, haurem de comprovar si `petit <= gran`.

Per processar les respostes 'sí' (és a dir, quan el primer jugador ha endevinat el nombre), observem que respondre 'sí' a $x$ és equivalent a respondre 'més' a $x-1$ i 'menys' a $x+1$. Alternativament, podríem tenir una variable booleana que ens diu si hem respost que sí ja a algun nombre, i guardar-nos també la $x$ a la que hem respost que sí, per comprovar que les respostes següents siguin coherents.   

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
  int n;
  while(cin >> n) {
    int minim = 1;
    int maxim = n;
    int x;
    while(cin >> x and x != 0) {
      string s;
      cin >> s;
      if(s == "mes")
        minim = max(minim, x + 1);
      else if(s == "menys")
        maxim = min(maxim, x - 1);
      else {
        minim = max(minim, x);
        maxim = min(maxim, x);
      }
    }
    if(minim > maxim) 
      cout << "trampa!" << endl;
    else if(minim == maxim) 
      cout << minim << endl;
    else 
      cout << "ok" << endl;
  }
}
```
</details>

## [Problema Q3. Pocs palíndroms](https://jutge.org/problems/P97623_ca) <a name="Q3"/>

En aquest problema ens demanen trobar la paraula de longitud 20 amb les lletres 'a' i 'b' que té el mínim nombre de subparaules palindròmiques.

Donat que el nombre de paraules de longitud 20 que podem fer amb 2 lletres és $2^{20} \cong 10^6$, podem iterar per totes les possibilitats i per cadascuna comprovar quantes subparaules palindròmiques té. Depèn de com ho implementem, el programa pot trigar una mica, però hauria d'acabar en uns pocs segons (que seria massa lent per enviar-lo al Jutge, però que no és un problema si només l'hem d'executar localment).

Per iterar per totes les paraules de longitud 20, podem utilitzar el següent truc. Observeu que hi ha una bijecció entre les paraules de longitud 20 amb les lletres 'a' i 'b' i els nombres de 20 dígits escrits en binari (per exemple, podem representar les 'a' com un '0' i les 'b' com un '1'). Així doncs, si iterem pels nombres des de $0$ fins a $2^{20} - 1$ (que són 20 '1's seguits en binari), i considerem que els '0' són 'a' i els '1' són 'b', estarem iterant per totes les paraules possibles de longitud 20. A més, estarem iterant en ordre lexicogràfic, cosa que ens ajuda perquè el problema ens demana donar la paraula lexicogràficament més petita (d'entre les que tenen el mateix nombre de subparaules palindròmiques).   

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
  int const n = 20;
  int ans = 1e9;
  for(int x = 0; x < (1 << n); ++x) {
    string s(n, 'a');
    for(int i = 0; i < n; ++i) {
      if(x&(1 << i)) s[n-1-i] = 'b';
    }
    int num_palindroms = 0;
    for(int i = 0; i < n; ++i) {
      for(int j = i; j < n; ++j) {
        bool palindrom = true;
        for(int k = 0; i+k < j-k and palindrom; ++k) {
          if(s[i+k] != s[j-k]) 
            palindrom = false;
        }
        if(palindrom) num_palindroms++;
      }
    }
    if(num_palindroms < ans) {
      ans = num_palindroms;
      cout << s << " : " << ans << endl;
    }
  }
}
```
</details>

## [Problema G3. Pinball](https://jutge.org/problems/P94446_ca) <a name="G3"/>

En aquest problema ens donen les especificacions d'una màquina de pinball, amb la posició dels obstacles, i ens demanen que dibuixem el recorregut que segueix la pilota, tenint en compte que aquesta gira 90 graus cada cop que xoca amb un obstacle. La clau del problema era adonar-se que és impossible que la pilota entri en un cicle (sabríeu dir per què?), així que en tenim prou amb simular el seu moviment fins a que surti de la graella.

La direcció actual de la pilota la podem guardar com un enter entre $0$ i $3$ (representant les 4 direccions cardinals), ja que llavors cada cop que es xoca augmentem la direcció en 1. Aleshores, els moviments els podem codificar a partir de dues llistes `dx = [0, 1, 0, -1]` i `dy = [-1, 0, 1, 0]`, de manera que si la direcció actual és `k`, la pilota es moura `dx[k]` caselles en l'eix horitzontal i `dy[k]` caselles en l'eix vertical.

<details>
  <summary><b>Codi (Python 3)</b></summary>

```py
from easyinput import read
from PIL import Image, ImageDraw

# Dibuixa un rectangle:
#    nw = cantonada superior esquerra
#    se = cantonada inferior dreta
def DrawRectangle(nw, se, color):
    dib.polygon([nw, (nw[0], se[1]), se, (se[0], nw[1])], color)


m, n = read(int,int)
a = [read(str) for _ in range(n)]

img = Image.new('RGB', (30*m, 30*n), 'Green')
dib = ImageDraw.Draw(img)

vist = [[False for _ in range(m)] for _ in range(n)]
pos = [m//2, n-1]
direccio = 0
dy = [-1, 0, 1, 0]
dx = [0, 1, 0, -1]
while pos[0] >= 0 and pos[0] < m and pos[1] >= 0 and pos[1] < n:
    vist[pos[1]][pos[0]] = True
    if a[pos[1]][pos[0]] == 'X':
        direccio = (direccio + 1) % 4
    pos[0] += dx[direccio]
    pos[1] += dy[direccio]

for i in range(n):
    for j in range(m):
        col = 'Blue'
        if vist[i][j]:
            DrawRectangle((30*j, 30*i), (30*j + 29, 30*i + 29), 'Yellow')
            col = 'Red'
        if a[i][j] == 'X':
            dib.ellipse([(30*j, 30*i), (30*j + 29, 30*i + 29)], col)

img.save('output.png')
```
</details>

## [Problema C5. Quadradets](https://jutge.org/problems/P76718_ca) <a name="C5"/>

En aquest problema tenim una quadrícula $n \times n$ (on la $n$ pot arribar fins a $10^9$) i $c \leq 10^5$ caselles marcades amb una creu. Volem saber el nombre de quadradets $2 \times 2$ que formen les creus, i el nombre de quadradets als quals els hi falta una creu per estar complets. No obstant, només considerem que un quadradet $2 \times 2$ és vàlid si no té cap altra creu en les 12 caselles que l'envolten.

Donat que la $n$ és massa gran com per guardar-nos la graella explícitament, podem guardar-nos la posició de les creus com un mapa `map<pair<int,int>, int>` que a cada casella marcada li assigna un identificador. Això ens permet fer un DFS per trobar tots els components connexos de caselles marcades, i per cada component comprovem si és un quadradet o un quasi-quadradet.

Per comprovar si un component és un quadradet $2 \times 2$, hem de veure que té mida 4 i que la diferència entre la màxima i la mínima $x$, i entre la màxima i mínima $y$ és com a molt $1$.

Per comprovar si un component és un quasi-quadradet, hem de veure que té mida 3 i que compleix la condició de ser un quadradet. Ara bé, podria passar que un quasi-quadradet no ho fos perquè si es completés la casella que falta per ser un quadradet, aquesta seria adjacent a una altra casella marcada (tot i que ara mateix el quasi-quadradet no és adjacent a cap casella marcada). Per comprovar aquest cas, donat un quasi-quadradet hem de trobar la casella que li falta, i comprovar els seus 3 veïns que no són adjacents al quasi-quadradet.

Al codi a continuació, trobem la casella que falta amb la fórmula `xmissing = sumx/3 + int(sumx%3 == 1)` (la idea és que si la $x$ més gran està repetida, la suma serà 2 mòdul 3, mentre que si és la $x$ petita la que està repetida, la suma serà 1 mòdul 3). Per comprovar els veïns no adjacents, utilitzem que són les 3 caselles oposades a les 3 caselles del quasi-quadradet. 

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

using ll = long long;
using ii = pair<int,int>;

vector<int> dx = {1, 1, 0, -1, -1, -1, 0, 1};
vector<int> dy = {0, -1, -1, -1, 0, 1, 1, 1};

int main()  {
  int n, m;
  while(cin >> n >> m) {
    vector<ii> v(m);
    map<ii, int> id;
    for(int i = 0; i < m; ++i) {
      cin >> v[i].first >> v[i].second;
      id[v[i]] = i;
    }
    vector<bool> vist(m, false);
    int quadrats = 0;
    int quasi = 0;

    function<void(int, vector<int>&)> dfs = [&](int i, vector<int>& comp) {
      ii pos = v[i];
      comp.push_back(i);
      for(int j = 0; j < 8; ++j) {
        ii newpos = pos;
        newpos.first += dx[j];
        newpos.second += dy[j];
        if(id.count(newpos)) {
          int newi = id[newpos];
          if(not vist[newi]) {
            vist[newi] = true;
            dfs(newi, comp);
          }
        }
      }
    };

    auto EsQuadrat = [&](vector<int> const& comp) {
      int xmin = n;
      int xmax = 1;
      int ymin = n;
      int ymax = 1;
      for(int i : comp) {
        xmin = min(xmin, v[i].first);
        xmax = max(xmax, v[i].first);
        ymin = min(ymin, v[i].second);
        ymax = max(ymax, v[i].second);
      }
      return xmax <= xmin + 1 and ymax <= ymin + 1;
    };

    auto EsQuasiQuadrat = [&](vector<int> const& comp) {
      if(not EsQuadrat(comp)) return false;
      ll sumx = 0, sumy = 0;
      for(int i : comp) {
        sumx += v[i].first;
        sumy += v[i].second;
      }
      int xmissing = sumx/3 + int(sumx%3 == 1);
      int ymissing = sumy/3 + int(sumy%3 == 1);

      vector<ii> oposats;
      for(int i : comp) {
        ii p = {xmissing + (xmissing-v[i].first), ymissing + (ymissing - v[i].second)};
        oposats.push_back(p);
      }
      for(ii const& p : oposats) {
        if(id.count(p)) return false;
      }
      return true;
    };

    for(int i = 0; i < m; ++i) {
      if(not vist[i]) {
        vector<int> comp;
        vist[i] = true;
        dfs(i, comp);
        if(comp.size() == 4 and EsQuadrat(comp)) 
          ++quadrats;
        if(comp.size() == 3 and EsQuasiQuadrat(comp))
          ++quasi;
      }
    }
    cout << quadrats << " " << quasi << endl;
  }
}
```
</details>

## [Problema Q4. Suma de cubs (2)](https://jutge.org/problems/P52098_ca) <a name="Q4"/>

Si en el problema Q1 demanàvem quin és el primer nombre que és la suma de 8 cubs consecutius i que és divisible per 2024 (a part del 2024 mateix), en aquest problema demanem quants nombres hi ha que compleixen aquesta propietat (on el cub més petit va des de $1$ fins a $2024^{2024}$). 

És a dir, volem trobar per a quantes $n$ amb $1 \leq n \leq 2024^{2024}$ es compleix que 

$$n^3 + (n+1)^3 + (n+2)^3 + (n+3)^3 + (n+4)^3 + (n+5)^3 + (n+6)^3 + (n+7)^3 \equiv 0  \mod 2024 $$

Si desenvolupéssim aquesta expressió, obtindríem un polinomi en la variable $n$ (on el grau màxim és 3, de fet). Això és molt útil, ja que per qualsevol $k \geq 0$ tenim que

<p>$$(n + 2024)^k = \sum_{i = 0}^k \binom{k}{i} n^i \cdot 2024^{k-i} \equiv n^k \mod 2024$$</p>

Per tant, si sumem 2024 a $n$, el mòdul de la suma de cubs no canvia, així que si la suma de cubs és múltiple de 2024 per a una certa $n$, també ho serà per a $n + 2024$, $n + 2\cdot 2024$, $n + 3 \cdot 2024$, i així successivament.

Així doncs, només hem de calcular quantes sumes de cubs són múltiples de 2024 per les $n$ entre $1$ i $2024$, i multiplicar el resultat per $2024^{2024}/2024$. 

<details>
  <summary><b>Codi (Python 3)</b></summary>

```py
valids = 0
for i in range(1, 2025):
  suma_cubs = 0
  for j in range(8):
    suma_cubs += (i + j)**3
  if suma_cubs % 2024 == 0:
    valids += 1

total = 2024**2024 * valids // 2024

suma_digits = 0
while total:
  suma_digits += total%10
  total //= 10

print("Suma dels digits: ", suma_digits)
```
</details>

## [Problema C6. Laberint](https://jutge.org/problems/P34055_ca) <a name="C6"/>

En aquest problema hem de dir quin és el mínim nombre d'obstacles que hem d'enderrocar per poder sortir d'un laberint (donat en forma de graella, amb certes caselles bloquejades).

El problema té un subcas que compta el 60% dels punts i que consisteix en casos on ens garanteixen que la resposta és sempre 0 o 1. Per resoldre'l, fem un DFS (o BFS) des de la posició inicial i mirem si arribem a sortir de la graella. Si és així, la resposta és 0; mentre que si no podem sortir, haurem d'enderrocar com a mínim un obstacle, i la resposta serà 1.

Tingueu en compte que el programa es pot passar del límit de temps si continueu la cerca un cop heu trobat una sortida del laberint, tot i que això no canviaria la complexitat assimptòtica (que és $\mathcal{O}(n + m)$, on $n$ i $m$ són el nombre de files i columnes de la graella).

<details>
  <summary><b>Solució parcial (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

using ii = pair<int,int>;
using iii = pair<int,ii>;

int const INF = 1e9;
vector<int> dr = {1, 0, -1, 0};
vector<int> dc = {0, 1, 0, -1};

int main() {
  int n, m;
  while(cin >> n >> m) {
    vector<string> v(n);
    for(string& s : v) 
      cin >> s;
    ii inicial;
    for(int r = 0; r < n; ++r) {
      for(int c = 0; c < m; ++c) {
        if(v[r][c] == 'I') {
          inicial = {r, c};
        }
      }
    }
    vector<vector<bool>> vist(n, vector<bool>(m, false));
    vist[inicial.first][inicial.second] = true;
    bool sortit = false;
    queue<ii> q;
    q.push(inicial);
    while(not q.empty()) {
      ii x = q.front();
      q.pop();
      int r = x.first;
      int c = x.second;
      if(r == 0 or c == 0 or r == n-1 or c == m-1) {
        sortit = true;
        break;
      }
      for(int k = 0; k < 4; ++k) {
        int rnou = r + dr[k];
        int cnou = c + dc[k];
        if(v[rnou][cnou] != 'X' and not vist[rnou][cnou]) {
          vist[rnou][cnou] = true;
          q.push({rnou, cnou});
        }
      }
    }
    cout << (sortit ? 0 : 1) << endl;
  }
}
```
</details>

Per resoldre els casos on hem d'enderrocar un nombre més gran d'obstacles, hem de transformar el problema en un problema de grafs amb pesos ('weighted graphs' en anglès). Per fer-ho, ens muntem un graf dirigit on cada casella és un vèrtex, i hi ha un arc de la casella $v$ a la casella $w$ si aquestes caselles comparteixen un costat en la graella. Per tal de representar les caselles bloquejades, li assignem un pes de $1$ als arcs que arriben a una casella bloquejada, i $0$ als arcs que arriben a una casella lliure.

Aleshores, el nombre de caselles bloquejades per les quals passem en un camí serà la suma dels costos dels arcs que el formen. Per tant, el problema es redueix a trobar el camí de mínim cost que surti de la graella.

La manera estàndard de trobar camins de mínim cost és amb l'[algorisme de Dijkstra](https://cp-algorithms.com/graph/dijkstra.html), que et troba el mínim cost per arribar des d'un vèrtex fix a tota la resta de vèrtexos del graf. Aquest algorisme té una complexitat assimptòtica de $\mathcal{O}((v+e) \log v)$ ([aquí](https://cp-algorithms.com/graph/dijkstra_sparse.html) podeu trobar una implementació més eficient). Per aquest problema, aquesta solució ja és prou bona, i hauria d'entrar per temps.

<details>
  <summary><b>Solució amb Dijkstra (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

using ii = pair<int,int>;
using iii = pair<int,ii>;

int const INF = 1e9;
vector<int> dr = {1, 0, -1, 0};
vector<int> dc = {0, 1, 0, -1};

int main() {
  int n, m;
  while(cin >> n >> m) {
    vector<string> v(n);
    for(string& s : v) 
      cin >> s;
    ii inicial;
    for(int r = 0; r < n; ++r) {
      for(int c = 0; c < m; ++c) {
        if(v[r][c] == 'I') {
          inicial = {r, c};
        }
      }
    }
    vector<vector<int>> dist(n, vector<int>(m, INF));
    dist[inicial.first][inicial.second] = 0;
    priority_queue<iii, vector<iii>, greater<iii>> pq;
    pq.push({0, inicial});
    int ans = -1;
    while(not pq.empty()) {
      iii x = pq.top();
      pq.pop();
      int d = x.first;
      int r = x.second.first;
      int c = x.second.second;
      if(r == 0 or c == 0 or r == n-1 or c == m-1) {
        ans = d;
        break;
      }
      for(int k = 0; k < 4; ++k) {
        int rnou = r + dr[k];
        int cnou = c + dc[k];
        int increment = int(v[rnou][cnou] == 'X');
        if(dist[rnou][cnou] > dist[r][c] + increment) {
          dist[rnou][cnou] = dist[r][c] + increment;
          pq.push({dist[rnou][cnou], {rnou, cnou}});
        }
      }
    }
    cout << ans << endl;
  }
}
```
</details>

Si volem filar més prim, podem utilitzar el fet que els pesos dels arcs són sempre 0 o 1 per obtenir una solució més eficient. A l'algorisme de Dijkstra, el factor logarítmic apareix perquè hem d'anar insertant els vèrtexos a una cua de prioritat, mantenint sempre el vèrtex més proper com el primer de la cua. Si els pesos dels arcs només poden ser 0 o 1, tenim que les distàncies dels vèrtexs de la cua diferiran en com a molt 1, de manera que la llista ordenada de distàncies dels vèrtexos de la cua tindria la forma $(d,\dots, d, d+1, \dots, d+1)$ o bé $(d, \dots, d)$. En aquest cas, per mantenir la cua ordenada només ens fa falta poder insertar un vèrtex al principi (si té distància $d$) o al final (si té distància $d + 1$). Per tant, ho podem implementar amb una `deque` en cost lineal.

Aquesta tècnica es coneix com a [0/1-BFS](https://cp-algorithms.com/graph/01_bfs.html).

<details>
  <summary><b>Solució amb 0/1-BFS (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

using ii = pair<int,int>;
using iii = pair<int,ii>;

int const INF = 1e9;
vector<int> dr = {1, 0, -1, 0};
vector<int> dc = {0, 1, 0, -1};

int main() {
  int n, m;
  while(cin >> n >> m) {
    vector<string> v(n);
    for(string& s : v) 
      cin >> s;
    ii inicial;
    for(int r = 0; r < n; ++r) {
      for(int c = 0; c < m; ++c) {
        if(v[r][c] == 'I') {
          inicial = {r, c};
        }
      }
    }
    vector<vector<int>> dist(n, vector<int>(m, INF));
    dist[inicial.first][inicial.second] = 0;
    deque<iii> dq;
    dq.push_back({0, inicial});
    int ans = -1;
    while(not dq.empty()) {
      iii x = dq.front();
      dq.pop_front();
      int d = x.first;
      int r = x.second.first;
      int c = x.second.second;
      if(r == 0 or c == 0 or r == n-1 or c == m-1) {
        ans = d;
        break;
      }
      for(int k = 0; k < 4; ++k) {
        int rnou = r + dr[k];
        int cnou = c + dc[k];
        int increment = int(v[rnou][cnou] == 'X');
        if(dist[rnou][cnou] > dist[r][c] + increment) {
          dist[rnou][cnou] = dist[r][c] + increment;
          if(dq.empty() or dq.front().first >= dist[rnou][cnou])
            dq.push_front({dist[rnou][cnou], {rnou, cnou}});
          else
            dq.push_back({dist[rnou][cnou], {rnou, cnou}});
        }
      }
    }
    cout << ans << endl;
  }
}
```
</details>

## [Problema C7. Collaret de perles (2)](https://jutge.org/problems/P89236_ca) <a name="C7"/>

En aquest problema ens donen un enter $n$ i ens demanen quants collarets de longitud $n$ hi ha que compleixin les condicions del problema C3.

Hi havia tres subcasos amb puntuació parcial. Obteníem un 25% de la puntuació per resoldre els casos amb $n \leq 15$, un altre 25% per resoldre els casos amb $n \leq 30$, un 25% per resoldre els casos amb $n \leq 10^5$, i el 25% restant per resoldre casos amb $n \leq 10^{12}$.

Per resoldre els casos amb $n \leq 15$, en tenim prou amb fer un programa que construeixi tots els collarets possibles i comprovi per cadascun si és vàlid. Per generar tots els collarets podem utilitzar el mateix truc que en el problema Q3, codificant cada collaret com una seqüència de $n$ zeros i uns que correspondrà a un nombre entre $0$ i $2^n - 1$. 

Al codi següent ho fem d'una manera diferent, generant-los amb una funció recursiva. Tenim una variable global `s`, on ens guardem el collaret que estem construint, i una funció `Resol`, que rep la posició actual que hem d'omplir, la omple de les dues maneres possibles i crida la funció `Resol` per la següent posició per cada una d'elles.

<details>
  <summary><b>Solució parcial 1 (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

string s;

bool Valid() {
  int n = s.size();
  for(int i = 0; i < n; ++i) {
    if(s[i] == 'N' and s[(i+1)%n] == 'N') 
      return false;
    if(n >= 4 and s[i] == 'B' and s[(i+1)%n] == 'B' and s[(i+2)%n] == 'B' and s[(i+3)%n] == 'B')
      return false;
  }
  return true;
}

int Resol(int actual, int n) {
  if(actual == n) {
    if(Valid()) return 1;
    return 0;
  }
  int resposta = 0;
  s[actual] = 'B';
  resposta += Resol(actual+1, n);
  s[actual] = 'N';
  resposta += Resol(actual+1, n);
  return resposta;
}

int main() {
  int n;
  while(cin >> n) {
    s = string(n, '?');
    cout << Resol(0, n) << endl;
  }
}
```
</details>

El codi anterior té complexitat $\mathcal{O}(n \cdot 2^n)$, ja que generem els $2^n$ possibles collarets i per cadascun comprovem si és vàlid. Per millorar-ho, cal adonar-se que la condició perquè un collaret sigui vàlid és una condició "local", en el sentit que si al començament del collaret hi tenim `BBBB` o `NN`, ja sabem que cap manera possible de completar el collaret serà vàlida. Així doncs, en el moment en què trobem un `BBBB` o `NN` podem parar de generar, estalviant-nos bona part dels $2^n$ collarets possibles. Aquesta tècnica es coneix com a [backtracking](https://ca.wikipedia.org/wiki/Backtracking)).

En el codi a continuació implementem la idea anterior, i això ens permet passar els casos amb $n \leq 30$. A cada posició, abans de provar amb una `N` comprovem que el collaret no acabi ja amb `N`, i abans de posar una `B` comprovem que el collaret no acabi ja amb `BBB`. Així ens estalviem de continuar generant el collaret si sabem segur que aquest serà invàlid.

<details>
  <summary><b>Solució parcial 2 (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

string s;

bool Valid() {
  int n = s.size();
  if(s[0] == 'N' and s[n-1] == 'N')
    return false; 
  if(n < 4) return true;
  for(int i = 0; i < 4; ++i) {
    int j = n-1-i;
    if(s[j] == 'B' and s[(j+1)%n] == 'B' and s[(j+2)%n] == 'B' and s[(j+3)%n] == 'B')
      return false;
  }
  return true;
}

int Resol(int actual, int n) {
  if(actual == n) {
    if(Valid()) return 1;
    return 0;
  }
  int resposta = 0;
  if(actual < 3 or s[actual-1] == 'N' or s[actual-2] == 'N' or s[actual-3] == 'N') {
    s[actual] = 'B';
    resposta += Resol(actual+1, n);
  }
  if(actual == 0 or s[actual-1] == 'B') {
    s[actual] = 'N';
    resposta += Resol(actual+1, n);
  }
  return resposta;
}

int main() {
  int n;
  while(cin >> n) {
    s = string(n, '?');
    cout << Resol(0, n) << endl;
  }
}
```
</details>

Per resoldre els casos amb $n \leq 10^5$ necessitem canviar radicalment l'enfocament del problema, comptant el nombre de collarets sense necessàriament haver d'iterar per tots els collarets vàlids. Per fer-ho, utilitzem la tècnica coneguda com a [programació dinàmica](https://aprende.olimpiada-informatica.org/algoritmia-dinamica-1).

La idea és que podem calcular el nombre de collarets de longitud $n$ a partir dels de longituds menors. La manera més clara de veure-ho és definir-se una funció $f(n)$ que ens diu el nombre de collarets vàlids de longitud $n$ <u>que acaben en `N`</u> i una funció $g(n)$ que ens diu el nombre de collarets vàlids de longitud $n$ <u>que acaben en `B`</u>.

Aleshores, tenim que $f(n) = g(n-1)$ (ja que si volem posar una perla negra, la última ha de ser blanca) i $g(n) = f(n-1) + f(n-2) + f(n-3)$ (ja que si volem posar una blanca, tenim que el collaret pot acabar en `...N`, `...NB` o `...NBB`). Substituint la segona equació en la primera, obtenim que $f(n) = f(n-2) + f(n-3) + f(n-4)$. 

Per assegurar-nos que els extrems del collaret encaixen sense generar cap seqüència `BBBB` o `NN`, a la funció `Resol()` iterem per tots els possibles començaments (`N...`, `BN...`, `BBN...`, `BBBN...`) i per tots els possibles acabaments (`...N`, `...NB`, `...NBB`, `...NBBB`) i comprovem si encaixen bé.  

<details>
  <summary><b>Solució parcial 3 (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int const MOD = 1e8 + 7; 
int const N = 1e5;
vector<int> dp;

int Resol(int n) {
  int ans = 0;
  for(int esq = 0; esq <= 3; ++esq) {
    for(int dre = 0; dre <= 3; ++dre) {
      if(esq + dre >= 4) continue;
      if(esq + dre == 0) continue;
      if(esq + dre >= n) continue;
      ans += dp[n-esq-dre];
    }
  }
  if(n <= 3) ans++; // tot B's
  return ans%MOD;
}

int main(){
  // dp[i] := nombre de paraules vàlides (sense comprovar que els 
  //          extrems encaixin bé) de llargada 'i' que comencen i
  //          acaben amb la lletra 'N'
  dp = vector<int>(N, 0); 
  dp[1] = 1;
  dp[2] = 0;
  dp[3] = 1;
  dp[4] = 1;
  int max_n = 4;

  int n;
  while(cin >> n) {
    for(int i = max_n + 1; i <= n; ++i) {
      dp[i] = (dp[i-2] + dp[i-3] + dp[i-4]) % MOD;
    }
    max_n = max(max_n, n);
    cout << Resol(n) << endl;
  } 
}
```
</details>

Per resoldre l'últim cas, amb $n \leq 10^{12}$, utilitzem la següent optimització, típica de les DPs "lineals" (és a dir, que satisfan una recurrència de l'estil $f(n) = a_1 \cdot f(n-1) + a_2 \cdot f(n-2) + \dots + a_k \cdot f(n-k)$ per un cert $k$ no gaire gran).

La idea és que si ens escrivim els últims $k$ valors en un vector, podem calcular el vector següent multiplicant-lo per una matriu:
<p>$$ \begin{pmatrix}f(n) \\ f(n-1) \\ \vdots \\ f(n-k+1) \end{pmatrix} = \begin{pmatrix} a_1 & a_2 & \dots & a_k \\ 1 & 0 & \dots & 0 \\ 0 & 1 & \dots & 0 \\ \vdots &&\ddots & \\ 0 & 0 & \dots & 1 \end{pmatrix} \cdot \begin{pmatrix}f(n-1) \\ f(n-2) \\ \vdots \\ f(n-k) \end{pmatrix} $$</p>

Iterant aquest procediment, tenim que podem calcular $f(n)$ multiplicant els valors inicials per una certa potència de la matriu:
<p>$$ \begin{pmatrix}f(n) \\ f(n-1) \\ \vdots \\ f(n-k+1) \end{pmatrix} = \begin{pmatrix} a_1 & a_2 & \dots & a_k \\ 1 & 0 & \dots & 0 \\ 0 & 1 & \dots & 0 \\ \vdots &&\ddots & \\ 0 & 0 & \dots & 1 \end{pmatrix}^{n-k} \cdot \begin{pmatrix}f(k) \\ f(k-1) \\ \vdots \\ f(1) \end{pmatrix} $$</p>

La gràcia d'aquest mètode és que podem calcular la potència $n$-èssima d'una matriu $m \times m$ en cost $\mathcal{O}(m^3 \log n)$, utilitzant [exponenciació binària](https://cp-algorithms.com/algebra/binary-exp.html).

<details>
  <summary><b>Solució completa (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;
 
using ll = long long;
int const MOD = 1e8 + 7; 
int const N = 1e5;
vector<vector<ll>> A;

vector<ll> Producte(vector<vector<ll>> const& A, vector<ll> const& v) {
  int r = A.size();
  int c = A[0].size();
  assert(int(v.size()) == c);
  vector<ll> ans(r, 0);
  for(int i = 0; i < r; ++i) {
    for(int j = 0; j < c; ++j) {
      ans[i] += (A[i][j] * v[j]) % MOD;
    }
    ans[i] %= MOD;
  }
  return ans;
}

vector<vector<ll>> Producte(vector<vector<ll>> const& A, vector<vector<ll>> const& B) {
  int r = A.size();
  int c = B[0].size();
  int m = B.size();
  vector<vector<ll>> ans(r, vector<ll>(c, 0));
  for(int i = 0; i < r; ++i) {
    for(int j = 0; j < c; ++j) {
      for(int k = 0; k < m; ++k) {
        ans[i][j] += (A[i][k] * B[k][j]) % MOD;
      }
      ans[i][j] %= MOD;
    }
  }
  return ans;
}

vector<vector<ll>> Exp(vector<vector<ll>> const& A, ll k) {
  int n = A.size();
  vector<vector<ll>> ans(n, vector<ll>(n, 0));
  if(k == 0) {
    for(int i = 0; i < n; ++i)
      ans[i][i] = 1;
    return ans;
  }
  ans = Exp(A, k/2);
  ans = Producte(ans, ans);
  if(k&1) ans = Producte(ans, A);
  return ans;
}

// Torna un vector amb {dp[n], dp[n-1], dp[n-2], dp[n-3]}.
// Pre: n >= 4
vector<ll> GetDP(ll n) {
  vector<ll> v = {1, 1, 0, 1}; // {dp[4], dp[3], dp[2], dp[1]}
  return Producte(Exp(A, n-4), v);
}

int Resol(ll n) {
  if(n == 1) return 2;
  if(n == 2) return 3;
  if(n == 3) return 4;
  vector<ll> dp = GetDP(n);
  int ans = 0;
  for(int esq = 0; esq <= 3; ++esq) {
    for(int dre = max(1, esq); dre + esq < min(4ll, n); ++dre) {
      if(esq == dre) ans += dp[esq+dre];
      else ans += 2 * dp[esq+dre];
    }
  }
  return ans%MOD;
}

int main(){
  A = vector<vector<ll>>(4, vector<ll>(4, 0));
  A[0][1] = A[0][2] = A[0][3] = A[1][0] = A[2][1] = A[3][2] = 1;

  ll n;
  while(cin >> n) {
    cout << Resol(n) << endl;
  } 
}
```
</details>

## [Problema C8. Arbres Red-Black](https://jutge.org/problems/P13747_ca) <a name="C8"/>

Aquest era el problema més difícil de la competició. La idea és anar construint l'arbre en pre-ordre, calculant el nombre d'arbres que hi ha si hi posem un vèrtex buit, un vèrtex vermell o un vèrtex negre, i triant la opció que toca depenent de l'índex de l'arbre que volem aconseguir.

Tot i que la idea és més o menys senzilla, la implementació està plena de detalls i no és gens fàcil quadrar-ho tot. Enhorabona si ho aconseguiu!

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include <iostream>
#include <vector>
using namespace std;


const int H = 6 + 1;
const int PW = (1<<(H+1));


using ll = long long;
using VL = vector<ll>;
using VVL = vector<VL>;
using VI = vector<int>;


const vector<char> car = { '.', 'R', 'B' };


VVL R(H, VL(H));  // R[h][b] = arbres amb arrel R, d'alcada h, amb b negres als camins
VVL AR(H, VL(H)); // acumulat per h
VVL B(H, VL(H));  // B[h][b] = arbres amb arrel B, d'alcada h, amb b negres als camins
VVL AB(H, VL(H)); // acumulat per h

VI prof(PW); // profunditat dels nodes de l'arbre binari (guardat com un heap)

VI qb(PW);  // nombre de bs amb que arribem a cada posicio del heap
VI res(PW); // 0: fulla; 1: red; 2: black;


int h;          // alcada objectiu
ll num;         // numero que encara falta
bool ok;        // indica si ja s'ha aconseguit alcada h
int bmin, bmax; // rang per a les bs possibles


ll quad(ll x) {
  return x*x;
}


void calcula_profs(int pos, int p) {
  if (pos >= PW) return;
  prof[pos] = p;
  calcula_profs(2*pos, p + 1);
  calcula_profs(2*pos + 1, p + 1);
}


void calcula(int pos, int fill, int b, ll& bons, ll& sobren) {
  if (pos == 0) return;

  if (fill == 2*pos) { // venim del fill esquerra
    if (res[pos] == 1) {
      bons *= AB[h-prof[pos]][b-qb[pos]];
      sobren *= AB[h-prof[pos]-1][b-qb[pos]];
    }
    else {
      bons *= AR[h-prof[pos]][b-qb[pos]] + AB[h-prof[pos]][b-qb[pos]];
      sobren *= AR[h-prof[pos]-1][b-qb[pos]] + AB[h-prof[pos]-1][b-qb[pos]];
    }
  }

  calcula(pos/2, pos, b, bons, sobren);
}


void solu(int pos) {
  if (prof[pos] > h) {
    res[pos] = 0;
    cout << car[0];
    return;
  }

  int bs = qb[pos] = qb[pos/2];
  if (bs == bmin) {
    ll bons = 1;
    ll sobren = 1;
    calcula(pos/2, pos, bmin, bons, sobren);
    if (not ok) bons -= sobren;

    if (bons > num) {
      res[pos] = 0;
      cout << car[0];
      bmax = bmin;
      return;
    }
    num -= bons;
  }

  if (res[pos/2] == 2) {
    ll quants = 0;
    for (int b = bmin; b <= bmax; ++b) {
      ll bons = AR[h-prof[pos]+1][b-bs];
      ll sobren = AR[h-prof[pos]][b-bs];
      calcula(pos/2, pos, b, bons, sobren);
      if (not ok) bons -= sobren;
      quants += bons;
    }

    if (quants > num) {
      res[pos] = 1;
      cout << car[1];
      if (prof[pos] == h) ok = true;
      bmax = min(bmax, bs + (h - prof[pos]));
      solu(2*pos);
      solu(2*pos + 1);
      return;
    }

    num -= quants;
  }

  ++qb[pos];
  res[pos] = 2;
  cout << car[2];
  if (prof[pos] == h) ok = true;
  bmin = max(bmin, bs + 1);
  solu(pos*2);
  solu(pos*2 + 1);
}


int main() {
  B[0][0] = AB[0][0] = 1;
  R[1][0] = AR[1][0] = 1;
  AB[1][0] = 1;
  B[1][1] = AB[1][1] = 1;

  for (h = 2; h < H; ++h) {
    for (int b = 0; b <= h; ++b) {
      R[h][b] = quad(AB[h-1][b]) - quad(AB[h-2][b]);
      AR[h][b] = AR[h-1][b] + R[h][b];
    }
    for (int b = 0; b <= h; ++b) {
      if (b) B[h][b] = quad(AR[h-1][b-1] + AB[h-1][b-1]) - quad(AR[h-2][b-1] + AB[h-2][b-1]);
      AB[h][b] = AB[h-1][b] + B[h][b];
    }
  }

  calcula_profs(1, 1);

  qb[0] = 0;
  res[0] = 2;

  while (cin >> h >> num) {
    --num;
    ok = false;
    bmin = h/2;
    bmax = h;
    solu(1);
    cout << endl;
    if (num != 0) cerr << "PIFIA!!! " << num << endl;
  }
}
```
</details>


