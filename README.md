# Mandor

Primary agent config untuk OpenCode yang berperan sebagai Project Manager - mengoordinasikan subagent untuk menyelesaikan task user.

## Deskripsi

Mandor adalah konfigurasi agent untuk OpenCode yang mengadopsi pendekatan multi-agent. Alih-alih satu agent tunggal yang mencoba mengerjakan semuanya, Mandor berperan sebagai koordinator (Project Manager) yang mendelegasikan pekerjaan ke subagent sesuai keahlian masing-masing: perancangan arsitektur, penulisan kode, review, audit keamanan, dan manajemen memory project.

Pendekatan ini cocok untuk developer yang ingin membangun project kompleks dengan bantuan AI, tetapi ingin menjaga pemisahan tanggung jawab antar peran - sehingga setiap subagent fokus pada satu domain. Mandor memastikan workflow tetap terstruktur: rancangan dibuat sebelum kode ditulis, setiap perubahan dicatat ke memory project, dan pemilihan mode eksekusi disesuaikan dengan preferensi user.

## Fitur Utama

- 6 agents: 1 primary (Mandor sebagai koordinator) + 5 subagent spesialis (`project-design`, `write-code`, `code-reviewer`, `security-auditor`, `memorize`)
- 8 slash commands untuk workflow umum (build, review, ship, spec, test, dll)
- 24 skill yang dapat direkomendasikan oleh Mandor ke subagent sesuai kebutuhan task
- Workflow wajib: design-before-code, memory persistence setelah perubahan, sinkronisasi status todo antar subagent, pemilihan mode eksekusi di awal sesi
- Sistem rules: user menetapkan aturan operasional yang disimpan di `.mandor/agents/rules.md` dan dipatuhi semua agent; mandor tanya user saat instruksi bertolak belakang dengan aturan aktif
- Disable 4 agent default OpenCode (`plan`, `build`, `general`, `explore`) - hanya pakai agent kustom Mandor
- Memory project tersimpan lokal (di-gitignore, tidak dipublikasi)

## Struktur Direktori

```text
.opencode/
  opencode.jsonc        # config utama: schema, subagent_depth, disable agent default
  agents/
    mandor.md           # primary agent - Project Manager, koordinator
    project-design.md   # subagent - merancang arsitektur sistem
    write-code.md       # subagent - menulis/mengedit/memperbaiki kode
    code-reviewer.md    # subagent - review kode 5-axis
    security-auditor.md # subagent - audit keamanan mendalam
    memorize.md         # subagent - manajemen memory project
  commands/
    build.toml          # implementasi inkremental: build, test, verify, commit
    code-simplify.toml  # sederhanakan kode tanpa ubah behavior
    planning.toml       # pecah kerja jadi task kecil verifiable
    review.toml         # code review 5-axis
    ship.toml           # pre-launch checklist + go/no-go decision
    spec.toml           # spec-driven development
    test.toml           # TDD workflow
    webperf.toml        # web performance audit
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
| `project-design` | subagent | Merancang arsitektur sistem | Wajib dipanggil sebelum `write-code` untuk kode baru |
| `write-code` | subagent | Menulis/mengedit/memperbaiki kode | Menerima konteks lengkap dari mandor; sinkronkan status todo ke file shared via memorize; update memory setelah perubahan |
| `code-reviewer` | subagent | Review kode menyeluruh | 5 axis: correctness, readability, architecture, security, performance |
| `security-auditor` | subagent | Audit keamanan mendalam | Vulnerability, threat modeling, hardening |
| `memorize` | subagent | Manajemen memory project | Satu-satunya pemilik direktori `.mandor/` (memory + rules.md); kelola rules (Save/Load/Remove) |

## Commands

| Command | Deskripsi |
|---|---|
| `/build` | Implementasi inkremental: build, test, verify, commit. Argumen `auto` jalankan seluruh plan dengan satu approval |
| `/code-simplify` | Sederhanakan kode untuk clarity tanpa mengubah behavior |
| `/planning` | Pecah kerja jadi task kecil verifiable dengan acceptance criteria + dependency ordering |
| `/review` | Code review 5-axis: correctness, readability, architecture, security, performance |
| `/ship` | Pre-launch checklist via parallel fan-out specialist persona, lalu go/no-go decision |
| `/spec` | Spec-driven development: tulis spec terstruktur sebelum kode |
| `/test` | TDD workflow: failing test, implement, verify. Untuk bug pakai Prove-It pattern |
| `/webperf` | Web performance audit via web-performance-auditor persona |

> Catatan: "persona" pada `/ship` dan `/webperf` adalah peran inline di dalam file command, bukan subagent terpisah. Total tetap 6 agent.

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
   Mandor menyapa user, inisialisasi tools reference (memorize fetch docs OpenCode),
   load memory + load rules, dan tanyakan pilihan mode eksekusi.

2. Pilih mode
   User pilih: Mode Delegasi (default) atau Mode Langsung.

3. Eksekusi task (Mode Delegasi)
   a. project-design    -> rancang arsitektur (wajib sebelum kode baru)
   b. write-code        -> implementasi kode berdasarkan rancangan
   c. code-reviewer     -> review 5-axis (opsional, sesuai kebutuhan)
   d. security-auditor  -> audit keamanan (opsional, sesuai kebutuhan)
   e. memorize          -> catat perubahan ke memory project (wajib setelah batch perubahan)
   f. write-code        -> sinkronkan status todo ke file shared via memorize (wajib setelah batch)

4. Respond ke user
   Mandor rangkum hasil delegasi dan laporkan balik.
```

Sistem rules: jika user menetapkan aturan, mandor simpan ke `.mandor/agents/rules.md` via memorize; jika instruksi bertolak belakang dengan aturan aktif, mandor tanya user (1x saja / seterusnya / jangan jalankan).

## Mode Eksekusi

Mandor menyediakan dua mode eksekusi yang dipilih di awal sesi:

- **Mode Delegasi (default)**: Mandor hanya berperan sebagai koordinator. Semua implementasi, review, dan audit didelegasikan ke subagent sesuai keahlian. Memory project tetap dikelola oleh `memorize`. Mode ini memberikan pemisahan tanggung jawab yang jelas.

- **Mode Langsung**: Mandor mengerjakan task sendiri tanpa delegasi ke subagent, kecuali update memory dan pengelolaan rules yang tetap didelegasikan ke `memorize`. Cocok untuk task kecil yang tidak memerlukan spesialisasi subagent.

## License

[MIT](./LICENSE), Copyright (c) 2026 Rafie Hasannudin.
