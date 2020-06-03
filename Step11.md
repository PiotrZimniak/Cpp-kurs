
# STEP 11 - Dziedziczenie
1. [Czym jest dziedziczenie i dlaczego jest użyteczne?](#dziedziczenie) 
2. [Terminologia i notacja](#terminologia)
3. [Różnica pomiędzy dziedziczeniam a "posiadaniem" (Composition)](#composition)
4. [Tworzenie klas z już istniejących klas (dziedziczenie)](#kod)
5. [Konstruktory i destruktory w klasach pochodnych](#konstruktor)
6. [Nadpisywanie (override) metod klasy bazowej (wstep do polimorfizmu)](#override)
7. [Zadnie](#zadanie)

---

<a name="dziedziczenie"></a>
### Czym jest dziedziczenie i dlaczego jest użyteczne?

* Umożliwia nam tworzenie nowych klas na podstawie już istniejących klas
* Nowe klasy zawierają atrybuty i metody klasy bazowej
* Umożliwia reużycie już isniejących klas (reużywanie kody w programowaniu to główny paradygmat
* Pozwala nam się skupić na podstawowych atrybutów wśród zbioru klas
* Nowe klasy mogą zmieniać zachowanie klas po których dziedziczą bez zmian w samej klasie bazowej

Przykłady powiązanych ze sobą klas które mogłyby dziedziczyć po sobie i mieć wspólne
zależności:
* Player, Enemy, Stage Boss, Hero, Super Hero itd.
* Account, Individual Account, Bussines Account, Savings Account itd.
* Shape, Line, Circle, Squere, Triangle itd.
* Person, Employee, Student, Staff, Administrator itd.

Charakterystyczne dla klas powiązanych ze sobą zadaniem, przeznaczeniem,  itp.
jest to że posiadają podobny zestaw atrybutów (cech) i logiki (metod).

Przykład powiązanych ze sobą klas `Accounts`:

* Account
  * balance, deposit, withdraw...
* Individual Account
  * **balance**, **deposit**, **withdraw**, owner...
* Bussines Account
  * **balance**, **deposit**, **withdraw**, **owner**, company...
* Savings Account
  * **balance**, **deposit**, **withdraw**, **owner**, interest rate...

Na tym przykładzie łatwo można zauważyć podobieństwa klas z rodziny "Accounts"
głownym zadaniem dziedziczenia jest aby wykorzystać już logike zapisaną w jednej klasie
w innych klasach powiązanych zastosowaniem, tematem.

Bez zastosowania dziedzicznia pisząc osobne klasy dla `Account`,`Individual Account`...
łatwo zauważyć że duplikowalibyśmy kod który już mamy zapisany i w przypadku gdy byśmy 
chcieli zmienić jakąś ogólną logike działania kont, musilibyśmy edytować kod w każdej klasie
powiązanej z kontem (W tym przypadku w każdej klasie typu konta).

---

<a name="terminologia"></a>
### Terminologia i notacja

* Dziedziczenie (Inheritance)
  * Proces tworzenia nowej klasy przy pomocy już istniejącej/ych klas/y
* Pojedyńcze dziedziczenie (Single Inheritance)
  * Proces tworzenia nowej klasy przy pomocy jednej już istniejącej klasy
* Wielodziedziczenie (Multiple Inheritance)
  * Proces tworzenia nowej klasy przy pomocy dwóch lub więcej już istniejących klas
* Klasa bazowa (base class, super class, parent class)
  * Klasa po której dziedziczy nasza nowa klasa
* Klasa pochodna (derived class, child class, sub class)
  * Klasa która dziedziczy po klasie bazowej

> Klasa pochodna dziedziczy po klasie bazowej

* 'Is-A' relationship
  * publiczne dziedziczenie
  * klasa pochodna jest podtypem klasy bazowej
  * klasy pochodnej możemy używać jako klasy bazowej (Czyli obiekt o typie pochodnym możemy 
  przypisać do zmiennej o typie klasy bazowej - jednak wtedy możemy korzystać tylko z atrybutów i metod
  klasy bazowej)

![Is-A](https://i.stack.imgur.com/JPs84.png)

* Generalizacja (Generalization)
  * Stworzenie jednej bardziej ogólnej klasy na podstawie atrybutów klas podobnych do siebie

![Generalizacja](https://sourcemaking.com/files/sm/images/uml/img_121.jpg)

* Specjalizacja (Specialization)
  * Odwrotnie do generalizacji jest to tworzenie nowych klas na podstawie klasy ogólnej
  dostarczając nowe, bardziej wyspecjalizowane atrybuty i operacje

![Specjalizacja](https://sourcemaking.com/files/sm/images/uml/img_122.jpg)

* Dziedziczenie lub hierarchia klas
  * jest to organizacja klas które posiadają zależności pomiędzy sobą

---

<a name="composition"></a>
### Różnica pomiędzy dziedziczeniam a "posiadaniem" (Composition)

* Dziedziczenie
  * typ relacji `is-a`
    * Employee `is-a` Person
    * Individual Account `is-a` Account
    * Dog `is-a` Pet
  * dziedziczenie w najłatwiejszy sposób wytłumaczyć jako jedna klasa wynika z drugiej

* Kompozycja
  * typ relacji `has-a`
    * Person `has-a` Pet
    * Player `has-a` Special Attack
    * Circle `has-a` Location
  * Kompozycja wystpuję wtedy kiedy jedna klasa zawiera w sobie inną klase jako atrybut

Przedstawienie kompozycji i dziedziczenia widzieliśmy na obrazku powyżej
![Is-A](https://i.stack.imgur.com/JPs84.png)

Widać na niej klase `Person` która zawiera atrybut `pets`. Atrybut `pets` w klasie `Person`
jest np. tablicą obiektów klasy `Pet`, możemy powiedzieć że Właściciel posiada zwierzęta.
Odwrotny przypadek klasa `Pet` posiada atrybut `owner`. Atrybut `owner` jest obiektem typu `Person`. Można
powiedzieć że zwierzę posiada właściciela. Takie relacje nazywamy kompozycyjnymi (Composite)
(Nie mam zielonego pojęcia tak naprawdę jak takie powiązanie nazywa się po polsku).

Następnie widzimy że mamy dwie klasy `Dog` oraz `Cat`. Z logicznego punktu widzenia zarówno
pies jak i kot są zwierzętami, posiadają swój `age`, `name`, `weight` i `owner` tak jaki 
i klasa `Pet`. Dlatego można powiedzieć że klasy `Dog` i `Cat` są 'wyspecjalizowanymi'
klasami lub konkretnymi typami klasy `Pet`. Takie powiązanie nazywamy dziedziczeniem. 

---

<a name="kod"></a>
### Tworzenie klas z już istniejących klas (dziedziczenie)

Ogólny zapis dziedziczenia w c++:

```c++
class Base
{
    //base class members
}

class Derived: access-specifer Base {
    //derived class members 
}
```

Dostępne `access-specifer` dla dziedziczenia to `private`, `public` i `protected`

* public
  * Najczęściej stosowane, wszystkie atrybuty z klasy bazowej w klasie pochodnej zachowują
  taką samą dostępność
    * private - private
    * protected - protected
    * public - public
* protected
  * Atrybutu public z klasy bazowej zmieniają swoją dostępność na protected w klasie pochodnej,
  reszta zostaję bez zmian
    * private - private
    * protected - protected
    * public - protected
* private
  * Dostępne tylko jeżeli klasy są zaprzyjaźnione, wszystkie atrybuty,metody zmieniają swoją 
  dostępność na private
    * private - private
    * protected - private
    * public - private
  
Klasa pochodna nigdy nie ma dostępu da atrybutów i metod prywatnych klasy bazowej chyba że jest jej przyjacielem.

Przykład dziedziczenia:

```c++
//Person.h
class Person
{
    public:
        string name;
        int age;
        Person(_name, _age);
    protected:
        int iq;
    private:
        string idNumber;
}

//Person.cpp
Person::Person(_name, _age)
    : name {_name}, age {_age}
{ }

//Student.h
class Student: public Person
{
    public:
        int semestr; 
        Student(_name, _age, _semestr);
    private:
        double avarageGrade;
        void Identyfikator();
}

//Student.cpp
Student::Student(_name, _age, _semestr)
    : name {_name}, age {_age}, semestr {_semestr}
{ }

void Student::Identyfikator()
{
    cout << semestr; //OK;
    cout << name; //OK - dostęp do publicznych atrybutow klasy bazowej
    cout << iq; //OK - dostęp do chronionych atrybutow klasy bazowej
    cout << idNumber; //Błąd - brak dostępu do  prywatnyh atrybutów klasy bazowej
}


//main.cpp
int main()
{
    Person person_a { "A", 25 }
    Student student_b { "B", 30, 1 }

    string name = person_a.name; //OK - dostęp do publicznych metod
    int age = person_a.age; //OK
    int iq = person_a.iq; //Błąd - po za klasą nie mamy dostępu do chronionych atrybutów
    string idNumber = person_a.idNumber; //Bład - po za klasą nie mamy dostępu do prywatnych atrybutów
    int semestr_a = person_a.semestr; //Błąd - klasa bazowa nie ma dostępu do atrybutów i metod klas pochodnych

    string name_b = student_b.name; //OK - dostęp do publicznych metod klasy bazowej
    int age_b = student_b.age; //OK
    int iq_b = student_b.iq; //Błąd - po za klasą nie mamy dostępu do chronionych atrybutów klasy bazowej
    string idNumber_b = student_b.idNumber; //Bład - po za klasą nie mamy dostępu do prywatnych atrybutów klasy bazowej
    int semestr_b = student_b.semestr; //OK - publiczny atrybut
    double avarageGrade_b = student_b.avarageGrade; //Bład - prywtny atrybut   

    person_a = student_b; //Przypisanie obiektu klasy pochodnej do obiektu klasy bazowej
    semestr_b = person_a.semestr; //Błąd - obiekt klasy Person nie ma dostępu do atrybutów klasy bazowej 
    name_b = person_b.name; //OK - wybisze imie wczesniej przypisane do obiektu studen_b "B"
}
```

---

<a name="konstruktory"></a>
### Konstruktory i destruktory w klasach pochodnych

Wywołując konstruktor bezprametryczny i destruktor z klasy pochodnej wywołujemy również konstruktor bezparametryczny i destruktor 
z klasy bazowej. Ważne jest że wywołując konstruktor parametryczny z klasy pochodnej nie wywołujemy
konstruktora parametrycznego z klasy pochodnej!!

```c++
class Base
{
    public:
        Base();
        ~Base();
}   

class Derived: public Base
{
    public:
        Derived();
        ~Derived();
}

Base::Base()
{
    cout << "Base constructor";
}

Base::~Base()
{
    cout << "Base destructor";
}

Derived::Derived()
{
    cout << "Derived constructor";
}

Derived::~Derived()
{
    cout << "Derived destructor";
}


int main() {
    Derived *derived = new Derived(); //Wywołanie konstruktora bezparametrycznego klasy Derived
    /*
    Kolejność wywołania
        Base constructor
        Derived constructor
    */

    delete derived; //Wywołanie destruktora klasy Derived
    /*
    Kolejność wywołania
        Derived destructor
        Base destructor
    */
}
```

To co warto zauważyć to kolejność wywoływania. W przypadku konstruktora najpierw wywoływana
jest implementacja konstruktora bazowego, a dopiero później konstruktora dziedziczącego.
W przypadku destruktorów jest odwrotnie.

**** Wywoływanie konstruktorów parametrycznych z klasy bazowej

Pisząc konstruktory w klasie pochodnej mamy możliwość wywołania konstruktorów z klasy 
bazowej np. w celu uzupełnienia atrybutów.

Przykład

```c++
class Base
{
    private:        
        int a;
    public:
        Base(int _a);
}   

class Derived: public Base
{
    private:        
        int b;
    public:
        Derived(int _a, int _b);
}

Base::Base(int _a)
    : a {_a}
{ }

Derived::Derived(int _a, int _b)
    : base(_a), b {_b} //Wywołanie konstruktora z klasy bazowej i nadanie wartości atrybutowo 'b'
{ }

```

Na przykładzie powyżej widzimy konstruktor z dwoma parametrami `a` i `b`. W jego definicji
wywołujemy konstruktor jednoparametrowy `Base(int a)` poprzez wywołanie po `:` słowa kluczowego
`base` oraz przekazaniu parametrów do tego konstruktora, w tym przypadku `a` (`:base(_a)`).
Następnie po przecinku już w tradycyjny sposób przypisujemy do atrybutu `b` przekazany parametr `_b`.
Kolejność wywołania jest taka sama jak w przypadku konstruktora bezparametrycznego, czyli najpierw
zostanie wykonany kod z wywołanego konstruktora bazowego. W klasie pochodnej może odwoływać
się do jakiego chcemy konstruktora z klasy bazowej.

---

<a name="override"></a>
### Nadpisywanie (override) metod klasy bazowej (wstep do polimorfizmu)

Aby nadpisać metode z klasy bazowej w klasie pochodnej jedyne co musimy zrobić to w klasie
pochodnej zdefiniować metode o takiej samej nazwie, z takim samym zwracanym typem i o takich
samych parametrach. Daje to nam możliwość zmiany domyślnego zachowania matody z klasy bazowej
na rzecz 'wyspecjalizowanego' działania z klasy pochodnej.

Przykład:

```c++
class Account
{
    protected:
       int deposit;    
    public:
        void AddToDeposit(int value);
        void SubtractFromDeposit(int value);        
}

void Account::AddToDeposit(int value)
{
    deposit += value;
}

void Account::SubtractFromDeposit(int value)
{
    deposit -= value;
}

class SavingAccount: public Account
{
    private:
       int percent;    
    public:
        void AddToDeposit(int value); //nadpisanie metody z klasy bazowej
}

void SavingAccount::AddToDeposit(int value)
{
    value += value * (percent/100); 
    Account::AddToDeposit(value); //Wywołanie metody AddToDeposit z klasy bazowej Account
        // z powiększoną kwotą
}

int main()
{
    Account account { 1000 };
    SavingAccount savingAccount {1000, 2};

    account.AddToDeposit(2000); //Wywołanie metody z klasy Account
    account.SubtractFromDeposit(100); //Wywołanie metody z klasy Account

    savingAccount.AddToDeposit(2000); //Wywołanie metody z klasy SavingAccount
    savingAccount.SubtractFromDeposit(100); //Wywołanie metody z klasy Account
}
```

Na przykładzie wydzimy klase bazową `Account` i jej klase pochodną `SavingAccount`. 
Jak możemy zauważyć zarówno w klasie bazowej jak i pochodnej mamy metode `void AddToDeposit(int value)`.
W takim przypadku dla obiektów typu `Account` zawsze zostanie wywołana metoda z klasy `Account`.
Natomiast dla obiektów typu `SavingAccount` zawsze zostanie wywołana matoda z klasy `SavingAccount`.
Jak spojrzymy do definicji metody `AddToDeposit` w klasie `SavingAccount` zauważymy wywołanie
`Account::AddToDeposit(value);` jest to wywołanie funkcji `AddToDeposit` z klasy bazowej
`Account`, w tym przypadku robimy to dlatego aby nie powielać kodu. Metoda `AddToDeposit` w klasie
bazowej ma za zadanie dodać pieniądze do naszego depozytu, natomiast metoda `AddToDeposit` w klasie
`SavingAccount` przed dodaniem pieniedzy zwiększa je o konkretny procent. Aby nie powielać
kodu dodawania pieniedzy do depozytu w metodzie `AddToDeposit` w klasie pochodnej wywołujemy
metode z klasy bazowej z już powiększoną kwotą.

Drugą rzeczą wartom zauważenia jest wywołanie metody `SubtractFromDeposit`. Zarówno dla
obiektu typu `Account` jaki i `SavingAccount` zostanie wywołana metoda z klasy bazowej
`Account`, ponieważ klasa `SavingAccount` nie nadpisuje jej w żaden sposób.

---

<a name="zadanie"></a>
### Zadanie

Wraca do nas nasz ulubiony sklep ZofiaPol aby rozszerzyć działanie aplikacji którą dla nich napisaliśmy.
Zainwestowali i oprócz warzyw sprzedają również książki.

Założenia:
* Produkty są przechowywane w magazynie
* Każdy produkt ma swoją nazwe, cene, dostępną ilość oraz typ
* Produkty dzielą się na dwie kategorie
  * Warzywa
    * posiadają termin ważności
    * nazwe dostawcy
  * Książki
    * posiadają autora
    * ilość stron
    
Działanie:
* Sklep ma jeden magazyn do którego możemy dodać wiele róźnych produktów.
* Pracownik powinien móc dodawać produkty różnego typu do magazynu.
* Powinien móc wyświetlić produkty w magazynie