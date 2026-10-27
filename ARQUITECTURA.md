\# ARQUITECTURA — Los Rezagados (Colegio La Salle)



\## 1. Plan de Nombres (Zonas DNS)

\- \*\*Zona interna:\*\* `lasalle.internal` (para la red local)

\- \*\*Zona inversa:\*\* `20.30.10.in-addr.arpa` (para traducir IPs a nombres)

\- \*\*Zona pública:\*\* `lasallesantacruz.net` (simulada en el laboratorio)



\## 2. Diagrama y Direccionamiento de Red



La red base será `10.30.20.0/24` con los siguientes 7 nodos:



| Nodo | Rol | IP Fija | Puertos expuestos |

| :--- | :--- | :--- | :--- |

| \*\*ns1\*\* | DNS Primario (BIND) | 10.30.20.11 | 53/udp, 53/tcp |

| \*\*ns2\*\* | DNS Secundario (BIND) | 10.30.20.12 | 53/udp, 53/tcp |

| \*\*resolver\*\*| Resolutor (Unbound) | 10.30.20.13 | 53/udp, 53/tcp, 853/tcp |

| \*\*web\*\* | Servidor Web (Nginx) | 10.30.20.21 | 80/tcp, 443/tcp |

| \*\*correo\*\* | Servidor Correo | 10.30.20.31 | 25, 587, 993 |

| \*\*bastion\*\*| Acceso SSH | 10.30.20.40 | 22/tcp |

| \*\*cliente\*\*| Cliente de pruebas | 10.30.20.50 | (sin puertos abiertos) |



\*(Aquí irá una imagen del diagrama cuando lo tengamos en Docker)\*



\## 3. Decisiones Justificadas



1\. \*\*Uso de la zona `.internal` para la red interna.\*\*

&#x20;  \*Justificación:\* El dominio `.internal` está reservado por la ICANN para redes privadas. Evita colisiones con dominios reales de internet y protege la topología interna de filtraciones.



2\. \*\*Separación del resolutor (Unbound) del DNS autoritativo (BIND).\*\*

&#x20;  \*Justificación:\* Buenas prácticas de seguridad. BIND solo atiende consultas de nuestras zonas, sin recursión. Unbound se encarga de la recursión hacia internet, aplicando seguridad extra (DoT) y filtrado de dominios (RPZ).



3\. \*\*Implementación de una CA (Autoridad Certificadora) Interna.\*\*

&#x20;  \*Justificación:\* Permite emitir certificados SSL/TLS propios para los servicios internos (`web.lasalle.internal`) sin depender de entidades externas, garantizando cifrado en la red local.



4\. \*\*Transferencia de zona segura (TSIG y NOTIFY) entre ns1 y ns2.\*\*

&#x20;  \*Justificación:\* TSIG (Transaction SIGnatures) autentica que solo ns1 puede enviar actualizaciones a ns2. NOTIFY asegura que los cambios se propaguen en segundos, garantizando alta disponibilidad.



5\. \*\*Uso de Vistas (Views) en BIND para separar tráfico interno y externo.\*\*

&#x20;  \*Justificación:\* Las vistas permiten que el servidor DNS responda con diferentes IPs dependiendo de quién pregunta. Protege la red interna mostrando solo información pública a consultas externas.

