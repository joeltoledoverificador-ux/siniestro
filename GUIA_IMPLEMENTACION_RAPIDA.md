# GUÍA RÁPIDA: CREAR ARCHIVO XLSX DESDE ESTOS DATOS

## 🎯 OPCIÓN MÁS RÁPIDA (5 minutos)

### Paso 1: Ir a Google Sheets
1. Abre https://sheets.google.com
2. Click en "Crear nuevo documento de hojas de cálculo"
3. Dale nombre: "SISTEMA_GESTION_SINIESTROS"

### Paso 2: Crear las 7 hojas
En la parte inferior, haz clic derecho en la pestaña de hoja y:
- Renombra "Hoja 1" → "Dashboard"
- Crea 6 hojas nuevas: Siniestros, Seguimiento, Visitas, Finanzas, Ficha_Consulta, Listas

### Paso 3: Descargar datos CSV desde GitHub
Descarga estos 5 archivos desde:
https://github.com/joeltoledoverificador-ux/siniestro/

- DATOS_HOJA_01_SINIESTROS.csv
- DATOS_HOJA_02_SEGUIMIENTO.csv
- DATOS_HOJA_03_VISITAS.csv
- DATOS_HOJA_04_FINANZAS.csv
- DATOS_HOJA_05_LISTAS.csv

### Paso 4: Importar datos a Google Sheets
Para cada hoja:
1. Abre el CSV con bloc de notas
2. Copia TODO (Ctrl+A, Ctrl+C)
3. Va a Google Sheets → hoja correspondiente → celda A1
4. Pega (Ctrl+V)

### Paso 5: Agregar fórmulas en Google Sheets

**HOJA DASHBOARD (Fila 2):**

Celda B2: `=COUNTIFS(Siniestros!H:H,"<>Cerrados",Siniestros!K:K,"<"&TODAY())`
Celda B3: `=COUNTIF(Visitas!E:E,TODAY())`
Celda B4: `=COUNTIFS(Visitas!E:E,">"&TODAY(),Visitas!E:E,"<="&TODAY()+7)`

**HOJA SINIESTROS (Columna L - Días Pendientes):**

Celda L2: `=IF(H2="Cerrados","",K2-TODAY())`

**HOJA SEGUIMIENTO (Columna B - Asegurado):**

Celda B2: `=IFERROR(VLOOKUP(A2,Siniestros!A:C,2,FALSE),"")`

**HOJA VISITAS (Columnas B, C, D):**

Celda B2: `=IFERROR(VLOOKUP(A2,Siniestros!A:B,2,FALSE),"")`
Celda C2: `=IFERROR(VLOOKUP(A2,Siniestros!A:C,3,FALSE),"")`
Celda D2: `=IFERROR(VLOOKUP(A2,Siniestros!A:D,4,FALSE),"")`
Celda J2: `=IF(I2="","",I2-E2)`

**HOJA FINANZAS (Columnas B, C, D):**

Celda B2: `=VLOOKUP(A2,Siniestros!A:B,2,FALSE)`
Celda C2: `=VLOOKUP(A2,Siniestros!A:C,3,FALSE)`
Celda D2: `=VLOOKUP(A2,Siniestros!A:H,8,FALSE)`

**TABLA RESUMEN FINANZAS (I1:K5):**

Celda J2: `=SUMIF(G:G,"Pagado",E:E)`
Celda K2: `=COUNTIF(G:G,"Pagado")`
(Repetir para Pendiente y Por Generar)

### Paso 6: Configurar validaciones de datos (Desplegables)

Para HOJA SINIESTROS, Columna C (Compañía):
1. Selecciona C2:C1000
2. Data → Validity
3. Tipo: List
4. Fuente: Listas!COMPAÑÍAS (o copiar valores manualmente desde Listas)

Repetir para:
- D: Localidades
- G: Tareas
- H: Estados
- I: Prioridad
- J: Responsables

### Paso 7: Aplicar formatos condicionales

HOJA SINIESTROS - Columna L:
1. Selecciona L2:L1000
2. Format → Conditional formatting
3. Si L < 0 Y H ≠ "Cerrados" → Fondo Rojo

HOJA SINIESTROS - Fila completa:
1. Selecciona A2:M1000
2. Si H = "Cerrados" → Fondo Gris

### Paso 8: Descargar como XLSX

1. File → Download → Excel (.xlsx)
2. ¡Listo! Abre en Excel, OnlyOffice o LibreOffice

---

## 🔄 ALTERNATIVA: CREAR EN LIBREOFFICE (Más control local)

### Paso 1: Abrir LibreOffice Calc
`sudo apt install libreoffice` (si no está instalado)

### Paso 2: Crear 7 hojas
Renombra según estructura

### Paso 3: Importar CSV
Para cada archivo CSV:
1. Sheet → Insert Sheet from File
2. Selecciona el CSV
3. Configura separadores (coma)

### Paso 4: Agregar fórmulas (adaptar sintaxis a LibreOffice)
- Google Sheets usa: TODAY(), COUNTIF, SUMIF
- LibreOffice usa: HOY(), CONTAR.SI, SUMAR.SI
- Cambiar separadores de `,` a `;`

Ejemplo:
```
Google: =COUNTIF(Siniestros!E:E,TODAY())
LibreOffice: =CONTAR.SI(Siniestros.E:E;HOY())
```

### Paso 5: Guardar como XLSX
File → Save As → Format: Excel 2007-365 (.xlsx)

---

## ✅ ARCHIVOS DISPONIBLES EN GITHUB

Carpeta: https://github.com/joeltoledoverificador-ux/siniestro/

📄 SISTEMA_GESTION_SINIESTROS_v1.0.md (Especificación completa)
📄 DATOS_HOJA_01_SINIESTROS.csv
📄 DATOS_HOJA_02_SEGUIMIENTO.csv
📄 DATOS_HOJA_03_VISITAS.csv
📄 DATOS_HOJA_04_FINANZAS.csv
📄 DATOS_HOJA_05_LISTAS.csv
📄 GUIA_IMPLEMENTACION.md (Este archivo)

---

## 🆘 SI TIENES PROBLEMAS

**Problema: Las fórmulas no funcionan**
→ Cambiar separadores `,` a `;` (LibreOffice)
→ Cambiar TODAY() a HOY(), COUNTIF a CONTAR.SI

**Problema: Desplegables no funcionan**
→ Verificar que Hoja Listas existe y tiene datos
→ Validación de datos debe apuntar a Listas!A:A (ajustar rango)

**Problema: VLOOKUP no encuentra valores**
→ Verificar que ID existe en columna A de Siniestros
→ Verificar que no hay espacios en blanco extra

---

## 🎯 RESULTADO FINAL

✅ 7 hojas funcionales
✅ Todas las fórmulas configuradas
✅ Desplegables funcionando
✅ Dashboard actualizado automáticamente
✅ Listo para producción

**Archivo XLSX 100% compatible con Excel, OnlyOffice y LibreOffice**

---

Cualquier duda, avísame. 📞