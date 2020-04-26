
# STEP 1 - Introduction
1. [Keywords](#keywords) 
2. [Main function](#main)
3. [Includes](#includes)
4. [Comments](#comments)
5. [Namespaces](#namespaces)
6. [I/O](#io)
7. [Przykład](#przyklad)

---

<a name="keywords"></a>
### Keywords

Tak jak w każdym języku tak i również w `c++` występują zastrzeżone słowa kluczowe, któch
nie można używać w innym konktekście niż ich przeznaczenie. Do słów kluczowych należą m.in.
typy zmiennych, takie słowa jak `class`, operatory `<<` i '>>'. 

Pełną liste można znaleść na stronie: https://pl.cppreference.com/w/cpp/keyword
  
---

<a name="main"></a>
### Main function

Funkcja `main` jest główną funkcją programu. Program uruchamiany jest od tej funkcji oraz
cały kod programu znajduje się w funkcji `main`. Występuje ona w dwóch odmianach

* bezparametrycznej
```c++
int main()
{
    //kod programu
    return 0;
}
```
* parametrycznej
```c++
int main(int argc, char *args)
{
    //kod programu
    return 0;
}
```  
W funkcji parametrycznej pierwszy parametr `argc` zawiera ilość przekazanych do funkcji dodatkowych
parametrów uruchomieniowych. Natomiast drugi parametr `args` zawiera przekazane parametry.

---

<a name="includes"></a>
### Includes

Przy pomocy dyrektywy `#include` możemy importować do naszego programu standardowe biblioteki
`c++` takie jak biblioteka `<iostream>`, zewnętrzne biblioteki utworzone przez społeczność
lub naszę biblioteki lub klasy nagłówkowe. `Include` zawsze umieszcza się na początku pliki
w którym chcemy skorzystać z danego importu.

```c++
#include <iostream> //pobranie standardowej biblioteki c++
#include "moja_klasa.h" //pobranie pliku nagłówkowego
```

---

<a name="comments"></a>
### Comments

W `c++` występują dwa typy komentarzy

* jednolinijkowe
```c++
// Komentarz jednolinijkowy
```
* wielolinjkowe
```c++
/*
    Komentarz
    wielolinijkowy
*/
```

Ważne jest aby nie używać komentarzy do opisywania każdej linii kodu. Dobrą praktyką
jest tak nazywać zmienne czy funkcje aby kod 'sam się opisywał'. Komentarze należy
stosować tylko w newraligicznych miejscach.

---

<a name="namespaces"></a>
### Namespaces

Przestrzeń nazw określa w którym miejscu i do jakiej biblioteki lub zbioru bibliotek należy
dana funkcja czy klasa. Łatwo sobie wyobraźić że w dwóch zainkludowanych bibliotekach
znajdzie się metoda o tej samej nazwie. W takiej sytuacji kompilator nie wie jakiej funkcji
z której biblioteki chcemy użyć. Dlatego przy użyci funkcji, klas czy czegokolwiek z bibliotek
zewnętrznych przy wywołaniu dodajemy jej `namespace`.

```c++
#include <iostream>

int main()
{
    std::cout << "abc";
}
```

Biblioteka `iostream` swoje funkcje przechowuje w przestrzeni nazw `std` dlatego przy
wywołaniu funkcji `cout` z tej biblioteki przed `::` wskazujemy z jakiego namespaca funkcje
chcemy wywołać.


Nie jest to zalecane w dużych projektach ale na początku pliku możemy określić jaki `namespace`
jest domyślnie używany. 

```c++
#include <iostream>

using namespace std;

int main()
{
    cout << "abc";
}
```

W takim przypadku przy wywoływaniu funkcji z przestrzeni nazw `std` nie musimy dodawać za
każdym razem `std::`.

---

<a name="io"></a>
### I/O

`c++` posiada standardową biblioteke `iostream` do wypisywania i pobierania danych na ekran
konsoli. Służą do tego dwie metody:
* `cout` - do wypisywania
* `cin` - do przypisywania do zmiennych wartości wprowadzonej przez użytkownika

```c++
#include <iostream>

int main()
{
    int i;
    std::cout << "Podaj liczbę: "; //Wysyłam do (<<) streamu cout wybrany tekst
    std::cin >> i; //zapisujemy do zmiennej i wartość wpisaną przez użytkownika
}
```

Wprowadzone dane przez użytkownika automatycznie konwertowane są na typ do którego
chcemy przypisać wartość. W przypadku powyżej jest to liczba całkowita. Jeżeli to się
nie uda w programie wystąpi błąd w działaniu. 

---

<a name="przyklad"></a>
### Przykład

* Wypisanie argumentów uruchomieniowych funkcji `main`
* Deklaracja zmiennych
* Wypisywanie wartości na ekran
* Pobieranie wpisanej wartości przez użytkownika

```c++
#include <iostream>

using namespace std;

int main(int argc, char *argv[])
{
    for (int i = 1; i < argc; i++)
    {
        cout << argv[i] << "\n";
    }

    int favorite_number;
    int favorite_number2;

    cout << "Podaj swoje dwie ulubione liczby oddzielone spacją: ";
    cin >> favorite_number >>  favorite_number2;

    cout << favorite_number <<  endl;
    cout << favorite_number2 << endl;
}
```