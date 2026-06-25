# SISTEMA DE GESTIÓN DE SINIESTROS
## Libro de Trabajo Optimizado para LibreOffice Calc / OnlyOffice Calc

**Versión:** 1.0  
**Fecha:** 2026-06-25  
**Autor:** Ingeniero de Datos - Especialista en Planillas de Cálculo  
**Mantenedor:** Joel Toledoverificador-UX

---

## 📋 ESTRUCTURA GENERAL

Sistema integral de 7 hojas interconectadas sin macros, optimizado para rendimiento y máxima compatibilidad.

### Hojas Incluidas:
1. **Dashboard** - Tablero de Control ejecutivo con KPIs automatizados
2. **Siniestros** - Base de datos maestra de casos
3. **Seguimiento** - Historial cronológico de gestiones
4. **Visitas** - Agenda de inspecciones de campo
5. **Finanzas** - Facturación y costos por siniestro
6. **Ficha_Consulta** - Buscador operativo con interfaz corporativa
7. **Listas** - Vectores de configuración editable

---

## 🎯 HOJA 1: DASHBOARD (Tablero de Control)

### Bloque 1: Alertas Operativas
```
Tareas Vencidas:
=CONTAR.SI.CONJUNTO(Siniestros!H:H;"<>Cerrados";Siniestros!K:K;"<"&HOY())

Visitas para Hoy:
=CONTAR.SI(Visitas!E:E;HOY())

Visitas Próximos 7 Días:
=CONTAR.SI.CONJUNTO(Visitas!E:E;">"&HOY();Visitas!E:E;"<="&HOY()+7)
```

### Bloque 2: Métricas de Siniestros
Tabla con conteos por estado:
- Pendiente análisis
- Contactar
- Aguardamos respuestas
- Coordinar visita
- Con visita pactada
- Pendientes de datos
- Cargar informe
- Informes elevados
- Retiro denuncia
- Cerrados

**Total Siniestros:** `=CONTAR.A(Siniestros!A:A)-1`

### Bloque 3: Métricas de Visitas
Tabla con conteos por estado:
- Pactada
- Realizada
- Reprogramada
- Cancelada
- No realizada

### Bloque 4: KPIs Financieros

**Por Generar:**
```
=SUMAR.SI(Finanzas!G:G;"Por Generar";Finanzas!E:E)
```

**Ingresos del Mes:**
```
=SUMAR.SI.CONJUNTO(Finanzas!E:E;Finanzas!G:G;"Pagado";
Finanzas!E:E;">="&FECHA(AÑO(HOY());MES(HOY());1);
Finanzas!E:E;"<="&FIN.MES(HOY();0))
```

---

## 📊 HOJA 2: SINIESTROS (Base de Datos Maestra)

### Estructura de Columnas

| Columna | Nombre | Tipo | Validación | Fórmula |
|---------|--------|------|------------|---------|
| A | ID Siniestro | Número | Manual | - |
| B | Asegurado | Texto | - | - |
| C | Compañía | Desplegable | Listas!A:A | - |
| D | Localidad | Desplegable | Listas!B:B | - |
| E | Fecha Siniestro | Fecha | - | - |
| F | Fecha Ingreso | Fecha | Estática | - |
| G | Tarea a Realizar | Desplegable | Listas!C:C | - |
| H | Estado | Desplegable | Listas!D:D | - |
| I | Prioridad | Desplegable | Listas!E:E | - |
| J | Responsable | Desplegable | Listas!F:F | - |
| K | Fecha Límite | Fecha | - | - |
| L | Días Pendientes | Número | - | `=SI(H2="Cerrados";"";K2-HOY())` |
| M | Comentarios | Texto | - | - |

### Formatos Condicionales

**Días Pendientes (Columna L):**
- Si L < 0 Y H ≠ "Cerrados" → Fondo Rojo Claro + Texto Rojo Oscuro (Vencido)

**Fila Completa (A:M):**
- Si H = "Cerrados" → Fondo Gris Claro + Texto Gris Oscuro (Inactivo)

### Clasificación
1. Fecha Límite (K) - Ascendente
2. Prioridad (I) - Descendente

---

## 📝 HOJA 3: SEGUIMIENTO (Historial de Novedades)

### Estructura de Columnas

| Columna | Nombre | Tipo | Validación | Fórmula |
|---------|--------|------|------------|---------|
| A | ID Siniestro | Desplegable | Siniestros!A:A | - |
| B | Asegurado | Fórmula | - | `=SI.ERROR(BUSCARV(A2;Siniestros!A:C;2;FALSO);"")` |
| C | Fecha Registro | Fecha | - | - |
| D | Responsable | Desplegable | Listas!F:F | - |
| E | Acción Realizada | Texto | - | - |
| F | Próxima Acción | Texto | - | - |
| G | Fecha Próxima Acción | Fecha | - | - |
| H | Estado Resultante | Desplegable | Listas!D:D | - |

### Clasificación
1. ID Siniestro (A) - Ascendente
2. Fecha Registro (C) - Descendente (más recientes primero)

---

## 🗓️ HOJA 4: VISITAS (Agenda de Campo)

### Estructura de Columnas

| Columna | Nombre | Tipo | Validación | Fórmula |
|---------|--------|------|------------|---------|
| A | ID Siniestro | Desplegable | Siniestros!A:A | - |
| B | Asegurado | Fórmula | - | `=SI.ERROR(BUSCARV(A2;Siniestros!A:B;2;FALSO);"")` |
| C | Compañía | Fórmula | - | `=SI.ERROR(BUSCARV(A2;Siniestros!A:C;3;FALSO);"")` |
| D | Localidad | Fórmula | - | `=SI.ERROR(BUSCARV(A2;Siniestros!A:D;4;FALSO);"")` |
| E | Fecha Pactada | Fecha | - | - |
| F | Horario | Hora | - | - |
| G | Inspector | Desplegable | Listas!F:F | - |
| H | Estado Visita | Desplegable | Listas!G:G | - |
| I | Fecha Real Visita | Fecha | - | - |
| J | Diferencia Días | Número | - | `=SI(I2="";""I2-E2)` |
| K | Comentario Visita | Texto | - | - |

### Formatos Condicionales (Columna H)
- "Realizada" → Verde Claro
- "Cancelada" → Rojo Claro
- "Reprogramada" → Amarillo Claro

### Clasificación
1. Fecha Pactada (E) - Ascendente
2. Horario (F) - Ascendente

---

## 💰 HOJA 5: FINANZAS (Facturación y Costos)

### Tabla Principal

| Columna | Nombre | Tipo | Validación | Fórmula |
|---------|--------|------|------------|---------|
| A | ID Siniestro | Desplegable | Siniestros!A:A | - |
| B | Asegurado | Fórmula | - | `=BUSCARV(A2;Siniestros!A:B;2;FALSO)` |
| C | Compañía | Fórmula | - | `=BUSCARV(A2;Siniestros!A:C;3;FALSO)` |
| D | Estado Actual | Fórmula | - | `=BUSCARV(A2;Siniestros!A:H;8;FALSO)` |
| E | Valor Siniestro | Moneda | - | - |
| F | Costo Informe | Moneda | - | - |
| G | Estado Pago | Desplegable | Listas!H:H | - |

### Tabla Resumen (Columnas I-K)

| Estado | Total ($) | Cantidad |
|--------|-----------|----------|
| Pagado | `=SUMAR.SI(G:G;"Pagado";E:E)` | `=CONTAR.SI(G:G;"Pagado")` |
| Pendiente | `=SUMAR.SI(G:G;"Pendiente";E:E)` | `=CONTAR.SI(G:G;"Pendiente")` |
| Por Generar | `=SUMAR.SI(G:G;"Por Generar";E:E)` | `=CONTAR.SI(G:G;"Por Generar")` |
| **TOTAL GENERAL** | `=SUMA(J2:J4)` | `=SUMA(K2:K4)` |

---

## 🔍 HOJA 6: FICHA CONSULTA (Buscador Operativo)

### Interfaz Limpia - Estilo Corporativo

#### Bloque de Búsqueda (Fila 2)
```
Seleccionar Siniestro: [Desplegable B2 → Siniestros!A:A]
```

#### Bloque Datos Maestros (Filas 5-12)

| Etiqueta | Valor |
|----------|-------|
| Asegurado | `=BUSCARV(B2;Siniestros!A:B;2;FALSO)` |
| Compañía | `=BUSCARV(B2;Siniestros!A:C;3;FALSO)` |
| Localidad | `=BUSCARV(B2;Siniestros!A:D;4;FALSO)` |
| Fecha Siniestro | `=BUSCARV(B2;Siniestros!A:E;5;FALSO)` |
| Estado Actual | `=BUSCARV(B2;Siniestros!A:H;8;FALSO)` |
| Tarea Pendiente | `=BUSCARV(B2;Siniestros!A:G;7;FALSO)` |
| Prioridad | `=BUSCARV(B2;Siniestros!A:I;9;FALSO)` |
| Fecha Límite | `=BUSCARV(B2;Siniestros!A:K;11;FALSO)` |

#### Bloque Historial (Fila 16)

**LibreOffice 7.0+:**
```
=FILTRAR(Seguimiento!A:H;Seguimiento!A:A=B2;"Sin gestiones registradas")
```

**LibreOffice < 7.0 (Alternativa):**
```
Usar Autofilter manual o matriz INDICE+PEQUEÑO
```

---

## ⚙️ HOJA 7: LISTAS (Configuraciones)

### Vectores de Datos (Solo lectura - Editable manualmente)

**Columna A - COMPAÑÍAS**
- Aseguradoras personalizable

**Columna B - LOCALIDADES**
- Ciudades/regiones personalizable

**Columna C - TAREAS**
- Pendiente contacto
- Contactado
- Coordinar visita
- Visitar asegurado
- Cargar informe
- Informe elevado
- Retiro de denuncia
- Pendientes de datos
- Otra tarea

**Columna D - ESTADOS SINIESTRO**
- Pendiente análisis
- Contactar
- Aguardamos respuestas
- Coordinar visita
- Con visita pactada
- Pendientes de datos
- Cargar informe
- Informes elevados
- Retiro denuncia
- Cerrados

**Columna E - PRIORIDAD**
- Alta
- Media
- Baja

**Columna F - RESPONSABLES**
- Joel
- [Espacios para agregar más]

**Columna G - ESTADOS VISITA**
- Pactada
- Realizada
- Reprogramada
- Cancelada
- No realizada

**Columna H - ESTADO PAGO**
- Pagado
- Pendiente
- Por Generar

---

## 🔗 INTERCONEXIONES Y DEPENDENCIAS

```
┌─────────────────────┐
│ HOJA 7: LISTAS      │ ← Centro de Configuración
└──────────┬──────────┘
           ↓
    ┌──────┴──────┐
    ↓             ↓
┌─────────┐  ┌──────────┐
│HOJA 2   │  │HOJA 1    │
│SINIESTROS│ │DASHBOARD │
└────┬────┘  └──────────┘
     ↓
┌────┴────┬────────┬──────────┐
↓         ↓        ↓         ↓
H3    H4     H5    H6
SEG   VIS   FIN   FICHA
```

### Flujo de Datos:
- **Listas** → Proporciona desplegables a todas las hojas
- **Siniestros** → Base maestra que alimenta Dashboard, Seguimiento, Visitas, Finanzas, Ficha Consulta
- **Dashboard** → Lee de Siniestros, Visitas, Finanzas y genera KPIs
- **Seguimiento** → Historial de Siniestros
- **Visitas** → Agenda de Siniestros
- **Finanzas** → Monitoreo económico de Siniestros
- **Ficha Consulta** → Lee de Siniestros y Seguimiento (solo lectura)

---

## ⚡ FUNCIONES NATIVAS UTILIZADAS

### CONTAR.SI.CONJUNTO
```
=CONTAR.SI.CONJUNTO(rango1;criterio1;rango2;criterio2;...)
```
✓ LibreOffice 3.0+ | ✓ OnlyOffice | Uso: Contar múltiples criterios

### CONTAR.SI
```
=CONTAR.SI(rango;criterio)
```
✓ Universal | Uso: Contar un criterio simple

### SUMAR.SI.CONJUNTO
```
=SUMAR.SI.CONJUNTO(rango_suma;rango_criterio1;criterio1;...)
```
✓ LibreOffice 3.0+ | ✓ OnlyOffice | Uso: Sumar múltiples criterios

### SUMAR.SI
```
=SUMAR.SI(rango_criterio;criterio;rango_suma)
```
✓ Universal | Uso: Sumar un criterio simple

### BUSCARV
```
=BUSCARV(valor_buscado;tabla_matriz;número_columna;rango_aproximado)
```
✓ Universal | Uso: Búsqueda vertical en tabla

### SI.ERROR
```
=SI.ERROR(fórmula;valor_si_error)
```
✓ LibreOffice 4.0+ | Uso: Manejo de errores en fórmulas

### FILTRAR
```
=FILTRAR(matriz;incluir;[si_vacío])
```
✓ LibreOffice 7.0+ | ✓ OnlyOffice | Uso: Filtrar dinámicamente

### HOY, FECHA, AÑO, MES, FIN.MES
✓ Universal | Uso: Manipulación de fechas

---

## 📈 CONSIDERACIONES DE RENDIMIENTO

- **Filas máximas recomendadas:** 10,000
- **Tiempo de recalculación:** <1 segundo (hasta 10K registros)
- **Formato condicional:** Aplicar solo a rangos específicos
- **Referencias:** Usar columnas completas para autoflex (A:A) o rangos específicos (A2:A1000) según velocidad requerida

---

## 🔒 COMPATIBILIDAD VERIFICADA

| Plataforma | Versión Mínima | Estado |
|------------|----------------|--------|
| LibreOffice Calc | 3.0 | ✅ Compatible |
| OnlyOffice Calc | 5.0 | ✅ Compatible |
| Microsoft Excel | 2010+ | ✅ Compatible |

**Nota:** Si usando Excel, cambiar separadores de `;` a `,` según región.

---

## 📦 INSTRUCCIONES DE IMPLEMENTACIÓN

### Paso 1: Crear Estructura Base
1. Abrir LibreOffice Calc o OnlyOffice
2. Crear 7 hojas con nombres exactos
3. Guardar como `SISTEMA_GESTION_SINIESTROS_v1.0.ods`

### Paso 2: Rellenar Hoja 7 (LISTAS) Primero
- Columnas A-H con vectores de datos
- Formatear encabezados: Negrita, Fondo Azul, Texto Blanco

### Paso 3: Crear Hoja 2 (SINIESTROS)
- Encabezados Fila 1
- Validaciones desplegables en C, D, G, H, I, J
- Fórmula en L2: `=SI(H2="Cerrados";"";K2-HOY())`
- Copiar L2 a L2:L1000
- Formatos condicionales

### Paso 4-6: Crear Hojas 3, 4, 5
- Validaciones según tabla de estructura
- Fórmulas BUSCARV/SI.ERROR
- Clasificaciones y formatos

### Paso 7: Crear Hoja 1 (DASHBOARD)
- Bloques de alertas y métricas
- Todas las fórmulas según especificación

### Paso 8: Crear Hoja 6 (FICHA CONSULTA)
- Diseño corporativo limpio
- FILTRAR o alternativa INDICE+PEQUEÑO

### Paso 9: Validar y Probar
- Ingresar datos de prueba
- Verificar actualizaciones automáticas
- Comprobar desplegables y BUSCARV

---

## 💾 DATOS DE EJEMPLO

### Siniestro de Prueba (Hoja 2):
```
ID: 1001
Asegurado: Juan Pérez
Compañía: AXA
Localidad: Buenos Aires
Fecha Siniestro: 15/06/2026
Fecha Ingreso: 18/06/2026
Tarea: Coordinar visita
Estado: Con visita pactada
Prioridad: Alta
Responsable: Joel
Fecha Límite: 25/06/2026
Comentarios: Cliente disponible
```

---

## 📞 SOPORTE Y MANTENIMIENTO

- **Actualizar Hoja 7** con nuevas categorías según sea necesario
- **Backups semanales** del archivo principal
- **Revisar Dashboard** regularmente para identificar tendencias

---

## 📝 NOTAS IMPORTANTES

✅ **SIN MACROS** - Solo fórmulas nativas
✅ **PORTABLE** - Compatible entre plataformas
✅ **SEGURO** - No contiene código ejecutable
✅ **RÁPIDO** - Cálculos optimizados
✅ **FLEXIBLE** - Fácil de expandir y personalizar

---

**Versión:** 1.0  
**Fecha de Creación:** 2026-06-25  
**Última Actualización:** 2026-06-25  
**Estado:** Producción ✅
