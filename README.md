# trazabilidad-documental

Portales internos desarrollados para digitalizar el ciclo completo de
custodia de documentos físicos en una: desde la recepción hasta el 
resguardo en bóveda, con trazabilidad por registro.

Antes, el proceso se controlaba en hojas de cálculo compartidas: sin
control de accesos, sin historial de quién hizo qué, y con la ubicación
física de cada documento dependiendo de multiples bases.

**Stack:** Google Apps Script · JavaScript · HTML/CSS · Google Sheets

> Este repositorio contiene únicamente documentación del diseño y las
> decisiones técnicas. El código y los datos son propiedad de la
> institución y no se publicaran.

---

## 1. Portal de Recepción y Asignación

Punto de entrada del proceso: registro de guías recibidas, identificación
de su contenido y asignación de carga a los analistas.

**Qué resuelve**
- Registro por origen y control de guías pendientes de identificar.
- Asignación de trabajo con seguimiento de avance por analista.
- Control de devoluciones a sucursal con motivo y destino.

**Decisiones técnicas**
- Estados explícitos por registro para saber en qué punto del flujo está
  cada documento en todo momento.
- Bitácora de auditoría de cada acción, con usuario y fecha.

---

## 2. Portal de Validación y Registro

Captura y control de los expedientes que se integran a cada caja antes de
pasar a resguardo.

**Qué resuelve**
- Carga masiva desde reporte y filtrado automático de los registros que
  corresponden a cada analista.
- Detección de duplicados contra la base histórica antes de registrar.
- Agrupación de expedientes que comparten folder físico, para que se
  ordenen juntos.

**Decisiones técnicas**
- Normalización de identificadores (formatos y ceros iniciales) para que
  el cruce entre fuentes no genere falsos negativos.
- Bloqueo de edición sobre registros ya procesados anteriormente.

---

## 3. Portal de Revisión y Resguardo

Verificación física del contenido de cada caja contra lo capturado, y
asignación de su ubicación en bóveda.

**Qué resuelve**
- Cotejo expediente por expediente entre lo registrado y lo que está
  físicamente en la caja.
- Registro de faltantes y sobrantes con su motivo, que regresan al área
  anterior como aclaración.
- Asignación de coordenada física y generación del listado impreso que
  acompaña la caja.

**Decisiones técnicas**
- Control de concurrencia: varios operadores trabajan sobre la misma base
  al mismo tiempo, con bloqueos para evitar escrituras cruzadas.
- Persistencia local del avance: si se cierra el navegador a media
  revisión, el trabajo no se pierde.
- Una misma columna es escrita por dos áreas distintas, así que las notas
  se fusionan en lugar de sobrescribirse.

---

## 4. Dashboard de Indicadores

Consolidación de los tres portales en una vista única de productividad y
tiempos de proceso.

**Qué resuelve**
- Volumen procesado, tiempos de ciclo y carga por analista.
- Vistas separadas por área, con selector de mes.

**Decisiones técnicas**
- Los meses cerrados se calculan una sola vez y se guardan digeridos;
  solo el mes en curso se recalcula en vivo. Sin esto, cada consulta
  recorría el histórico completo.
- Regeneración automática mensual mediante trigger programado.

---

## Pruebas

- Diseño y ejecución de casos de prueba sobre los flujos completos.
- Detección, documentación y corrección de defectos en producción,
  con seguimiento hasta su cierre.
- Validación de casos límite: concurrencia entre usuarios, registros
  incompletos y estados intermedios del flujo.


  ---

*Los sistemas están en operación.*
