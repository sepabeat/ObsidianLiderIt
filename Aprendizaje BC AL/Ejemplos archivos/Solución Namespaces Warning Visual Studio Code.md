# Propuesta de Mapa de Namespaces para Proyecto Arruzafa

## 🎯 Namespace Base Recomendado

- 
- 
- 
- 

**Justificación:**

- `LiderIT` = Publisher del [app.json](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html) (empresa)
- `Arruzafa` = Nombre del [app.json](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html) (módulo/cliente específico - Clínica oftalmológica de Córdoba)
- Claridad sobre el dominio de negocio del cliente específico

---

## 📋 Mapa de Namespaces por Carpeta/Módulo

|**Carpeta**|**Namespace Propuesto**|**Motivo**|**Objetos Ejemplo**|
|---|---|---|---|
|[codeunit](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html) **(GS1 & Traceability)**|`LiderIT.Arruzafa.Traceability`|Funcionalidad de trazabilidad farmacéutica con códigos GS1, escaneo de productos y tracking de lotes|`GS1BarcodeParserLDR`, `ItemJournalScanLDR`, `ItemTrackingFixLDR`, `ItemJournalScanDTOLDR` (table)|
|[codeunit](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html) **(Patient Management)**|`LiderIT.Arruzafa.HealthCare`|Gestión de pacientes en contexto clínico oftalmológico (validación, transferencia de datos, dimensiones)|`PatientValidationLDR`, `PatientDataTransferLDR`, `CustomerPacienteDimValueLDR`|
|[codeunit](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html) **(Payroll)**|`LiderIT.Arruzafa.Payroll`|Sistema de importación de nóminas desde Excel específico para Arruzafa|`PayrollImportMgtArruzafaLDR` (codeunit), `PayrollImportSetupArruzafaLDR` (table), `PayrollImportLineArruzafa` (table), `PayrollImportHistoryArruzafa` (table), `PayrollImportStatus` (enum)|
|[codeunit](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html) **(Purchase Pricing)**|`LiderIT.Arruzafa.PurchasePricing` ⚠️|Importación de precios de compra desde Excel. **Ya tiene namespace:** `COC.PurchasePricing` - considerar migración a `LiderIT.Arruzafa.PurchasePricing` para consistencia|`COCPurchPriceImportMgt`, `COCPurchPriceImportBuffer` (table)|
|[codeunit](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html) **(Banking)**|`LiderIT.Arruzafa.Banking`|Importación de extractos bancarios Norma 43|`Norm43ImportLDRLDR`, `CuentaConcepto43LDR` (table), `LDRCargaConciliacionNorma43Lsc` (xmlport)|
|[table](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html) **(shared entities)**|Según el módulo funcional|Las tablas deben agruparse según su propósito: Traceability, HealthCare, Payroll, Banking, PurchasePricing|Ver distribución en filas anteriores|
|[page](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)|Mismo namespace que su tabla relacionada|Las páginas heredan el namespace de las entidades que gestionan|`PayrollImportArruzafaLDR` → `LiderIT.Arruzafa.Payroll`, `ScanProductLDR` → `LiderIT.Arruzafa.Traceability`|
|[pageextension](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)|`LiderIT.Arruzafa.Extensions`|Extensiones a objetos estándar de BC (Sales, Purchase, Item, etc.) - agrupa todas las extensiones|`SalesHeaderTELDR`, `ItemCardLDR`, `SalesOrderPELDR`, `PurchaseOrderSubformLDR`, etc.|
|[tableextension](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)|`LiderIT.Arruzafa.Extensions`|Extensiones a tablas estándar de BC - coherente con pageextension|`CompanyInformationTELDR`, `GenJournalLineLDR`, `ItemLDR`, `SalesHeaderTELDR`, etc.|
|[enum](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)|Según el módulo funcional|Los enums van al namespace del módulo que los utiliza|`PayrollImportStatus` → `LiderIT.Arruzafa.Payroll`|
|[report](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)|`LiderIT.Arruzafa.Reporting`|Informes personalizados (facturas, abonos, etc.)|`SalesInvoiceReportLDR`, `FacturaVentaProformaLDR`, `AbonoVentaLDR`|
|[xmlport](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)|Según el módulo funcional|Los XMLPorts van al namespace del módulo que los utiliza|`LDRCargaConciliacionNorma43Lsc` → `LiderIT.Arruzafa.Banking`|
|[Core](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)|`LiderIT.Arruzafa.Shared`|Componentes compartidos y reutilizables entre módulos (actualmente vacía)|Interfaces, helpers, utilidades comunes|
|[UI](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)|`LiderIT.Arruzafa.Shared`|Componentes de UI compartidos (actualmente vacía)|Páginas/controles reutilizables|

---

## 🔗 Reglas de Referencia entre Namespaces

### **1. Referencias entre módulos principales**

Cuando un objeto de un namespace necesita referenciar otro de distinto namespace, **usa el nombre completo del objeto**:

- 
- 
- 
- 

### **2. Extensions → Otros módulos**

Los objetos en `LiderIT.Arruzafa.Extensions` frecuentemente **invocarán** objetos de módulos funcionales:

- 
- 
- 
- 

### **3. Objetos estándar de BC**

Los objetos estándar de Microsoft **no requieren namespace** (ya vienen con su propio namespace implícito):

- 
- 
- 
- 

### **4. Uso del namespace completo es obligatorio cuando:**

- Hay ambigüedad (dos objetos con el mismo nombre en diferentes namespaces)
- Se invoca desde un objeto extension que extiende un objeto estándar
- Se necesita claridad en el origen del objeto para mantenimiento futuro

### **5. Within-namespace (mismo namespace)**

Dentro del mismo namespace, **NO es necesario** especificar el namespace completo:

- 
- 
- 
- 

---

## 🚀 Archivos Prioritarios para Empezar (Quick Wins)

Estos archivos tienen **pocas o ninguna dependencia cruzada** y son buenos candidatos iniciales:

### **1️⃣ [PayrollImportMgtArruzafaLDR.Codeunit.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)** ✅

- **Namespace:** `LiderIT.Arruzafa.Payroll`
- **Motivo:** Módulo autocontenido, solo referencia tablas propias del payroll + objetos estándar de BC
- **Dependencias:** `PayrollImportSetupArruzafaLDR`, `PayrollImportLineArruzafa`, `PayrollImportHistoryArruzafa` (todas en el mismo namespace)

### **2️⃣ [PayrollImportStatus.Enum.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)** ✅

- **Namespace:** `LiderIT.Arruzafa.Payroll`
- **Motivo:** Enum simple, solo usado por tablas de Payroll
- **Dependencias:** Ninguna

### **3️⃣ [GS1BarcodeParserLDR.Codeunit.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)** ✅

- **Namespace:** `LiderIT.Arruzafa.Traceability`
- **Motivo:** Lógica de parseo pura, pocas dependencias externas
- **Dependencias:** Solo objetos estándar de BC

### **4️⃣ [Norm43ImportLDRLDR.Codeunit.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)** ✅

- **Namespace:** `LiderIT.Arruzafa.Banking`
- **Motivo:** Módulo banking independiente, referencia tabla propia + tablas estándar de BC
- **Dependencias:** `CuentaConcepto43LDR` (misma namespace), objetos estándar de BC

---

## ⚠️ Conflictos Potenciales y Resoluciones

### **Conflicto 1: Namespace existente `COC.PurchasePricing`**

**Problema:** Ya existe el namespace `COC.PurchasePricing` en:

- [COCPurchPriceImportMgt.Codeunit.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
- [COCPurchPriceImportBuffer.Table.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)
- [COCPurchPriceListsExtLDR.PageExt.al](vscode-file://vscode-app/c:/Users/spavila/AppData/Local/Programs/Microsoft%20VS%20Code%20Insiders/f8f8ba0eed/resources/app/out/vs/code/electron-browser/workbench/workbench.html)

**Resolución sugerida:**

- **Opción A (consistencia total):** Migrar a `LiderIT.Arruzafa.PurchasePricing` para uniformidad con el resto del proyecto
- **Opción B (mantener):** Dejar `COC.PurchasePricing` si "COC" es una marca/submarca reconocida del cliente
- **Recomendación:** Opción A para mantener coherencia (`LiderIT.Arruzafa` como raíz única)

### **Conflicto 2: Extensions que referencian múltiples módulos**

**Problema:** Las extensiones (pageextension/tableextension) necesitan acceso a objetos de varios namespaces funcionales.

**Resolución:**

- 
- 
- 
- 

### **Conflicto 3: Objetos con nombres largos + namespace**

**Problema:** AL tiene límite de 30 caracteres para nombres de objetos, y el namespace cuenta aparte, pero en referencias completas puede complicarse.

**Resolución:**

- Los nombres de objetos actuales son correctos (ej: `PayrollImportMgtArruzafaLDR` = 29 caracteres)
- El namespace se añade como prefijo en referencias pero no cuenta para el límite del nombre
- Usar `using` statements para evitar repetir el namespace completo

---

## 📝 Notas Finales

### **Orden de implementación sugerido:**

1. **Fase 1 - Módulos independientes (sin dependencias cruzadas):**
    
    - `LiderIT.Arruzafa.Payroll` (4 archivos: 1 codeunit, 3 tables, 1 enum, 2 pages)
    - `LiderIT.Arruzafa.Banking` (2 archivos: 1 codeunit, 1 table, 1 xmlport, 1 page)
2. **Fase 2 - Módulos core con pocas dependencias:**
    
    - `LiderIT.Arruzafa.Traceability` (3 codeunits, 1 table, 2 pages)
    - `LiderIT.Arruzafa.HealthCare` (3 codeunits)
3. **Fase 3 - Reporting y otros:**
    
    - `LiderIT.Arruzafa.Reporting` (3 reports)
    - `LiderIT.Arruzafa.PurchasePricing` (migración desde `COC.PurchasePricing`)
4. **Fase 4 - Extensions (al final, porque dependen de todos):**
    
    - `LiderIT.Arruzafa.Extensions` (todas las pageext/tableext)

### **Ventajas de esta estructura:**

✅ **Claridad:** Cada namespace refleja un dominio de negocio claro  
✅ **Escalabilidad:** Fácil añadir nuevos módulos (ej: `LiderIT.Arruzafa.Inventory`)  
✅ **Mantenibilidad:** Separación de responsabilidades tipo SOLID  
✅ **Consistencia:** Patrón `LiderIT.Arruzafa.Module` uniforme en todo el proyecto  
✅ **Extensibilidad:** Las extensions agrupadas facilitan identificar modificaciones a estándar