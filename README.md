# Mandor

Primary agent config untuk OpenCode yang berperan sebagai Project Manager - mengoordinasikan subagent untuk menyelesaikan task user.

## Deskripsi

Mandor adalah konfigurasi agent untuk OpenCode yang mengadopsi pendekatan multi-agent. Alih-alih satu agent tunggal yang mencoba mengerjakan semuanya, Mandor berperan sebagai koordinator (Project Manager) yang mendelegasikan pekerjaan ke subagent sesuai keahlian masing-masing: perancangan arsitektur, penulisan kode, review, audit keamanan, dan manajemen memory project.

Pendekatan ini cocok untuk developer yang ingin membangun project dengan bantuan AI sambil menjaga pemisahan tanggung jawab antar peran. Mandor memilih kedalaman workflow sesuai risiko: pekerjaan kecil berjalan singkat, sedangkan perubahan kompleks tetap memakai design, review, audit, dan memory yang terstruktur.

## Fitur Utama

- 6 agents: 1 primary (Mandor sebagai koordinator) + 5 subagent spesialis (`project-design`, `write-code`, `code-reviewer`, `security-auditor`, `memorize`)
- 7 slash commands OpenCode berbasis Markdown untuk workflow umum (build, review, ship, spec, test, dll)
- 24 skill yang dapat direkomendasikan oleh Mandor ke subagent sesuai kebutuhan task
- Workflow adaptif tiga tingkat (`Quick`, `Normal`, `Full`), memory terarah, review proporsional, Doxygen compliance saat relevan, user confirmation hard-stop, dan pemilihan mode eksekusi di awal sesi
- Sistem rules: user menetapkan aturan operasional yang disimpan di `.mandor/agents/rules.md` dan dipatuhi semua agent; mandor tanya user saat instruksi bertolak belakang dengan aturan aktif
- Least-privilege runtime permissions: memory edit di-scope ke `.mandor/**`, specialist task fan-out dibatasi, dan shell berisiko memerlukan approval
- Disable 4 agent default OpenCode (`plan`, `build`, `general`, `explore`) - hanya pakai agent kustom Mandor
- Memory project tersimpan lokal (di-gitignore, tidak dipublikasi)

## Struktur Direktori

```text
.opencode/
  opencode.jsonc        # config utama: schema, default_agent, subagent_depth, disable agent default
  agents/
    mandor.md           # primary agent - Project Manager, koordinator
    project-design.md   # subagent - merancang arsitektur sistem
    write-code.md       # subagent - menulis/mengedit/memperbaiki kode
    code-reviewer.md    # subagent - review kode six-axis + Doxygen gate
    security-auditor.md # subagent - audit keamanan mendalam
    memorize.md         # subagent - manajemen memory project
  commands/
    build.md            # implementasi inkremental; commit butuh approval terpisah
    code-simplify.md    # sederhanakan kode tanpa ubah behavior
    planning.md         # pecah kerja jadi task kecil verifiable
    review.md           # code review six-axis + Doxygen gate
    ship.md             # readiness checklist + go/no-go, bukan deploy
    spec.md             # spec-driven development
    test.md             # TDD workflow
  references/
    definition-of-done.md # completion gate bersama
  skills/               # 24 direktori, masing-masing berisi SKILL.md
    api-and-interface-design/
    browser-testing-with-devtools/
    ci-cd-and-automation/
    code-review-and-quality/
    code-simplification/
    context-engineering/
    debugging-and-error-recovery/
    deprecation-and-migration/
    documentation-and-adrs/
    doubt-driven-development/
    frontend-ui-engineering/
    git-workflow-and-versioning/
    idea-refine/
    incremental-implementation/
    interview-me/
    observability-and-instrumentation/
    performance-optimization/
    planning-and-task-breakdown/
    security-and-hardening/
    shipping-and-launch/
    source-driven-development/
    spec-driven-development/
    test-driven-development/
    using-agent-skills/
```

> Catatan: direktori `.mandor/` (memory project lokal + rules) di-exclude oleh `.gitignore` dan tidak dipublikasi ke repository.

## Quick Start

```bash
# 1. Clone repo ini
git clone <url-repo-ini>
cd mandor

# 2. Salin direktori .opencode/ ke root project Anda
cp -r .opencode/ /path/to/your-project/

# 3. Jalankan opencode di root project Anda
cd /path/to/your-project
opencode
```

Setelah `opencode` berjalan, Mandor otomatis menjadi primary agent. Mandor akan menyapa, menginisialisasi tools reference, dan menanyakan pilihan mode eksekusi (Delegasi atau Langsung).

## Agents

| Agent | Mode | Peran | Catatan Kunci |
|---|---|---|---|
| `mandor` | primary | Project Manager, koordinator | Tidak menulis kode sendiri (Mode Delegasi default); mendelegasikan ke subagent. Temperature 0.1 |
| `project-design` | subagent | Merancang arsitektur sistem | Wajib pada Full; digunakan pada Normal bila keputusan desain/handoff memberi nilai |
| `write-code` | subagent | Menulis/mengedit/memperbaiki kode | Shell default `ask`; read-only Git di-allow; task hanya `memorize` atau `write-code` dengan approval |
| `code-reviewer` | subagent | Review kode menyeluruh | 6 axis + Doxygen gate; edit deny, bash ask, task hanya `memorize` |
| `security-auditor` | subagent | Audit keamanan mendalam | Edit deny, bash ask, task hanya `memorize` |
| `memorize` | subagent | Manajemen konteks project | Kelola memory, todo, rules, dan query terarah tools reference di `.mandor/**` |

## Commands

| Command | Deskripsi |
|---|---|
| `/build` | Implementasi inkremental: build, test, verify. Commit selalu membutuhkan approval terpisah |
| `/code-simplify` | Sederhanakan kode untuk clarity tanpa mengubah behavior |
| `/planning` | Pecah kerja jadi task kecil verifiable dengan acceptance criteria + dependency ordering |
| `/review` | Code review 6-axis dengan Doxygen compliance gate |
| `/ship` | Readiness assessment melalui code review + security review, lalu GO/NO-GO; tidak menjalankan deploy |
| `/spec` | Spec-driven development: tulis spec terstruktur sebelum kode |
| `/test` | TDD workflow: failing test, implement, verify. Untuk bug pakai Prove-It pattern |

`/ship` menggunakan dua spesialis yang benar-benar tersedia (`code-reviewer` dan `security-auditor`) pada Mode Delegasi. Tidak ada agent fiktif `test-engineer` atau `web-performance-auditor`.

## Skills

24 skill tersedia. Mandor wajib merekomendasikan skill yang relevan di prompt delegasi ke subagent.

| Skill | Deskripsi Singkat |
|---|---|
| `api-and-interface-design` | Desain API dan boundary antar modul |
| `browser-testing-with-devtools` | Testing di browser real via Chrome DevTools MCP |
| `ci-cd-and-automation` | Setup/modifikasi build dan deployment pipeline |
| `code-review-and-quality` | Review kode multi-axis sebelum merge |
| `code-simplification` | Refactor kode untuk clarity tanpa ubah behavior |
| `context-engineering` | Optimasi context setup untuk agent |
| `debugging-and-error-recovery` | Root-cause debugging sistematis |
| `deprecation-and-migration` | Manajemen deprecation dan migrasi |
| `documentation-and-adrs` | Catat keputusan arsitektur dan dokumentasi |
| `doubt-driven-development` | Adversarial review sebelum keputusan penting |
| `frontend-ui-engineering` | Build UI production-quality, accessible, responsive |
| `git-workflow-and-versioning` | Struktur git workflow, branching, versioning |
| `idea-refine` | Refine ide mentah jadi konsep actionable |
| `incremental-implementation` | Deliver perubahan inkremental lintas file |
| `interview-me` | Ekstrak intent user via one-question-at-a-time |
| `observability-and-instrumentation` | Instrumentasi logging, metrics, tracing |
| `performance-optimization` | Optimasi performance frontend, backend, query, DB |
| `planning-and-task-breakdown` | Pecah kerja jadi task terurut verifiable |
| `security-and-hardening` | Hardening terhadap vulnerability |
| `shipping-and-launch` | Pre-launch checklist, monitoring, rollback strategy |
| `source-driven-development` | Implementasi berbasis dokumentasi resmi |
| `spec-driven-development` | Tulis spec sebelum kode |
| `test-driven-development` | Drive development dengan test |
| `using-agent-skills` | Discover dan invoke skill yang relevan |

## Workflow

Alur kerja Mandor dari awal sesi sampai respond ke user:

```text
1. Initial chat
   Mandor menginisialisasi tools reference melalui memorize, load memory + rules,
   lalu selalu menanyakan pilihan mode eksekusi.

2. Pilih mode
   User pilih: Mode Delegasi (default) atau Mode Langsung.

3. Mandor memilih tingkat workflow
   a. Quick  -> implementasi, verifikasi relevan, self-review
   b. Normal -> design ringan bila perlu, implementasi, targeted test/review
   c. Full   -> design formal, persisted todo, implementasi, six-axis review,
               security audit bila sensitif, remediation, sinkronisasi memory

4. Respond ke user
   Mandor rangkum hasil delegasi dan laporkan balik.
```

Sistem rules: jika user menetapkan aturan, mandor simpan ke `.mandor/agents/rules.md` via memorize; jika instruksi bertolak belakang dengan aturan aktif, mandor tanya user (1x saja / seterusnya / jangan jalankan).

## Mode Eksekusi

Mandor menyediakan dua mode eksekusi yang dipilih di awal sesi:

- **Mode Delegasi (default)**: Mandor hanya berperan sebagai koordinator. Semua implementasi, review, dan audit didelegasikan ke subagent sesuai keahlian. Memory project tetap dikelola oleh `memorize`. Mode ini memberikan pemisahan tanggung jawab yang jelas.

- **Mode Langsung**: Mandor mengerjakan implementasi, review, dan audit sendiri tanpa delegasi spesialis; update memory dan rules tetap melalui `memorize`. Mode ini dapat dipilih untuk scope apa pun oleh user dan tidak dipilih otomatis berdasarkan ukuran task.

## Prinsip Operasional

### User confirmation hard-stop

Keputusan penting ditentukan oleh dampaknya, bukan oleh apakah agent merasa bingung. Pilihan stack/dependency/database, public contract, architecture, namespace utama, security boundary, penghapusan, tindakan destructive, perubahan behavior di luar requirement, material trade-off, external operation, dan perluasan scope harus berhenti sampai user memberi keputusan eksplisit. Subagent mengembalikan `DECISION REQUIRED`; Mandor menjadi pintu utama untuk bertanya kepada user.

Commit, push, merge, tag, release, dan deploy tidak pernah tersirat oleh approval implementasi. `/build` meminta approval commit secara terpisah setelah perubahan dan bukti verifikasi tersedia.

### Memory-first

Mandor memuat memory melalui `memorize` dan meneruskan ringkasan relevan ke subagent. Subagent tidak memindai ulang seluruh codebase bila konteks sudah tersedia, tetapi tetap membaca file aktual yang akan diedit atau direview. Konflik memory dengan file aktual wajib dilaporkan; file aktual menjadi bukti kondisi terbaru dan memory disinkronkan setelah perubahan.

`tools-reference.md` dikelola melalui `memorize` Mode 5 (`Refresh` dan `Query`). Reference digunakan secara terarah saat Mandor atau subagent perlu memilih tool, memahami parameter/permission, atau membedakan tool yang mirip. Hanya section yang relevan yang dibaca dan diteruskan; tool rutin tidak memicu lookup berulang. Schema tool runtime tetap menjadi sumber terbaru bila berbeda dari reference.

### Permission hardening

`memorize` hanya dapat mengedit `.mandor/**`. `project-design`, `code-reviewer`, dan `security-auditor` hanya dapat meluncurkan `memorize`; recursive `write-code` tetap memerlukan approval. Pada Mandor dan `write-code`, shell default adalah `ask`, sedangkan perintah Git read-only yang terdaftar dapat berjalan tanpa prompt. Permission runtime melengkapi confirmation hard-stop dan tidak menggantikannya.

### Namespace/class-first

Implementasi diprioritaskan di dalam namespace, class, struct, package, atau module dengan boundary yang jelas. Class dipakai bila memberi enkapsulasi, ownership, polymorphism, dependency management, atau manfaat struktural nyata; wrapper class kosong dilarang. Penyimpangan struktural penting memerlukan persetujuan user.

### Doxygen

Untuk public API bahasa C-like dalam scope Doxygen, format default adalah Javadoc-style `/** ... */` dengan `@brief` dan tag relevan yang cocok dengan signature/behavior aktual. Documentation comment, implementation comment, file-level documentation, dan komentar config/markup diperlakukan terpisah. `write-code` memiliki completion gate, sedangkan `code-reviewer` memberi `PASS`, `FAIL`, atau reasoned `N/A`; setiap `FAIL` adalah Important dan menghasilkan `REQUEST CHANGES`.

Setelah mengubah config, agent, command, atau skill, restart OpenCode agar konfigurasi baru dimuat.

## License

[MIT](./LICENSE), Copyright (c) 2026 Rafie Hasannudin.
