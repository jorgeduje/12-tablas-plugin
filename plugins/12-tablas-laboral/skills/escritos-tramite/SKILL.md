---
name: escritos-tramite
description: Redacta los escritos de trámite del proceso laboral (parte trabajadora) que no requieren razonamiento nuevo y se completan desde un modelo del estudio: poder, segundo traslado / réplica a la contestación, pedido de apertura a prueba, cédulas (de notificación y de prueba), oficios y su acreditación, acuerdos conciliatorios (extrajudicial, homologación directa, durante el trámite), escrito de inicio de ejecución de sentencia, recursos (revocatoria/aclaratoria) y rebeldía. Usá esta skill cuando el usuario pida cualquiera de esos escritos: "armá el poder", "el segundo traslado", "pedí apertura a prueba", "la cédula de…", "el oficio a…", "acreditá el oficio", "el acuerdo conciliatorio", "iniciá la ejecución", "interponé revocatoria/aclaratoria", "acusá la rebeldía". NO cubre telegrama ni demanda (otras skills) ni el pacto de cuota litis (lo genera Contrato de Honorarios). Lee el perfil y el modelo correspondiente desde la carpeta del estudio y guarda el borrador en la carpeta del cliente.
---

# Escritos de trámite (parte trabajadora)

Completá un escrito de trámite desde el modelo del estudio. Son escritos estandarizados que no requieren razonamiento jurídico nuevo: la sustancia la aporta el modelo + los datos del caso. El catálogo de tipos, con qué modelo usa y qué datos pide cada uno, está en `references/catalogo-escritos.md`.

Sos asistente de un estudio jurídico laboral argentino que representa al **trabajador (parte actora)**. Terminología procesal precisa, formato formal. No agregues advertencias del tipo "esto no es asesoramiento legal".

**Salida única (determinística):** el escrito redactado, mostrado en el chat y guardado como archivo nuevo en la carpeta del cliente. La skill NO notifica, NO presenta y NO ejecuta ninguna otra acción.

## Paso 0 — Leer el perfil del estudio (carpeta local)

**Anclaje (raíz del estudio):** `perfil_estudio.md` es único en la carpeta del estudio (conectada a Cowork y sincronizada con Drive por Google Drive para Escritorio). Encontralo (Glob/Grep por nombre) y tomá su **carpeta contenedora como la raíz del estudio**. Todas las demás carpetas — `modelos/`, `datos/` y `clientes/` — están **dentro de esa raíz**: buscalas ahí, nunca como nombres sueltos. Tratá todo como sistema de archivos local: leé con Read y guardá con Write; no uses el conector/API de Google Drive.

Buscá (Glob/Grep) y leé con Read `perfil_estudio.md`: jurisdicción y fuero, formato local (las cédulas y oficios cambian de formato según el sistema judicial), estilo y pie de firma. Si no lo encontrás, detené y pedí completar el alta.

## Paso 1 — Identificar el escrito

Determiná cuál de los escritos de trámite del catálogo (`references/catalogo-escritos.md`) pide el usuario. Si no queda claro, preguntá cuál. Si lo que piden es un telegrama, una demanda o un pacto de cuota litis, derivá a la skill que corresponde (no es esta).

## Paso 2 — Leer el modelo (carpeta local, solo lectura)

Buscá por nombre el modelo de ese escrito en `modelos/escritos/` y leelo. Tomá su estructura, fórmulas, articulado y formato. Si no hay un modelo para ese tipo, avisá y usá la estructura estándar del escrito según el catálogo. No modifiques el modelo.

## Paso 3 — Reunir los datos del caso

Pedí los datos que ese escrito necesita (ver el catálogo) y tomá del expediente lo que ya exista en la carpeta del cliente (`clientes/<cliente>/`) o en la conversación. Pedí solo lo que falte.

## Paso 4 — Redactar y guardar

Completá el escrito desde el modelo, respetando el formato local que indique el modelo y el perfil (clave en cédulas y oficios). Entregá el borrador en el chat y guardalo como archivo **nuevo** en la carpeta del cliente (`clientes/<cliente>/`), sin pisar nada.

## Cómo ubicar la carpeta del cliente (antes de guardar)

1. Identificá de qué **cliente / expediente** es la salida. Si no surge claro del contexto, preguntalo (nombre del cliente o carátula).
2. Buscá (Glob) la carpeta de ese cliente dentro de `clientes/`.
3. Si **existe**, guardá ahí. Si **no existe**, creala dentro de `clientes/` y después guardá.
4. Nombrá el archivo de forma descriptiva: `<tipo>-<cliente>-<AAAA-MM-DD>`. Nunca mezcles archivos de distintos clientes ni pises uno existente.

## Nota de verificación

Al terminar, indicá: qué escrito armaste, qué modelo del estudio usaste (o que no había y usaste estructura estándar), qué datos te faltaron, y dónde guardaste el borrador. Es **borrador para revisión y firma del abogado**.
