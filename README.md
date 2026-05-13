# amalgame-framework-encoding

Pure-Amalgame encoding facade for [Amalgame](https://github.com/amalgame-lang/Amalgame).
Three stateless helpers: **Base64** (RFC 4648), **Hex**, **percent-encoding** (RFC 3986).

Originally bundled in amc's `src/stdlib/encoding.am`; extracted into
this external package as part of the framework split (post-v0.7.5).

## Install

```bash
amc package add encoding                    # via the curated index
amc package add github.com/amalgame-lang/amalgame-framework-encoding@v0.1.0
```

Requires **amc 0.7.6+** for the facade pre-compile pipeline
(PR #377). On install, amc compiles `facade.am` into
`~/.amalgame/packages/.../build/<platform>/libamalgame-pkg-Encoding.a`,
and subsequent `amc test` / `amc -o` consume that archive at link
time without re-parsing the facade source.

## Surface

```amalgame
import Amalgame.Encoding

class Program {
    public static void Main() {
        // ── Base64 ───────────────────────────────────
        let s: string = "Hello, world!"
        let b64: string = Encoding.Base64Encode(s)
        Console.WriteLine(b64)                       // SGVsbG8sIHdvcmxkIQ==
        let back: string = Encoding.Base64Decode(b64)
        Console.WriteLine(back)                      // Hello, world!

        // ── Hex ──────────────────────────────────────
        let hex: string = Encoding.HexEncode("AB")   // 4142
        let bytes: List<int> = Encoding.HexDecode(hex)

        // ── Percent-encoding (URL) ───────────────────
        let url: string = Encoding.UrlEncode("hello world")   // hello%20world
        let decoded: string = Encoding.UrlDecode(url)
    }
}
```

See `facade.am` for the full method list — `Base64Encode` /
`Base64Decode` / `Base64IsValid` / `Base64EncodeUrlSafe` /
`Base64DecodeUrlSafe`, `HexEncode` / `HexDecode` / `HexIsValid`,
`UrlEncode` / `UrlEncodeComponent` / `UrlDecode`.

## Tests

```bash
./tests/run_tests.sh /path/to/amc
```

The runner compiles the smoke test using amc, links it against the
per-package archive built at install time, and checks the expected
stdout.

## License

Apache-2.0 — see `LICENSE`. No vendored third-party code.
