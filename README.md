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
  description: "Web Worker interop for handl-on-JS — JS call/invoke/await surface plus data plumbing (JSON, serialize port, str-js backend). Worker spawn/postMessage binding pending lib-worker.",
  license: "MIT",
  libs: ["js", "json", "serialize", "str-js"], }
```

`libs` entries are module names per each lib's `pkg.hnl` `modules:`
field (all verified to exist — see `AGENTS.md` for the table).

## Pending

- **Loader/validation**: kit.hnl v1 has no loader or gate yet — this
  repo intentionally ships no gate script; manifest correctness is by
  inspection. Validation is pending loader work.
- **`lib-worker` (planned, not listed)**: the actual Worker binding —
  spawn / `postMessage` / `onmessage` / terminate over `js.call`, a
  `self` side for worker-compiled entries, optional
  SharedArrayBuffer/Atomics. Blocked on:
  1. Worker WebIDL specs (none committed yet — lib-js currently has
     only `dom.idl`/`html.idl`),
  2. `tool-sdkgen-webidl` generation into a lib-js-style package,
  3. a host `worker.spawn` Rust primitive (does not exist today).
- **JS→handl callbacks**: no `on_event`/listener registration exists in
  `lib-js` — worker `onmessage` needs a host injection or JS-side
  trampoline.
- **`lib-concurrent-js`** (optional, planned): `concurrent.Run` backend
  over MessageChannel/setTimeout.
- **`lib-json-js`** (planned): generated JS JSON backend inside lib-js;
  `json` here is the portable codec.

## License

MIT.
