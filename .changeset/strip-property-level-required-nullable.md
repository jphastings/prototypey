---
"prototypey": patch
---

Stop emitting property-level `required: true` / `nullable: true` flags on object and params schemas.

The lexicon spec expresses required and nullable status through the parent's `required: [...]` / `nullable: [...]` arrays only; the previous output incorrectly included both the parent array and the per-property boolean. Nested `lx.object` results used directly as property values are preserved — their own `required: [...]` / `nullable: [...]` arrays are not touched.
