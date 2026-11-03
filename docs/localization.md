# Desktop UI localisation

The desktop app offers Thai (**ไทย**, `th`), Simplified Chinese (**简体中文**,
`zh-hans`) and Traditional Chinese (**繁體中文**, `zh-hant`), alongside English,
French and Japanese, in **Settings → General → Language**. The choice is saved
in the existing `language` setting. **System** uses the operating system's
preferred supported language; if none matches, it uses English. An explicit
choice takes precedence over the system language.

Thai locale tags such as `th` and `th-TH` select Thai.

Chinese locales without a script, including `zh`, `zh-CN` and `zh-SG`, select
Simplified Chinese. `zh-TW`, `zh-HK` and `zh-MO` select Traditional Chinese.
An explicit `Hans` or `Hant` script takes precedence over the region. Locale
matching accepts underscores, case differences and Unix encoding suffixes.

## Scope and format

`crates/ui-egui/src/i18n/th.tsv`, `zh-hans.tsv` and `zh-hant.tsv` contain
translations under the repository's MIT OR Apache-2.0 licence. Each UTF-8 line
has two columns: `English source<TAB>translation`. Comments begin with `#`;
`\n` represents a newline. The language registry in `i18n.rs` embeds the catalogs.

The complete Thai and Chinese catalogs cover first run, new projects, project
controls, Settings, the workbench, tool titles, tool groups, target
descriptions, tooltips and accessibility labels. The command palette searches
translated tool titles as well as English titles and stable tool IDs. Missing
translations in any language fall back to the original English text. Product
and technology names retain their spelling.

User text, AI responses, evidence and raw tool results, tool summaries and schema
descriptions, logs and exported reports retain their original text. The CLI,
MCP, tool IDs, stored project data and the separate browser viewer are unchanged.

Keep placeholders such as `{name}` and `{n}` intact, along with command IDs,
shortcuts, escapes and trailing `…`. Update every complete catalog when English
source keys change. Thai uses โหมด Clean room (Clean room), โหมด Deep (Deep),
สร้าง (Build), หลักฐาน (Evidence), and โปรเจกต์ (Project). Chinese uses 净室 /
淨室, 深度, 构建 / 建置, 证据 / 證據, and 项目 / 專案.

## Validation

Run:

```sh
cargo test -p craft-anything-ui-egui
cargo test -p craft-anything-ai
cargo test -p xtask
cargo clippy -p craft-anything-ui-egui -p craft-anything-ai -p xtask --all-targets -- -D warnings
cargo xtask i18n-coverage
```

Catalog tests check duplicate and empty entries, matching complete-catalog key
sets, placeholders and ellipses. The coverage command checks the Thai and both
Chinese catalogs against literal translation calls, common widget labels and
registered engine tool titles and groups; it also runs in `cargo xtask ci`. It
does not prove coverage of arbitrary runtime text or labels held in tables, so
review those when adding or changing UI code.

The existing snapshot example accepts `--lang th`, `--lang zh-hans` or
`--lang zh-hant`, for example:

```sh
cargo run -p craft-anything-ui-egui --example snapshot -- --lang th --only settings --out-dir target/snapshots/th
```

Thai and Chinese glyphs use the existing craft-fonts fallback, which release
and CI builds already include. Local builds must set `CRAFT_FONTS_DIR` to a
craft-fonts checkout before compiling to get the same glyph coverage. The
catalogs add no font assets. Check Thai and both Chinese scripts, clipping,
narrow windows and native file dialogs visually on each platform; catalog
coverage alone does not verify layout or fonts.
