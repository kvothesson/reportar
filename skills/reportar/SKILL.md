---
description: Registra un issue en GitHub cuando detectás un problema usando un plugin — fuente caída, edge case no cubierto, mejora obvia. Se activa con /reportar o automáticamente cuando el agente detecta algo accionable que vale registrar en lugar de solo mencionarlo en el chat.
---

# Skill: /reportar

Abrí un issue en el repo correcto cuando detectás algo que vale registrar. Siempre con aprobación del usuario antes de abrir.

---

## Cuándo activarse

Activá esta skill (proponiéndola al usuario) cuando detectes cualquiera de estas situaciones mientras trabajás con un plugin o ecosistema:

- Una URL del skill no responde o cambió de dominio
- El usuario pidió algo que el plugin casi cubre pero le falta un caso concreto
- Hay un edge case que el skill no contempla y que va a aparecer de nuevo
- Falta un subcomando que sería evidente para cualquier usuario
- Documentación desactualizada o contradictoria que va a confundir a otro agente

No activarse si:
- El problema ya se resolvió en esta sesión
- Es vago o es una preferencia de estilo
- El usuario no lo consideraría un problema real

---

## Paso 1 — Determinar el repo destino

Inferí el repo a partir del contexto de la conversación:

**Si el problema es específico de un plugin** (SKILL.md roto, fuente caída, edge case de ese plugin):
```
kvothesson/<nombre-del-plugin>
```

**Si el problema es del ecosistema o tooling** (AGENT.md, SPEC.md, marketplace, workflow de dev):
```
kvothesson/ar-plugins
```

**Si estás en otro ecosistema**: usá el repo del plugin activo o el repo raíz del marketplace que instaló este plugin.

Si no podés inferirlo con certeza, preguntale al usuario antes de continuar.

---

## Paso 2 — Verificar que no existe un issue similar

```bash
gh issue list --repo <repo-destino> --state open --search "<palabras clave>"
```

Si existe uno que cubre el mismo problema: mencioná el número al usuario y no abras uno nuevo.

---

## Paso 3 — Redactar el issue

**Título:** `[tipo]: descripción concreta en una línea`

Tipos:
- `fix` — algo roto
- `feat` — algo que falta
- `source` — fuente de datos cambió o dejó de responder
- `edge-case` — caso no cubierto

**Body:**

```
## Contexto

<Una o dos oraciones: en qué situación se detectó. Sin datos personales.>

## Problema

<Qué falla o qué falta. Específico y accionable.>

## Cómo reproducir / evidencia

<URL que falló, comando que se intentó, respuesta obtenida vs esperada.>

## Propuesta (opcional)

<Solo si tenés una idea concreta de cómo resolverlo.>
```

El issue tiene que ser resoluble por otro agente sin contexto de esta sesión.

---

## Paso 4 — Mostrar y pedir aprobación

Mostrá el borrador completo en el chat antes de abrir nada:

```
Encontré algo que vale registrar como issue. ¿Lo abro?

Repo: <repo-destino>
Título: [tipo]: descripción
---
[body completo]
---
```

Esperá confirmación explícita. Si el usuario ajusta algo, incorporalo.

---

## Paso 5 — Abrir el issue

```bash
gh issue create \
  --repo <repo-destino> \
  --title "[tipo]: descripción" \
  --body "$(cat <<'EOF'
## Contexto

<contexto>

## Problema

<problema>

## Cómo reproducir / evidencia

<evidencia>
EOF
)"
```

Compartí el link del issue abierto en el chat.

---

## Tono

Directo y útil. No dramatices el problema ni lo minimices. El objetivo es que otro agente pueda tomarlo y resolverlo sin fricción.
