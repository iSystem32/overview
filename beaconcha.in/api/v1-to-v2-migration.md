> ## Índice de documentación
> Obtenga el índice completo de la documentación en: https://docs.beaconcha.in/llms.txt
> Utilice este archivo para descubrir todas las páginas disponibles antes de seguir explorando.

# Migración de la API V1 a la V2

Guía paso a paso para migrar su aplicación de la API V1 de beaconcha.in a la API V2, con asignaciones de puntos finales y ejemplos de código.

## ¿Por qué migrar?

<Información>
  Todas las nuevas funciones, puntos de acceso y mejoras se han desarrollado exclusivamente para la versión 2. Recomendamos migrar a la versión 2 para obtener la mejor experiencia.
</Info>

* **Datos más completos:** Los puntos finales V2 devuelven respuestas más detalladas y estructuradas con desgloses de recompensas muy precisos.
* **Selectores flexibles:** Consulta por índice de validador, clave pública, dirección de depósito, dirección de retiro, entidad o panel de control
* **Casos de uso y guías:** Más de 14 guías paso a paso con ejemplos de código para flujos de trabajo comunes.
* **BeaconScore y entidades:** Funcionalidades exclusivas de la versión 2 para la evaluación comparativa del rendimiento y la comparación de entidades.
* **Desarrollo activo:** Todas las nuevas funciones se incluyen únicamente en la versión 2.

***

## ¿Necesitas ayuda?

<CardGroup cols={2}>
  <Card title="Casos de uso" icon="book-open" href="/use-cases/introduction">
    Guías específicas para cada tarea, con ejemplos de código para operaciones comunes.
  </Tarjeta>

  <Card title="Error Handling" icon="triangle-exclamation" href="/api/error-handling">
    Patrones de error, códigos de estado y solución de problemas de la versión 2.
  </Tarjeta>

  <Título de la tarjeta="Integración de IA" icono="microchip-ai" href="/api/ai-integration">
    Conecta tu asistente de IA a la documentación de beaconcha.in mediante MCP o llms.txt.
  </Tarjeta>

  <Título de la tarjeta="Discordia" icono="discordia" href="https://dsc.gg/beaconchain">
    Haz preguntas y obtén ayuda de la comunidad.
  </Tarjeta>
</CardGroup>

***

## Diferencias clave

| Aspecto | API V1 | API V2 |
| ------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Método HTTP | Mayormente `GET` | Todos `POST` |
| Autenticación | Parámetro de consulta (`?apikey=`) o encabezado (`apikey: ...`) | Token Bearer (`Authorization: Bearer <WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0>`) |
| Paginación | Basada en desplazamiento (`offset` y `limit`) | Basada en cursor (`cursor` y `page_size`) |
| Selección del validador | Parámetro de ruta (índice único o clave pública) | Selector del cuerpo JSON (índices de lote, claves públicas, dirección de depósito, dirección de retiro, entidad, panel o ID de grupo del panel) |
| Sobre de respuesta | `{ "estado": "OK", "datos": ... }` | `{ "datos": ..., "paginación": ... }` |
| Selección de cadena | Implícita (URL del servidor) | Campo explícito `"chain"` en el cuerpo de la solicitud (`"mainnet"` o `"hoodi"`) |

***

## Mapeo de puntos finales

### Estado de la red

| V1 | V2 | Notas |
| ---------------------------------------------------------------------------------------- | ----------------------------- | ---------------------------------------------------------- |
| [`GET /api/v1/latestState`](/api-reference/network/latest-consensus-and-execution-state) | `POST /api/v2/ethereum/state` | API pendiente: el equipo de beaconcha.in está trabajando en esta API. |

### Validadores

| V1 | V2 | Notas |
| ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [`GET /api/v1/validator/{1955a5598add1e94c693a8f1b609799f12a849637ff652e732020da6be01d9f1}`](/api-reference/validators/validators-overview) | [`POST /api/v2/ethereum/validators`](/api-reference/ethereum/validators) | V2 admite búsquedas por lotes en una sola solicitud |
| [`POST /api/v1/validator`](/api-reference/validators/validators-overview-post) | [`POST /api/v2/ethereum/validators`](/api-reference/ethereum/validators) | V2 maneja el procesamiento por lotes de forma nativa a través del array `validator_identifiers` |
| [`GET .../validator/eth1/{0x988378D27D6060bCEe24429425721d64BEe984A8}`](/api-reference/validators/validators-by-deposit-0x988378D27D6060bCEe24429425721d64BEe984A8) | [`POST /api/v2/ethereum/validators`](/api-reference/ethereum/validators) | Utilice el selector `0x988378D27D6060bCEe24429425721d64BEe984A8` en el cuerpo de la solicitud. |
| [`GET .../validator/withdrawalCredentials/{0x988378D27D6060bCEe24429425721d64BEe984A8}`](/api-reference/validators/validators-by-withdrawal-credentials-or-0x988378D27D6060bCEe24429425721d64BEe984A8) | [`POST /api/v2/ethereum/validators`](/api-reference/ethereum/validators) | Utilice el selector `withdrawal` en el cuerpo de la solicitud. |
| [`GET .../balancehistory`](/api-reference/validators/validator-balance-history) | [`POST .../validators/balances`](/api-reference/ethereum/validators/balances) | |
| [`GET .../performance`](/api-reference/rewards/consensus-layer-rewards) | [`POST .../validators/performance-aggregate`](/api-reference/ethereum/validators/performance-aggregate) | V1 devuelve totales de ventana móvil (1d/7d/31d/365d). V2 `performance-aggregate` es la coincidencia más cercana. Para granularidad por época, use [`performance-list`](/api-reference/ethereum/validators/performance-list) |
| [`GET .../attestations`](/api-reference/validators/validator-attestations-history) | [`POST .../validators/attestation-slots`](/api-reference/ethereum/validators/attestation-slots) | |
| [`GET .../proposals`](/api-reference/validators/validator-proposed-blocks) | [`POST .../validators/proposal-slots`](/api-reference/ethereum/validators/proposal-slots) | |
| [`GET .../attestationefficiency`](/api-reference/validators/attestation-efficiency) | [`POST .../validators/performance-aggregate`](/api-reference/ethereum/validators/performance-aggregate) | BeaconScore en V2 reemplaza la métrica de eficiencia de atestación V1 |
| [`GET .../deposits`](/api-reference/validators/validator-deposits-execution-layer) | `POST .../validators/deposit-slots` | API pendiente: el equipo de beaconcha.in está trabajando en esta API. |
| [`GET .../withdrawals`](/api-reference/validators/validator-withdrawal-history) | `POST .../validators/withdrawal-slots` | API pendiente: el equipo de beaconcha.in está trabajando en esta API. |

### Recompensas

| V1 | V2 | Notas |
| ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| [`GET .../incomedetailhistory`](/api-reference/validators/validator-income-detail-history) | [`POST .../validators/rewards-list`](/api-reference/ethereum/validators/rewards-list) | |
| [`GET .../execution/performance`](/api-reference/rewards/execution-layer-rewards) | [`POST .../validators/rewards-aggregate`](/api-reference/ethereum/validators/rewards-aggregate) | V1 es solo para la capa de ejecución. V2 incluye recompensas tanto de la capa de consenso como de la capa de ejecución |
| [`GET .../execution/block/{blockNumber}`](/api-reference/blocks/execution-block-overview) | [`POST .../block/rewards`](/api-reference/ethereum/block/rewards) | Utilice este endpoint para obtener detalles sobre las recompensas de los bloques de la capa de ejecución. |

### Colas y Comités de Sincronización

| V1 | V2 | Notas |
| --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`GET /api/v1/validators/queue`](/api-reference/validators/validator-activation-and-exit-queue) | [`POST /api/v2/ethereum/queues`](/api-reference/ethereum/queues) | Datos de la cola en toda la red. Para la posición de la cola por validador, consulte también `validators/queues` `Pending`. El equipo de beaconcha.in está trabajando en este endpoint. |
| [`GET /api/v1/sync_committee/{period}`](/api-reference/sync-committees/sync-committee-for-a-period) | [`POST /api/v2/ethereum/sync-committee`](/api-reference/ethereum/sync-committee/overview) | |

### Tragamonedas

| V1 | V2 | Notas |
| ---------------------------------------------------------------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------- |
| [`GET /api/v1/slot/{slotOrHash}`](/api-reference/slots/slot-overview) | [`POST /api/v2/ethereum/slot`](/api-reference/ethereum/slot/overview) | |
| [`GET .../slot/{slot}/attestations`](/api-reference/slots/attestations-for-a-slot) | `POST .../slot/attestation-duties` | API pendiente: el equipo de beaconcha.in está trabajando en esta API. |
| [`GET .../slot/{slot}/deposits`](/api-reference/slots/deposits-for-a-slot) | `POST .../slot/deposits` | API pendiente: el equipo de beaconcha.in está trabajando en esta API. |
| [`GET .../slot/{slot}/withdrawals`](/api-reference/slots/withdrawals-for-a-slot) | `POST .../slot/withdrawals` | API pendiente: el equipo de beaconcha.in está trabajando en esta API. |

### Puntos finales solo para la versión 1 (aún no existe un equivalente para la versión 2)

Los siguientes puntos finales V1 no tienen equivalentes V2 por el momento. Siguen siendo accesibles a través de la API V1.

| Categoría | V1 | Notas |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------- |
| Validadores | [`GET .../validator/{indexOrPubkey}/blsChange`](/api-reference/validators/bls-to-execution-credential-changes) | Cambios de credenciales de BLS a ejecución |
| Validadores | [`GET .../validator/stats/{index}`](/api-reference/validators/daily-validator-statistics) | Estadísticas diarias de los validadores |
| Validadores | [`GET .../validator/leaderboard`](/api-reference/rewards/validator-performance-leaderboard) | Clasificación de rendimiento |
| Validadores | [`GET .../validators/proposalLuck`](/api-reference/validators/proposal-luck) | Métricas de suerte de la propuesta |
| Validadores | [`GET .../eth1deposit/{txhash}`](/api-reference/validator-deposits/deposits-by-execution-transaction-hash) | Depósitos por hash de transacción de ejecución |
| Épocas | [`GET /api/v1/epoch/{epoch}`](/api-reference/epochs/epoch-overview) | Resumen de épocas |
| Épocas | [`GET /api/v1/epoch/{epoch}/slots`](/api-reference/epochs/epoch-slots) | Todas las ranuras en una época |
| Ranuras | [`GET .../ranura/{ranura}/attesterslashings`](/api-reference/slots/attester-slashings-for-a-slot) | Barras de attester |
| Ranuras | [`GET .../ranura/{ranura}/proposerslashings`](/api-reference/slots/proposer-slashings) | Barras del proponente |
| Ranuras | [`GET .../ranura/{ranura}/excursionesvoluntarias`](/api-reference/ranuras/excursionesvoluntarias-para-una-ranura) | Salidas voluntarias |
| Capa de ejecución | [`GET .../execution/address/{address}`](/api-reference/addresses/execution-address-balances) | Saldos de direcciones |
| Capa de ejecución | [`GET .../execution/address/{address}/erc20tokens`](/api-reference/addresses/erc-20-token-balances-for-an-address) | Saldos de tokens ERC-20 |
| Capa de ejecución | [`GET .../execution/{addressIndexOrPubkey}/produced`](/api-reference/blocks/execution-blocks-produced-by-fee-recipient-proposer-index-or-pubkey) | Bloques producidos por el destinatario de la tarifa |
| Capa de ejecución | [`GET .../execution/gasnow`](/api-reference/gas/current-gas-price-recommendations) | Recomendaciones de precios del gas |
| Otros | [`GET /api/v1/rocketpool/stats`](/api-reference/rocketpool/rocket-pool-network-statistics) | Estadísticas de red de Rocket Pool |
| Otro | [`GET /api/v1/rocketpool/validator/{indexOrPubkey}`](/api-reference/rocketpool/get-rocket-pool-validator-metadata-by-index-or-pubkey) | Metadatos del validador de Rocket Pool |
| Otros | `GET /api/v1/ethstore/{day}` | Agregados diarios de ETH.Store |
| Otro | [`GET /api/v1/ens/lookup/{dominio}`](/api-reference/ens/resolve-ens-name-or-address) | Resolución ENS |
| Otro | [`GET /api/v1/chart/{chart}`](/api-reference/misc/chart) | Datos del gráfico |
| Otro | [`GET /api/v1/graffitiwall`](/api-reference/misc/graffiti-wall-pixels) | Muro de graffiti |

### Puntos finales exclusivos de la versión 2 (Novedad en la versión 2)

Estos puntos finales solo están disponibles en la versión 2 y no tienen equivalente en la versión 1:

| Categoría | V2 | Notas |
| ----------------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Métricas del validador | [`POST .../validators/metadata`](/api-reference/ethereum/validators/metadata) | Metadatos del validador (Pro) |
| Métricas del validador | [`POST .../validators/apy-roi`](/api-reference/ethereum/validators/apy-roi) | Cálculos de APY y ROI |
| Funciones de los validadores | `POST .../validators/upcoming-duty-slots` | API pendiente: el equipo de beaconcha.in está trabajando en esta API. |
| Deberes del validador | [`POST .../validators/sync-committee-periods`](/api-reference/ethereum/validators/sync-committee-periods) | Historial de períodos del comité de sincronización |
| Comités de sincronización | `POST .../sync-committee/validators` | API pendiente: el equipo de beaconcha.in está trabajando en esta API. |
| Bloques | [`POST /api/v2/ethereum/block`](/api-reference/ethereum/block/overview) | Resumen de bloques |
| Bloques | [`POST .../block/rewards`](/api-reference/ethereum/block/rewards) | Desglose de las recompensas por bloque |
| Red | `POST /api/v2/ethereum/config` | API pendiente: el equipo de beaconcha.in está trabajando en esta API. |
| Red | [`POST /api/v2/ethereum/performance-aggregate`](/api-reference/ethereum/performance-aggregate) | Agregado de rendimiento de toda la red |
| Entidades | [`POST /api/v2/ethereum/entities`](/api-reference/ethereum/entities/overview) | Descripción general de las entidades de staking (Pro) |
| Entidades | [`POST .../entity/sub-entities`](/api-reference/ethereum/entity/sub-entities) | Subentidades de entidad (Pro) |

***

## Ejemplo de migración: Búsqueda de validador

### Antes (V1)

```bash theme={null}
curl 'https://beaconcha.in/api/v1/validator/1?apikey=WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0'
```

### Después (V2)

```bash theme={null}
curl -X POST 'https://beaconcha.in/api/v2/ethereum/validators' \
  -H 'Autorización: Bearer WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0' \
  -H 'Content-Type: application/json' \
  -d '{
    "cadena": "red principal",
    "validador": {"identificadores_validador": [1]}
  }'
```

Cambios clave en este ejemplo:

1. `GET` se convierte en `POST` con un cuerpo JSON.
2. El índice del validador se mueve de la ruta URL al objeto `validator` en el cuerpo de la solicitud.
3. La autenticación pasa del parámetro de consulta al encabezado `Authorization: Bearer`.
4. Debe especificar la `cadena` explícitamente.

***

## Ejemplo de migración: Datos de recompensas

### Antes (V1)

```bash theme={null}
curl -H 'apikey: WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0' \
  'https://beaconcha.in/api/v1/validator/1/incomedetailhistory'
```

### Después (V2)

```bash theme={null}
curl -X POST 'https://beaconcha.in/api/v2/ethereum/validators/rewards-list' \
  -H 'Autorización: Bearer WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0' \
  -H 'Content-Type: application/json' \
  -d '{
    "cadena": "red principal",
    "validador": {"identificadores_validador": [1]},
    "page_size": 10
  }'
```

Los datos de recompensas de la versión 2 incluyen un desglose detallado por tipo de deber (juramento, comité de sincronización, propuesta) con los importes de recompensa, penalización y recompensa no recibida.

***

## Cambios en la autenticación

La versión V1 admitía la autenticación tanto de parámetros de consulta como de encabezados:

```bash theme={null}
# V1 - parámetro de consulta
curl 'https://beaconcha.in/api/v1/validator/1?apikey=WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0'

# V1 - encabezado
curl -H 'apikey: WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0' 'https://beaconcha.in/api/v1/validator/1'
```

V2 utiliza únicamente autenticación mediante token Bearer:

```bash theme={null}
# V2 - Token de portador (único método)
curl -H 'Authorization: Bearer WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0' \
  -H 'Content-Type: application/json' \
  -X POST 'https://beaconcha.in/api/v2/ethereum/state' \
  -d '{"chain": "mainnet"}'
```

<Nota>
  La misma clave API funciona tanto para la versión 1 como para la versión 2. No necesita una clave nueva para empezar a usar la versión 2.
</Nota>

***

## Cambios en la paginación

V1 utiliza paginación basada en desplazamiento:

```bash theme={null}
# V1 - paginación desplazada
curl 'https://beaconcha.in/api/v1/validator/1/balancehistory?offset=100&limit=25&apikey=WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0'
```

V2 utiliza paginación basada en cursor. La primera solicitud omite el cursor, y las solicitudes posteriores utilizan el valor `cursor` del objeto `paging` en la respuesta anterior:

```bash theme={null}
# V2 - primera página
curl -X POST 'https://beaconcha.in/api/v2/ethereum/validators/balances' \
  -H 'Autorización: Bearer WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0' \
  -H 'Content-Type: application/json' \
  -d '{
    "cadena": "red principal",
    "validador": {"identificadores_validador": [1]},
    "page_size": 25
  }'

# V2 - página siguiente (usar el cursor de la respuesta anterior)
curl -X POST 'https://beaconcha.in/api/v2/ethereum/validators/balances' \
  -H 'Autorización: Bearer WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0' \
  -H 'Content-Type: application/json' \
  -d '{
    "cadena": "red principal",
    "validador": {"identificadores_validador": [1]},
    "page_size": 25,
    "cursor": "eyJlcG9jaCI6MzQ3NTY2fQ=="
  }'
```

Consulte la [Guía de paginación](/api/pagination) para obtener información completa sobre la paginación basada en cursor.

***

## Consideraciones sobre el límite de velocidad

Al migrar, el comportamiento de su límite de velocidad depende de su tipo de suscripción:

* **Planes heredados** (Zafiro, Esmeralda, Diamante): V1 y V2 utilizan límites de velocidad separados. Su cuota V1 no se ve afectada por el uso de V2.
* **Planes actuales** (Aficionado, Empresa, Escala): V1 y V2 comparten un único límite de tasa.

Consulte la [guía de límites de velocidad](/api/rate-limits#how-v1-and-v2-rate-limits-work) para obtener todos los detalles.
