
# STEP 6 - Functions
1. [Wstęp](#wstep) 
2. [Definicja](#definicja)
3. [Prototypy](#prototypy)
4. [Parametry](#parametry)
5. [Domyślna wartość](#domyslna)
6. [Przeładowanie](#przeladowanie)
7. [Tablica jako parametr](#parametr)
8. [Referencje](#referencje)
9. [Zmienne statyczne](#statyczne)
10. [Rekurencja](#rekurencja)
11. [Zadania](#zadania)

---

<a name="wstep"></a>
### Wstęp

Funkcje pozwalają nam w łatwy sposób zmodularyzować nasz program. Dwoma głównymi 
paradygmatami funkcji są:
* wydzielenie logiki odpowiadającej za konkretne zadanie
* reużywalność w wielu kontekstach bez dublowania pisanego kodu

---

<a name="definicja"></a>
### Definicja

Definicja funkcji składa się z 4 głównych części `zwracany_typ nazwa_funkcji(parametry_funkcji) { ciało_funkcji }`
* Pierwsza wartość to `zwracany_typ` funkcji, może to być każdy typ znajdujący się w `c++` 
(`int`, `string` itd.) lub typ klasy wbudowanej czy napisanej przez nas. 
Jeżeli funkcja nie zwraca żadnej wartości w miejsce wpisujemy `void`.
* Następnie definiujemy `nazwe_funkcji`. Nazwa funkcji powinna jasno określać co robi nasza funkcja.
* Funkcja może przyjmować dowolną liczbę `parametrów_funkcji` różnych typów.
* `ciało_funkcji` to cała logika potrzebna do wykonania zadania do którego została stworzona.

Zdefiniowaną funkcje wywołujemy poprzez odwołanie do jej nazwy i uzupełnienie wymaganych parametrów

```c++
#Include <iostream>
#Include <string>

//Przyklad funkcji z dwoma parametrami int zwracającej większy element
int get_max(int a, int b)
{
    if (a > b)
    {
        return a;
    }
    return b;
}

//Funkcja wypisująca na ekran przekazany text
void write_line(std::string text)
{
    std::cout << text;
}

int main()
{
    int a { 5 };
    int b { 6 };

    int c = get_max(a, b);

    std::cout << "Większa wartość to: " << c;
}
```

Aby kompilator mógł 'odnależć' zdefiniowaną przez nas funkcje. Musi być ona zdefiniowana przed jej
wywołaniem (wyżej niż wywołanie).

---

<a name="prototypy"></a>
### Prototypy

Z racji tego że kompilator musi wiedzieć o istnieniu funkcji przed jej użyciem, zamiast pisać
definicje funkcji zawsze na górze pliku, możemy użyć prototypu funkcji.

Prototyp funkcji wygląda dokładnie tak samo jak jej definicja, z tą różnicą że nie zawiera logiki,
ciała funkcji. 
Prototypy funkcji umieszczamy
* na górze pliku (praktykowane w małych programach)
* w oddzielnym pliku nagłówkowym `.h` (O tym w późniejszym czasie)

W przykładzie powyżej aby kompilator nie rzucał nam błędu całą definicje funkcji musieliśmy
umieścić przed główną funkcją `main`. Przy użyciu prototypów już nie musimy się o to martwić.

```c++
#Include <iostream>
#Include <string>

//prototypy naszych funkcji
int get_max(int a, int b);
void write_line(std::string text);

int main()
{
    int a { 5 };
    int b { 6 };

    int c = get_max(a, b);

    std::cout << "Większa wartość to: " << c;
}

//Przyklad funkcji z dwoma parametrami int zwracającej większy element
int get_max(int a, int b)
{
    if (a > b)
    {
        return a;
    }
    return b;
}

//Funkcja wypisująca na ekran przekazany text
void write_line(std::string text)
{
    std::cout << text;
}

```

---

<a name="parametry"></a>
### Parametry

Ważne jest aby wiedzieć że zmienne przekazywane do funkcji są kopią przekazanych zmiennych.
(Oczywiście da się przekazać dokładnie tą zmienną którą chcemy i na niej operować już wewnątrz
funkcji ale o tym później).

```c++
void param_test(int a);

int main()
{
    int a { 5 };

    cout << a << endl; //Wypiszę 5
    param_test(a);//Przekazujemy zmienną "a" jako parametr funkcji
    cout << a << endl; //Wypiszę 5

}

void param_test(int a) //przekazany parametr "a" to kopia zmiennej "a" z main()
{
    cout << a << endl; //Wypiszę 5
    a = 10;
    cout << a << endl; //Wypiszę 10    
}
```

W przykładzie powyżej widać że mimo tego iż w funkcji `param_test` zmieniliśmy wartość przekazanej
zmiennej `a`, to po wykonaniu funkcji i wypisaniu wartości tej zmiennej w funkcji `main` nie uległa
ona zmianie, a to dlatego że właśnie dlatego że w funkcji `param_test` operowaliśmy na kopi.

---

<a name="domyslna"></a>
### Domyślne wartości argumentów

Wywołując funkcje musimy podać jej wszytskie wymagane parametry. Istnieje jednak możliwość 
nadania parametrą funkcji wartości domyślniej. W takiej sytuacji jeżeli nie podamy takie parametru
przy wywołaniu funkcji, za wartość tego parametru zostanie przyjęta wartość domyślna. Wartości  domyślne
parametrów możemy przypisać w prototypie funkcji.

```c++

void param_test(int b, int a = 10);

int main()
{
    int a { 5 };
    int b { 1 };
    
    //W takim przypadku funkcje param_test możemy wywołać na dwa sposoby
    param_test(b, a);//przekazując parametr "a"
    param_test(b);//oraz nie przekazując go wtedy jako jego wartość zostanie 
                  //przekazana wartość domyślna

}

void param_test(int b, int a)
{
    ...
}
```

---

<a name="przeladowanie"></a>
### Przeladowanie

W `c++` możemy mieć zdefiniowane wiele funkcji o różnych nazwach. Niekiedy jednak chcemy
zdefiniować funjcje zajmujące się tym samym jednak na trochę innym zestawie danych (parametrach).
Jest to jak najbardziej możliwe i nazywamy to przeładowaniem funkcji. Zasada definiowania przeładowania
dla funkcji jest taka że
* funkcja musi mieć tą samą nazwe
* ale inny zestaw parametrów wejściowych (inna ilość parametrów lub różne typy parametrów)

Dzięki takiemu podejściu mamy również zachowaną zasade polimorfizmu, czyli inna forma dla tego samego
konceptu pod tą samą nazwą.

Załóżmy że chcemy  mieć funkcje wypisującą na ekran przekazaną wartość, niezależnie jaka ona by nie
była. Jednak napotykamy na problem ponieważ nie chcemy mieć wielu funkcji w stylu `write_string`, `write_int`
itd. Wygodniej było by używać jednej nazwy `write`. Dla tego zadania możemy użyć właśnie przeładowania funkcji.

```c++
write(int i);
write(string a);

int main()
{
    string name { "abc" };
    int number { 1 };
    
    write(number);
    write(name);
}

void write(int i)
{
    cout << i;
}

void write(string a)
{
    cout << a;
}
```

W przykładzie powyżej dzięki temu że użyliśmy innych typów dla parametrów funkcji kompilator wie
kiedy której funkcji ma użyć w zależności od przekazanego parametru

W przeładowaniu funkcji należy uważać na nadawanie domyślnch wartości parametrom. Wyobraźmy sobie
taką sytuacje. 

```c++

void test(int a, int b = 100);
void test(int a);

int main()
{
    test(5);
}

void test(int a, int b)
{
    ...
}

void test(int a)
{
    ...
}

```

W przykładzie powyżej kompilator zwróci błąd ponieważ mimo tego że obie funkcje mają inną liczbe
parametrów, kompilator nie będzie w stanie określić czy chcemy wywołać funkcje z jednym podanym parametrem,
czy może funkcje z dwoma parametrami w tym jednym z domyślną wartością.

<a name="parametr"></a>
### Tablica jako parametr

Przekazując tablice jako parametr funkcji, elementy tablicy nie są kopiowane, kopiowany jest jedynie
adres w pamieci w jakim znajduje się tablica (adres pierwszego elementu tablicy). Dlatego aby móc
iterować po przekazanej tablicy wewnątrz funkcji i nie przekraczać jej zakresu, wraz z tablicą musimy
przekazać również jej rozmiar.

```c++
void test(int size,int tab[])
{
    for (int i = 0; i < size; i++)
    {
        cout << tab[i] << " ";
    }
}
```

Jako że w przypadku tablic nie przekazujemy kopii obiektu tablicy tylko jej adres, wewnątrz funkcji operujemy
dokładnie na tym samym obiekcie tablicy co w funkcji wywołującej. Należy uważać bo zmieniając wartości
tablicy wewnątrz funkcji zmieniamy je również wszędzie indziej gdzie ta tablica jest używana.

```c++
void test(int size,int tab[]);

int main()
{
    int tab[] { 1,2,3,4,5 }
    print_tab(tab); //Wypisze 1 2 3 4 5
    test(5, tab);
    print_tab(tab); //Wypisze 2 3 4 5 6
}

void test(int size,int tab[])
{
    for (int i { 0 }; i < size; i++)
    {
        tab[i]++; //tab[i] = tab[i] + 1;
    }
    print_tab(tab); //Wypisze 2 3 4 5 6
}
```

Jak widać zarówno w funkcji `main` jak i w funkcji `test` operowaliśmy na tym samym obiekcie tablicy

Jednym z sposobów zabezpieczenie przed zmianą wartości tabeli w funkcji. Jest oznaczenie parametru w prototypie
jako wartość stała `const` (Każdy parametr funkcji możemy oznaczyć jako stała). Wtedy każda jego
próba zmiany wewnątrz funkcji będzie kończyła się błędem kompilatora;

```c++
void test(int size,const int tab[]);

int main()
{
    int tab[] { 1,2,3,4,5 }
    print_tab(tab); //Wypisze 1 2 3 4 5
    test(5, tab);
    print_tab(tab); //Wypisze 2 3 4 5 6
}

void test(int size,int tab[])
{
    for (int i { 0 }; i < size; i++)
    {
        tab[i]++; //compile error
    }
    print_tab(tab); //Wypisze 2 3 4 5 6
}
```

---

<a name="referencje"></a>
### Referencje

Czasem do funkcji nie chcemy przekazywać kopii naszej zmiennej, a dokladnie zmienną i na niej operować
(referencje). Możemy to zrobić przekazując referencje do funkcji zamiast kopii. Robimy to oznaczając 
parametr znakiem `&` przed nazwą parametru w funkcji. W takim zapisie kompilator wie że ma przekazać
referencje do zmiennej jako parametr a nie go kopiować.

```c++
void param_test(int &a);

int main()
{
    int a { 5 };

    cout << a << endl; //Wypiszę 5
    param_test(a);//Przekazujemy zmienną "a" jako parametr funkcji
    cout << a << endl; //Wypiszę 10

}

void param_test(int &a) //przekazany parametr "a" jako referencja
{
    cout << a << endl; //Wypiszę 5
    a = 10;
    cout << a << endl; //Wypiszę 10    
}
```

Jak widać na przykładzie powyżej przekazując referencje do funkcji i zmieniając wartość parametru
zmieniamy również 'originalną' zmienną

---

<a name="statyczne"></a>
### Zmienne statyczne

Zdefiniowana zmienna statyczna jest inicjalizowana tylko raz. W każdym kolejnym przebiegu kiedy 
program trafia na jej inicjalizacje ignorują ją. Do definicji zmiennej statycznej używamy słowa 
kluczowego `static`.

```c++
void func_with_static();
void func_without_static()

int main()
{
    func_with_static(); //Wypisze 500 (piewsze wywołanie zmienna statyczna zostanie zainicjalizowana)
    func_with_static(); //Wypisze 501
    func_with_static(); //Wypisze 502

    func_with_static(); //Wypisze 500 
    func_with_static(); //Wypisze 500
    func_with_static(); //Wypisze 500 (Za każdym razem wypisze tą samą wartość ponioeważ
                        //             przy każdym uruchomieniu zmienna jest inicjalizowana na nowo)
}

void func_with_static()
{
    static int a { 500 };
    cout << a << " ";
    a += 1;
}

void func_without_static()
{
    int a { 500 };
    cout << a << " ";
    a += 1;
}

```

---

<a name="rekurencja"></a>
### Rekurencja

Funkcjami rekurencyjnymi nazywamy funkcje które w własnym ciele wywołują samą siebie. Funkcja
rekurencyjna musi posiadać określony warunek zakończenie aby nie doprowadzić do zapętlenia i w
rezultacie wysypania programu.

```c++
void reverse(int n)
{
    if (n == 1)
    {
        cout << n;
    }
    else
    {
        cout << n;
        return reverse(n-1);
    }
}
```

Powyżej widać prostą funkcje rekurencyjną która wypisuję liczby od `n` do 1. Warunkiem końca wykonywania
rekurencji jest `n == 1`

---

<a name="zadania"></a>
### Zadania

```c++
#include <iostream>

int main()
{
    /*
    Zadanie 1.
    Przerób zadanie z step 5 tak aby program zawierał dwie funkcje wraz z swoimi prototypami
        * Funkcja szyfrująca (jako parametr przyjmuje string tekstu, zwraca zaszyfrowany tekst)
        * Funkcja deszyfrująca (jako parametr przyjmuje zaszyfrowany string, zwraca odszyfrowany tekst)
    */

    /*
    Zadanie 2.
    Napisz funkcje zwracającą odsetek zmarłych osób na Covid-19.
    Funkcja ma przyjmować dwa parametry, liczbe zarażonych osób danego dnia, oraz liczbe zmarłych.
    Z założeniem że liczba zmarłych osób na dzień wynosi 0.
    Funkcje wywołaj kilka razy ręcznie w funkcji main();
    */

    /*
    Zadanie 3.
    Napisz funkcje wraz z przeładowaniami obliczającą średnią ocene ucznia.
    Funkcja ma mieć trzy postacie:
        * Pierwsza funkcja przyjmuje jako paremetr sume ocen oraz ich ilosc
        * Druga funkcja przyjmuje jako parametr tablice ocen końcowych (oraz ilosc ocen w tablicy)
        * Trzecia funkcja przyjmuje jako parametr wektor ocen koncowych
    */

    /*
    Zadanie 4.
    Sklep internetowy poprosił Cię o napisanie aplikacji dla ich pracowników ułatwiającą wprowadzanie nowych
    artykułów do bazy.
        * Użytkownik wprowadza w konsoli ile produktów chce wprowadzić
        * Następnie w pętli użytkownik wprowadza nazwy oraz ceny (int) produktów
            * Dodawanie produktów do dwoch wektorow (jednego dla ceny, drugiego dla nazwy) wykonujemy w osobnej funkcji
            * W funkcji przy pomocy zmiennej statycznej obliczamy ogólny rabat % na stronie zwiększający się o 0.2% za każdym razem kiedy
              cena dodawanego produktu jest wieksza niz 100
            * Z funkcji zwracamy aktualny rabat
        * Po dodaniu wsztystkich produktów wyświetlamy podsumowanie:
        [1] nazwa_produktu cena
        [2] nazwa_produktu cena
        [3] nazwa_produktu cena
        [4] nazwa_produktu cena
        [N] nazwa_produktu cena
        Wartosc całkowita: suma_cena
        Naliczony rabat: X.X %
    */

    /*
    Zadanie 5.
    Napisz funkcje rekurencyjną obliczającą silnie z N. N podajemy jako parametr funkcji
    */
}
```
