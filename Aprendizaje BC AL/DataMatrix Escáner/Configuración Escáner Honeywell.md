
Modelo de pistola --> Honeywell Voyager 1400g 
[Tabla De Contenido - Honeywell Voyager 1400g Serie Guia De Inicio Rapido [Página 17] | ManualsLib](https://www.manualslib.es/manual/166193/Honeywell-Voyager-1400G-Serie.html?controller=view&page=17#manual)

[Manual de usuario Honeywell Voyager 1400g (216 páginas)](https://www.manual.ec/honeywell/voyager-1400g/manual)

**GS1 DataMatrix** y **GS1‑128**, incluyendo el **FNC1**, y que además haga **Enter** al final

En Honeywell debes activar explícitamente:
Si Honeywell está en modo “raw”, tus desarrollos fallarán.

- DataMatrix
- QR
- GS1 DataMatrix
- GS1-128
- Code128
## Configura la Honeywell 1400g del cliente para que funcione EXACTAMENTE igual que tu Zebra:


### 🔸 Habilitar DataMatrix

### 🔸 Habilitar GS1 DataMatrix

### 🔸 Interpretar FNC1 como separador

### 🔸 Enviar ENTER / CR al final

### 🔸 Habilitar Code128 / GS1‑128

### 🔸 Desactivar prefijos extraños

### 🔸 Limpiar toda configuración anterior

## 1) Simbologías a **habilitar**

Prioriza estas; son las que utilizas en el flujo de farmacia:

- **Data Matrix (2D)**
    - _Sub‑opción_: **GS1 DataMatrix** (a veces aparece como “Decode GS1 DataMatrix” o “Enable FNC1 in Data Matrix”).
- **QR Code (2D)** _(opcional, si tu app también lee QR no‑GS1)_.
- **Code 128 (1D)**.
    - _Sub‑opción_: **GS1‑128** (a veces listado como “UCC/EAN‑128”, “EAN‑128” o “GS1‑128”).
- **Code 39** _(opcional; algunos proveedores lo usan para IDs internos)_.
- **EAN‑13 / UPC‑A** _(opcional; por si hay GTIN lineales de consumo)_.
- **GS1 DataBar** (antes “RSS”): Omnidirectional / Limited / Expanded _(opcional; según catálogos del cliente)_.


pag 81 índice de simbologías

# 🔥 **Resumen perfecto para tu manual del cliente**

Aquí tienes la **lista mínima y correcta**, basada en tu foto:

## 🟩 **1. Habilitar (necesario)**

- **Data Matrix ECC‑200**
- **GS1 DataMatrix**
- **FNC1 in DataMatrix**
- **Code 128**
- **GS1‑128 (UCC/EAN‑128)**
- **EAN‑13 / UPC‑A**

## 🟨 **2. Opcional pero recomendado**

- **QR Code**
- **GS1 DataBar (RSS)**

## 🔴 **3. Deshabilitar**

- Todo lo que no sea utilizado en farmacia/UDI.
# 🟩 **Simbologías 2D (sí o sí necesarias)**

### ✔ **DataMatrix ECC‑200**

Muchísimos productos en tu collage llevan el típico cuadrado blanco y negro:

Ejemplos típicos visibles:

- Envases de B. Braun
- Envases de Esnoper
- Envases de Albomed
- Etiquetas con SN / PC
- Cajas blancas con UDI
- Medicamentos hospitalarios

👉 **Conclusión:**  
**Obligatorio habilitar “Data Matrix” + “GS1 DataMatrix” + FNC1.**
👉 **Obligatorio habilitar:**

- **Code 128** (inicio FNC1) 
- **GS1‑128 (también aparece como UCC/EAN‑128)**

### ✔ **GS1 DataBar (RSS)**

A veces aparecen en productos pequeños o ampollas.

### ✔ **QR Code**

No aparece ninguno en tu collage, pero algunos proveedores empiezan a usarlos.

👉 Recomendado habilitar:

- **QR Code**
- **GS1 DataBar Omnidirectional / Expanded**
# 🔴 **Simbologías que NO necesitas (no presentes en la imagen)**

Para evitar ruido innecesario del escáner, puedes deshabilitar:

- Codabar
- Code 93
- MSI
- Interleaved 2 of 5 (ITF)
- Matrix 2 of 5
- Standard 2 of 5
- Telepen
- Postales (PDF417, USPS, etc.) → _salvo que tú quieras, pero no aparecen en tu collage_



