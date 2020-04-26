
# STEP 2 - Variables
1. [Memory](#memory) 
2. [Initialization](#initialization)
3. [Scope var](#scope)
4. [Types](#types)
5. [Constants](#constants)
6. [Przykład](#przyklad)
7. [Zadanie](#zadanie)

---

<a name="memory"></a>
### Memory

W `c++` tak jak w innych językach programowania mamy kilka standardowych typów danych. 
Przy inicjalizacji zmiennej, jest rezerwowane dla niej miejsce w pamięci w którym jej wartość
będzie przechowywana. Każdy typ danych zajmuje określoną ilość bajtów pamięci.

![](https://www.ntu.edu.sg/home/ehchua/programming/cpp/images/MemoryAddressContent.png)

---

<a name="initialization"></a>
### Initialization

W c++ możemy inicjalizować zmienne na kilka sposóbów:

```c++
int age = 21;
int age (21);
int age {21}; //Ten sposób inicjalizacji jest najnowszym standardem
```

Przy deklarowaniu zmiennej nie musimy od razu nadawać jej wartości;

```c++
int age;
cout << age;
```

Jednak w takiej sytuacji musimy pamiętać o tym że w komórkach pamięci przydzielonych dla tej zmiennej
znajdują się dane (ciąg 0 i 1) które już tam wcześniej były. Dlatego przy wypisaniu takiej niezinicjalizowanej
zmiennej dostaniemy jakąś losową liczbe wynikającą z wartości która była w tym obszarze pamięci.
Dopiero przy inicjalizacji zostanie ona nadpisana.

---

<a name="scope"></a>
### Scope

Stworzone zmienne posiadają swój odpowiedni zakres widoczności w którym mogą być używane. 
'Widoczność' zmiennych najczęście zamyka się w `{ }` klamrach w których zostały zadeklarowane.

```c++
int main()
{
    int a {5};

    if (a == 5)
    {
        int b {6};
        cout << a << " " << b; //Wypisze 5 6
    }
}
```
W przykładzie powyżej zmienna `a` będzie widoczna w całej funkcji `main`.
Natomiast zmienna `b` w bloku `if`. 

W `c++` mamy możliwość tworzenia zmiennych globalnych. Deklarujemy je w głównym 'ciele' pliku i ich 
zakres widoczności jest globalny dla pliku.

```c++
int a {5};

int main()
{
    if (a == 5)
    {
        int b {6};
        cout << a << " " << b; //Wypisze 5 6
    }
}
```

Jedną z właściwości zmiennych w `c++` jest to że zmienne mogą być 'przykrywane' w 'podzakresach'.
Kompilator widząc użycie zmiennej 'szuka' jej dekleracji najpierw w scopie w którym się znajduje,
jeżeli jej nie znajdzie szuka w zakresach wyżej. Najlepiej to wyjaśnić na przykładzie.

```c++
int main()
{
    int a {5};

    if (a == 5)
    {
        int a {6};
        cout << a; //Wypisze 6
    }
        
    cout << a; //Wypisze 5
}
```  

---

<a name="types"></a>
### Types

W `c++` mamy standardowe typy zmiennych przechowujące różne typy danych, takie jak wartości całkowite `int`, `long`.
Liczby zmiennoprzecinkowe `double`, `float` czy znaki `char`. Kilka rzeczy które warto wiedzieć o typach.
Każdy typ ma maksymalną wartość którą może przyjąć. Dla liczb możemy zwiększyć tą wartość poprzez ograniczenie
możliwych do przypisania liczb do wartości dodatnich.

```c++
int a { 2147483647 } //signet int max value
unsigned a { 4294967295 } //unsigned int max value
```

Przy przypisywaniu wartości zmiennym należy uważać aby nie przekraczać ich maksymalnej lub minimalnej wartości. 
Przy przekroczeniu wartości typu następuję przepełnienie i wartość która w nim się znajduję staję się
'losowa'. W pewien sposób można uchronić się przed inicjalizacją zmiennej wartością z poza zakresu. 
Używając sposobu inicjalizacji z standardu c++11.

```c++
int a = 3000000000 //kompilator nie zwróci błędu. Program odpali się z błędnymi danymi
int a { 3000000000 } //kompilator zwróci błęd. Program nie wykona się
```

---

<a name="constants"></a>
### Constants

Zmienne statyczne charakteryzują się tym iż podczas deklaracji przypisujemy im wartość i nigdy
później nie może ona zostać zmieniona. Używa ich się dla wartości stałych, niezmiennych.
Zmienne statyczne deklarujemy poprzez dodanie do doklaracji słowa kluczowego `const`

```c++
const double pi {3.14};
a = 3.15; //Błąd kompilacji
```

---

<a name="przyklad"></a>
### Przykład
* Przykład przedstawiający widoczność oraz zakres zmiennych
```c++
#include <iostream>

int b{ 4 };

int main()
{
    int a { 5 };

    if (a == 5)
    {
        int a { 6 };
        std::cout << a << std::endl; //Wypisze 6
    }

    std::cout << a << std::endl; //Wypisze 5
    std::cout << b << std::endl; //Wypisze 4
}
```


---

<a name="zadanie"></a>
### Zadanie

```c++
int main()
{
    //test();

    /*
     Zadanie
     ------------
     Sklep oferujący usługi czyszczenia pomieszczeń poprosił Cię o napisanie programu
     estymującego cene ich pakietu usług dla subskrypcji 30 dniowej.

     Cena sprzątania małego pokoju: 40 zł
     Cena sprzątania dużego pokoju: 70 zł
     Zniżka dla pakietu 30 dniowego: 6%;

     Użytkownik na początku programu musi mieć możliwość wprowadzenia ilości małych i dużych pokoi.
     Po obliczeniach musi dostać informacje w formie:

     Cena sprzątania małego pokoju: 40 zł
     Cena sprzątania dużego pokoju: 70 zł
     Zniżka dla pakietu 30 dniowego: 6%
     Wybrana ilość małych pokoi: X
     Wybrana ilość dużych pokoi: Y
     Cene po obniżce: Z zł
     Estymacja na 30 dni.

     Pokoje sprzątane są codziennie
    */
    return 0;
}
```