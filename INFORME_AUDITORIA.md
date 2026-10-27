# Informe de auditoría — lasalle.internal

Fecha: 2026-10-27 07:10 · equipo auditor: cliente · resolutor: el del sistema

**Resultado: 29 de 29 comprobaciones (100 %)**

## DNS

- ✅ Al menos 2 servidores de nombres (2)
- ✅ Todos los NS responden con autoridad 
- ✅ Mismo serial en todos los NS (2023102706 )
- ✅ Transferencia de zona sin clave rechazada en ns1.lasalle.internal.
- ✅ Los autoritativos no hacen recursión
- ✅ Versión de BIND oculta
- ✅ Zona inversa del servidor web (PTR)
- ✅ El resolutor valida DNSSEC (flag ad en cloudflare.com)
- ✅ El resolutor rechaza firmas falsas (dnssec-failed.org → SERVFAIL)
- ✅ CAA publicado

## Sitio web (www.lasalle.internal)

- ✅ Responde por HTTPS con certificado válido (200)
- ✅ http:// redirige a https:// (301)
- ✅ Negocia TLS 1.3
- ✅ HSTS
- ✅ Content-Security-Policy
- ✅ X-Content-Type-Options: nosniff
- ✅ Servidor sin versión en la cabecera Server
- ✅ Certificado vigente por más de 14 días (364 días)

## Acceso remoto (SSH en www.lasalle.internal)

- ✅ Solo acepta claves (sin contraseñas)
- ✅ Intercambio de claves post-cuántico
- ✅ TELNET cerrado (puerto 23)

## Correo (lasalle.internal)

- ✅ MX publicado (mail.lasalle.internal)
- ✅ SPF con -all
- ✅ Clave DKIM publicada (mail._domainkey)
- ✅ DMARC con política quarantine o reject
- ✅ IMAPS (993) con certificado válido
- ✅ IMAP sin cifrar (143) cerrado
- ✅ Envío (587) sin TLS no ofrece autenticación

## Puertos expuestos (www.lasalle.internal)

- ✅ Solo 22, 80 y 443 abiertos en el servidor web (abiertos: 22 80 443 )

> Las comprobaciones de la vista externa (zona pública firmada, validación con delv) se ejecutan desde el nodo
> externo con auditar_publico.sh.
