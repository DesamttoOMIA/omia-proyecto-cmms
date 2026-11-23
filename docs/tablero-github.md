# Tablero de GitHub y convenciones

Documentación de la organización del trabajo en GitHub para `omia-proyecto-cmms`.

## Enlaces

- Repositorio: <https://github.com/DesamttoOMIA/omia-proyecto-cmms>
- Project: <https://github.com/users/DesamttoOMIA/projects/1>
- Vista principal: <https://github.com/users/DesamttoOMIA/projects/1/views/5>

## Estructura del trabajo

La jerarquía tiene tres niveles:

```
Entrega 1
└── Épica 1 · Implementación
    ├── 1.1 Repositorio y entorno de trabajo
    ├── 1.2 Configuración de la aplicación
    └── 1.3 Modelo de datos
```

1. **Entrega** — campo de selección única (`Entrega 1`, `Entrega 2`, …). Agrupa
   el trabajo de un periodo o release. Es la división grande del tablero.
2. **Épica** — issue con label `type/epic`. Agrupa un frente de trabajo.
3. **Tarea** — issue enlazado como **sub-issue real** de la épica. No se usan
   checklists en el cuerpo para representar jerarquía.

## Convención de nombres

| Nivel | Formato | Ejemplo |
|---|---|---|
| Épica | `Épica N · Nombre` | `Épica 1 · Implementación` |
| Tarea | `N.M Descripción` | `1.1 Repositorio y entorno` |
| Subtarea | `N.M.K Descripción` | `1.1.1 Crear repositorio` |

- La numeración es secuencial por épica y no se reutiliza.
- El nombre de la épica lleva el número para mantener el orden alfabético.
- Las tareas heredan el número de su épica.

## Vista `Control`

- Layout: **tabla**.
- Agrupación: **Entrega**.
- Jerarquía: **activada** (`View → Show hierarchy`). Las vistas nuevas la traen
  activada por defecto desde marzo de 2026.
- Columnas visibles: `Title`, `Assignees`, `Status`, `Linked pull requests`,
  `Sub-issues progress`.
- Cada épica es una fila expandible; al expandirla se ven sus sub-issues.

## Campos del Project

| Campo | Tipo | Valores |
|---|---|---|
| `Entrega` | selección única | `Entrega 1`, `Entrega 2`, … |
| `Status` | selección única | `Todo`, `In Progress`, `Done` |
| `Kind` | selección única | `Epic`, `Task`, `Bug`, `Chore`, `Sub-epic` |
| `Epic` | selección única | `Épica 1`, `Épica 2`, … |
| `Sub-epic` | selección única | opcional, para subdivisiones |
| `Priority` | selección única | `P0`, `P1`, `P2`, `P3` |
| `Area` | selección única | `Backend`, `Frontend`, `DB`, `Infra`, `GitHub` |

## Crear una épica

1. Crear el issue con título `Épica N · Nombre` y label `type/epic`.
2. Agregarlo al Project:

   ```bash
   gh project item-add 1 --owner DesamttoOMIA \
     --url https://github.com/DesamttoOMIA/omia-proyecto-cmms/issues/N
   ```

3. Completar `Entrega`, `Epic`, `Kind = Epic`, `Priority`, `Area` y `Status`.

## Crear una tarea

1. Crear el issue con título `N.M Descripción` y los labels `type/*` y `area/*`.
2. Agregarlo al Project.
3. Completar `Entrega`, `Epic`, `Kind = Task`, `Priority`, `Area` y `Status`.
4. Enlazarlo como sub-issue de la épica:

   ```bash
   gh api -X POST repos/DesamttoOMIA/omia-proyecto-cmms/issues/<EPICA>/sub_issues \
     -F sub_issue_id=<ID_DE_LA_TAREA>
   ```

   El `ID_DE_LA_TAREA` es el `id` numérico del issue, no el número visible:

   ```bash
   gh api repos/DesamttoOMIA/omia-proyecto-cmms/issues/<NUMERO> --jq .id
   ```

## Crear una vista nueva

El endpoint REST de vistas acepta el **login** del owner, no el ID numérico.
Ejemplo de una tabla agrupada por `Entrega`:

```bash
gh api -X POST users/DesamttoOMIA/projectsV2/1/views -H "X-GitHub-Api-Version: 2026-03-10" --input - <<'JSON'
{
  "name": "Control",
  "layout": "table",
  "visible_fields": [449102587, 449102588, 449102589, 449102591, 449102598],
  "group_by": [449114998],
  "sort_by": [[449102587, "asc"]]
}
JSON
```

Los IDs de campo son numéricos (`databaseId`), no los `PVTSSF_…`.

## Flujo de cambios

- Rama de trabajo desde `dev` → PR a `dev` → 1 aprobación cruzada → squash merge.
- `dev` → `main` solo lo promueve el owner y exige el check
  `solo-owner-promueve`.
- Detalle completo en [CONTRIBUTING.md](../CONTRIBUTING.md).

## Notas técnicas

- La jerarquía del tablero depende del vínculo real de sub-issue de GitHub; un
  label o una checklist no la crean.
- Borrar un issue no elimina su item del Project de inmediato; puede quedar como
  registro huérfano en la API.
- El Project es público y está vinculado al repositorio, por lo que los
  colaboradores pueden verlo.
