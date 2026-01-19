El "codex" que se utiliza en el desarrollo de Getronix de Fármacos es GS1 DataMatrix, específicamente el **FNC1 (Function Code 1)**
Proceso para obtener la información del escáner/datamatrix de un código QR sin perder información y conseguir los caracteres invisibles.
Instrucciones del escáner ZEBRA DS22 --> [DS2278 Digital Scanner Product Reference Guide (en)](https://www.zebra.com/content/dam/support-dam/en/documentation/unrestricted/guide/product/ds2278-prg-en.pdf) 

1. Descargar la imagen adjunta. (Indicándome cuál de los tres códigos se va a escanear De arriba abajo 1-2-3)
2. Abrir un archivo nuevo .txt con el bloc de notas
3. Colocar cursor en la primera línea del txt, teniendo abierta la imagen del código QR en una pantalla, escanear código con la pistola.
4. Si se ha realizado correctamente, ha generado un código como si lo hubiera hecho un teclado. Visualmente son letras y números pero hay más información “invisible”.
5. Guardar el archivo .txt, comprimir como .ZIP
6. Enviar esta carpeta comprimida .ZIP por e-mail.

Este proceso es necesario para obtener el formato utilizado para escanear por el modelo/marca del escáner sin tener acceso físico a él.


Previo a usar desarrollo de Diaro de productos, hace falta establecer una relación GTIN con cada producto, esto se establece en Ficha producto --> Relacionado --> Producto --> Referencias artículo --> Establecer el primer campo "Tipo referencia" con el valor Cód. Barras, y el campo Nº Referencia es el que tiene que tener el valor del GTIN. 
Para obtener el GTIN de un código de un producto nuevo 



### **Sobre la configuración del escáner (Zebra DS22 y Honeywell)**

El comportamiento que describes (GS aparece solo cuando escaneas en BC y no en Notepad) indica que el escáner está en modo **Keyboard Wedge** y probablemente **no está transmitiendo FNC1 como GS (ASCII 29)** en todos los contextos. Esto depende de la configuración.

#### **Qué debes hacer en Zebra DS22**

1. **Activar GS1 DataMatrix y FNC1**
    - Escanea el código de configuración: _“Enable GS1 DataMatrix”_.
2. **Transmitir FNC1 como GS (ASCII 29)**
    - Busca en el manual el código: _“Transmit FNC1 as GS”_ (Group Separator).
3. **Desactivar sustituciones**
    - Asegúrate de que no esté configurado para convertir GS en TAB o eliminarlo.
4. **Verificar sufijos**
    - Mantén solo `CRLF` como terminador, sin filtros adicionales.

#### **Para Honeywell (clientes)**

- Igual: activar GS1 DataMatrix y FNC1.
- Escanear el código de configuración: _“Send FNC1 as ”_.
- Desactivar reglas ADF que puedan eliminar caracteres de control.

## 1. ¿Qué es el GTIN y cómo se obtiene al escanear?

- El **GTIN** (Global Trade Item Number) es un identificador internacional único para productos, normalmente de 13 o 14 dígitos, que viene codificado en los códigos de barras (EAN/UPC) y en los DataMatrix/QR de los envases.
- Cuando escaneas un DataMatrix/QR, el primer bloque de datos con el AI (Application Identifier) **01** es el GTIN.  
    Ejemplo:  
    Si el código escaneado es `01084700066159801728033110BX593221VG4R5TWP1`,  
    el GTIN es **08470006615980** (los 14 dígitos después de "01").




# Resumen Desarrollo DataMatrix/GS1 Parser - Estado Actual

## Contexto General

Desarrollo de escáner de códigos GS1 (DataMatrix, GS1-128, QR) para Business Central con soporte de múltiples formatos de códigos de barras. El sistema debe funcionar en 3 módulos distintos: **Item Card**, **Item Journal** y **Transfer/Purchase Orders**.

## Archivos Principales Modificados

### 1. **[GS1BarcodeParserLDR.Codeunit.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/03f79ac30b/resources/app/out/vs/code/electron-browser/workbench/workbench.html)** (Parser Centralizado)

- **Ubicación**: [GS1BarcodeParserLDR.Codeunit.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/03f79ac30b/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
- **Función**: Parser centralizado para todos los formatos GS1 y propietarios
- **Soporta**:
    - **GS1 estándar**: AIs 01 (GTIN), 10 (Lote), 11 (Producción), 17 (Caducidad), 21 (Serial), 30/37 (Cantidad)
    - **Formato DRN**: Johnson&Johnson Vision (`DRN00V` + potencia + serial + fecha AAMMDD)
    - **Separador GS**: ASCII 29 para delimitación de campos
- **Características clave**:
    - `ParseGS1Code()`: Punto de entrada principal
    - `ExtractVariableLengthData()`: Lógica inteligente para campos alfanuméricos
        - Si última letra en posición 1 (ej: "G1060328"): toma todo sin buscar AIs dentro
        - Si letras mezcladas (ej: "G1225"): busca AI después de la última letra
        - Solo numérico: busca AI normalmente
    - `FindNextAI()`: Busca siguiente AI validando que sea un AI conocido
    - `IsKnownAI()`: Valida AIs reconocidos (01, 02, 10, 11, 15, 17, 20, 21, 30, 37)

### 2. **ItemLDR.TableExt.al** (Extensión de Tabla Item)

- **Ubicación**: [ItemLDR.TableExt.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/03f79ac30b/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
- **Campo**: `"Scanned Code_LDR"` para entrada manual de códigos
- **Características**:
    - `ProcessingScannedCode`: Flag para evitar doble entrada del escáner
    - `CleanScannedCode()`: Limpia caracteres especiales (mantiene alfanuméricos + '-', '_', '*')
    - `TryValidateGTIN()`: Validación segura de GTIN con TryFunction
    - `CreateOrUpdateItemReference()`: Crea automáticamente Item Reference con GTIN o código completo
    - **NO limpia** el campo después de procesar (requisito del usuario)

### 3. **ItemJournalScanLDR.Codeunit.al** (Lógica Item Journal)

- **Ubicación**: [ItemJournalScanLDR.Codeunit.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/03f79ac30b/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
- **Función**: Parseo de códigos escaneados en diarios de productos
- **Lógica Serial-as-Lot**: Si AI 10 no existe pero AI 21 sí, usa AI 21 como lote (líneas 38-45)
- **DEBUG ACTIVO** (líneas 30-36): Muestra código recibido, longitud y valores parseados

### 4. **ItemCardLDR.PageExt.al** (Extensión Página Item Card)

- **Ubicación**: [ItemCardLDR.PageExt.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/03f79ac30b/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
- **Campo "Item Category Code"**: `Editable = false` (líneas 22-24) para evitar overflow del escáner escribiendo "306"

### 5. **ScanProductLDR.Page.al** (Página Transfer/Purchase Orders)

- **Ubicación**: [ScanProductLDR.Page.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/03f79ac30b/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
- **Lógica Serial-as-Lot**: Implementada en dos bloques (líneas 50-57 PEDIDOTRANSFERENCIAQR, líneas 89-96 PEDIDOCOMPRA)

## Formatos de Código Soportados

### Códigos de Prueba (16 tipos: 1002-1016)

- **Código 1004**: DataMatrix con ceros a la izquierda ✓ Funciona
- **Código 1005**: Usa AI 21 como lote (Serial-as-Lot) ✓ Funciona
- **Código 1006**: `0100892064002119112506261727062610G122521G1060328` 🔴 **PROBLEMA ACTUAL**
    - **Esperado**: Lote="G1225", Serial="G1060328"
    - **Actual**: Se parsea incorrectamente
- **Código 1015**: DRN propietario (Johnson&Johnson) ✓ Funciona

## Problema Actual (Código 1006)

### Debug Output Más Reciente

DEBUG INPUT:
Código recibido: 0100892064002119112506261727062610G122521G1060328
Longitud: 50

DEBUG AI 10:
Dato extraído: G1225
Código restante: 21G1060328

DEBUG AI 21:
Dato extraído: G     ← MAL (debería ser G1060328)
Código restante: 1060328

DEBUG AI 10:
Dato extraído: 60328  ← Sobrescribe el lote correcto
Código restante:

DEBUG Parsing:
GTIN: 00892064002119 ✓
Lote: 60328          ✗ (debería ser G1225)
Serial: G            ✗ (debería ser G1060328)
Fecha: 26/06/27      ✓

### Causa Raíz

En "G1060328":

1. Mi lógica detecta letra "G" en posición 1
2. Busca AI después de posición 1
3. Encuentra "**10**" en "G**10**60328" y lo trata como AI 10
4. Extrae solo "G", dejando "1060328"
5. El parser vuelve a procesar "10" como AI 10, sobrescribiendo el lote correcto

### Solución Implementada (Última Versión)

En `ExtractVariableLengthData()`:

- Si `LastLetterPos = 1`: NO buscar AIs, tomar TODO el dato (para "G1060328")
- Si `LastLetterPos > 1`: Buscar AI solo después de última letra (para "G1225")
- Sin letras: Buscar AI normalmente

**⚠️ PENDIENTE DE PROBAR** - El usuario necesita publicar la extensión y escanear código 1006 para validar.

## Mensajes DEBUG Activos (QUITAR DESPUÉS)

### ItemJournalScanLDR.Codeunit.al (líneas 30-36)

// DEBUG: Mostrar código completo recibido
Message('DEBUG INPUT:\nCódigo recibido: %1\nLongitud: %2', ScanDTO.ScanInput, StrLen(ScanDTO.ScanInput));

// DEBUG: Mostrar valores parseados
Message('DEBUG Parsing:\nGTIN: %1\nLote: %2\nSerial: %3\nFecha: %4', GTIN, LotNumber, SerialNumber, ExpirationDate);

### [GS1BarcodeParserLDR.Codeunit.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/03f79ac30b/resources/app/out/vs/code/electron-browser/workbench/workbench.html) (líneas ~156-169)
'10': // Lote
    begin
        ExtractedData := ExtractVariableLengthData(RemainingCode);
        LotNumber := CopyStr(ExtractedData, 1, 50);
        Message('DEBUG AI 10:\nDato extraído: %1\nCódigo restante: %2', ExtractedData, RemainingCode);
    end;
'21': // Serial
    begin
        ExtractedData := ExtractVariableLengthData(RemainingCode);
        SerialNumber := CopyStr(ExtractedData, 1, 50);
        Message('DEBUG AI 21:\nDato extraído: %1\nCódigo restante: %2', ExtractedData, RemainingCode);
    end;

## Requisitos Críticos del Usuario

1. **"No destruyas lo anterior"**: TODOS los 16 códigos (1002-1016) deben seguir funcionando
2. **Cambios aditivos**: No modificar lógica que rompa funcionalidad existente
3. **Campo no se limpia**: El "Scanned Code" en Item Card NO debe limpiarse automáticamente
4. **Tres módulos independientes**: Item Card, Item Journal, Transfer/Purchase Orders usan el mismo parser pero flujos distintos
5. **Compilación**: Priorizar corrección sobre compilación inmediata si hay dudas de nombres de funciones BC

## Próximos Pasos

1. **VALIDAR**: Usuario debe publicar y escanear código 1006 con la solución actual
2. **SI FUNCIONA**: Eliminar TODOS los mensajes DEBUG
3. **SI NO FUNCIONA**: Analizar nuevo output debug y ajustar `ExtractVariableLengthData()`
4. **TESTING COMPLETO**: Validar todos los códigos 1002-1016 para regresión
5. **DOCUMENTACIÓN**: Una vez estable, documentar patrones de códigos soportados

## Notas Técnicas Importantes

- **MaxLength = 50**: Aumentado desde 20 para soportar lotes alfanuméricos largos
- **FindNextAI empieza en posición 1**: No en 2, para buscar correctamente en código restante
- **Serial-as-Lot**: Implementado en 3 lugares (ItemJournalScanLDR línea 38, ScanProductLDR líneas 50 y 89)
- **Item Reference**: Se crea automáticamente al escanear en Item Card, evitando configuración manual

## Contacto con Usuario

Si hay problemas y necesitas empezar chat nuevo, mencionar:

- "Continuando desarrollo DataMatrix/GS1 Parser"
- "Código 1006 tiene problema con lotes alfanuméricos tipo G1225 + serial G1060328"
- "DEBUG messages están activos en ItemJournalScanLDR y GS1BarcodeParserLDR"
- "Última solución: detectar LastLetterPos=1 para NO buscar AIs dentro del dato"