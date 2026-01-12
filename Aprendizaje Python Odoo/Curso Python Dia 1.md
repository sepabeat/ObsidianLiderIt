
  

## ✅ Objetivos  

- Comprender tipos básicos: `str`, `int`, `float`, `bool`.  

- Manejar colecciones: `list`, `tuple`, `dict`, `set`.  

- Control de flujo: `if`, `for`, `while`.  

- Funciones y parámetros.  

- Buenas prácticas (PEP8, formateo con Black).  



### Tipos básicos  

```python  

nombre = "Salvador" # str  

edad = 35 # int  

precio = 19.99 # float  

activo = True # bool 
#Para poner un comentario solo es necesaria la almohadilla 
```


### Listas y diccionarios

```Python

frutas = ["manzana", "pera", "uva"]  

persona = {"nombre": "Salvador", "edad": 35}  

```

### Control de flujo

```Python

if edad >= 18:  

print("Mayor de edad")  

else:  

print("Menor de edad")  

```

### Bucles

```Python

for fruta in frutas:  

print(fruta)  

  

contador = 0  

while contador < 3:  

print(contador)  

contador += 1  

```

### Funciones

```Python

def saludar(nombre: str) -> str:  

return f"Hola, {nombre}"  

```

---

## 2. Ejercicios prácticos

📂 **Crea carpeta `src/` para tus soluciones.**
Para comprobar si funcionan correctamente, navegar desde la terminal de Visual Studio a la carpeta exacta en la que se encuentran los archivos .py a ejecutar. A continuación usar el comando "python nombrearchivo.py"
```shell
python ej1.py
```
### Ejercicio 1 — Suma de dos números

**Archivo:** `src/ej1.py`

```Python

def suma(a: int, b: int) -> int:  

# Devuelve la suma de a y b  

return a + b  

print(suma(3,5))

```

### Ejercicio 2 — Par o impar

**Archivo:** `src/ej2.py`

```Python

def es_par(n: int) -> bool:  

# Devuelve True si n es par, False si es impar  

return n % 2 == 0  

print(esPar(4))
```

### Ejercicio 3 — Contar vocales

**Archivo:** `src/ej3.py`

```Python

def contar_vocales(texto: str) -> int:

    # Contar el número de vocales en el texto dado

    vocales = "aeiouAEIOU"

    return sum(1 for letra in texto if letra in vocales)

  

#esto es equivalente a:


#contador = 0

#for letra in texto:  # ← Aquí se crea "letra"

#   if letra in vocales:

#       contador = contador + 1

#return contador


print(contar_vocales("Salvador"))

```

### Ejercicio 4 — Máximo en lista

**Archivo:** `src/ej4.py`

```Python

def maximo(lista: list[int]) -> int:  

# Devuelve el número máximo de la lista  

return max(lista)  
print(maximo([3, 5, 2, 8, 1]))
```

### Ejercicio 5 — Invertir cadena

**Archivo:** `src/ej5.py`

```Python

def invertir(texto: str) -> str:  

# Devuelve el texto invertido  

return texto[::-1]  

```

### Ejercicio 6 — Diccionario de cuadrados

**Archivo:** `src/ej6.py`

```Python

def cuadrados(n: int) -> dict[int, int]:  

# Devuelve un diccionario {i: i**2} para i en 1..n  

return {i: i**2 for i in range(1, n+1)}  

```

### Ejercicio 7 — Filtrar pares

**Archivo:** `src/ej7.py`

```Python

def filtrar_pares(lista: list[int]) -> list[int]:  

# Devuelve solo los números pares de la lista  

return [x for x in lista if x % 2 == 0]  

```

### Ejercicio 8 — Factorial

**Archivo:** `src/ej8.py`

```Python

def factorial(n: int) -> int:  

# Calcula el factorial de n (n!)  

resultado = 1  

for i in range(1, n+1):  

resultado *= i  

return resultado  

```

### Ejercicio 9 — Palíndromo

**Archivo:** `src/ej9.py`

```Python

def es_palindromo(texto: str) -> bool:  

# Devuelve True si texto es palíndromo  

texto = texto.lower().replace(" ", "")  

return texto == texto[::-1]  

```

### Ejercicio 10 — Merge de listas

**Archivo:** `src/ej10.py`

```Python

def combinar(lista1: list[int], lista2: list[int]) -> list[int]:  

# Devuelve una lista combinada y ordenada  

return sorted(lista1 + lista2)  

```

---

## 3. Tests automáticos (pytest)

📂 **Archivo:** `tests/test_semana1.py`

```Python

from src.ej1 import suma  

from src.ej2 import es_par  

from src.ej3 import contar_vocales  

from src.ej4 import maximo  

from src.ej5 import invertir  

from src.ej6 import cuadrados  

from src.ej7 import filtrar_pares  

from src.ej8 import factorial  

from src.ej9 import es_palindromo  

from src.ej10 import combinar  

  

def test_ej1():  

assert suma(2, 3) == 5  

  

def test_ej2():  

assert es_par(4) is True  

assert es_par(5) is False  

  

def test_ej3():  

assert contar_vocales("Hola") == 2  

  

def test_ej4():  

assert maximo([1, 5, 3]) == 5  

  

def test_ej5():  

assert invertir("abc") == "cba"  

  

def test_ej6():  

assert cuadrados(3) == {1: 1, 2: 4, 3: 9}  

  

def test_ej7():  

assert filtrar_pares([1, 2, 3, 4]) == [2, 4]  

  

def test_ej8():  

assert factorial(5) == 120  

  

def test_ej9():  

assert es_palindromo("Anita lava la tina") is True  

  

def test_ej10():  

assert combinar([3, 1], [2, 4]) == [1, 2, 3, 4]  

```

---

## 4. Cómo ejecutar los tests

En la terminal (con el venv activo):

```Shell

pytest -v  

```

---

## ✅ Reto final de la semana

- Completa los 10 ejercicios.
- Ejecuta `pytest` y asegúrate de que **todos los tests pasan**.
