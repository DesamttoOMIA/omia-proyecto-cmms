# Flujo de solicitud de revisión

Guía para el **reviewer** (`gajhomia`): qué hacer cuando te llega una
solicitud de revisión en `omia-proyecto-cmms`.

## Cómo te llega la solicitud

1. **Campana de GitHub** (esquina superior derecha): notificación
   "Review requested".
2. **Email** a la dirección de tu cuenta de GitHub.
3. En el PR, pestaña **Reviewers**: tu nombre aparece como reviewer solicitado.

> Confirma por el canal interno en qué canal la viste (paso 5 de la issue #3).

## Qué debes revisar

1. **Diff** (pestaña `Files changed`): ¿el cambio hace lo que dice el título?
2. **Checklist de la plantilla del PR**: que el autor la haya completado.
3. **Checks en verde**: `gh pr checks <número>`.
4. **Base correcta**: el PR debe ir contra `dev`, nunca contra `main`.
5. **Sin secretos**: nada de `.env`, claves, tokens ni archivos de agentes.

## Cómo aprobar

```bash
gh pr review <número> --approve --body "LGTM: ..."
```

O en la UI: `Review changes` → `Approve` → `Submit review`.

## Cómo pedir cambios

```bash
gh pr review <número> --request-changes --body "Falta: ..."
```

O en la UI: `Review changes` → `Request changes` → `Submit review`.
Explica qué debe cambiar el autor antes de aprobar.

## Reglas que aplican

- Tú **no tocas la rama** del autor: solo revisas, comentas y apruebas
  o pides cambios.
- Sin tu aprobación no hay merge (revisión cruzada obligatoria).
- El merge a `dev` lo hace el autor con **squash** tras tu aprobación.

Relacionado: [CONTRIBUTING.md](../CONTRIBUTING.md), issue #3.
