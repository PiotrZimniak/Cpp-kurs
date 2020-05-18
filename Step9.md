
# STEP 9 - Programowanie obiektowe cz.2
1. [Delegowanie konstruktorów](#delegowanie) 
2. [Wartości domyślne](#domyslne)
3. [Konstruktor kopiujący](#kopiujacy)
4. [r-value - konstruktor kopiujący (move constructor)](#move) 
5. [Słowo kluczowe this](#this)
6. [Const i klasy](#const)
7. [Atrybuty i metody statyczne w klasie](#static)
8. [Struktura a klasa](#struct)
---

<a name="delegowanie"></a>
### Delegowanie konstruktorw?

Podczas definicji konstróktorow możemy odwoływać się do innych ich implementacj. 
Pomaga to w zbędnym inicjowaniu czy powtarzaniu kodu

```c++

//kons.1
Gracz::Gracz(string _imie, int _lvl, int _xp)
    : imie { _imie }, lvl { _lvl }, xp { _xp }
{ }

//kons.2
Gracz::Gracz(string _imie)
    : Gracz { _imie, 0 , 0}
{ }

//kons.3
Gracz::Gracz()
    : Gracz { "Janek" }
{}
```

W przykładzie poniżej mamy trzy konstruktory klasy `Gracz`. 
* (kons.1) Przypisujemy wartości przekazane w parametrze do atrybutów klasy
* (kons.2) Jako parametr przyjmuje imie gracza i wywołuje `kons.1` z dodatkowymi 
parametrami (0,0)
* (kons.3) Konstruktor bezparametryczny, wywołuje `kons.2` z imieniem 'Janek', który
następnie wywołuje `kons.1`

---

<a name="domyslne"></a>
### Wartości domyślne parametrów?

Konstruktory tak jak zwykłe funkcje czy metody mogą posiadać domyślne wartości parametrów
których nie trzeba podawać przy tworzeniu obiektu.

```c++
//Gracz.h

class Gracz
{
    private:
        string nazwa;
        int xp;
    public:
        Gracz(string _nazwa, int _xp = 10);
}

//main.cpp

int main()
{
    Gracz zofia {"Zofia"};
    Gracz piotr {"Piotr", 1000000000};
}
```

W przykładzie powyżej widzimy utworzenie dwóch obiektów klasy `Gracz`. Korzystając
z tego samego konstruktora tworzymy gracza zofia z doświadczeniem równym 10 (przypisanym domyślnie)
jako że jest niedoświadczonym i nowym graczem, oraz gracza piotr który choć młody
to już na starcie bije od niego doświadczeniem i pewnością siebie.

---

<a name="kopiujacy"></a>
### Konstruktor kopiujący

Konstruktor kopiujący tworzy nowy obiekt klasy na podstawie referencji wartości przekazanego obiektu tego samego
typu. Definiuje się go tak samo jak zwyczajne konstruktory z tą róźnicą że jako parametr podajemy
referencje do innego obiektu. W definicji takiego konstruktora przepisujemy wartości z 
jednego obiektu do drugiego.
 
```c++
//Gracz.h
public Gracz
{
    private:
        string nazwa;
        int xp;
    public:
        Gracz(const Gracz &gracz);
}
```
```c++
//Gracz.cpp
Gracz::Gracz(const Gracz &gracz)
    : nazwa {gracz.nazwa}, xp {gracz.xp}
{ }
```

W większości przypadków tworzenie własnego konstruktora kopiującego nie jest wymagane.
Klasy domyślnie posiadają taki konstruktor. Jednak jeżeli nasza klasa posiada atrybuty
referencyjne, aby niedopuścić do skopiowania wskaźnika na to samo miejsce w pamięci
w dwóch róźnych obiektach, należy napisać własny konstruktor który zaalokuje nowe
miejsce dla atrybutu i przepisze jego wartość.

---

<a name="move"></a>
### R-Value - konstruktor kopiujcy

W `c++` rozróżniamy dwa typy wartości
* l-value
* r-value

Najprościej wytłumaczyć to tak że do wartości l-value (przez konstruktor czy bezpośrednio) możemy odwołać się dłużej
niż przez jedno wyrażanie (jedną linijke), natomiast do wartości r-value możemy odwołać się
tylko w wyrażeniu które występuję.

```c++
int a = 2 + 3;
```

W zapisie powyżej `a` jest typem `l-value` natomiast wyrażenie `2+3` jest typer `r-value`
ponieważ przy wykonaniu dodawania wynik `5` nie jest od razu zapisywaniany jako wartość zmiennej 
`a`. Mamy do niego dostęp jeszcze tylko w tym wyrażeniu np. możemy go zrzutować na inny typ.

```c++
float a = (float) 2 + 3;
```

Do wartości `r-values` możemy odwoływać się poprzez referencje tak jak i do wartości `l-values`. Zapis 
jest jednak inny ponieważ zamiast pojedyńczego `&` używamy `&&`.

```c++
int x { 100 }
int &l_ref = x; //referencja l-value
l_ref = 10; //zmiana x na 10

int &&r_ref = 200; //referencja r-value
r_ref = 300; //zmiana r_ref na 300

int &&x_ref = x; //błąd - do referencji r-value nie możemy przypisać 
                 //referencji l-value
```

Parametry referencyjne l-value
```c++
int x { 100 }; //x jest l-value

void func(int &num); // przykładowa deklaracja funkcji
                     // która przyjmuje za parametr referncje do l-value

func(x); // wywołanie func 
func(200); //Błąd - 200 jest r-value i nie można go przekazać jako parametr
           //dla tej funkcji
```

Parametry referencyjne r-value
```c++
int x { 100 }; //x jest l-value

void func(int &&num); // przykładowa deklaracja funkcji
                     // która przyjmuje za parametr referncje do r-value

func(200); // wywołanie func 
func(x); //Błąd - x jest l-value i nie można go przekazać jako parametr
           //dla tej funkcji
```

Parametry referencyjne r-value i l-value nie są tożsame
dlatego możemy definiować funkcje i metody któe przyjmują jako parametr
te dwie referencje.

```c++
int x { 100 }; //x jest l-value

void func(int &&num);
void func(int &num);

func(200); // wywołanie func z r-value
func(x); //wywołanie func z l-value
```

Spójrzmy w końcu na taki przypadek
```c++
vector<Gracz> vec;

vec.push_back(Gracz{"Jan"});
vec.push_back(Gracz{"Jaś"});
```

W powyższym przykładzie tworzymy dwa obiekty `Gracz`
które są bezpośrednio dodawane do wektora `vec`. W tym przypadku
Pokolei obydwa obiekty gracz są r-value ponieważ mamy do nich dostęp
tylko w lini w której są tworzone i dodawane do wektora. Lecz nie są 
one zwyczajnie dodawane lecz są kopiowane oraz ich kopia
dopiero jest dodawana do wektora. Do kopiowania obiektów
typu r-value jest używany tzw. `move constructor`. Każda klasa
posiada swój domyślny move constructor ale tak jak w przypadku
konstruktora kopiującego możemy napisać swój. Robimy to
w takim samym przypadku jak dla konstruktorów kopiujących
czyli wtedy kiedy mamy w swojej klasie zdefiniowane atrybuty wskaźnikowe.


```c++
class Gracz
{
    private:
        Ekwipunek *ekwipunek;
    public:
        Gracz(Gracz &&gracz);
}
```
```c++
Gracz::Gracz(Gracz &&gracz)
    : ekwipunek{gracz.ekwipunek}
{
    gracz.ekwipunek = nullptr;
}
```

Dzięki takiemu podejściu unikamy niepotrzebnego kopiowania
obiektu po kilka razy, plus dodatkowo po przepisaniu wartości
zerujemy wskaźniki klasy z której kopiujemy (jest to r-value i już jej i tak nigdzie nie użyjemy)

---

<a name="this"></a>
### Słowo kluczowe this

* może być używane tylko w klasach
* jest wskaźnikiem na adres obiektu
* ma dostęp do wszytskich attrybutów i metod w obiekcie

```c++
Gracz:Gracz dodaj_sily(int sila)
{
    this->sila = sila; //this pomaga nam w tym przypadku stwierdzic ktora 'sila' jest 
                       //atrybutem klasy, a ktora przekazanym parametrem
}
```
---

<a name="const"></a>
### Const i klasy

Zmienne tworzone na podstawie klasy mogą tak samo jak zmienne być oznaczone jako stałe (const). W przypadku obiektów 
oznacza to że nie możemy zmieniać wartości w obiektcie który utworzyliśmy w żaden sposób.

```c++
const Gracz gracz { "Janek" }

gracz.wyswietl_imie(); //error poniewaz obiekt gracz jest oznaczony jako stala
```

Jednak co w przypadku kiedy mamy funkcje ktore nie zmieniaja wartosci obiektu a np. tylko wyswietlaja jego atrybuty?
W takim przypadku w klasie na koncu metody dodajemy slowo kluczowe `const`. Oznacza to że metoda może zostać wykonana
nawet kiedy obiekt tej klasy jest stały. Jednak jeżeli metoda oznaczona jako `const` wewnatrz zmienia jakąś wartość
kompilator zwróci błąd.

```c++
class Gracz
{
    private:
        strign name;
    public:
        void wypisz_nazwe() const; //metoda moze byc uruchomiona dla stalych
}
``` 

---

<a name="static"></a>
### atrybuty i metody statyczne w klasie

Klasy mogą posiadać swoje atryybuty i metody statyczne opisywane słowem kluczowym `static`.
Są one niezależne od obiektów i należą do klasy. Atrybuty statyczne przydają się do przechowywania
ogólnych informacji na temat klasy (np. ilość utworzonych obiektów klasy). Zarówno do metod jak i 
atrybutów statycznych klasy możemy się odwołać poprez nazwe klasy `::` i nazwe atrybutu/klasy.

```c++
//Gracz.h
class Gracz
{
    private:
        static int liczba_graczy;

    public:
        Gracz();
        ~Gracz();
        static void wyswietl_liczbe_graczy();
}
```
```c++
//Gracz.cpp

Gracz::liczba_graczy=0;

Gracz::Gracz()
{
    liczba_graczy++;
}

Gracz::~~Gracz()
{
    liczba_graczy--;
}

void Gracz::wyswietl_liczbe_graczy()
{
    cout << liczba_graczy;
}
```
```c++
//main.cpp
int main()
{
    Gracz::wyswietl_liczbe_graczy(); //wywołanie metody statycznej 
}
```

---

<a name="struct"></a>
### Struktura a klasa

Struktura jest identyczna jak klasa z tą róźnicą że wszystjie atrybuty i metody w struktorze są publiczne.

```c++
struct Gracz
{
    string nazwa;
    int hp;
    int xp;
    string pobierz_nazwe(); //niepotrzebna skoro atrybut nazwa i tak jest prywatny
}
```

Kiedy powinnismy uzywac klas a kiedy struktor?
* Struktór
  * Dla prostych obiektow z publicznym dostępem
  * Kiedy nie potrzebujemy metod
* Klas
  * W każdym innym przypadku

---

<a name="zad1"></a>
### Zadanie 1

Napisz prosty pseudo manager "walki" dwuch postaci w grze.
 * Walka ma się odbywać turami
 * Walka trwa dopuki jeden z graczy nie zginie (hp = 0)
 * Walka odbywa się na podstawie parametrów siły, zwinności i szczęścia (trzeba wymyślić jakiś wzór)

Wymagania Ogolne:
* Opisz i zaproponuj rozwiazanie (jakie funkcje potrzebujemy itd.)
* Potrzebujemy dwie klasy
  * Gracz
  * Menadzer walki 
* Dla testow
  * Klasa gracz musi posiadać atrybut zliczajacy liczbe aktywnych graczy oraz funkcje zwracajaca liczbe tych graczy (static)
  * Klasa gracz musi posiadać konstruktory kopiujacy i move, oraz dwa inne z roznymi ilosciami parametrow i wartosciami domyslnymi
  * Obiekt menadzera walki musi byc stałą
  * Nalezy unikac kopiowania obiektow (Gdzie sie da uzywamy wskaznikow)