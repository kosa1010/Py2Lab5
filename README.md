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

### Wykres `x` względem `y`
```python
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.show()
```

## Formatowanie stylu wykresu

Każda para argumentów `x, y` może mieć dodatkowy argument `fmt`, który określa kolor i styl linii.

```python
plt.plot([1, 2, 3, 4], [1, 4, 9, 16], 'r.')
plt.axis([0, 6, 0, 20])
plt.show()
```

Pełna lista stylów linii znajduje się [tutaj](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html).

### Wiele linii na jednym wykresie
```python
import numpy as np
import matplotlib.pyplot as plt

t = np.arange(0., 5., 0.2)
plt.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
plt.show()
```

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
```python
x = [1, 2, 3]
y = np.array([[1, 2], [3, 4], [5, 6]])
plt.plot(x, y)
plt.show()
```

## Praca z wieloma figurami i osiami

Matplotlib umożliwia rysowanie wykresów względem wielu osi.

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

## Zadania do wykonania

1. Uruchom kod z listingów i sprawdź ich działanie.
2. Utwórz wykres słupkowy przedstawiający temperatury w tygodniu.
3. Narysuj dwa wykresy kursów walut na jednym wykresie.
4. Narysuj wykres funkcji $y = x^2$ dla $x \in [-5,5]$.
5. Narysuj wykres `sin(x)` i `cos(x)` dla $x \in [0, 2\pi]$.
6. Narysuj wykres 3D powierzchni $z = x^2 + y^2$.
7. Narysuj wykres słupkowy liczby urodzeń wg płci.
8. Narysuj wykres punktowy zależności masy ciała od wzrostu.
9. Narysuj wykres kołowy rozkładu różnych owoców w koszu.
10. Narysuj histogram wyników testu studentów.
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

**Zadania 2-12 mają zostać dodane na platformę Moodle**.
