# kit-web-worker-iterop

Web Worker interop kit — a `kit.hnl` v1 curation manifest
(`{ name, description, license, libs }`; no plugins, no entry/glue —
see `host/project/proposals/proposal-0004-*`).

Curates the `lib-*` modules that exist today for handl code running on
a JS host in a worker context: the `js` interop surface plus data
plumbing (`json`, `serialize`, `str-js`).

## Manifest

`kit.hnl`:

```hnl
{ name: "kit-web-worker-iterop",
  description: "Web Worker interop for handl-on-JS — JS call/invoke/await surface plus data plumbing (JSON, serialize port, str-js backend). Worker spawn/postMessage binding pending worker WebIDL bindings.",
  license: "MIT",
  libs: ["js", "concurrent", "concurrent-js", "json", "json-js", "serialize", "str-js", "log", "log-js"], }
```

`libs` entries are module names per each lib's `pkg.hnl` `modules:`
field (all verified to exist — see `AGENTS.md` for the table).

## Pending

- **Loader/validation**: kit.hnl v1 has no loader or gate yet — this
  repo intentionally ships no gate script; manifest correctness is by
  inspection. Validation is pending loader work.
- **Worker bindings — LANDED (hand-written, `lib-js/src/worker`)**:
  `js.construct`/`js.on`/`js.off` shims in `engine-compile-js`'s
  `__handl_rt` prelude + `worker.{spawn,spawn_code,post_message,
  terminate,on_message,on_message_self,self_post_message}` module.
  Worker/`DedicatedWorkerGlobalScope`/`MessagePort` IDL already lives
  in `lib-js/specs/html.idl:2657-2810`; durable fix is sdkgen
  constructor/receiver-aware emission (T-backlog), worker module is
  hand-written meanwhile. Still missing: worker-side compile entry
  convention, `onerror` convenience, transfer-list ergonomics,
  SharedArrayBuffer/Atomics.
- **`lib-concurrent-js`**: landed — module `concurrent-js` (deps
  `concurrent` + `js`), `concurrent.Run` over the JS scheduler
  primitives; listed above.
- **`lib-json-js`**: landed as its own package — module `json-js`
  (deps `json` + `js`) over the JS `JSON` global; listed above. `json`
  remains the portable codec.

## License

MIT.
