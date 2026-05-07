# reportar

Plugin de Claude Code para registrar issues de mejora o fix en GitHub cuando el agente detecta un problema mientras trabaja con un plugin o ecosistema.

Se instala una vez y funciona en cualquier contexto: detecta el repo correcto por inferencia y siempre pide aprobación antes de abrir algo.

## Instalación

```bash
claude --plugin-dir /ruta/al/plugin
```

O agregalo al marketplace que uses.

## Uso

**Manual:**
```
/reportar
```

**Automático:** el agente lo propone cuando detecta algo accionable — una fuente caída, un edge case no cubierto, una mejora obvia. Solo lo abre si el usuario confirma.

## Ejemplo real

Trabajando con el plugin `arca` y la URL del dólar blue no responde:

```
Encontré algo que vale registrar como issue. ¿Lo abro?

Repo: kvothesson/arca
Título: [source]: URL del dólar blue dejó de responder
---
## Contexto

Consultando el tipo de cambio informal. La URL principal retorna 503.

## Problema

https://api.bluelytics.com.ar/v2/latest no responde desde 2026-05-07.

## Cómo reproducir / evidencia

curl https://api.bluelytics.com.ar/v2/latest → HTTP 503 Service Unavailable

## Propuesta

Agregar bluelytics-unofficial como fallback o cambiar a dolarapi.com/v1/cotizaciones/blue.
---
¿Abro el issue?
```

Si el usuario confirma, abre el issue y comparte el link.

## Qué detecta

- Fuentes de datos caídas o con URL cambiada
- Edge cases que el skill no contempla
- Features obvias que faltan
- Documentación desactualizada

## Qué no hace

- No abre issues sin aprobación explícita del usuario
- No duplica issues existentes (verifica antes)
- No registra datos personales del usuario
- No abre issues vagos o de preferencia de estilo

## Fuentes

- GitHub CLI (`gh`) — debe estar instalado y autenticado
