
# STEP 10 - przeładownanie operatorów
1. [Czym jest przeładowanie operatorow?](#czym) 
2. [Operator przypisania (=)](#przypisanie)
2. [Operator inicjalizacji (=)](#inicjalizacja)
4. [Przyklady przeladowan operatorow (wewnatrz klasy)](#przyklady)
5. [Przyjaciele funkcji](#przyjazn)
6. [Przyklady przeladowan operatorow (zewnatrz klasy)](#zewnatrz)
7. [Przeładowanie streamow wejscia i wyjscia (<< >>)](#stream)
8. [Zadanie](#zadanie)
---

<a name="czym"></a>
### Czym jest przeładowanie operatorów?

Przeładowanie operatorów umożliwia nam używanie tradycyjnych operatorów takich jak `+,-,*,/`
z typami zdefiniowanymi przez użytkownika (klasami). Dzięki temu kod jest bardziej przejrzysty
, naturalny. Operacja z użyciem operatorów nie są automatycznie zdefiniowane dla typów 
zdefiniowanych przez użytkownika. Trzeba je napisać samemy (Oprócz operatora przypisania `==`)

Co zmienia preładowanie operatorów? Wyobraźmy sobie że mamy zdefiniowaną klase `Zasoby`
która zawiera ilość jabłek i gruszek w magazynie. Do tej pory gdybyśmy chcieli dodać
do siebie dwa obiekty `Zasobów`, musielibyśmy zdefiniować metode która by to dla nas robiła, np.
`dodajZasoby`.

```c++
//Zasoby.h
class Zasoby
{
    private:
        int jablka;
        int gruszki;
    public:
        Zasoby(int _jablka, int _gruszki); 
        int dajJablka();
        int dajGruszki();
        Zasoby& dodajZasoby(Zasoby *zasoby);
}

//Zasoby.cpp

Zasoby::Zasoby(int _jablka, int _gruszki)
    : jablka { _jablka}, gruszki { _gruszki }
{ }

Zasoby* Zasoby::dodajZasoby(Zasoby* zasoby)
{
    int iloscJablka = jablka + zasoby->dajJablka();
    int iloscGruszki = gruszki + zasoby->dajGruszki();
    return new Zasoby(iloscJablka, iloscGruszki);
}

...

//main.cpp

int main() 
{
    Zasoby* a = new Zasoby(1,2);
    Zasoby* b = new Zasoby(10,23);

    Zasoby* suma = a.dodajZosoby(b);
}
``` 

Zapis `a.dodajZasoby(b)` nie jest intuicyjny i przejrzysty. Przeładowanie operatorów 
pozwoli nam wykonać operacje dodawania i przypisania do nowej zmiennej poprzez wywołanie
`c = a + b`.

---

<a name="przypisanie"></a>
### Operator przypisania (=)

Słowem wstępu jest róźnica pomiędzy operatorem przypisania a inicjalizacji. Znak `=` co 
innego robi podczas inicjalizacji, a co innego podczas przypisania.

```c++
Zasoby a {1,2};
Zasoby b = a; //inicjalizacja - wowołany zostanie konstruktor kopiujacy

b = a; //przypisanie - wywolana zostanie domyslna lub zdefiniowane przeladowanie operatora =
```

Kiedy należy zaimplementować własne przeładownie operatora przypisania?
Kiedy w naszej klasie znajdują się dynamicznie tworzone wskaźniki. Domyślna metoda
operatora przypisania skopiuje w takim przypadku adresy z wskaźnika a nie wartości.

Definicja przeładowania operatora przypisania:

```c++
//Zasoby.h
class Zasoby
{
    private:
        int jablka;
        int gruszki;
    public:
        Zasoby(int _jablka, int _gruszki); 
        int dajJablka();
        int dajGruszki();
        Zasoby& operator=(const Zasoby& zasoby); //przeładowanie operatora =
}

//Zasoby.cpp
...

Zasoby& Zasoby::operator=(const Zasoby& zasoby)
{
    if (this == &zasoby)
    {
        return *this;
    }

    this.jablka = zasoby.dajJablka();
    this.gruszki = zasoby.dajGruszki();

    return *this;
}

...

//main.cpp

int main()
{
    Zasoby a {1,2};
    Zasoby b {2,3};

    a = b; wywołani operator=
}
```

Przeładowując operatory należy pamiętać że w metodzie przeładowywującą wywołujemy dla obiektu
będącego po lewej stronie operatora (w tym przypadku `a`), natomiast parametr przeładowania
to obiekt będący po prawej stronie operatora (`b`). To co zwraca metod przeładowywująca w
tym przypadku zostanie przypisane do `a`

--- 

<a name="inicjalizacja"></a>
### Operator inicjalizacji (=)

Operator inicjalizacji przeładowuję się w identyczny sposób jak operator przypisania z tą
róźnicą że jako parametr podaję się referencje do r-value obiektu którym chcemy zainicjalizować
nasz nowy obiekt. Przeładowujemy operator przypisanie w takich samych przypadkach
jak konstruktory kopiujace czy przeladowanie przypisaujące, wtedy kiedy w klasie mamy
wskaźniki.

```c++

Zasoby& operator=(Zasoby &&zasoby);

```

Należy zwrócić uwage na podwójny ampersand w parametrze.
Definicja takiego przeładowania nie róźni się niczym oprócz tego
że po skopiowaniu wartości z obiektu w parametrze musimy znullować jego atrybuty
wskaźnikowe.

Przyklady inicjalizacji:

```c++
Zasoby a {1,2} //konstruktor - Zasoby(jablka, gruszki)
Zasoby b = a //operator inicjalizujacy
Zasoby c { a } //operator inicjalizujacy

c = a; //operator przypisujacy
```

---

<a name="przyklady"></a>
### Przyklady przeladowan operatorow (wewnatrz klasy)

W c++ mozemy przeładowywać wiele operatorow logicznych jak i artmetycznych.
Jednym z sposobów przeładowania jest implementacja wewnatrz klasy ktorej operatory chcemy przeładować.

```c++
//Zasoby.h
class Zasoby
{
    private:
        int jablka;
        int gruszki;
    public:
        Zasoby(int _jablka, int _gruszki); 
        int dajJablka();
        int dajGruszki();
        //przeładowania
        Zasoby operator+(const Zasoby &zasoby);
        bool operator==(const Zasoby &zasoby);
        Zasoby& operator++();
        Zasoby operator+(const int &&wartosc);
}

//Zasoby.cpp

//operator + (a+b)
Zasoby Zasoby::operator+(const Zasoby &zasoby)
{
    int noweJablka = this.jablka + zasoby.dajJablka();
    int noweGruszki = this.gruszki + zasoby.dajGruszki();     
    Zasoby noweZasoby { noweJablka, noweGruszki };
    return noweZasoby;
}

//operator == (a==b)
bool Zasoby::operator==(const Zasoby &zasoby)
{
    return this.jablka == zasoby.dajJablka()
        && this.gruszki == zasoby.dajGruszki()
}

//operator ++ (a++)
Zasoby& Zasoby::operator++()
{
    this.jablka++;
    this.gruszki++;     
    return *this;
}

//operator + (a+2)
Zasoby Zasoby::operator+(const int &&wartosc)
{
    int noweJablka = this.jablka + wartosc;
    int noweGruszki = this.gruszki + wartosc;     
    Zasoby noweZasoby { noweJablka, noweGruszki };
    return noweZasoby;
}

```
---

<a name="przyjazn"></a>
### Przyjaciele funkcji

Zdarza się niekiedy że obiekty jeden klasy lub funkcje potrzebują mieć dostęp do prywatnych
atrybutów czy metod obiektów innej klasy. `c++` pozwala na to poprzez określenie 
dwóch różnych klas jako przyjaciół, lub funkcji jako przyjaciel (`friend`). 

Aby określić klase B jako przyjaciel klasy A w definicji klasy A dodajemy zapis że klasa B
jest jej przyjacielem.

```c++
// Zasoby.h
class Zasoby 
{
    friend class Magazyn; //Okreslamy ze klasa Magazyn jest zaprzyjazniona
                          //i ma dostep do prywatnym atrybutow i metod klasy Zasoby
    private:
        int jablka;
}

// Magazyn.h
class Magazyn 
{
    public:
        bool sprawdzCzySieZmiesci(Zasoby& zasoby);
}

//Magazyn.cpp

bool Magazyn:sprawdzCzySieZmiesci(Zasoby& zasoby)
{
    return zasoby.jablka < 20
}

```

W powyzszym przykladzie klasa `Zasoby` posiada prywatny atrybut `jablka` i określenie
klasy `Magazyn` jako przyjaciel. Dzieki takiemu zapisowi w metodzie `sprawdzCzySieZmiesci`
ktora jako parametr przyjmuje referencje do obiektu klasy `Zasoby` możemy odwołać się do
atrbutu `jablka` obiektu `zasoby` nawet mimo tego że jest on prywatny.

Aby okreslić funkcje jako przyjaciel klasy, w definicji klasy dodajemy nazwe funkcji ktora
ma byc przyjacielem klasy z dopiskiem slowa `friend`. Funkcja zaprzyjazniona najczesciej 
zdefiniowana jest wewnatrz klasy.

```c++
// Zasoby.h
class Zasoby 
{
    friend bool sprawdzCzyPelny(Zasoby& zasoby); //Okreslamy ze funkcja sprawdzCzyPelny jest zaprzyjazniona
    private:
        int jablka;
}

// Zasoby.cpp

bool sprawdzCzyPelny(Zasoby& zasoby)
{
    return zasoby.jablka > 20;
}

// main.cpp

int main()
{
    Zasoby zasoby {20};
    sprawdzCzyPewlny(zasoby);
}
```

W przykladzie powyzej w klasie `Zasoby` widzimy definicje funkcji `sprawdzCzyPelny` jako przyjaciel.
Funkcja ta przyjmuje jako parametr referencje do obiektu `zasoby` i dzięki temu że
jest przzyjacielem może korzystać z atrybutu `jablka` mimo tego że jest on prywatny.
W klasie `main` widzimy wywołanie takiej funkcji.


Taka dygresja: według mnie to jest antypatern i nie powinno się tego używać jeżeli
nie jest to absolutnie konieczne. Jeżeli ustawiamy dostępność zmiennych w klasie jako prywatne
to po właśnie po to aby nikt ninny po za klasą nie miał do nich dostępu.

---

<a name="zewnatrz"></a>
### Przyklady przeladowan operatorow (zewnatrz klasy)

Drugim sposobem na przeładowanie operatorow jest definicja metod jako funkcje globalne, nazywane `przyjacielem`
klasy.

Funkcje określone jako przyjaciele klasy mają dostęp do jej prywatnych atrybutow i funkcji.
Przeładowanie przy pomocy funkcji globalnych za parametry przyjmują zarówno obiekt po lewej
jaki i po prawej stronie operatora (Jeżeli istnieją).

Do deklaracji funkcji jako przyjaciel klasy uzywamy slowa kluczowego `friend` przed jej
definicja w klasie.

```c++
//Zasoby.h
class Zasoby
{
    //przeładowania globalne
    friend Zasoby operator+(const Zasoby &l_zasoby, const Zasoby &r_zasoby);
    friend bool operator==(const Zasoby &l_zasoby, const Zasoby &r_zasoby);
    friend Zasoby& operator++(const Zasoby &zasoby);
    friend Zasoby operator+(const int &&l_wartosc, const Zasoby &r_zasoby);
    
    private:
        int jablka;
        int gruszki;
    public:
        Zasoby(int _jablka, int _gruszki); 
        int dajJablka();
        int dajGruszki();
}

//Zasoby.cpp

//operator + (a+b)
Zasoby operator+(const Zasoby &l_zasoby, const Zasoby &r_zasoby)
{
    int noweJablka = l_zasoby.jablka + r_zasoby.jablka;
    int noweGruszki = l_zasoby.gruszki + r_zasoby.gruszki;     
    Zasoby noweZasoby { noweJablka, noweGruszki };
    return noweZasoby;
}

//operator == (a==b)
bool operator==(const Zasoby &l_zasoby, const Zasoby &r_zasoby)
{
    return l_zasoby.jablka == r_zasoby.jablka
        && l_zasoby.gruszki == r_zasoby.gruszki
}

//operator ++ (a++)
Zasoby& operator++(const Zasoby &zasoby)
{
    zasoby.jablka++;
    zasoby.gruszki++;     
    return zasoby;
}

//operator + (2+a)
Zasoby operator+(const int &&l_wartosc, const Zasoby &r_zasoby)
{
    int noweJablka = r_zasoby.jablka + l_wartosc;
    int noweGruszki = r_zasoby.gruszki + l_wartosc;     
    Zasoby noweZasoby { noweJablka, noweGruszki };
    return noweZasoby;
}

```

Warto zauważyć że definicje funkcji określonych jako przyjaciele nie zawierają przed
nazwą funkcji nazw klasy `Zasoby::nazwa_funkcji`, a to dlatego że funkcje określone jako
przyjaciele nie są częścią funkcji. Są tylko w niej zdefiniowane bo logika w nich zawarta 
dotyczy danej klasy.

---

<a name="stream"></a>
### Przeładowanie streamow wejscia i wyjscia (<< >>)

W c++ mamy możliwość przeładowania streamow `<<` `>>`. 
Najbardziej znanym przykladem przeładowania streamow jest wypisywnie i zczytywanie 
wartości z ekranu. Przeładowując operator wysłania do streama `<<` `cout` wenatrz funkcji
wypisujemy to co chcemy wypisac na ekranie i zwracamy stream do dalszego przetwarzania.
Przeładowując operator odczytu z streama `>>` `cin`
jako parametr oczymujemy to co zostało wypisane na ekranie i możemy tą wartość przypisać do 
jednego z atrybutow w obiekcie.

```c++
//Zasoby.h
class Zasoby
{
    //przeładowania globalne
    friend std::ostream& operator<<(std::ostream os, const Zasoby &zasoby);
    friend std::istream& operator>>(std::istream is, const Zasoby &zasoby);
    
    private:
        int jablka;
        int gruszki;
    public:
        Zasoby(int _jablka, int _gruszki); 
        int dajJablka();
        int dajGruszki();
}

//Zasoby.cpp

//operator << (cout << a)
std::ostream& operator<<(std::ostream os, const Zasoby &zasoby)
{
    os << "Jablka: " + zasoby.jablka + ", Gruszki: " + zasoby.gruszki;
    return os;
}

//operator >> (cin >> a)
std::istream& operator>>(std::istream is, const Zasoby &zasoby)
{
    int noweJablka = 0;
    int noweGruszki = 0;
    is >> noweJablka >> noweGruszki;
    zasoby = Zasoby { noweJablka, noweGruszki };
    return is;
 
}
```

W pierwszej metodzie przeładowujemy operator wypisania `<<`. Jako parametry otrzymujemy stream
będący po lewej stronie `<<` (czyli `cout`), jako drugi parametr przyjmujemy obiekt bedacy
po prawej stronie `<<`. To co robimy w funkcji to wypisanie na ekran poprzez przekazany stream
`os` ilości gruszek i jablek w obiekcie `Zasoby` przekazanym po prawej stronie. Z funkcji
zwracamy stream tak aby można było zachować dalsze wypisywanie.

W drugiej metodzie przeładowujemy operator odczytu `>>`. Jako parametry otrzymujemy stream
będący po lewej strine `>>` (czyli `cin`), jako drugi parametr obiekt typu `Zasoby` będący
po prawej stronie `>>`. To co robimy w funkcji to przy pomocy strama do odczytu `is` przekazanego
jako parametr odczytujemy wpisane na konsoli wartosci ilosci jablek i gruszek. Następnie do
inicjalizujemy przekazany obiekt `zasoby` tymi wartośćiami. Z funkcji zwracamy stream tak
aby móc z niego skorzystać po wykonaniu funkcji.

---

<a name="zadanie"></a>
### Zadanie

W ramach zadania należy przyrobić program który wykonywał obliczenia na macierzach i wektorach
tak aby korzystał z przeładowań operatorów `+`, `*` ,zamiast zdefiniowanych funkcji "dodajMacierz", "pomnozMacierz" itd.
Należy także  zastąpić metode wypisz przełądowaniem operatora `<<`. W klasie macierz wykorzzystaj
przeładowane operatory w klasie Wektor, oraz w pliku main użyj przeładowań z klasy macierz.
