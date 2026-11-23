# Contribuir a omia-proyecto-cmms

## Modelo de ramas

| Rama | Propósito | Quién escribe |
|---|---|---|
| `main` | Producción | Solo mediante PR desde `dev`, promovido por el owner |
| `dev` | Integración (rama por defecto) | Mediante PR desde ramas de trabajo |
| `feat/*`, `fix/*`, `chore/*`, `docs/*`, `refactor/*` | Trabajo diario | Autor del cambio |

Reglas:

1. Nunca se hace push directo a `dev` ni a `main`.
2. Toda rama de trabajo nace de `dev` y muere al mergearse.
3. Los PRs de trabajo apuntan siempre a `dev`.
4. `dev` → `main` es un PR abierto por el owner y revisado por el otro
   colaborador. Si un colaborador abre un PR a `main`, el check
   `solo-owner-promueve` lo bloquea: se cierra y el owner abre el suyo.
5. No se permite bypass de los rulesets, ni siquiera para el owner.

## Revisión cruzada

Somos dos colaboradores y GitHub no permite autoaprobarse, por lo que cada PR
necesita la revisión del otro:

- PR de `gajhomia` → lo revisa `DesamttoOMIA`.
- PR de `DesamttoOMIA` → lo revisa `gajhomia`.

Un PR no se mergea con conversaciones abiertas ni con cambios posteriores sin
aprobar.

## Tablero de trabajo

El seguimiento se hace en [GitHub Projects](https://github.com/users/DesamttoOMIA/projects/1)
con la estructura **Entrega → Épica → sub-issues**:

- Épicas: `Épica N · Nombre`.
- Tareas: `N.M Descripción`, enlazadas como sub-issues reales.
- Vista `Control`: tabla agrupada por `Entrega`, con jerarquía activada.

Detalle, campos y comandos en [docs/tablero-github.md](docs/tablero-github.md).

## Commits

Se usa [Conventional Commits](https://www.conventionalcommits.org/):

```text
feat: agrega registro de órdenes de trabajo
fix: corrige cálculo de disponibilidad
docs: actualiza guía de despliegue
chore: actualiza dependencias
refactor: separa capa de persistencia
test: cubre validación de formularios
```

Tipos permitidos: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`,
`build`, `ci`, `chore`, `revert`.

## Pull requests

- Título en formato Conventional Commits.
- Descripción con contexto, cambios y cómo probarlo.
- Enlazar el issue con `Closes #N` cuando aplique.
- PRs pequeños y con un solo propósito.
- Marcar el tipo con los labels `type/*`.

## Merges

| Origen → destino | Método |
|---|---|
| Rama de trabajo → `dev` | Squash merge |
| `dev` → `main` | Merge commit |

El merge a `dev` requiere 1 aprobación, rama actualizada y conversaciones
resueltas. El merge a `main` requiere 1 aprobación y el check
`solo-owner-promueve` en verde.

## Labels

- `type/*`: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`, `ci`
- `area/*`: `backend`, `frontend`, `db`, `infra`, `github`
- `priority/*`: `p0-critical`, `p1-high`, `p2-medium`, `p3-low`
- `status/*`: `needs-triage`, `in-progress`, `needs-review`, `blocked`

## Entorno local

El proyecto usa Python 3.12. El entorno virtual `venv/` está excluido del
repositorio:

```bash
python3 -m venv venv
source venv/bin/activate
```

## Reglas de higiene

- No commitear secretos, `.env`, claves ni credenciales.
- No commitear `venv/` ni archivos de agentes/IDE locales.
- No reescribir historia de ramas compartidas (`force push` bloqueado).
