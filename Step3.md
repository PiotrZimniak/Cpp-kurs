
# STEP 3 - Arrays and Vectors
1. [Arrays](#arrays) 
2. [Multidimensionl arrays](#multidimension)
2. [Vectors](#vectors)
3. [Multidimensionl vectors](#multidimensionV)
5. [Zadanie](#zadanie)

---

<a name="arrays"></a>
### Arrays

Tablice w `c++` służą do przechowywania danych o tym samym typie powiązanych z sobą jakąś logiką. 
Deklarując tablice zmiennych musimy podać jej rozmiar i jest on niezmienialny. Dobrym zwyczajem jest
inicjalizowanie tablicy w momencie deklaracji, tak aby dane zawerta w poszczególnych miejscach w tablicy
nie były losowe.

```c++
int tab[5] { 2,10,12,1,4 }; //deklarujemy tablice piecioelementowa i przypisujemy wartości poszczególnym komórką
int tab2[10] {2,10}; //przypisujemy pierwszym dwóm miejscą 2 i 10 następne wypełniamy 0

constint days {365};
int tab3[days] {0}; //deklarujemy tablice 365 elementowa i wypelniamy 0

int tab4[] {1,2,3,4,5} //rozmiar tablicy automatycznie ustawia sie na ilosc elementow (5)
```

Indeksy w poszczególnych komórek w tablicy zaczynają się od `0` i kończą na indeksie `n-1` gdzie `n` 
to rozmiar tablicy.

```c++
int tab[5] { 2,10,12,1,4 };

tab[1] = 1; //przypisujemy wartość do komórki tablicy
cout << tab[0]; //Wypisze 2
cout << tab[1]; //Wypisze 1
```

---

<a name="multidimension"></a>
### Multidimensionl arrays

Tablice mogą posiadać kilka wymiarów. Podczas deklaracji każdy wymiar musi mieć określony rozmiar.
Wielowymiarowe tablice deklarujemy w analogiczny sposób ja jednowymiarowe i analogicznie odwołujemy się
do wartości w nich zawartch.
```c++
int tab[2][2] {
    {1,2},
    {3,4}
}; // deklaracja tablicy dwuwymiarowej z inicjalizacją

tab[0][0] = 5;
cout << tab[0][0] //Wypisze 5
cout << tab[1][0] //Wypisze 3
```

---

<a name="vectors"></a>
### Vectors

Wektor to dynamiczny rodzaj tablicy. Należy do standardu `c++`. Przy deklaracji wektoru nie musi podawać
jego rozmiaru, możemy go dowolnie zmieniać w czasie trwania programu.

Wektor znajduje się w bibliotece `<vector>` w namespace `std`, także aby używać wektorów na początku
naszej klasy musimy zaimportować ten typ.

Deklaracja typu `vector`:

```c++
#include <vector>;

int main()
{
    std::vector<int> int_vec; //deklaracja wektora przechowującego wartości typu int
    std::vector<double> dou_vec { 10.0, 1.3 }; //deklaracja wektora dla wartosci double oraz wstawienie do niego wartości 10.0 i 1.3
    std::vector<char> char_vec (10,'e'); //deklaracja wektora char oraz wstawienie do niego 10 wartości 'e'  
}
```

Tak jak w tablicy każdy element w wektorze ma swój indeks i tak samo indeksy są liczone od 0.
Aby odwołać się do konkretnego elementu w wektorze używamy funkcji `at(index)`.

```c+++
vector<double> dou_vec { 10.0, 1.3 };
cout << dou_vec.at(1); //Wypisze 1.3 
```

Aby dodać nowy element do wektora na jego koniec używamy funkcji `push_back(element)`

```c+++
vector<double> dou_vec { 10.0, 1.3 };
dou_vec.push_back(2.0);
cout << dou_vec.at(2); //Wypisze 2.0 
```

Aby sprawdzić rozmiar naszego wektora używamy funkcji `size()`
```c+++
vector<double> dou_vec { 10.0, 1.3 };
dou_vec.push_back(2.0);
dou_vec.push_back(3.0);
dou_vec.push_back(4.1);
cout << dou_vec.size(); //Wypisze 5 
```

---

<a name="multidimensionV"></a>
### Multidimensionl vectors

Tak jak tablice wektory mogą posiadać kilka wymiarów. Deklaruje się je na zasadzie wektora w wektorze.

```c++
vector<vector<int>> vec {  
    {1,2},
    {2,3}
}; //Deklaracja wektora dwuwymiarowego z przypianymi wartościami
```

Do konkretnego miejsca w takim wektore odwołujemy się tak samo jak do elementów tablicy wielowymiarowej.
Gdzie pierwsza wartość oznacza z którego wektora wartość zczytujemy a druga którą wartość w nim chcemy zczytać.
To zamo tyczy sie przypisania.

```c++
vector<vector<int>> vec {  
    {1,2},
    {2,3}
}; //Deklaracja wektora dwuwymiarowego z przypianymi wartościami

vector[0][1] = 5;
cout << vector[0][1]; //wypisze 5
cout << vector[1][1]; //wypisze 3
```

Do takiego dwu wymiarowego wektora możemy dodawać kolejne wektory (wiersze).

```c++
vector<vector<int>> vec {  
    {1,2},
    {2,3}
};

vector<int> new_vec { 4, 5 };
vec.push_back(new_vec);
cout << vec.size(); //Wypisze 3
```

Aby dodawać wartości do poszczególnych wektorów wewnątrz, najpierw poprzez `[]` mówimy do jakiego wektora chcemy
sie odnieść, następnie metodą `push_back(element)` dodajemy wartość.

```c++
vector<vector<int>> vec {  
    {1,2},
    {2,3}
};

vec[0].push_back(5);
```

---

<a name="zadanie"></a>
### Zadanie

```c++
#include <iostream>

int main()
{
    /*
    Zadanie 1.
    1. Zadeklaruj wektor przechowująy wartości całkowite z przypisanymi dwoma warościami na start
    2. Dodaj do wektora kolejne dwie wartości wprowadzone przez użytkownika z konsoli
    3. Wypisz wszystkie wartości zawarte w wektorze w formacie
       [1]: X
       [2]: X
       [3]: X
       [4]: X
       Size: 4
    */

    /*
    Zadanie 2.
    1. Zadeklaruj wektor przechowujący wektor przechowujący wartości całkowite oraz przypisz mu dwa wektory z przypisanymi dwoma wartościami
    2. Do pierwszego wektora dodaj jedną a do drugiego dwie liczby wprowadzone przez użytkownika z konsoli
    3. Wypisz wszystkie wartości zawarte w wektorze w formacie
       [1][1]: X
       [1][2]: X
       Size: 2
       [2][1]: X
       [2][2]: X
       [2][3]: X
       Size: 3
    */
}
```