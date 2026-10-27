# Arquitectura propuesta — Los Rezagados (Colegio La Salle)

## Plan de nombres
| Zona | Tipo | Quién la ve | Dónde vive |
|---|---|---|---|
| `lasalle.internal` | interna | solo la red de la organización (vista interna) | ns1 (primario) + ns2 (secundario TSIG) |
| `lasallesantacruz.net` | pública | internet (vista externa, firmada con DNSSEC) | ns1 + DNSControl |
| `20.0.10.in-addr.arpa` | inversa | red interna | ns1 + ns2 |

## Plan de direcciones (laboratorio)
| Nodo | IP interna | Función |
|---|---|---|
| ns1 | 10.20.0.2 (+ 172.31.0.2 pública) | DNS autoritativo primario, vistas, DNSSEC |
| ns2 | 10.20.0.3 | DNS secundario |
| resolutor | 10.20.0.4 | Unbound: caché, DoT, RPZ |
| web | 10.20.0.20 | Nginx + HTTPS (+ aplicación si aplica) |
| mail | 10.20.0.25 | Postfix + Dovecot + OpenDKIM |
| cliente | 10.20.0.50 | PC de la organización (pruebas y auditoría) |
| externo | 172.31.0.100 | "internet" (pruebas de la vista externa) |

## Diagrama
(Dibujar: redes, nodos, flujos de DNS, web, SSH y correo; qué puertos abre el firewall.)

## Decisiones y su justificación (mínimo 5)
| Decisión | Alternativas consideradas | Por qué |
|---|---|---|
| Usar red `.internal` | Dominio público inventado | Evita colisiones, oculta topología interna |
| Separar Unbound de BIND | Usar BIND para todo | BIND es autoritativo; Unbound es recursivo y aplica RPZ/DoT |
| Usar Vistas (Views) | Una sola zona para todos | Separa el tráfico de la organización del tráfico de internet |
| Secundario con TSIG | Transferencia abierta | Seguridad: solo ns1 puede actualizar a ns2 |
| Firmar con DNSSEC | Solo HTTPS | Protege la integridad del DNS contra ataques de envenenamiento |