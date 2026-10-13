# Práctica 1 de SAD · SecureCorp — Respuestas


**Nombre y apellidos:Arturo Villalba**
**Usuario: Avillalba**


Responde con tus palabras, en 1-3 líneas. En la defensa te preguntaré lo mismo en voz alta.


**Contraseñas que has usado** (solo porque es un laboratorio; en una empresa, jamás en un fichero):


- Tu usuario: arturosad1_practica
- mtorres: securecorp2026
---


**1. (A1)** ¿Quién es el `issuer` de tu `ca.crt`? ¿Hasta qué fecha es válido? ¿Por qué el `subject`
y el `issuer` de la CA son iguales y los de `ldap.crt` no?




**2. (A3)** Pega el comando y el resultado de tus dos búsquedas:


ldapsearch -x -LLL -H ldaps://ldap.securecorp.local \
-D "cn=admin,dc=securecorp,dc=local" -w 'SecureCorp2026' \
-b "cn=rrhh,ou=groups,dc=securecorp,dc=local" -s base member


```
a) miembros de rrhh:

dn: cn=rrhh,ou=groups,dc=securecorp,dc=local
member: uid=lromero,ou=people,dc=securecorp,dc=local
member: uid=mtorres,ou=people,dc=securecorp,dc=local




ldapsearch -x -LLL -H ldaps://ldap.securecorp.local \
-D "cn=admin,dc=securecorp,dc=local" -w 'SecureCorp2026' \
-b "ou=people,dc=securecorp,dc=local" \
"(objectClass=inetOrgPerson)" cn mail


b) cn y mail de todas las personas:


dn: uid=lromero,ou=people,dc=securecorp,dc=local
cn: Lucia Romero
mail: lromero@securecorp.local

dn: uid=avillalba,ou=people,dc=securecorp,dc=local
cn: Arturo Villalba Garcia
mail: avillalba@securecorp.local

dn: uid=mtorres,ou=people,dc=securecorp,dc=local
cn: Marta Torres
mail: mtorres@securecorp.local



```


**3. (A4)** ¿Por qué la clave `ldap.key` tiene que ser de `openldap` y tener permisos 600?


Porque el servicio slapd se ejecuta como openldap y necesita leer la clave privada para establecer conexiones. Los permisos 600 permiten que solo el propietario pueda leerla y escribirla, protegiendo la clave frente a otros usuarios.




**4. (A4)** ¿Qué valor has puesto en `SLAPD_SERVICES` y por qué?


SLAPD_SERVICES="ldapi:/// ldaps:///"


ldapi:/// permite la administración local mediante socket y ldaps:/// habilita las conexiones LDAP cifradas. No he incluido ldap:/// para evitar conexiones sin cifrar por el puerto 389.




**5. (A4)** Antes de añadir `TLS_CACERT` en el cliente, `ldaps://` no funcionaba. ¿Por qué?




Porque el cliente no tenía configurada la CA que firma el certificado del servidor y no podía verificar su confianza. Al añadir TLS_CACERT /pki/ca/ca.crt, puede validar el certificado del servidor y ya si nos deja.


**6. (B3)** Pega la salida de `klist` con tus dos tickets. ¿Para qué sirve cada uno? ¿Ha viajado tu
contraseña por la red?


```
<!-- # Clave para Kerberos




<!-- root@CLIENTE:~# klist
Ticket cache: FILE:/tmp/krb5cc_0
Default principal: avillalba@SECURECORP.LOCAL




Valid starting     Expires            Service principal
10/12/26 16:54:42  10/13/26 02:54:42  krbtgt/SECURECORP.LOCAL@SECURECORP.LOCAL




# Clave de la web #




  renew until 10/19/26 16:54:42
10/12/26 16:55:29  10/13/26 02:54:42  host/web.securecorp.local@SECURECORP.LOCAL
  renew until 10/19/26 16:54:42 --> -->








```


**7. (C)** En el `docker-compose.yml`, ¿qué diferencia hay entre `build:` e `image:`? ¿Qué
significa la línea `- "8081:80"` del servicio `phpldapadmin`?


build: indica que Docker debe construir una imagen a partir de un directorio y su Dockerfile. image: indica qué imagen se va a utilizar. La línea - "8081:80" publica el puerto 80 del contenedor en el puerto 8081 de la máquina del host, permitiendo acceder a phpLDAPadmin mediante el puerto 8081.




**8. (C)** ¿Por qué en la máquina `web` no has tenido que escribir a mano `TLS_CACERT`, y en el
cliente sí? ¿Qué pasaría con esa línea del cliente si hicieras `./lab.sh reset`?




En la máquina web, la configuración de confianza en la CA ya está preparada por el laboratorio o por su configuración inicial. En el cliente, he tenido que añadir manualmente TLS_CACERT /pki/ca/ca.crt para que pueda verificar el certificado de LDAP. Si ejecuto ./lab.sh reset, esa configuración podría perderse si el archivo se restaura o se vuelve a crear; tendría que comprobarlo y añadirla de nuevo si fuera necesario.


