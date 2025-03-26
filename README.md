# Biblioteka Matplotlib

## Wprowadzenie do `pyplot`

`matplotlib.pyplot` jest zbiorem funkcji, które sprawiają, że matplotlib działa jak MATLAB. Każda funkcja `pyplot` dokonuje jakiejś zmiany w figurze: np. tworzy figurę, tworzy obszar wykreślania, wykreśla linie, dekoruje wykres etykietami itp.

W `matplotlib.pyplot` różne stany są zachowywane przez wywołania funkcji, dlatego funkcje kreślenia są kierowane do bieżących osi.

### Przykładowy wykres
```python
import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4])
plt.ylabel('Etykieta osi Y')
plt.xlabel('Etykieta osi X')
plt.show()
```

### Dlaczego oś x ma zakres od 0-3, a oś y od 1-4?
Jeśli podasz pojedynczą listę lub tablicę do wykreślenia, `matplotlib` zakłada, że jest to sekwencja wartości `y` i automatycznie generuje dla Ciebie wartości `x`. Ponieważ zakresy Pythona zaczynają się od 0, domyślny wektor x ma taką samą długość jak `y`, ale zaczyna się od 0. Stąd wartości `x` to `[0, 1, 2, 3]`.
Plot jest uniwersalną funkcją i przyjmuje dowolną liczbę argumentów. Na przykład, aby wykreślić `x` względem `y`, można napisać:
```python
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.show()
```

## Formatowanie stylu wykresu

Każda para argumentów `x, y` może mieć dodatkowy argument `fmt`, który określa kolor i styl linii. Litery i symbole łańcucha formatu pochodzą z MATLABa, i łączy się łańcuch koloru z łańcuchem stylu linii. Domyślnym łańcuchem formatu jest `b-`, który jest niebieską linią ciągłą. 
Na przykład, aby wykreślić wcześniejszy wykres używając czerwonych punktów, należy wydać polecenie:

```python
plt.plot([1, 2, 3, 4], [1, 4, 9, 16], 'r.')
plt.axis([0, 6, 0, 20])
plt.show()
```

Pełna lista stylów linii znajduje się [tutaj](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html).

Funkcja `axis` w powyższym przykładzie przyjmuje listę `[xmin, xmax, ymin, ymax]` i określa zakresy wartości przedstawiane na osiach.
Gdyby matplotlib był ograniczony do pracy z listami, byłby dość bezużyteczny do przetwarzania liczb. W rzeczywistości wszystkie sekwencje są wewnętrznie konwertowane na tablice `numpy`. Poniższy przykład ilustruje wykreślanie kilku linii o różnych stylach formatowania w jednym wywołaniu funkcji przy użyciu tablic.

### Wiele linii na jednym wykresie
```python
import numpy as np
import matplotlib.pyplot as plt

t = np.arange(0., 5., 0.2)
plt.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
plt.show()
```
![image](https://github.com/user-attachments/assets/0fe97029-799e-4f20-b00c-1d4e3a7f411c)


## Wiele wykresów w jednym obszarze

Możemy rysować wiele wykresów, używając kilku metod:

### Wielokrotne użycie `plot`
```python
x = [1, 2, 3]
y1 = [2, 4, 6]
plt.plot(x, y1, 'bo')
plt.plot(x, [value * 3 for value in y1], 'go')
plt.show()
```

### Wykresy dla tablicy 2D
`x` i/lub `y` są tablicami 2D, wtedy dla każdej kolumny zostanie narysowany osobny zestaw danych
```python
x = [1, 2, 3]
y = np.array([[1, 2], [3, 4], [5, 6]])
plt.plot(x, y)
plt.show()
```
```python
x = [1, 2, 3]
y = np.array([[1, 2], [3, 4], [5, 6]])
for col in range(y.shape[1]):
plt.plot(x, y[:, col])
plt.show()
```

### określenie wielu zestawów składających się z współrzędnych `[x]`, `y` oraz z stringa definiującego kolor i styl linii `[fmt]`:
```python
y1 = [2, 4, 6]
plt.plot(x, y1, 'bo', x, [value * 3 for value in y1], 'go')
plt.show()
```
Dodatkowo dla serii danych można parametry tj. szerokość linii, etykietę legendy i inne
```python
plt.plot([1, 2, 3], [1, 2, 3], 'go-', label='line 1', linewidth=4)
plt.plot([1, 2, 3], [1, 4, 9], 'md:', label='line 2', markersize=15)
plt.legend()
plt.show()
```
Więcej na temat legend znajduje się [tutaj](https://matplotlib.org/stable/users/explain/axes/legend_guide.html).

## Praca z wieloma figurami i osiami

Matplotlib umożliwia rysowanie wykresów względem wielu osi. Wszystkie funkcje kreślenia odnoszą się do bieżących osi. Poniżej znajduje się skrypt tworzący dwa podwykresy.

```python
def f(t):
    return np.exp(t) * np.cos(2*np.pi*t)

t1 = np.arange(0.0, 5.0, 0.15)
t2 = np.arange(0.0, 5.0, 0.01)

plt.figure()
plt.subplot(211)
plt.plot(t1, f(t1), 'bo', t2, f(t2), 'k')

plt.subplot(212)
plt.plot(t2, np.cos(2*np.pi*t2), 'r--')
plt.show()
```
![image](https://github.com/user-attachments/assets/9ad2df1c-98d0-41ef-a211-b872e0d1e7d4)

Wywołanie funkcji `figure` jest opcjonalne, ponieważ figura zostanie utworzona, jeśli nie istnieje, tak samo jak oś zostanie utworzona (odpowiednik jawnego wywołania `subplot()`), jeśli nie istnieje. Wywołanie `subplot` określa `numrows`, `numcols`, `plot_number`, gdzie `plot_number` mieści się w zakresie od `1` do `numrows*numcols`. Przecinki w wywołaniu `subplot` są opcjonalne, jeśli `numrows*numcols<10`. Zatem `subplot(211)` da taki sam rezultat jak `subplot(2, 1, 1)`.
Możesz stworzyć dowolną liczbę podwykresów i osi. Jeśli chcesz umieścić oś ręcznie, tj. nie na siatce prostokątnej, użyj `axes`, który pozwala określić położenie jako `axes([left, bottom, width, height])`, gdzie wszystkie wartości są we współrzędnych ułamkowych (0 do 1).
Przykład ręcznego umieszczania osi [tutaj](https://matplotlib.org/stable/_downloads/fbec90da3a9f58258ab121e0d2037693/axes_demo.py)
Przykładu z dużą ilością podwykresów [tutaj](https://matplotlib.org/stable/gallery/subplots_axes_and_figures/subplots_demo.html)

## Inne typy wykresów

### Wykres kołowy
```python
labels = ['Frogs', 'Hogs', 'Dogs', 'Logs']
sizes = [15, 30, 45, 10]
explode = (0, 0.1, 0, 0)

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%', shadow=True, startangle=90)
ax1.axis('equal')
plt.show()
```
Inne warianty wykresów kołowych dostępne pod adresem [https://matplotlib.org/stable/gallery/pie_and_polar_charts/index.html](https://matplotlib.org/stable/gallery/pie_and_polar_charts/index.html)

### Wykres słupkowy
```python
fruits = ['apple', 'blueberry', 'cherry', 'orange']
counts = [40, 100, 30, 55]
bar_labels = ['red', 'blue', '_red', 'orange']
bar_colors = ['tab:red', 'tab:blue', 'tab:red', 'tab:orange']

fig, ax = plt.subplots()
ax.bar(fruits, counts, label=bar_labels, color=bar_colors)
ax.set_ylabel('Fruit supply')
ax.set_title('Fruit supply by kind and color')
ax.legend(title='Fruit color')
plt.show()
```
Inne warianty wykresów słupkowych i liniowych: [https://matplotlib.org/stable/gallery/lines_bars_and_markers/index.html](https://matplotlib.org/stable/gallery/lines_bars_and_markers/index.html)

### Wykres 3D
```python
import matplotlib.pyplot as plt
import numpy as np
ax = plt.axes(projection="3d")
u = np.linspace(0, 2 * np.pi, 100)
v = np.linspace(0, np.pi, 100)
r = 10
x = r * np.outer(np.cos(u), np.sin(v))
y = r * np.outer(np.sin(u), np.sin(v))
z = r * np.outer(np.ones(np.size(u)), np.cos(v))
ax.plot_surface(x, y, z, rstride=5, cstride=5, cmap=plt.cm.coolwarm)
plt.show()
```
Inne warianty wykresów 3D: [https://matplotlib.org/stable/gallery/lines_bars_and_markers/index.html](https://matplotlib.org/stable/gallery/lines_bars_and_markers/index.html)
## Zadania do wykonania

1. Uruchom kod z listingów i sprawdź ich działanie.
2. Utwórz wykres słupkowy przedstawiający temperatury w tygodniu.
3. Narysuj dwa wykresy kursów walut na jednym wykresie. Dodaj legendę. Do pobrania wartości kuru z przedziału dat skorzystaj np. z [API](https://exchangerate.host/#/)
   > Przykład pobiera słownik zawierający kurs EURO w ostatnich dwóch tygodniach
```python
import requests
import datatime

date_to = datetime.date.today()
date_from = date_to - datetime.timedelta(days=14)
base = "EUR"
url = 'https://api.exchangerate.host/timeseries?start_date=' + str(date_from) +
'&end_date=' + str(
date_to) + '&base=' + base
response = requests.get(url)

data = response.json()
print(data["base"])
for key, value in data.items():
    if key == "rates":
        for k, v in value.items():
            print(k, v["PLN"])
```
4. Narysuj wykres funkcji $y = x^2$ dla $x \in [-5,5]$.
5. Narysuj wykres `sin(x)` i `cos(x)` dla $x \in [0, 2\pi]$.
6. Narysuj wykres 3D powierzchni $z = x^2 + y^2$.
7. Narysuj wykres słupkowy liczby urodzeń wg płci.
    > Przyjmij, że dane na temat urodzeń to lista krotek np.
        `dane = [(100, 90), (110, 95), (120, 105), (130, 110)]`
    > Dla podanej listy wykres może wyglądać następująco:
    
    ![image](https://github.com/user-attachments/assets/47bc7614-565e-4d76-a60b-0291d440ab05)

8. Narysuj wykres punktowy zależności masy ciała od wzrostu dla grupy osób.
9. Narysuj wykres kołowy rozkładu różnych owoców w koszu.
    > Przyjmij, że dane na temat owoców to lista krotek np.
    `data = [('jabłka', 30), ('gruszki', 20), ('śliwki', 15), ('banany', 25), ('cytryny', 10)]`
10. Narysuj histogram wyników testu studentów.
    > Przyjmij, że dane odnośnie do wyników to lista przechowująca procent uzyskanych punktów przez studentów np.
`dane = [60, 70, 80, 90, 100, 70, 80, 80, 85, 95]`
> Dla podanej listy histogram będzie wyglądał następująco:
![image](https://github.com/user-attachments/assets/ec53b253-e0d8-4714-ae7c-381a610b6d60)

11. Narysuj funkcję `sine(x)` i jej odbicie lustrzane, aby utworzyć serce.

```python
def sine(x):
    return np.power(x, (2.0 / 3)) + 0.9 * (3.3 - x ** 2) ** (1 / 2) * np.sin(10 * np.pi * x)
```

12. Rozbuduj kod pobierający temperatury europejskich miast o dodatkowe miasto.

```python
coordinates.append((52.2298, 21.0122))  # Warszawa
towns.append('Warszawa')
```

**Zadania 2-12 mają zostać dodane na repozytorium**.
