
# STEP 8 - Programowanie obiektowe cz.1
1. [Czym jest programowanie obiektowe](#oop) 
2. [Klasa i obiekt](#klasaobiekt)
3. [Definicja klasy i tworzenie obiektów](#definicja)
4. [Dostęp do metod i atrybutów klasy](#dostep)
5. [Akcesory dostępu](#akcesory)
6. [Implementacja metod w klasie](#implementacjametod)
7. [Zadanie 1](#zadanie1)
8. [Konstruktor i destruktor](#konstruktor)
9. [Konstruktory inicjalizujące](#inicjalizujace)
10. [Zadanie 2](#zadanie2)
---

<a name="oop"></a>
### Czym jest programowanie obiektowe?

Do tej pory pisaliśmy aplikacje proceduralnie. Charakteryzuje się ono:
* Koncentruje się ono na akcjach które ma wykonywać program
* Rozkład zadań w programie dzieli się i jest skoncentrowany na funkcjach
* Dane są przekazywane do funkcji w formie parametrów

Ograniczenia programowania proceduralnego:
* Funkcje muszą znać strukture danych
  * Jeżeli zmieni się struktura danych wiele funkcji musi ulec zmianie
* Wraz z rozrastaniem się programu robi się on bardziej skomplikowany
  * trudny do utrzymania
  * trudny do ogarnięcia
  * trudny do rozbudowy
  * trudno jest pisać kod któy może być reużywalny
  * trudny do debugowania
 
##### To czym jest to programowanie obiektowe?

* Ogólnie rzecz ujmując są to klasy i obiekty
  * koncentruje się na klasach które reprezentują byt/podmiot z rzeczywistego świata
  * pozwala myśleć progamiście na wyższym poziomie abstrakcji   
* Działa na zasadzie hermetyzacji
  * Zamykamy w jednej klasie wszystkie właściwości które dotyczą jednego buty
    oraz wszystkie funkcje które działają na danych które ten byt posiada
  * Tak stworzona klasa jest abstrakcyjnym `typem` danych stworzynym przez nas opisującym
    dany byt
* Reużywlność
  * Klasy które tworzymy są łatwiejsze do użycia w wielu miejscach w programie czy w całkiem 
    innych programach
  * Przyśpiesza pisanie aplikacji
  * Zwiększa jakość
* Dziedziczenie
  * Podejście obiektowe pozwala nam tworzyć klasy na podstawie innych klas
  * Co zwiększa reużywalność kodu
  * Pozwala używać polimorfizmu (o polimorfizmie później)

---

<a name="klasaobiekt"></a>
### Klasa i obiekt

#### Klasa
* Jest 'planem' na podstowie którego tworzymy obiekty
* Jest typem zdefiniowanym przez użytkownika
* Posiada atrybuty (dane)
* Posiada metody (funkcje)
* Możliwość ustanawiania dostępu dla danych i funkcji (private, public...)
* Przykłady
  * std::vector
  * std::string
  * Użytkownik
  * Obraz
  * Konto

#### Obiekt
* Powstaje na podstawie klasy
* Reprezentuje specyficzną instancje klasy
* Na podstawie jednej klasy możemy tworzyć wiele obiektów
* Ma swojój identyfikator (nazwe)
* Każdy obiekt może korzystać z metod i atrybutów zdefiniowanych w klasie
* Przykłady
  * Konto `Admin` jest instanją klasy `Konto`
  * Konto `Zosia` jest intancją klasy `Konto`
  * Każde obiekt konta posiada swoje własne uprawnienia

```c++
int a;
int b;

std:string str_a;
std:string str_b;

//Tworzenie obiektów na podstawie klasy
Account admin;
Account zosia;
```

Tak jak było wspomniane wcześniej klasy są typem zdefiniowanym przez użytkownika/programiste
dlatego obiekty na podstawie klas tworzymy w taki sam sposób jak inne typy w c++.

---

<a name="definicja"></a>
### Definicja klasy i tworzenie obiektów

Z zasady definicje każdej klasy umieszczamy w oddzielnych plikach o nazwie
takiej samej jak klasa. Podstawowa definicja klasy zawiera słowo `class` oraz jej nazwe oraz 
methody i atrybuty klasy.

```C++
class NazwaKlasy
{
    //Atrybuty
    //Metody
}

class Player
{
    string name;
    int health;
    int xp;

    void talk(string to_say);
    bool is_alive();
}
```

Aby stworzyć obiekt klasy, możemy stworzyć zmienną o typie klasy lub dla wskaźnika o typie klasy musimy zaalokować miejsce w pamięci
o 'rozmiarze' klasy (tak samo jak alokolwaliśmy dynamicznie miejsce w pamięci dla tablic słowem
kluczowym `new`)

```c++
Player zofia; //Zmienna typu Player o nazwie zofia
Player *piotr = new Player(); //Dynamicznie stworzony wskaźnik na obiekt typu Player
Player *janek { &zofia };//Wskaźnik na obiekt zofia

std::vector<Player> players;
players.push_back(*piotr);
players.push_back(*janek);
players.push_back(zofia);


delete *piotr;
delete *janek;
```

Z zmiennymi czy wskaźnikami o typie klasy możemy działać tak jak z każdym innym typem w `c++`
np. dodawać je do wektorów i tablic.

---

<a name="dostep"></a>
### Dostęp do metod i atrybutów klasy

Każdy stworzony obiekt ma dostęp do atrybutów i metod które klasa zawiera.
Odwołujemy się do nich poprzez operator kropki `.` lub strzałki `->` w przypadku
wskaźników.

```c++
class Player
{
    string name;
    int health;
    int xp;

    void talk(string to_say)
    {
        cout << to_say;
    }

    bool is_alive()
    {
        return health > 0
    }
}
```

Nasza klasa `Player` posiada trzy właściwości i dwie funkcje.
```c++
int main()
{
    //zmienne
    Player zofia;
    zofia.name = "Zofia";
    zofia.health = 10;
    zofia.xp = 10;

    cout << zofia.Name << "Żyje?: " << zofia.is_alive(); 

    Player piotr;
    piotr.Name = "Piotr";
    piotr.health = 100;
    piotr.xp = 100000;

    cout << piotr.Name << "Żyje?: " << piotr.is_alive();    

    //wskaźniki
    Player *janek = new Player();
    janek->name = "Zofia";
    janek->health = 10;
    janek->xp = 10;

    cout << janek->Name << "Żyje?: " << janek->is_alive(); 
}
```

---

<a name="akcesory"></a>
### Akcesory dostępu

W `c++` wyróźniami trzy podstawowe akcesory dostępu:
* `public` - dostęp z każdego miejsca (do danej zmiennej czy metody w klasie
           oznaczonej jako publiczna mamy dostęp wewnątrz klasy ale również w klasach
           dziedziczących oraz również z poziomu utworzonych obiektów)
* `private` - dostęp tylko w klasie gdzie atrybut czy metody jest zadeklarowana
* `protected` - dostęp w klasie gdzie metoda czy atrubut jest zadeklarowana oraz
              w klasach dziedziczących

Akcesory w klasach dodajemy poprzez słowa kluczowe (nazwe akcesora) z dwukropkiem `:`.
Wszystkie atrybuty czy metody poniżej takiego akcesora mają dostępność taką jaką akcesor
określa.

```c++
class Player
{
    public:
        string name;
        bool is_alive()
        {
            return health > 0
        }
        
    private:
        int health;
        int xp;
        void talk(string to_say)
        {
            cout << to_say;
        }
}

int main()
{
    Player zofia;
    zofia.name = "Zofia"; //OK
    zofia.health = 100; //Błąd kompilacji
    zofia.xp = 1000; //Błąd kompilacji

    zofia.is_alive(); //OK
    zofia.talk("MORDO!"); //Błąd kompilacji
}
```

---

<a name="implementacjametod"></a>
### Implementacja metod w klasie

Implementacja metod w klasie: 
* bardzo podobna do implementacji funkcji
* metody mają dostęp do atrybutów klasy
  * nie musimy ich podawać jako parametry
* Może być implementowana wewnątrz klasy
* Może być implementowana zewnątrz klasy
  * Przy pomocy odwołania `Class_Name::Method_Name`
* Można oddzielić specyfikacje metody od definicji
  * Plik `.h` dla specyfikacji
  * Plik `.cpp` dla definicji

#### Implementacja wewnątrz klasy

```c++
class Player
{
    private:
        int health;

    public:
        int get_health()
        {
            return health;
        }
}
```

W powyższym przykładzie widzimy implementacje funkcji `get_health()` wewnątrz klasy
`Player`. Funkcja `get_health` ma dostęp do atrybutu `health` klasy `Player` dlatego
nie musimy przekazywać go jako parametr funkcji. Warto zauważyć że tak zbudowane
akcesory zmiennych umożliwiają nam z poziomu obiektu pobrać wartość atrybutu `health`
ale po za klasą nie możemy go zmienić ponieważ sam atrybut jest prywatny.

#### Implementacja zewnątrz klasy

```c++
class Player
{
    private:
        int health;

    public:
        int get_health();
}

Player::get_health()        
{
    return health;
}
_
```

W przykładzie powyżej widzimi implementacje metody `get_health()` po za
ciałem funkcji. Działa to dokładnie na takich samych zasadach jak implementacja
wewnętrz. Jednak z czasem gdy klasa się powiększa chcemy rozdzielić specyfikacje
funkcji od implementacji jej funkcji.

#### Separacja implementacji od specyfikacji

Tak jak było wcześniej wspomniane w `c++` mamy możliwość oddzielania specyfikacj i 
implementacji elementów klasy do oddzielnnych plików.
* plik `nazwa_klasy.h` zawiera specyfikacje klasy:
    ```c++
    //Player.h
    #pragma once

    class Player
    {
        public:
            string name;
            bool is_alive();
        
        private:
            int health;
            int xp;
            void talk(string to_say);
    }
    ```
    W kodzie powyżej na początku pliku widzimy zapis `#pragma once` jest on wymagany dla pliku specyfikacji
    ponieważ jeżeli będziemy mieli zainkludowany plik `Player.h` w kilku plikacg `.cpp`
    mogł by wystąpić błąd podwójnej implementacji. Dyrektywa `#pragma once` zapobiega takiej sytuacji.
* plik `nazwa_klasy.cpp` zawiera implementacje:
    ```c++
    //Player.cpp
    #include "Player.h"

    Player::is_alive()
    {
        return health > 0;
    }

    Player::talk(string to_say)
    {
        cout << to_say;
    }
    ```
    W pliku z implementacją method klasy pierwsze co musimy zrobić to zainkludować
    plik nagłówkowy `.h` z specyfikacją. Definicje metod implementujemy tak jak jest to opisane
    w zewnętrznej implementacji method.

Aby używać naszej zdefiniowanej klasy w innych miejscach np. w funkcji `main`
Musimy do pliku w któym znajduje się funkcja zainkludować nasz plik nagłówkowy
`nazwa_klasy.h`

```c++
#include <iostream>
#include "Player.h"

int main()
{
    Player piotr;
    cout << piotr.is_alive();
}
```

---

<a name="zadanie1"></a>
### Zadanie 1

Sklep internetowy dla którego napisałaś ostatnio aplikacje był tak bardzo zadowolony
z ostatniej aplikacji że poprosił się o napisanie kolejnej.
Tym razem masz napisać aplikacje dla użytkowników. Aplikacja musi posiadać
kilka funkcji:
   * Powinna pozwalać użytkownikowi na wybór produktów z sklepu wraz z ilością
   * Wybrany produkt musi trafić do koszyka użytkownika
   * Użytkownik może usuwać produkty z swojego koszyka
   * Użytkownik może wyświetlić produkty w koszyku
     * z cenną całkowitą
     * naliczoną obniżką (każdy produkt ma przypisane ile obilżki dodaje
       do ogólnej ceny)
     * cene po obniżce
     * ilość produktów
Aplikacja musi zostać napisana w pełni obiektowo aby łatwo ją było w przyszłości 
rozwijać.

Wymagania:
* Na start definiujemy 10 produktów znajdujących się w sklepie 
  * Tworzymy klase produktu
    * musi zawierac id, cene, nazwe, oraz rabat jaki nalicza się za dodanie produktu do koszyka
    * zawiera metode w ładny sposób wypisującą nazwę i cenę produktu
  * Tworzymy klase magazyn
    * magazyn zawiera wektor produktów dostępnych w sklepie
      * wektor musi byc prywatny
    * dodawać produkty możemy tylko przez dodatkową metode 
    * metode wypisującą wszystkie produkty wraz z ich numeramie porządkowymi
    * metode która zwróci nam wybrany produkt
      * zwrócony produkt musi być kopią produktu z magazynu 
  * Dodajemy utworzone produkty do magazynu
* Wyświetlamy użytkownikowi menu z 3 dostępnymi opcjami
  * 1 Sklep
  * 2 Koszyk
  * 3 Wyjście

(Sklep)
* Wyświetlamy użytkownikowi produkty dostępne w magazynie
    * Użytkownik ma możliwość wybrania produktu
    * Tworzymy klase Zakup
      * Zakup zawiera wybrany produkt
      * Oraz ilość produktów tego typu
    * Tworzymy klase Koszyk
      * zawiera liste zakupów
      * prywatną cene całkowitą
      * prywatną naliczoną zniźkę
      * prywatną cene po obniżce
      * metode dodają Zakup do listy zakupow
        * Jeżeli istnieje w koszyku Zakup o tym samym id zwiększamy jego ilosć
      * metode wyświetlającą nasz koszyk wraz z wszytskimi informacjami

(Koszyk)
* Wyświetlamy koszyk

(Wyjście)
* Kończymy działanie programu

---

<a name="konstruktor"></a>
### Konstruktor i destruktor

Kostruktor to specjany rodzaj metody w klasie która jest wywoływana podczas 
tworzenia obiektó poprzez alokacje pamięci dla obiektu (przez `new`)
* Kostruktor nie posiada typu zwracanego
* Wykonywany jest podczas tworzenia obiektu
* Jest pomocny przy inicjalizacji atrybutów
* Ma taką samą nazwe jak klasa
* Może być przeładowywany

```c++
//Player.h
class Player
{
    private:
        string name;
        int hp;
        int xp;

    public:
        //Konstruktory wraz z przeładowaniami
        Player();
        Player(string name);
}
```
```c++
//Player.cpp
//Definicje konstruktorów
Player::Player()
{
    this.name = "Janek";
    this.hp = 10;
    this.xp = 0;
}

Player::Player(string name)
{
    this.name = name;
    this.hp = 10;
    this.xp = 0;
}
```
```c++
//main.cpp
//Tworzenie graczy używając kostruktorów
int main()
{
    Player piotr; //konstruktor bezparametryczny
    Player janek { "Janek" } //Player(string name)
    Player *standard = new Player; //konstruktor bezparametryczny
    Player *zofia = new Player("Zofia"); //Player(string name)
}
```

Powyżej mamy przykład klasy `Player` z konstruktorem bezparametrycznym wraz
z przeładowaniem nadającym imię graczowi.

Dekonstruktor ma za zadanie 'posprzątać' w klasie po jej usunięciu. Wywoływany jest
podczas usuwania pamięci przypisanej podczas dynamicznej alokacji pamięci przy
tworzeniu obietku `delete nazwa_obiektu`.
* Nie posiada typu zwracanego
* Nazywa się tak samo jak nazwa klasy
* Do nazwy klasy dodaje się tyldę `~` 
* Nie przyjmuje parametrów
* Klasa może posiadać jeden dekonstruktor
* Przydatna do zwalniania pamięci

```c++
//Player.h
class Player
{
    private:
        vector<Items> *items;
    public:
        //Dekonstruktor
        ~Player();
}
```
```c++
\\Player.cpp
Player::~Player()
{
    delete items;
}
```
```c++
\\main.cpp
int main()
{
    Player *zofia = new Player;

    delete zofia;//Wywołanie dekonstruktora
}
```

Usuwając obiekt klasy musimy w dekonstruktorze zwolnić pamięć dla wszystkich
obiektów/atrybutów dynamicznie zaalokowanych w klasie. W tym przypadku zwalniamy
pamięć dla dynamicznie zainicjowanego vectora items.

Nie każda klasa musi posiadać konstruktor, w `c++` istnieje coś takiego jak
domyślny konstruktor który jest wywoływany podczas tworzenia obiektu. Jest 
to konstruktor bezparametryczny który nadaje atrybutą wartości domyślne. Domyślny
konstruktor bezparametryczny możemy sami zdefiniować w klasie.

Jeżeli nasza klasa posiada jakikolwiek zdefiniowany konstruktor tylko z niego
możemy korzystać przy tworzeniu obiektów klasy.

```c++
//Player.h
class Player
{
    private:
        string name;
        int hp;
        int xp;

    public:
        //Konstruktory wraz z przeładowaniami
        Player(string name);
}
```
```c++
//Player.cpp
//Definicje konstruktorów
Player::Player(string name)
{
    this.name = name;
    this.hp = 10;
    this.xp = 0;
}
```
```c++
//main.cpp
//Tworzenie graczy używając kostruktorów
int main()
{
    Player piotr; //Bład (nie ma takiego konstruktora)
    Player janek { "Janek" } //OK
    Player *standard = new Player; //Błąd (nie ma takiego konstruktora)
    Player *zofia = new Player("Zofia"); //OK
}
```
---

<a name="inicjalizujace"></a>
### Konstruktory inicjalizujące

W `c++` istnieje coś takiego jak konstruktory inicjalizujące.
Jest to skrócony zapis konstruktora który inicjalizuje wartościami
atrybuty klasy.

```c++
//Player.cpp

//Zapis klasyczny
Player::Player()
{
    name = "Zofia";
    hp = 100;
    xp = 0;
}

Player::Player(string _name)
{
    name = _name;
    hp = 100;
    xp = 0;
}

//Zapis skrócony
Player::Player()
    : name { "Zofia" }, hp { 100 }, xp { 0 }
{ }

Player::Player(string _name)
    : name { _name_}, hp { 100 }, xp { 0 }
{ }

```
---

<a name="zadanie2"></a>
### Zadanie 2

Przygotuj program który umożliwi nam wykonywać podstawowe operacje na macierzach i wektorach. Zapisz schemat przedstawiający klasy, ich
atrybuty i metody. (dodawanie, odejmowanie, mnozenie, mnozenie skalarne macierzy i wektorow, odwracanie macierzy). Napisz klase pomocniczą
która tworzy macierz lub wektor o wielkosci X Y wypełnioną losowymi wartościami 0-10

---

<a name="zadanie3"></a>
### Zadanie 3 

Napisz trzy klasy reprezentujace figury geometryczne koło, kwadrat, trójkąt.
Dodaj do nich metody obliczające pole i obwód.