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
| `json`      | lib-json       | JSON codec + `json.parse`/`json.stringify`       |
| `serialize` | lib-serialize  | format-agnostic Serializer/Encoder/Decoder port  |
| `str-js`    | lib-str-js     | `str` ports backend via `js.call`                |

These are the *existing* modules a worker program needs on the JS host:
the interop surface plus the data-marshalling plumbing. The Worker
binding itself is **not yet available** — see Pending.

## Pending / known gaps

- **`lib-worker` does not exist.** The proposal's `worker` module
  (spawn / postMessage / onmessage / terminate over `js.call`, `self`
  side for worker-compiled entries, optional SharedArrayBuffer/Atomics)
  is greenfield. It is deliberately **not** listed in `kit.hnl`.
- **Host blocker**: the host has no `worker.spawn` primitive, and there
  is no Worker WebIDL spec yet — the path is Worker specs →
  `tool-sdkgen-webidl` → a `lib-js`-style generated package + the host
  `worker.spawn` Rust primitive.
- **JS→handl callback**: same gap as kit-web-browser-iterop — no
  `on_event`/listener registration in `lib-js`; worker `onmessage`
  needs the host injection or JS-side trampoline.
- **Optional later**: `lib-concurrent-js` backend implementing
  `concurrent.Run` over MessageChannel/setTimeout so the task model
  runs in-browser.
- **`lib-json-js`**: no JS-generated JSON backend yet (`json` above is
  the portable codec); planned as a generated package inside lib-js.
- The proposal's `plugins: ["compile-js"]` + `glue` are dropped under
  kit.hnl v1; `engine-compile-js` browser targets remain host-side.
- **Validation**: kit.hnl has no loader/gate yet — manifest correctness
  is by inspection against the lib repos' `pkg.hnl`.

## License

MIT.
