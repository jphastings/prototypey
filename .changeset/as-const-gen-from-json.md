---
"prototypey": patch
---

Add `as const` to generated lexicon TypeScript output, ensuring JSON literals passed to `fromJSON()` are narrowed to their readonly literal types instead of being widened. This enables stronger type inference downstream.
