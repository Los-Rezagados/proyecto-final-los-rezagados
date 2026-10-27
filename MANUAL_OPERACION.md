# Manual de Operación — Infraestructura Colegio La Salle
**Elaborado por:** Equipo Los Rezagados

## 1. Mapa Rápido de la Red
| Nodo | IP | Servicio |
| :--- | :--- | :--- |
| ns1 | 10.20.0.2 | DNS Primario (BIND) |
| ns2 | 10.20.0.3 | DNS Secundario (BIND) |
| resolutor | 10.20.0.4 | Resolutor y Filtro Web (Unbound) |
| web | 10.20.0.20 | Servidor Web (Nginx) y SSH |
| mail | 10.20.0.25 | Servidor de Correo (Postfix/Dovecot/OpenDKIM) |
| cliente | 10.20.0.50 | Equipo de pruebas (Auditoría) |
| externo | 172.31.0.100 | Simulación de Internet |

## 2. Tareas Frecuentes (Paso a paso)
**A. Agregar un nuevo nombre a la red interna:**
1. Entrar a `ns1`.
2. Editar `/proyecto/dns/zonas/db.lasalle.internal` y agregar el nuevo registro (ej. `impresora IN A 10.20.0.30`).
3. **Muy importante:** Subir el número de serial (ej. de `2023102706` a `2023102707`).
4. Reiniciar BIND: `systemctl restart named`.
5. Verificar que `ns2` se actualizó: `dig @10.20.0.3 impresora.lasalle.internal`.

**B. Renovar el certificado HTTPS de la web:**
1. Entrar a `web`.
2. Ejecutar el script de renovación (creado en la Fase 5).
3. Reiniciar Nginx: `systemctl restart nginx`.

**C. Agregar una regla al filtro de phishing (RPZ):**
1. Entrar al `resolutor`.
2. Editar `/etc/unbound/rpz.zone` y agregar `dominio-malo.com CNAME .`.
3. Reiniciar Unbound: `systemctl restart unbound`.

## 3. Tareas Periódicas
- **Semanal:** Revisar los logs de `fail2ban` en `web` para ver si hay ataques bloqueados.
- **Mensual:** Revisar que la zona `ns2` esté sincronizada con `ns1` (comparar seriales con `dig`).
- **Trimestral:** Renovar certificados.

## 4. Solución de Problemas (Troubleshooting)
| Síntoma | Causa probable | Solución |
| :--- | :--- | :--- |
| Los usuarios no pueden entrar a la web. | Nginx caído o certificado vencido. | `systemctl status nginx` en nodo `web`. |
| Un correo rebota por `SPF fail`. | La IP de salida cambió (no aplica en lab). | Revisar registro TXT en `ns1`. |
| `ns2` no tiene los cambios nuevos. | No se subió el serial en `ns1`. | Subir serial, borrar `.slave` en `ns2` y reiniciar. |
| La validación DNSSEC falla. | Las firmas expiraron. | Reiniciar BIND en `ns1` para forzar la refirma. |

## 5. Recuperación ante Desastres
- **Respaldos:** Los archivos de configuración y zonas están versionados en el repositorio de GitHub.
- **Restauración:** Si un nodo se borra, basta con hacer `docker compose up -d` y volver a ejecutar los scripts de configuración (idempotentes). Las zonas DNS se restauran clonando el repositorio.