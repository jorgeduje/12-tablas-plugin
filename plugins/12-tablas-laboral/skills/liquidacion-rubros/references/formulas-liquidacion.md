# Fórmulas de liquidación — despido (parte trabajadora, LCT 20.744)

> Referencia técnica para armar la planilla Excel. Cada rubro va como **fórmula en celda**, referenciando los inputs. Donde hay criterio (tope/Vizzoti, qué multas, tasa), manda el `perfil_estudio.md`. Todo es borrador a validar por el abogado. La aplicabilidad concreta y la vigencia de doctrina/jurisprudencia las verifica el abogado.

## Inputs base (hoja "Datos")

- `fecha_ingreso`, `fecha_despido`
- `MRMNyH` = mejor remuneración mensual, normal y habitual del último año (base art. 245)
- `rem_mensual` = remuneración mensual de referencia para preaviso/integración/SAC/vacaciones (normalmente = MRMNyH, salvo criterio del estudio)
- `tope_CCT` = base máxima del art. 245 según el CCT aplicable (tope del convenio)
- `preaviso_otorgado` (sí/no), `dia_despido` (día del mes)
- `mejor_rem_semestre` (para SAC), `dias_vacaciones_pendientes`, `haberes_mes_adeudados`
- Flags del perfil: `usa_24013` (sí/no → si no, reparación integral), `aplica_vizzoti` (sí/no), `tasa_interes`, `fecha_calculo`

## Antigüedad computable

- `anios = años completos entre fecha_ingreso y fecha_despido`
- Si la **fracción** restante es **mayor a 3 meses**, suma **1 año** (art. 245).
- `anios_computables = anios + (1 si fracción > 3 meses, si no 0)`

## 1) Indemnización por antigüedad (art. 245)

Base con tope y corrección *Vizzoti* (CSJN, "Vizzoti", 2004: el tope no puede reducir la base por debajo del 67% de la MRMNyH):

```
base_topeada   = MIN(MRMNyH, tope_CCT)
base_vizzoti   = MAX(base_topeada, 0,67 * MRMNyH)     // solo si aplica_vizzoti
base_245       = base_vizzoti (si aplica_vizzoti) ; si no, base_topeada
indem_245      = base_245 * anios_computables
```

Piso legal (art. 245, último párrafo): `indem_245 >= 1 * base_245` (nunca menor a un mes de la base).

## 2) Preaviso omitido (art. 232) + SAC

Plazo de preaviso según antigüedad (art. 231):

```
plazo_preaviso =  0,5 mes  si antigüedad <= 3 meses (período de prueba)
                  1   mes  si antigüedad > 3 meses y <= 5 años
                  2   meses si antigüedad > 5 años
```

```
indem_preaviso = rem_mensual * plazo_preaviso           // solo si preaviso_otorgado = no
sac_preaviso   = indem_preaviso / 12
```

## 3) Integración del mes de despido (art. 233) + SAC

Si el despido NO se produce el último día del mes:

```
dias_mes        = días del mes del despido
dias_a_integrar = dias_mes - dia_despido
integracion     = (rem_mensual / dias_mes) * dias_a_integrar
sac_integracion = integracion / 12
```

## 4) SAC proporcional (art. 123)

```
dias_semestre_trab = días trabajados en el semestre de la extinción
sac_proporcional   = (mejor_rem_semestre / 2) * (dias_semestre_trab / dias_del_semestre)
```

(Equivalente usual: `mejor_rem_semestre/12 * meses_trabajados_del_semestre`.)

## 5) Vacaciones no gozadas proporcionales (art. 156)

Días de vacaciones según antigüedad (art. 150):

```
dias_vac_anuales = 14 si antigüedad <= 5 años
                   21 si > 5 y <= 10
                   28 si > 10 y <= 20
                   35 si > 20
dias_vac_prop = dias_vac_anuales * (días trabajados en el año / 365)
valor_dia_vac = rem_mensual / 25
indem_vacaciones = dias_vac_prop * valor_dia_vac
```

(Sumar SAC sobre vacaciones si el estudio lo liquida: `indem_vacaciones / 12`.)

## 6) Haberes adeudados del mes / días trabajados

`haberes_mes_adeudados` = días trabajados del mes de despido no abonados, según recibos.

## 7) Multas / agravantes — RÉGIMEN VIGENTE (Ley 27.742 y 27.802)

**Para extinciones desde el 9/7/2024 están DEROGADAS** (Ley 27.742, régimen mantenido por la Ley 27.802) y **no se cargan como rubro directo**:

- **Ley 24.013** (arts. 8/9/10 registración + art. 15 duplicación) — derogadas.
- **Ley 25.323 art. 1** (duplica `indem_245` por relación no/deficientemente registrada) — derogado.
- **Ley 25.323 art. 2** (incremento 50% por obligar a litigar) — derogado.
- **Art. 80 LCT** (multa de `3 * MRMNyH` por no entregar certificados) — derogada (la Ley 27.802 digitaliza el certificado vía ARCA; la **obligación de entrega subsiste** y el juez puede ordenarla y fijar compensación).

**Qué se reclama en su lugar por la registración deficiente / trabajo en negro:**

- **Reparación integral** del daño (CCyCN) — rubro **no tarifado**, el monto lo fija el abogado. Es la vía de los estudios de la parte trabajadora.
- **Planteo de inconstitucionalidad** de las derogaciones (art. 14 bis CN, progresividad) → habilita reclamar las multas en **subsidio**.
- **Remisión a ARCA** (art. 278 LCT): el juez remite los antecedentes para la liquidación de aportes/contribuciones + multas administrativas (vía administrativa, no es un rubro de la liquidación del trabajador).
- **Art. 132 bis LCT** — sanción por aportes retenidos y no ingresados (si aplica; verificar vigencia).

> Por defecto, los interruptores de multas van **apagados**. Sólo se activan si el perfil del estudio lo indica o si se sostiene el planteo de inconstitucionalidad (y ahí van como rubro **subsidiario**, no sumados al capital directo).

## 8) Diferencias salariales (semiautomático)

Requiere la **escala del CCT vigente a la fecha del despido** (la aporta el estudio). Hoja aparte:

```
para cada mes del período reclamado (hasta 2 años):
   dif_mes = remuneración_según_escala_CCT(mes)  -  remuneración_percibida(mes)
diferencias = SUMA(dif_mes)  +  incidencia SAC y aportes según criterio del estudio
```

**Nunca** uses el salario del telegrama como base: siempre la escala del CCT vigente a la fecha del despido. Si no tenés la escala, no estimes: pedila.

## 9) Total e intereses

```
subtotal = suma de los rubros aplicables
actualizacion = subtotal * tasa * (días desde la mora hasta fecha_calculo / 365)   // modo "actualizada"
TOTAL = subtotal + actualizacion
```

**Actualización (Ley 27.802, nuevo art. 276 LCT):** para **causas nuevas**, los créditos se actualizan por **IPC-INDEC + 3% anual** (tasa pura) desde la mora — reemplaza la tasa activa BNA / actas CNAT. Para **juicios en trámite**, rige el **régimen transitorio del art. 55** (tasa pasiva BCRA, con techo IPC+3% y piso del 67% de la fórmula plena). El criterio y la fecha salen del perfil. En modo "definitiva para demanda" puede ir el capital histórico; en "actualizada", con la actualización a la fecha de la audiencia.
