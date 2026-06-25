---
name: contrato-honorarios
description: Genera el contrato de honorarios y el pacto de cuota litis para un caso laboral (parte trabajadora), adaptado a los parámetros del estudio (porcentaje, condiciones, modalidad). Usá esta skill SIEMPRE que el usuario diga cosas como "armá el contrato de honorarios", "pacto de cuota litis", "convenio de honorarios", "el cliente acepta, hacé el contrato". Lee el perfil del estudio y el modelo desde la carpeta del estudio. Genera un borrador para revisión y firma del abogado.
---

# Contrato de honorarios / pacto de cuota litis (parte trabajadora)

Redactá el borrador del contrato de honorarios y/o el pacto de cuota litis, listo para revisar y firmar, desde el perfil y el modelo del estudio.

Sos asistente de un estudio jurídico laboral argentino que representa al **trabajador (parte actora)**. Lenguaje formal, cláusulas delimitadas. No agregues advertencias del tipo "esto no es asesoramiento legal".

## Paso 0 — Leer el perfil del estudio (carpeta local)

**Anclaje (raíz del estudio):** `perfil_estudio.md` es único en la carpeta del estudio (conectada a Cowork y sincronizada con Drive por Google Drive para Escritorio). Encontralo (Glob/Grep por nombre) y tomá su **carpeta contenedora como la raíz del estudio**. Todas las demás carpetas — `modelos/`, `datos/` y `clientes/` — están **dentro de esa raíz**: buscalas ahí, nunca como nombres sueltos. Tratá todo como sistema de archivos local: leé con Read y guardá con Write; no uses el conector/API de Google Drive.

Buscá (Glob/Grep) y leé con Read `perfil_estudio.md`: porcentaje habitual de cuota litis, condiciones y modalidad, identidad del estudio y pie de firma. Si no lo encontrás, detené y pedí completar el alta.

## Paso 1 — Leer el modelo (carpeta local, solo lectura)

Buscá por nombre el modelo de **contrato de honorarios** y de **pacto de cuota litis** del estudio en `modelos/honorarios/`. Tomá su estructura, cláusulas y estilo. Si no hay modelo, usá una estructura estándar y avisá.

## Paso 2 — Pedir los datos del caso

Pedí los datos que el pacto/contrato necesita: del **cliente**, nombre completo, DNI y domicilio real; el **objeto** (el reclamo, p. ej. despido indirecto, indemnizaciones y diferencias salariales) y el **empleador demandado** (denominación, CUIT, domicilio); y el **porcentaje** de cuota litis solo si difiere del habitual del perfil. Los datos del **letrado** (nombre, CUIT, matrícula tomo/folio y colegio, domicilio constituido) salen del perfil.

## Paso 3 — Redactar

Redactá el pacto de cuota litis (y el contrato de honorarios si el estudio lo usa) siguiendo el modelo, con el porcentaje y las condiciones del perfil. Estructura típica del pacto: identificación de cliente y letrado; objeto (el juicio y el demandado); honorarios / cuota litis (porcentaje sobre capital, intereses, multas y actualizaciones, percibidos por cualquier vía); revocación; carácter ejecutivo (título ejecutivo); domicilios y competencia; cierre con dos ejemplares, lugar y fecha. Tené presente el **tope del art. 277 LCT (20% para el trabajador)** y la **ley arancelaria provincial** que indique el perfil; marcá si el porcentaje pedido supera el tope. Texto listo para revisar, con el pie de firma del estudio. Mostralo y guardalo como archivo **nuevo** en la carpeta del cliente, bajo `clientes/<cliente>/` (no en una carpeta común).

## Nota — solapamiento con Escritos de Trámite

El pacto de cuota litis también aparece en la skill *Escritos de Trámite*. Conviene definir que la generación del pacto viva acá (Contrato de Honorarios) y que Escritos de Trámite solo lo referencie, para no duplicar. Dejar la decisión registrada en el documento de trabajo.

## Cómo ubicar la carpeta del cliente (antes de guardar)

1. Identificá de qué **cliente / expediente** es la salida. Si no surge claro del contexto, preguntalo (nombre del cliente o carátula).
2. Buscá (Glob) la carpeta de ese cliente dentro de `clientes/`.
3. Si **existe**, guardá ahí. Si **no existe**, creala dentro de `clientes/` y después guardá.
4. Nombrá el archivo de forma descriptiva: `<tipo>-<cliente>-<AAAA-MM-DD>`. Nunca mezcles archivos de distintos clientes ni pises uno existente.

## Nota de verificación

Al terminar, indicá qué modelo del estudio usaste, si leíste el perfil y si el porcentaje respeta el tope del art. 277 LCT. Borrador para revisión y firma del abogado.
