curl -X POST 'https://beaconcha.in/api/v2/ethereum/state' \
  -H 'Authorization: Bearer <WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0>' \
  -H 'Content-Type: application/json' \
  -d '{"chain": "mainnet"}'

  Ejemplo completo: recompensas para validadores
Este ejemplo utiliza el punto final Rewards para obtener datos de recompensas del validador.
curl -X POST 'https://beaconcha.in/api/v2/ethereum/validators/rewards-list' \
  -H 'Authorization: Bearer <WnqQ4rhQNYISqXvffZcJrND3i39tLPO0tgcN7awwIE0>' \
  -H 'Content-Type: application/json' \
  -d '{
    "validator": {
      "validator_identifiers": [1]
    },
    "chain": "mainnet",
    "page_size": 10,
    "epoch": 347566
  }'
