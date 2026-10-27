\# BRIEF — Colegio La Salle Santa Cruz



\## 1. Contexto de la Organización

\- \*\*Nombre:\*\* Unidad Educativa La Salle Santa Cruz.

\- \*\*Tipo:\*\* Institución educativa católica.

\- \*\*Ubicación:\*\* Av. Cristóbal de Mendoza, 2do anillo, Santa Cruz de la Sierra, Bolivia.

\- \*\*Sitio Web:\*\* https://www.lasallesantacruz.net

\- \*\*Contacto:\*\* recepcion@lasallescz.edu.bo

\- \*\*Tamaño estimado:\*\* Comunidad educativa mediana (estudiantes, docentes, personal administrativo).



\## 2. Situación Actual (Diagnóstico DNS — Datos Públicos)



\### Infraestructura detectada

\- \*\*Proveedor de hosting y DNS:\*\* A2 Hosting (ns1-ns4.a2hosting.com).

\- \*\*Servidor web:\*\* IP 68.66.224.46 (hosting compartido).

\- \*\*Correo:\*\* alojado en el mismo servidor (registro MX apunta al propio dominio).

&#x20; → \*Riesgo:\* un solo punto de falla para web y correo.



\### Registros DNS encontrados



| Registro | Valor detectado | Estado |

|---|---|---|

| \*\*NS\*\* | ns1, ns2, ns3, ns4.a2hosting.com | ✅ Funcional, dependiente del proveedor |

| \*\*MX\*\* | lasallesantacruz.net (preferencia 0) | ⚠️ Correo en el mismo hosting |

| \*\*SPF (TXT)\*\* | `v=spf1 +a +mx +ip4:68.66.224.46 \~all` | ⚠️ `\~all` (softfail) = permisivo |

| \*\*DMARC\*\* | No existe | 🔴 Ausente — vulnerable a suplantación |

| \*\*CAA\*\* | No existe | 🟡 Ausente — no se restringen las CA emisoras |



\### Hallazgos preliminares (ordenados por gravedad)



1\. 🔴 \*\*CRÍTICO — Sin DMARC:\*\* cualquier atacante puede enviar correos suplantando el dominio del colegio (phishing a padres y estudiantes).

2\. 🟠 \*\*ALTO — SPF permisivo (`\~all`):\*\* aunque existe SPF, la política final es "aceptar aunque sospeche", en lugar de "rechazar" (`-all`).

3\. 🟡 \*\*MEDIO — Sin control directo del DNS:\*\* el colegio depende de las herramientas que A2 Hosting le ofrece en su panel.

4\. 🟡 \*\*MEDIO — Web y correo en el mismo servidor:\*\* un fallo del hosting afecta ambos servicios simultáneamente.

5\. 🟡 \*\*BAJO — Sin registro CAA:\*\* no se ha definido qué Autoridades de Certificación pueden emitir certificados SSL/TLS para el dominio.



\## 3. Objetivos Medibles del Proyecto



1\. \*\*Fortalecer la política anti-suplantación del correo institucional.\*\*

&#x20;  - Diagnóstico: sin DMARC + SPF permisivo (`\~all`).

&#x20;  - Meta: diseñar una propuesta con SPF `-all` y DMARC `p=quarantine`, validada en el laboratorio con `dkim=pass` y `dmarc=pass`.



2\. \*\*Documentar la dependencia técnica del DNS actual del colegio.\*\*

&#x20;  - Meta: entregar un diagrama de su infraestructura actual (hosting + DNS + correo) y un análisis de riesgos en lenguaje no técnico, apto para la dirección del colegio.



3\. \*\*Construir una infraestructura DNS moderna y segura como referencia.\*\*

&#x20;  - Meta: levantar en el laboratorio una arquitectura con DNS primario/secundario, DNSSEC, vistas y resolutor Unbound, logrando ≥ 90 % en la auditoría interna y 100 % en la externa.



\## 4. Alcance

\- \*\*Incluye:\*\* Diagnóstico público DNS, propuesta de arquitectura, manual de operación, entrega de resultados al colegio.

\- \*\*No incluye:\*\* Acceso a sistemas internos del colegio, modificación de su infraestructura real, gestión de contraseñas o credenciales.



\## 5. Entregables para la Organización

\- \*\*Diagnóstico técnico\*\* del dominio `lasallesantacruz.net` en lenguaje comprensible.

\- \*\*Recomendaciones priorizadas\*\* (SPF estricto, DMARC, CAA, separación de servicios).

\- \*\*Manual de operación\*\* simplificado para su equipo administrativo.



\## 6. Declaración Ética y de IA

\- Todos los datos recopilados son \*\*públicos\*\* (consultas DNS abiertas vía `Resolve-DnsName`).

\- No se solicitó ni utilizará ningún tipo de acceso o credencial al sistema del colegio.

\- Uso de IA declarado en `bitacora.md` del equipo.

