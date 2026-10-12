# Parte C · La máquina web.securecorp.local

# 1. Imagen de partida: misma Debian que el laboratorio
FROM debian:bookworm-slim

ENV DEBIAN_FRONTEND=noninteractive

# 2. Instalar clientes de Kerberos, LDAP y certificados
RUN apt-get update && apt-get install -y \
      krb5-user ldap-utils ca-certificates \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

# 3. Confiar en la CA de SecureCorp
RUN echo "TLS_CACERT /pki/ca/ca.crt" >> /etc/ldap/ldap.conf

# 4. Mantener el contenedor encendido
CMD ["sleep", "infinity"]
