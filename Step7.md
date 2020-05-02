
# STEP 7 - Wskaźniki i referencje
1. [Wstęp](#wstep) 
2. [Deklaracja](#deklaracja)
3. [Dostęp i przechowywanie wartości](#dostep)
4. [Odwołania do wartości *](#odwolania)
5. [Dynamiczna alokacja pamięci](#dynamiczna)
6. [Relacja pomiędzy tablicami a wskaźnikami](#tablica)
7. [Operacje na wskaźnikach](#artmetyka)
8. [Wskaźniki i stałe](#stale)
9. [Wskaźnik jako parametr funkcji](#funkcja)
10. [Zwracanie wskaźnika z funkcji](#zwracanie)
11. [Problemy i błędy](#bledy)
12. [Co to jest referncja?](#referencja)
13. [Podsumowanie](#podsumowanie)

---

<a name="wstep"></a>
### Wstęp

Czym jest wskaźnik?
* Jest zmienną - wartością wskażnika jest adres
* Czego adresy może zawierać?
  * do innej zmiennej
  * do funkcji
* aby użyć danych na które wskaźnik wskazuje musimy znać ich typ

Po co używać wskaźników?
* Jeżeli istnieje możliwość powinno się używać zmiennych, a nie wskaźników
* Jednak niektóre sytuacje wymagają od nas użycia wskaźnika:
  * Jeżeli nie mamy dostępu do originalnej zmiennej bo jesteśmy po za jej zakresem widoczności i niemożemy
    odwołać się do niej bezpośrednio po nazwie, np. wewnątrz funkcji
  * Ułatwiają zarządzanie i operowanie na tablicach
  * Możemy dynamicznie allokować pamięć na stercie:
    * taka pamięć nie posiada nazwy zmiennej
    * możemy się do niej odwołać tylko przez wskaźnik
  * Kiedy chcemy odwołać się do specyficznego miejsca w pamięci
    * programując urządzenie zewnętrze
    * albo komunikując się z aplikacjami systemowymi/zewnętrznymi 

---

<a name="deklaracja"></a>
### Deklaracja

Dekalracja wskaźników wygląda bardzo podobnie jak deklaracja normalniej zmiennej.

```c++
int *int_ptr;
double *double_ptr;
string *string_ptr;
```

Jedną różnicą jest dodanie znaku `*` który mówi nam że deklarujemy wskażnik.

Wskażniki posiadają typ, musi on byc taki sam jak typ zmiennej czy funkcji na jaki wskazują.
W przeciwnym wypadku kompilator zwróci błąd. Deklaracje wskaźników czytamy od prawej
do lewej. np. `int *int_ptr;` odczytujemy jako zmienna `int_ptr` wskazuje (`*`) na typ `int`

Dobrą praktyką przy deklaracja wskaźników jest ich inicjalizacja aby nie wskazywały na losowe
miejsce w pamięci z śmieciowymi wartościami. Inicjalizacja wskaźnika odbywa się w taki sam sposób
jak normalnych zmiennych.

```c++
int *int_ptr { };
double *double_ptr { nullptr }; 
string *string_ptr { nullptr };
```

Jeżeli nie wiemy na jaki obiekt wskaźnik będzie wskazywał, inicjalizujemy go przy pomocy `{ }` bez 
nadanej wartości lub słowem kluczonwym `nullptr`. Oznacza to że wskażnik wskazuje 'nigdzie' (na odres 0).
Jest zasadnicza róźnicz pomiędzy tym że wskaźnik nigdzie nie wskazuję, a wskazuję gdziekolwiek na miejsce
w pamięci które nie wiemy gdzie się znajduję.

---

<a name="dostep"></a>
### Dostęp i przechowywanie wartości *

Szybkie przypomnienie, aby pobrać adres zadeklarowanej zmiennej używamy operatora '&'.

```c++
int num {10};

cout << "Value: " << num; // 10
cout << "Size: " << sizeof num; // 4
cout << "Address: " << &num; // 0x61ff1c
```

Teraz zobaczy jak działają te same opertory dla wskaźników.

```c++
int *num_ptr; //niezainicjalizowany wskaznik

cout << "Value: " << int_ptr; // 0x61ff223 (śmieciowy adres)
cout << "Addres: " << &int_ptr; // 0x616382 (adres wskaznika)
cout << "Rozmiar: " << sizeof int_ptr; // 4

num_ptr = nullptr; //inicjalizujemy 

cout << "Value: " << int_ptr; //0  
```

Jak widać nie zainicjowany wskaznik przechowuje jako wartość losowy adres w pamięci.
Dopiero przypisanie mu wartości `nullptr` ustawiamy jego wartość na 0 (czyli nie wskazuje
nigdzie). Wskaznik posiada tez własny adres pamięci w którym jest umieszczony i aby go pobrać
używamy operatora `&`. Rozmiar wskaźnika to nie rozmiar typu na jaki wskazuje, rozmiar jest
zależny od potrzebnej ilości pamięci potrzebnej do przechowania adresu który wskaźnik
przechowuje. 

Tak jak zostało powiedziane wcześniej wskaźniki mogą tylko wskazywać na zmienne tego
samego typu. Próba przypisania zmiennej innego typu niż wskażnik zakończy się
błędem kompilatora lub wysypaniem programu.

```c++
int num {10};
double double_num {3.14};

int *int_ptr { nullptr };

int_ptr = &num; //przypisujemy adres zmiennej num do wskaznika
int_ptr = &double_num; //Błąd kompilacji ponieważ próbujemy przypisać
                       //adres zmiennej innego typu niż wskaźnik
```

Wskaźniki są jak zmienne dlatogo:
* możemy zmieniać ich wartości (miejsca na jakie wskazują).
* mogą przyjmować wartość `null`
* mogą nie być inicjalizowane

```c++
int num {10};
int num2 {20};

int *int_ptr { nullptr };

int_ptr = &num; //przypisujemy adres zmiennej num do wskaznika
int_ptr = &num2; //zmieniamy adres przypisanej zmiennej na num2
```

---

<a name="odwolania"></a>
### Odwolania do wartosc

Przy pocy wskaznika mozemy przeprowadzan operacje na zmiennej na którą wskazuje.
Możemy to zrobić w przypadku gdy wskazuje na poprawny adres (na zmienną która istnieje)
Aby odwołać się do wartości jaka przechowywana jest w zmiennej na którą wskazuje
wskaźniek używamy operatora 'dereferencyjnego' `*`.
 
```c++
int num {10};
int *int_ptr { &num }; //inicjalizujemy wskaźnik adresem zmiennej num

cout << int_ptr; //0x61ffff (Adres zmiennej num)
cout << &num; //0x61ffff (Adres zmiennej)

cout << *int_ptr; //10 (wartosc zmiennej num)
cout << num; //10 (wartość zmiennej)

*int_ptr = 20; //zmieniamy wartość umieszczoną w adresie na który wskazuje wskaźnik

cout << *int_ptr; //20 (wartosc zmiennej num)
cout << num; //20 (wartość zmiennej)

num = 30;
cout << *int_ptr; //30 (wartosc zmiennej num)
cout << num; //30 (wartość zmiennej)
```

Jak łatwo zauważyć wartość którą przechowuje wskaźnik jest dokładnie adresem
zmiennej którą mu przypisaliśmy. Wynika z tego że odwołując się do wskaźnika
przez operator `*` operujemy dokładnie na miejscu w pamięci które wskaźnik przechowuje.
Dlatego zmieniając tą wartość w tym przypadku na `20`, nasza zmienna `num` również 
zmieniła wartość (Bo to to samo miejsce w pamięci). Również odwrotnie zmieniając
wartość zmiennej `sum` nasz wskaźnik `int_pre` odwołując się do wartości zmiennej 
na jaką wskazuje (poprzez `*`) równiż się zmieni.

Zmieniając miejsce w pamięci na jakie wskaźnik przechowuje, zmieniamy również
wartość przechowywaną pod tym adresem.

```c++
int num {10};
int num2 {20};

int *int_ptr { &num };

cout << *int_ptr; //10

int_ptr = &num2;

cout << *int_ptr; //20

 
```

---

<a name="dynamiczna"></a>
### Dynamiczna alokacja pamieci

Zasadniczą róźnicą pomiędzy dynamiczną alokacją pamięci a standardową deklaracją
jest to że alakując pamięć dynamicznie rezerwujemy ją na stosię (Ogólno dostępna pamieć programu) 
a nie w bloku funkcji, czy w innym miejscu zamkniętym w bloku. 

| Memory     |
|------------|
| Heap       |
| Function A |
| Function B |
| Globals    | 
| Code       |

W tabeli powyżej widzimy symboliczny podział pamięci programu. Jest on podzielony
na kilka części. W odrębnych obszarach jest przechowywany nasz kod, zmienne globalne,
oddzielna pamięć jest wydzielona na wartości i zmienne dla kokretnych funkcji i
na końcu mamy ogólno destępny stos pamięci z którego aplikacja może korzystać.

Dlaczego alokujemy pamięć?
* Ponieważ często przy deklaracji nie wiemy ile pamięci będziemy potrzebować
* Możemy alokować pamięć już podczas trwania programu
* Tabele
  * Możemy stworzyć tabele z rozmiarem na podstawie innych zmiennych w programie
* Możemy używać wskaźników aby dostać się do nowo zaalokowanej pamięci na stosię

Kilka słów wyjaśnienia. Wcześniej mówiłem żeby używać wektorów zamiast tabli
i to jest nadal prawda. Lecz warto wiedzieć jak allokować pamięć dla tablic
i ułatwia to zrozumienie samego zagadnienia alokacji pamięci.

Do dynamicznego alokowania pamięci służy słowo kluczowe `new` wraz z typem jaki
określającym rozmiar jaki będzie zaalokowany.

```c++
int *int_ptr { nullptr };
int_ptr = new int; 
cout << int_ptr; //0x6182797
cout << *int_ptr; //483247237

*int_ptr = 100;
cout << *int_ptr; //100
```

Jak widać na przykładzie powyżej stworzyliśmy wskaźnik nie wskazujący nigdzie.
Następnie przypisaliśmy do niego poprzez `new int` nowę miejsce w pamięci na stosie.
W tym momencie widzimy że wskaźnik wskazuje na jakieś miejsce w pamięci które
zawiera śmieciowe dane które się tam znajdowały. Przypisać wartość do nowo 
zaalokowanej pamięci możemy poprzez wskaźnik.

Alokując pamięć poprzez wskaźnik nie mamy żadnej zmiennej do której możemy 
się odwołać aby pobrać wartość. Jedyn połączeniem z tą komórką pamięci jest
wskaźnik. Dlatego należy pamiętać że w sytuacji kiedy nie potrzebujemy już
danych które znajdują się w pamięci którą zaalokowaliśmy należy ją usunąć. Robi 
się to poprzez słowo kluczowe `delete` wraz z nazwą wskaźnika.

```c++
int *int_ptr { nullptr };
int_ptr = new int; 
cout << int_ptr; //0x6182797
cout << *int_ptr; //483247237

*int_ptr = 100;
cout << *int_ptr; //100

delete int_ptr;
```

Ta same zasady obowiązują z dynamiczną alokacją pamięci również z tablicami,
chodciaż składnia się delikatnie róźni. Alokując pamięć dynamicznie dla tablic
za typem jaki przechowuje tablica w nawiasach kwadratowych podajemy alokowany
rozmiar. Przy zwalnianiu miejsca pamięci po tablicy po słówku `delete` dodajemy
pustę nawiasy kwadratowe `[]`.

```c++
int *array_ptr { nullptr };
int array_size;
cout << Podaj rozmiar tablicy:;
cin >> array_size;

array_ptr = new int[array_size];

delete [] array_ptr;
```

Jak na przykładzie powyżej dzięki dynamicznej alokacji pamięci dla tablicy 
możemy w trakcie programu pobrać rozmiar tablicy od użytkownika i na jego
podstawie zaalokować odpowiednią ilość miejsca w pamięci.

Podczas alokowania dynamicznie pamięci należy uważać aby nie stracić połączenia
wskaźnika z tą pamięcią. W przypadku kiedy to się stanie nie będziemy mogli
przez cały czas trwania programu zwolnić miejsca zajmowanego w pamięci przez
tą alokacje.

```c++
int *int_ptr { nullptr };
int_ptr = new int; 
int_ptr = nullptr;

delete [] int_ptr;
```

W kodzie powyżej zmieniliśmy miejsce na które wskazuje nasz wskaźnik, dlatego
nie uda nam się już dostać do pamięci zajmowanej przez wcześniejsze zainicjowanie
pamięci przez `new int`.

---

<a name="tablica"></a>
### Relacja pomiędzy tablicami a wskaźnikami

Nazwa zadeklarowaniej tablicy zawiera adres jej pierwszego elementu.
Jak już wiemy wskaźniki jako wartość przechowują adres pamięci na który wskazują.
Dletego używając zmiennej tablicowej możemy zainicjować wskaźnik adresem
pierwszego elementu tablicy.

```c++
int array[] { 1,2,3 };
cout << array; //0x61ffff
cout << *array; //1
cout << array[1]; //2
cout << array[2]; //3

int *array_ptr { array };
cout << array_ptr; //0x61ffff
cout << *array_ptr; //1
cout << array_ptr[1]; //2
cout << array_ptr[2]; //3
```

Czyli tablica i wskaźnik to to samo? Niezupełnie, poprostu w `c++` tablica
to zapisane obok siębie w pamięci wartości jadna po drugiej. Dlatego wskaźnik
może odnieść się do kolejnych elementów w tablicy w taki sam sposób jak normalna 
zmienna.

Jak to udowodnić? Mając wskaźnik z adresem pierwszego elementu tablicy, możemy poprzez
proste operacje artmetyczne na adresach przechodzić pomiędzy elementami tablic 
(ponieważ leżą obo siebie w pamięci)

```c++
int array[] { 1,2,3 };
int *array_ptr { array };

cout << array_ptr; //0x61fff1
cout << *array_ptr; //1
cout << (array_ptr + 1); //0x61fff5
cout << *(array_ptr + 1); //2
cout << (array_ptr + 2); //0x61fff9
cout << *(array_ptr + 2); //3
```
 
Dodawajc 1 do adresu pierwszego elementu dodajemy do adresu rozmiar przechowywanego
typu. W powyszym przypadku jest to int także jak spojrzymy na otrzymywane
po koleji adresy, zwiększają się one o 4. Mając adres elementu możemy pobrać
jego wartość poprzez `*` tak jak to robimy z pierwszym elementem tablicy 
lub innymi zmiennymi na które wskazuje wskaźnik.

Podsumowująć.
Zarówno do tablic jak i wskźników do tablic aby dostać się do poszczególnych
elementów możemy używać dwóch notacji.

```c++
int array[] { 1,2,3 };
int *array_ptr { array };

*(array_ptr + 1) //2
array_ptr[1]; //2

*(array + 1) //2
array[1]; //2
```

Po co to wszystko?
Akapit ten ma za zadanie uświadomić czym tak napawdę jest wskażnik. Przechowuje
on konkretny adres pamięci w którym przechowujem konkretny typ danych.
Tablica to typ danych który przechowuje swoje elementy w pamięci jeden za drugim,
dzięki czemu poprzez proste operacje na adresie możemy poruszać sie pomiędzy tymi 
elementami.

---

<a name="artmetyka"></a>
### Operacje na wskaźnikach

Wskaźniki w `c++` pozwalają na kilka rodzaji operacji:
* artmetyczne
* przypisania
* porównania

Operacje artmetyczne na wskaźnikach mają sens tylko w przypadku tablic.

```c++
array_ptr++; //następny element tablicy
array_ptr--; //poprzedni element tablicy

//n * sizeof(array type)
array_ptr += n; //przejście do 'n' elementu od którego się aktualnie znajdujemy
array_ptr -= n; //przejście do 'n' elementu w tył od którego się aktualnie znajdujemy

//Np. Wskaźnik int_ptr wskazuje na pierwszy element tablicy
//a int_ptr2 wskazuje na 10 element tablicy 
//wynikiem bedzie 9
int x = int_ptr - int_ptr2; zwraca ilość elementów między wskaźnikami

//Porównując dwa wskaźniki sprawdzamy czy wskazują na to samo miejsce w pamięci (nir wartość)

string str1 { "Jan" };
string str2 { "Jan" };

string *str_ptr1 { &str1 };
string *str_ptr2 { &str2 };
string *str_ptr3 { &str1 };

cout << (str_ptr1 == str_ptr2); //false
cout << (str_ptr1 == str_ptr3); //true

cout << (*str_ptr1 == *str_ptr2); //true
cout << (*str_ptr1 == *str_ptr3); //true
```

---

<a name="stale"></a>
### Wskaźniki i stałe

W `c++` mamy kilka rodzaji połączeń stałych i wskaźników:
* wskaźnik jako stała zmienna
    ```c++
    int num { 5 };
    int num2 { 10 };

    const int *ptr { &num };
    *ptr = 20; //ERROR
    ptr = &num2; //OK
    ```
    Jeżeli zadeklarujemy wskaźnik jako stałą zmienną (`const` przed typem)
    nie możemy zmieniać wartości w pamięci na którą wskazuje wskaźnik. 
    Możemy natomiast zmienić przypisany do wskaźnika adres.
* stały wskaźnik
    ```c++
    int num { 5 };
    int num2 { 10 };

    int *const ptr { &num };
    *ptr = 20; //OK
    ptr = &num2; //ERROR
    ```
    Jeżeli wskaźnik oznaczymy jako stała (`const` po `*`) wtedy nie możemy
    zmienić już raz przypisanej wartości do wskaźnika (adresu). Możemy natomiast
    dowolnie zmieniać wartość w pamięci na którą wskazuje.
* stały wskaźnik i zmienna
    ```c++
    int num { 5 };
    int num2 { 10 };

    const int *const ptr { &num };
    *ptr = 20; //ERROR
    ptr = &num2; //ERROR
    ```
    Jeżeli zarówno zmienna jaki i wskaźnik jest oznaczony jako stała, nie możemy
    zmienić adresu przypisanego do wskaźnika ani wartości na którą wskazuje.

---

<a name="funkcja"></a>
### Wskaźnik jako parametr funkcji

Wskaźnik jak każda zmienna może być użyty jako parametr funkcji. Składniowo
wygląda to analogiczne jak definicja normalnej funkcji.

```c++
void test(int *ptr)
{
    ...
}
```

Na przykładzie powyżej mamy funkcje test która jako parametr przyjmuje
wskaźnik na element o typie int.

Parametr wskaźnika w funkcji jest kopią przekazanego parametru. Jednak jak
wiemy wskaźnik przechowuje adres dlatego mimo tego że mamy doczynienia z kopią
przekazanego wskaźnika operujemy w funkcji na tej samym miejscu w pamięci.

```c++
void test(int *ptr)
{
    *ptr += 5; //zmieniamy wartość w pamięci o 5
}

int main()
{
    int num { 5 };

    cout << num; //5

    test(&num); //wskaźnik przechowuje adres dlatego jako parametr
                //możemy przekazać bezpośrednio adres naszej zmiennej

    //Jako że przekazaliśmy adres naszej zmiennej wskaźnik wewnątrz
    //funkcji operuje bezpośrednio na adresie naszej zmiennej
    cout << num; //10
    
    int *int_ptr { &num };
    cout << *int_ptr; //10

    test(int_ptr); //Wywołujemy funkcje test przekazując nasz wskaźnik
                   //w funkcji test parametrem jest jego kopia dlatego
                   //działamy na tym samym adresie

    cout << *int_ptr; //15
    cout << num; //15
}

Jak widać niezależnie od tego czy przekazujemy do funkcji bezpośrednio
adres naszej zmiennej czy wskaźnik na nią wskazujący, w funkcji jest
tworzony nowy wskaźnik wskazujący na przekazany adres.
```

---

<a name="zwracanie"></a>
### Zwracanie wskaźnika z funkcji

Funkcje mogą również zwracać wskaźniki, typ zwracany oznaczamy jako wskaźnik
dodając `*` po typie jaki funkcja zwraca.

```c++
type *function_name(type params)
{
    ...
}
```

Kiedy powinniśmy zwracać wskaźnik z funkcji:
* kiedy wewnątrz funkcji dynamicznie alokowaliśmy pamięć dla wskaźnika
    ```c++
    int *create_array(size_t size, int init_value = 0)
    {
        int *new_array_ptr { nullptr };
        new_array_ptr = new int[size];
        for (int i = 0; i < size; i++)
        {
            *(new_array_ptr + i) = init_value;
        }

        return new_array_ptr;
    }

    int main()
    {
        int *array_ptr { nullptr };
        array_ptr = create_array(20,3);

        for (auto val : array_ptr)
        {
            cout << val;
        }
        //Wypisze 3, 20 razy
    }
    ```
    Wewnątrz funkcji tworzymy tablice przy pomocy wskaźnika. Dynamicznie 
    alokujemy rozmiar takiej tablicy także jeżeli byśmy jej nie usuneli (`delete []`)
    lub zwrócili stracilibyśmy dostęp do tej pamięci i była by ona zarezerwowane
    aź do zakończenia programu.
* dla przekazanych parametrów 
    ```c++
    int *largest_int(int *a, int *b)
    {
        if (*a > *b)
        {
            return a;
        }
        return b;
    }

    int main()
    {
        int x { 5 };
        int y { 10 };
        int *largest_ptr { nullptr };

        largest_ptr = largest_int(&a, &b);        
        count << *largest_ptr; //10
    }
    ```
    Jak widać na przykładzie powyżej w funkcji `largest_int` sparwdzamy
    która wartość a czy b jest większa, aby dowiedzieć się jaki jest wynik
    musimy zwrócić wskaźnik wskazujący na większy element. W ten sposób w funkcji
    `main` wskaźnik `largest_ptr` wskazuje na miejsce w pamięci gdzie przechowywana
    jest wartość zmiennej `y`.

Ważne jest aby nigdy nie zwracać wskaźnika do zmiennej zadeklarowanej w funkcji.

```c++
int *example1()
{
    int size { 4 };
    ...
    return &size;
}

int *example2()
{
    int size { 4 };
    int *int_ptr { &size };
    ...
    return int_ptr;    
}
```

W obu przypadkach zmienna `size` zainicjowana w wewnątrz funkcji przestaje
istnieć po jej zakończeniu. W miejscu na jakie wskazuję wskaźnik będą śmieciowe wartości.

---

<a name="bledy"></a>
### Problemy i błędy

Zbiór najczęstrzych pomyłek związanych z wskaźnikami
* Niezaincjowany wskaźnik - Niezainicjalizowany wskaźnik może wskazywać
na miejsce w pamięci krytyczne dla działania aplikacji. Dane 
przechowywane w takim miejscu mogą ulec zmianie bez naszej wiedzy.
* Wskaźnik na element który został usunięty - np. w przypadku kiedy dwa
wskaźniki wskazują na to samo miejsce w pamięci i jeden z nich zwolni tą
pamięć (`delete`). W takim przypadku to miejsce w pamięci może zostać wykorzystane
przez inne operacje przez co wartość w nim przechowywana się zmieni. Taki
sam przypadek może wystąpić przy inicjowaniu wskaźnika przez lokalną zmienną
w funkcji.
* Podczas dynamicznej alokacji pamięci należy sprawdzać czy przydzielenie
powiodło się. (przechwytywanie wyjątków (później))
* Najczęstrzym problemem jest zapominanie o zwolnieniu pamięci kiedy nie 
używamy kursora (`delete`) doprowadza to do przepełnienia pamięci co skutkuje 
zawieszeniem programu.
                        
---

<a name="referencja"></a>
### Co to jest referencja?

Referencji najbliżej można porównać do wskaźników. Z tą róźnicą że do referencji
adres możemy przypisać tylko raz. Po przypisaniu adresu dalsze jej używanie 
nie różni się od zwykłej zmiennej, wszystkie zmieny na wartości referencji
dokonują się również na zmiennej.

Referencje deklarujemy przy pomocy operatora `&`.
Przykłady referencji

```c++
//Inicjalizacja i deklaracja referencji
int i = 0;
int &ref_i = i;
cout << i;      // wypisuje 0
ref_i = 1;
cout << i;      // wypisuje 1
cout << ref_i;  // wypisuje 1
```

```c++
//Pętla for-range z referencją i bez
vector<string> names { "Piotr", "Zofia", "Janek" };

for (string name : names)
{
    name = "Jas";
}

for (string name : names)
{
    cout << name;
}
//Zwróci Piotr Zofia Janek ponieważ zmienna name nie jest referencją tylko
//kopią elementu tablicy

for (string &name : names)
{
    name = "Jas";
}

for (string name : names)
{
    cout << name;
}
//Zwroci Jas Jas Jas ponieważ operujemy na referencji
```

```c++
//Przykład z tematu o funkcjach
void test(int a)
{
    ...
}

void test(int &a)
{
    ...
}
//W pierwszym przypadku operujemy na kopi obiektu w drugim
//na jego referencji
```

---

<a name="podsumowanie"></a>
### Podsumowanie

Kiedy używać wskaźników, referencji lub zmiennych?

Zmienne:
* kiedy funkcja nie zmienia wartości parametrów
* kiedy parametr jest mały i 'łatwy' do skopiowania (int,char itd.)

Wskaźnik:
* kiedy funkcja zmienia wartości parametrów
* i kopiowanie parametru jest 'kosztowne'
* i kiedy parametr może być nullptr

Referencja:
* kiedy funkcja zmienia wartości parametrów
* i kopiowanie parametru jest 'kosztowne'
* i kiedy parametr nie może przyjmować wartości nullptr

---

<a name="zadania"></a>
### Zadania

Zadanie 1.

Zadeklaruj dwie tablice zawierające po kilka liczb całkowitych.
Dodaj funkcje `union_all` która przyjmuje jako parametry te dwie tablice.
W funkcji utworz nową tablice dynamicznie alokując dla niej pamięć i dodaj do niej
wszytskie elementy z tablic przekazanych jako parametry i zwróć ją z funkcji.
W funkcji main wypisz nową tablice w formacie:
[0] 2
[1] 12
[2] 9
.
.
[N-1] 523

Zadanie 2.

W funkcji `main` zadeklaruj dwie zmienne `int` i przypisz im wartości.
Napisz funkcje `swap_pointer` która jako parametry przyjmuje dwa wskaźniki na liczby całkowite.
Zamień wartościami te dwa parametry. 
Napisz funkcje `swap_ref` która jako parametr przyjmuje dwie referencje do tych liczb całkowitych
i zamień ich wartości. 
W funkcji `main` wypisz wartości zmiennych 
po zmianie i przed zmieną oraz ich adresy:
Przed
a = X
a: 0x61asdfg
b = Y
b: 0x61kdjfd
Po
a = Y
a: 0x61asdfg
b = X
b: 0x61kdjfd

Zadanie 3.

Zadeklaruj tablice zawierającą pierwsze 20 pierwszch liter alfabetu.
Napisz funkcje która przyjmuje wskaźnik na tą tablice i wyświetla co 4 literę alfabetu.
Do iterowania po tablicy i pobierania wartości użyj `*(tablica + n)`.
W main wypisz te litery.

Zadanie 4.

Zadeklaruj wektor przechowujący 20 liczb całkowitych (losowych z zakresu 0-100).
Stwórz funkcje sort która jako parametr przyjmie wskaźnik do tego wektora
i posortuje jego elementy od najmniejeszej do największej.
W funkcji main wypisz wektor nieposorotowany i posortowany.

Zadanie 5.

