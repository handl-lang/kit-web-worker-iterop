<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->

# Agent guide — kit-web-worker-iterop

Manifest-only kit repo: `kit.hnl` is a **kit.hnl v1** pure-curation
manifest (`{ name, description, license, libs }` — no plugins, no
entry/glue; see `host/project/proposals/proposal-0004-*`). There is no
code here and no gate script yet — validation is pending loader work.

## Layout

- `kit.hnl` — the manifest. `libs` entries are module names per each
  lib's `pkg.hnl` `modules:` field.

## Curated libs (verified against pkg.hnl)

| module      | repo           | provides                                        |
|-------------|----------------|-------------------------------------------------|
| `js`        | lib-js         | JS interop — `call`/`invoke`/`await`, `JS` grade |
| `concurrent`| lib-concurrent | cooperative concurrency contract (step-arrow)   |
| `concurrent-js` | lib-concurrent-js | `concurrent` backend over `js.call`/`js.construct` |
| `json`      | lib-json       | JSON codec + `json.parse`/`json.stringify`       |
| `json-js`   | lib-json-js    | `json` backend over the JS `JSON` global         |
| `serialize` | lib-serialize  | format-agnostic Serializer/Encoder/Decoder port  |
| `str-js`    | lib-str-js     | `str` ports backend via `js.call`                |
| `log`       | lib-log        | leveled logging contract (levels, attrs)         |
| `log-js`    | lib-log-js     | `log` backend to `console.*` via the `js` ffi    |

These are the *existing* modules a worker program needs on the JS host:
the interop surface plus the data-marshalling plumbing. The Worker
binding itself is **not yet available** — see Pending.

## Pending / known gaps

- **No `lib-worker` repo** (renamed decision): worker support is
  WebIDL specs → `tool-sdkgen-webidl` → a generated package inside
  `lib-js` (the lib-dom pattern), curated by this kit — not a standalone
  lib. It is deliberately **not** listed in `kit.hnl` yet.
- **Interop primitives**: no Rust `worker.spawn` needed (`new Worker`
  is pure JS); Worker IDL already committed in `lib-js/specs/html.idl`.
  Missing: `js.construct` + `js.on` event trampoline (lib-js decls +
  engine-compile-js prelude shims) — implementation in progress.
- **JS→handl callback**: same gap as kit-web-browser-iterop — no
  `on_event`/listener registration in `lib-js`; worker `onmessage`
  needs the host injection or JS-side trampoline.
- **`lib-concurrent-js`** has landed (module `concurrent-js`, deps
  `concurrent` + `js`) and is listed above — `concurrent.Run` over the
  JS scheduler primitives.
- **`lib-json-js`** has landed as its own package (module `json-js`,
  deps `json` + `js`) and is listed above — `json` remains the portable
  contract.
- The proposal's `plugins: ["compile-js"]` + `glue` are dropped under
  kit.hnl v1; `engine-compile-js` browser targets remain host-side.
- **Validation**: kit.hnl has no loader/gate yet — manifest correctness
  is by inspection against the lib repos' `pkg.hnl`.

## License

MIT.
