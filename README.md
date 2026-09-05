# Mandor

Primary engineering agent untuk OpenCode yang memegang context pekerjaan dalam satu session dan menggunakan skills secara on-demand.

## Konsep

Mandor sebelumnya memakai rantai subagent untuk design, implementasi, review, security, dan memory. Karena subagent tidak berbagi context, rantai tersebut membutuhkan handoff panjang dan sering menghasilkan interpretasi ulang, scope creep, serta siklus revisi berulang.

Arsitektur sekarang memakai satu primary agent:

```text
User -> Mandor -> skill yang relevan -> implementasi -> verifikasi -> selesai
```

Skills menambahkan pedoman tanpa memindahkan pekerjaan ke session baru. Mandor tetap memegang requirement, hasil eksplorasi, keputusan, perubahan kode, dan bukti verifikasi.

## Workflow

- **Quick (default):** inspect terarah, implementasi, targeted verification, baca diff sekali.
- **Normal:** ringkasan pendek pendekatan, implementasi, targeted tests, focused self-review.
- **Full:** design eksplisit, implementasi bertahap, verifikasi menyeluruh, focused review, security review bila ada attack surface nyata, maksimal satu remediation pass.

Mandor berhenti ketika acceptance criteria terpenuhi. Workflow tidak boleh naik level hanya karena ada kemungkinan teoritis atau karena sebuah skill tersedia.

## Skills

Capability utama sudah tersedia sebagai skills:

- design: `spec-driven-development`, `planning-and-task-breakdown`, `api-and-interface-design`
- implementation: `incremental-implementation`
- debugging: `debugging-and-error-recovery`
- testing: `test-driven-development`
- review: `code-review-and-quality`
- security: `security-and-hardening`
- memory/rules/tools reference: `project-memory`

Mandor load maksimal satu skill utama pada satu waktu. Skill kedua digunakan hanya bila domain berbeda dan benar-benar memengaruhi hasil. Lifecycle skill tidak dijalankan berantai secara otomatis.

## Subagent

Subagent bukan bagian workflow utama. Subagent hanya boleh dipakai secara opsional untuk pekerjaan independen yang menghasilkan informasi, seperti eksplorasi module terpisah, pencarian dokumentasi, atau pemeriksaan read-only paralel.

Design, implementasi, review, perbaikan, dan memory tidak dipindahkan antar-subagent.

## Review dan Security

Finding blocking harus mempunyai bukti konkret, jalur kejadian yang reachable, dan dampak material terhadap scope. Risiko hipotetis, style preference, masalah lama di luar diff, dan refactor opsional tidak memblokir hasil.

Security review hanya digunakan ketika perubahan menyentuh attack surface nyata seperti auth, permission, secret, crypto, injection boundary, upload, deserialization, external command, pembayaran, atau data sensitif. Packet/network code dan input parsing tidak otomatis memerlukan audit penuh.

Maksimal satu remediation pass. Temuan baru di luar scope dilaporkan sebagai follow-up dan tidak otomatis dikerjakan.

## Memory

Memory bersifat lazy dan dikelola langsung oleh Mandor melalui skill `project-memory`.

- Tidak ada persisted todo.
- `.mandor/` tidak wajib ada pada project baru.
- Rules wajib dibaca satu kali sebelum mutation pertama dan dibaca ulang setelah compaction.
- Memory project dimuat secara terarah untuk area yang akan diubah.
- Update dilakukan sekali sebelum respons final untuk perubahan material.
- Pernyataan user yang jelas berlaku lintas task disimpan sebagai rule pada turn yang sama.
- Tools reference tidak di-fetch saat startup; digunakan hanya ketika schema runtime tidak cukup jelas.

Direktori `.mandor/` tetap lokal dan diabaikan Git.

## Struktur

```text
.opencode/
  opencode.jsonc
  agents/
    mandor.md
  commands/
  references/
    definition-of-done.md
  skills/
    project-memory/
    code-review-and-quality/
    security-and-hardening/
    ...
```

## Safety

Keputusan material seperti perubahan architecture/public contract, dependency/database, security boundary, destructive action, atau operasi eksternal tetap memerlukan konfirmasi. Commit, push, merge, tag, release, deploy, dan migration membutuhkan approval spesifik.

Setelah mengubah agent, command, atau skill, restart OpenCode agar konfigurasi baru dimuat.

## License

[MIT](./LICENSE), Copyright (c) 2026 Rafie Hasannudin.
