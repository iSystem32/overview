Vault Sandbox is a development and testing environment configured as "production-lite", and has limitations that you should be aware of.

Not production ready

This environment is not production-ready, as it does not follow recommendations for production cluster deployments. Refer to these resources for more information:

Vault with integrated storage reference architecture
Vault with integrated storage deployment guide
Harden server deployments
Community edition

Be aware that Vault Sandbox uses the Vault Community edition, and this imposes the limitation that you cannot use enterprise features here.

TLS

Vault Sandbox features mutual TLS between cluster nodes. You should be aware that the environment dynamically generates TLS material used to enable TLS with a 24 hour time to live.

Root token

Vault Sandbox uses and exposes the initial root token value for ease of development and testing. You should not rely on root token use for production implementations.

Other sandboxes
Explore sandboxes for other HashiCorp products that you might find useful.
