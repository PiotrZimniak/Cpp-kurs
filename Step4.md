
# STEP 4 - Control flow
1. [If](#if) 
2. [Switch](#switch)
3. [For](#for)
4. [Range-based for](#range)
5. [While](#while)
6. [Break/Continue](#break)
7. [Zadanie](#zadanie)

---

<a name="if"></a>
### If

`If` służy do porównania wartości. W `c++` jego składnia jest standardowa.

```c++
if (a == b)
{
    ...
}
else if (a == 5)
{
    ...
}
else
{
    ...
}
```

Warunek `if` posiada również skróconą jednolinijkową składnie. W przykładzie poniżej przypisujemy większą wartość z pary a, b 
do zmiennej a. Wszystko co znajduje się pomiędzy `=` a `?` to wyrażenie które sprawdzamy. Pierwsza wartość po `?` to wartość
która zostanie wybrana jeżeli wyrażenie jest prawdziwe, wartość po `:` to wartość która zostanie wybrana kiedy wyrażenie jest
nieprawdziwe.

```c++

a = c > b ? c : b; 

```

---

<a name="switch"></a>
### Switch

`Switch` to również wyrażenie warunkowe. To jaka część kodu zostanie wykonana zależy od dopasowanie zmiennej do zdefiniowanych
opcji.

```c++
int a;
cin << a;
switch (a)
{
    case 1:
        ....
        break;
    case 2:
        ....
        break;
    case 5:
        ....
        break;
    default:
        ....
}
```

W wyrażeniu `switch(..)` podajemy wartość z którą chcemy porównywać zdefiniowane przez nas `casy`. Jeżeli żadna wartość
nie zostanie dopasowana, wtedy zostanie wykonany kod domyślny (`default`).

`Switch` najczęściej wykonywany jest typami `enum`. `Enum` określa nam dostępny zestaw wartości jaki może przyjąć dlatego łatwo
i elegancko logike jest przedstawić przy pomocy `switcha`.

--- 

<a name="for"></a>
### For

Pętla `for` służy do wykonywania tego samego kodu określoną, znaną ilość razy. 
`for (inicjalizujemy zmienną określającą iteracje; warunek do kiedy chcemy iterować; operacja na iteratorze wykonywana po każym przejściu pętli)`
```c++
for (int i { 0 }; i < 5; i++)
{
    cout << i << endl;
} 
//Wypisze 0 1 2 3 4 
```

Operator `for` w `c++` umożliwia nam iterowanie po dwóch zmiennych naraz

```c++
for (int i { 0 }, j = { 5 }; i < 5;i++,j++)
{
    cout << i << " : " << j << endl;
}
/*Wypisze
0 : 5
1 : 6
2 : 7
3 : 8
4 : 9
*/
```

---

<a name="range"></a>
### Range-based for

Range based for to operator który umożliwia nam w łatwy sposób iterować po kolekcjach (tablicach, wektorach).
Wartość przed `:` to zmienna do której po każdej iteracji będą przypisywane elementy kolekcji podanej po `:`.

```c++
vector<int> vec {1,2,4}
for (int val : vec)
{
    cout << val << " "; //Wypisze 1 2 4
}
```

---

<a name="while"></a>
### While

Pętla `while` pozwala nam wykonowyć w pętli określone operacja dopóki podany warunek jest spełniony.

```c++
int a { 5 };

while (a > 0)
{
    cout << a << " "; 
    a--; //a = a - 1;
}
//Wypisze 5 4 3 2 1
``` 

W powyższym przypadku kod w pętli `while` nigdy się nie wykona jeżeli zmienna `a` na starcie jest <= 0.
Jeżeli chcemy mieć pewność że nasz kod w pętli while wykona się przynajmniej raz, używamy odminy pętli `while`, 
`do { } while`. W pętli `do while` warunek jest sprawdzany na końcu pętli.

```c++
int a { 0 };

do
{
    cout << a << " "; 
    a--;
} while (a > 0);

//Wypisze 0
```

<a name="break"></a>
### Break/Continue

Słowo kluczowe `break` służy do przerwania wykonywania pętli. Jeżeli program w czasie wykonywania natrafi na `break` przerywa
wykonywanie obecnej pętli/funkcji i przechodzi do wykonania dalszej części programu poza nią.

```c++
int a {0};

while (a < 5)
{   
    a++;
    if (a == 3)
    {
        break; 
        //W iteracji kiedy warunek a == 3 zostanie spełniony program trafi na break i przerwie wykonywanie 
        //pętli bez sprawdzania warunku w while
    }
    cout << a << " ";
}
//Wypisze 0 1 2
```

Słowo kluczowe `continue` służy do przerwania wykonywania opecnej iteracji w pętli i przenosi program do warunku sprawdzającego.

```c++
int a {0};

while (a < 5)
{   
    a++;
    if (a == 3)
    {
        continue; 
        //W iteracji kiedy warunek a == 3 zostanie spełniony program trafi na continue i zostanie 
        //przeniesiony na jej początek do sprawdzenia warunku a < 5. Warunek nadal jest spełniony
        //dlatego program wejdzie do kolejnej operacjki z a == 4
    }
    cout << a << " ";
}
//Wypisze 0 1 2 4
```

---

<a name="zadanie"></a>
### Zadanie


```c++
#include <iostream>

int main()
{
    /*
     Zadanie
        1. Zadeklaruj wektor przechowująy wartości całkowite z przypisanymi dwoma warościami na start
        2. Poproś użytkownika o wpisanie ile wartości chce dodać do naszego wektora
        3. Przy pomocy dowolnej petli popros uzytkownika o podanie wartosci do wpisania do tablicy (takiej ilosci jak podal w kroku 2)
        4. Wypisz pierwszych 5 wartości zawarte w wektorze oprócz 3 elementu w formacie
           [1]: X
           [2]: X
           [4]: X
           [5]: X
           Size: N
           Petla: XYZ
        4a. Przy pomocy pętli for
        4b. - || - range for
        4c. - || - while
     */
}
```

