# Iris – Verwaltung von Events

## Event

```__init__```: Konstruktor, der eine beliebige Anzahl an Funktionspointern nimmt, die ans Event gebunden werden.


```add_function```: Bindet neue Funktionen an das Event.


```remove_function```: Entbindet Funktionen vom Event.


```emit```: Löst das Event aus und ruft alle Funktionen nacheinaner auf mit den dieser Funktion gegebenen Argumenten auf.


```__iadd__```: Selbe Funktion wie ```add_function```


```__isub__```: Selbe Funktion wie ```remove_function```


```__call__```: Selbe Funktion wie ```emit```


```__repr_```: Representationsstring des Events mit den gebundenen Funktionen.


```function```: Property-Attribut, das die Liste der gebundenen Funktionen liefert zur einfachen Manipulation.


## TypeEvent

```__init__```: Konstruktor, der die Typen nimmt, die akzeptiert werden können.

```emit```: Löst das Event aus. Wirft TypeError, falls falsche Typen vorliegen oder AttributeError.

```typen```: Property-Attribut, das die akzeptablen Typen liefert.

```__repr__```: Representationsstring des TypeEvents mit den Typen und den gebundenen Funktionen.

```__copy__```: Liefert eine Kopie des Objekts allerdings mit leerer Funktionspointerliste.


## Handler

```__init__```: Konstruktor, der den Handler initialisiert.


```get_event_names```: Liefert die Namen aller erstellten Events.


```remove```: Löscht das Event mit dem gegebenem Namen.


```__repr__```: Representationsstring mit den Namen der Events


Möglichkeiten ein neues Event zu erstellen mit:
- Dem Namen des Events
- Den zu bindenen Funktionen

* ```new```
* ```__setitem__```

Möglichkeiten ein Event zu löschen:

* ```remove```
* ```__delitem__```
* ```__delattr__```

Möglichkeiten ein Event von seinem Namen her zu erhalten:

* ```__getitem__```
* ```__getattr__```


## StaticHandler

Basisklasse, von der verschiedene Handler abgeleitet werden können, deren Attribute als Events aufgefasst werden.

### Syntax mit Typvorgabe:

```python
class Irgendwas(StaticHandler):
    EVENT_1: Event
    EVENT_2: Event
```

### Syntax mit None-Wert

```python
class Irgendwas(StaticHandler):
    EVENT_1 = None
    EVENT_2 = None
```

### Syntax mit Typ und None

```python
class Irgendwas(StaticHandler):
    EVENT_1: Event = None
    EVENT_2: EVENT = None
```

Statt ```None``` kann auch ```Event()``` verwendet werden. 
Das macht allerdings keinen Unterschied.


### Syntax mit TypeEvent

```python
class IgendwasMitTypen(StaticHandler):
    T_EVENT_1 = TypeEvent(int, str, int)
    T_EVENT_2 = TypeEvent(str, int)
```


### Auslösen eines Events

```python
handler = Irgendwas()
handler.EVENT_1.add_function(print)
handler.EVENT_1.emit("Test")
```

## RecursiveHandler

Ein Dekorator, um eine Klasse dem Aufnehmen eines weiteren Handlers zu befähigen.

### Exceptions

Ein Auftreten der Exception ```AttributeError``` kann bedeuten, dass der Dekorator vergessen worden ist. 

### Syntax

```python
@RecursiveHandler
class A(StaticHandler):
    a1: Event

    @RecursiveHandler
    class B(StaticHandler):
        b1: Event
        
        class C(StaticHandler):
            c1: Event
```

### Event erreichen

```python
a = A()
print(a.a1)
print(a.B.b1)
print(a.B.C.c1)
```

Die definierten Events können so angesprochen werden.