---
"prototypey": minor
---

`lx.ref()` now accepts a raw imported lexicon JSON or a `fromJSON()` result in addition to an NSID string. When a lexicon object is passed, its `main` def is resolved at the type level so cross-file refs get full type inference without codegen:

```ts
import strongRef from "./lexicons/com/atproto/repo/strongRef.json" with { type: "json" };

const like = lx.lexicon("app.bsky.feed.like", {
	main: lx.record({
		key: "tid",
		record: lx.object({
			subject: lx.ref(strongRef, { required: true }),
			createdAt: lx.string({ required: true, format: "datetime" }),
		}),
	}),
});

type Like = (typeof like)["~infer"];
// {
//   $type: "app.bsky.feed.like"
//   subject: { $type: "com.atproto.repo.strongRef"; uri: string; cid: string }
//   createdAt: string
// }
```

Pass `def` to pick a non-`main` definition out of a multi-def lexicon (e.g. `app.bsky.feed.defs`):

```ts
import feedDefs from "./lexicons/app/bsky/feed/defs.json" with { type: "json" };

lx.ref(feedDefs, { def: "postView", required: true });
// runtime: { type: "ref", ref: "app.bsky.feed.defs#postView", required: true }
// type:    $type is "app.bsky.feed.defs#postView" with postView's fields
```

At runtime the ref is stored as the (fully-qualified) NSID string, so emit and validation are unchanged — the JSON object and `def` option are only consumed by the type system.
