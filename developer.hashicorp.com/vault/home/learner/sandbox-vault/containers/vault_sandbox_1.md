.
├── certs
│   ├── server_cert.pem
│   ├── server_key.pem
│   └── vault_sandbox_ca.pem
├── config
│   └── server.hcl
├── logs
│   └── vault_audit.log
└── plugins

También puedes examinar estos archivos desde la pestaña Editor de código.

Si necesita acceder directamente a un contenedor, puede usar 'docker exec'. Por ejemplo, para obtener una sesión de shell en vault-1, use:

$ docker exec -i -t vault-1 sh

Exporte uno de los siguientes archivos para indicar el nodo del clúster de Vault con el que necesita comunicarse (proxy inverso por defecto):

export VAULT_ADDR=https://localhost:8443# Nodo activo a través de proxy inverso
export VAULT_ADDR=https://localhost:8200# Nodo de bóveda 1
export VAULT_ADDR=https://localhost:8220# Nodo de bóveda 2
export VAULT_ADDR=https://localhost:8230# Nodo de bóveda 3
export VAULT_ADDR=https://localhost:8240# Nodo de bóveda 4
export VAULT_ADDR=https://localhost:8250# Nodo de bóveda 5
Puedes acceder a la interfaz de usuario de Vault para cada nodo mediante las pestañas etiquetadas como Interfaz de usuario de Vault 1 hasta Interfaz de usuario de Vault 5.

El entorno permite la telemetría del servidor Vault desde el nodo activo a Prometheus, y puede usar Grafana para acceder a un panel en la pestaña Grafana con estas credenciales:

Nombre de usuario:admin
Contraseña:2LearnVault
