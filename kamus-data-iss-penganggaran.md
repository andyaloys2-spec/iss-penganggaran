Saya belum bisa langsung menaruh file `.md` itu ke repo `andyaloys2-spec/iss-penganggaran` dari sisi saya saat ini. Tapi kamu bisa membuatnya sendiri dalam 1 menit lewat GitHub UI.

## Buat file Markdown di repo (tanpa terminal)
1) Buka: `https://github.com/andyaloys2-spec/iss-penganggaran`
2) Klik **Add file → Create new file**
3) Isi nama file: `docs/kamus-data-iss-penganggaran.md`
4) Copy–paste isi file Markdown di bawah ini
5) Klik **Commit changes** (branch `main`)

````markdown name=docs/kamus-data-iss-penganggaran.md
# Kamus Data (Data Dictionary)
## Aplikasi Integrated Support System (ISS) – Modul Penganggaran

**Tanggal:** 2026-03-30  
**Ruang lingkup:** RBAC, master referensi, register pinjaman/hibah, usulan & versioning, rincian bulanan (YYYY-MM), workflow per tahap (TM/SBPI/SBPA/HLM/OUTLOOK), validasi, Berita Acara (BA) + lampiran + metadata, output/report, integrasi, audit trail.

---

# Konvensi
- **PK**: Primary Key  
- **FK**: Foreign Key  
- **Null**: `YES` berarti boleh kosong  

---

## 1. ORGANIZATIONS
**Deskripsi:** Master instansi/unit pengguna (K/L, DJPPR, DJA).

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| org_id | int | PK | NO |  | ID organisasi |
| org_type | varchar |  | NO |  | Jenis organisasi: `KL`, `DJPPR`, `DJA` |
| org_code | varchar |  | YES |  | Kode organisasi |
| org_name | varchar |  | NO |  | Nama organisasi |
| is_active | bit |  | NO |  | Status aktif |
| created_at | datetime |  | NO |  | Waktu dibuat |

---

## 2. USERS
**Deskripsi:** Akun pengguna aplikasi.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| user_id | int | PK | NO |  | ID user |
| org_id | int | FK | NO | ORGANIZATIONS.org_id | Organisasi user |
| role_id | int | FK | NO | ROLES.role_id | Role user |
| username | varchar |  | NO |  | Username login |
| full_name | varchar |  | NO |  | Nama lengkap |
| email | varchar |  | YES |  | Email |
| mfa_status | varchar |  | NO |  | `ENABLED` / `DISABLED` |
| is_active | bit |  | NO |  | Status aktif |
| last_login_at | datetime |  | YES |  | Login terakhir |
| created_at | datetime |  | NO |  | Waktu dibuat |

---

## 3. ROLES
**Deskripsi:** Role untuk RBAC.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| role_id | int | PK | NO |  | ID role |
| role_name | varchar |  | NO |  | Nama role |
| access_level | varchar |  | NO |  | `FULL/HIGH/STANDARD/LIMITED` |
| description | varchar |  | YES |  | Deskripsi role |
| is_active | bit |  | NO |  | Status aktif |

---

## 4. PERMISSIONS
**Deskripsi:** Aksi yang bisa dilakukan per modul.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| permission_id | int | PK | NO |  | ID permission |
| module_name | varchar |  | NO |  | Nama modul |
| action | varchar |  | NO |  | `CREATE/EDIT/SUBMIT/VALIDATE/APPROVE/LOCK/DOWNLOAD/GENERATE/SYNC` |
| description | varchar |  | YES |  | Deskripsi |

---

## 5. ROLE_PERMISSIONS
**Deskripsi:** Penghubung many-to-many role ↔ permission.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| role_permission_id | int | PK | NO |  | ID |
| role_id | int | FK | NO | ROLES.role_id | Role |
| permission_id | int | FK | NO | PERMISSIONS.permission_id | Permission |

---

## 6. REF_CURRENCIES
**Deskripsi:** Referensi mata uang (mengacu referensi CDS).

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| currency_code | varchar | PK | NO |  | Kode (IDR, USD, dst) |
| currency_name | varchar |  | NO |  | Nama mata uang |
| is_active | bit |  | NO |  | Status aktif |

---

## 7. REGISTERS
**Deskripsi:** Register pinjaman/hibah (level register).

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| register_id | int | PK | NO |  | ID register |
| org_id | int | FK | NO | ORGANIZATIONS.org_id | Pemilik (K/L) |
| register_no | varchar |  | NO |  | Nomor register (unik) |
| financing_type | varchar |  | NO |  | `LOAN/GRANT` |
| project_name | varchar |  | NO |  | Nama proyek |
| currency_code | varchar | FK | NO | REF_CURRENCIES.currency_code | Mata uang register |
| plafond_amount | decimal |  | YES |  | Plafon/komitmen |
| effective_date | date |  | YES |  | Tanggal efektif |
| closing_date | date |  | YES |  | Tanggal penutupan |
| status | varchar |  | NO |  | `ACTIVE/CLOSED/CANCELLED` |
| created_at | datetime |  | NO |  | Waktu dibuat |
| updated_at | datetime |  | YES |  | Waktu update |

---

## 8. BUDGET_CYCLES
**Deskripsi:** Siklus penganggaran.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| cycle_id | int | PK | NO |  | ID cycle |
| fiscal_year | int |  | NO |  | Tahun anggaran |
| cycle_type | varchar |  | NO |  | `MEDIUM_TERM/ANNUAL/MONTHLY` |
| start_date | date |  | YES |  | Mulai |
| end_date | date |  | YES |  | Selesai |
| status | varchar |  | NO |  | `OPEN/LOCKED/CLOSED` |
| created_at | datetime |  | NO |  | Waktu dibuat |

---

## 9. REF_BUDGET_STAGES
**Deskripsi:** Tahap: TM, SBPI, SBPA, HLM, OUTLOOK.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| stage_id | int | PK | NO |  | ID stage |
| stage_code | varchar |  | NO |  | `TM/SBPI/SBPA/HLM/OUTLOOK` |
| stage_name | varchar |  | NO |  | Nama tahap |
| stage_order | int |  | NO |  | Urutan tahap |
| is_active | bit |  | NO |  | Status aktif |

---

## 10. BUDGET_PROPOSALS
**Deskripsi:** Header usulan penganggaran.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| proposal_id | int | PK | NO |  | ID proposal |
| register_id | int | FK | NO | REGISTERS.register_id | Register basis |
| cycle_id | int | FK | NO | BUDGET_CYCLES.cycle_id | Siklus |
| horizon | varchar |  | NO |  | `MEDIUM_TERM/MONTHLY` |
| proposal_kind | varchar |  | NO |  | `WITHDRAWAL/RMP/WITHDRAWAL_AND_RMP` |
| current_status | varchar |  | NO |  | Status umum |
| current_version_no | int |  | NO |  | Versi terakhir |
| submitted_at | datetime |  | YES |  | Waktu submit |
| created_at | datetime |  | NO |  | Waktu buat |
| updated_at | datetime |  | YES |  | Waktu update |

---

## 11. PROPOSAL_VERSIONS
**Deskripsi:** Riwayat perubahan (versioning).

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| version_id | int | PK | NO |  | ID versi |
| proposal_id | int | FK | NO | BUDGET_PROPOSALS.proposal_id | Proposal |
| version_no | int |  | NO |  | Nomor versi |
| created_by_user_id | int | FK | NO | USERS.user_id | Pembuat versi |
| change_reason | varchar |  | YES |  | Alasan perubahan |
| is_finalized | bit |  | NO |  | Final atau tidak |
| created_at | datetime |  | NO |  | Waktu dibuat |

---

## 12. PROPOSAL_MONTHLY_LINES
**Deskripsi:** Rincian bulanan (YYYY-MM) berisi total penarikan & RMP.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| line_id | int | PK | NO |  | ID line |
| version_id | int | FK | NO | PROPOSAL_VERSIONS.version_id | Versi |
| year_month | char(7) |  | NO |  | `YYYY-MM` |
| currency_code | varchar | FK | NO | REF_CURRENCIES.currency_code | Mata uang |
| total_withdrawal_amount | decimal |  | NO |  | Total penarikan |
| rmp_amount | decimal |  | NO |  | RMP per bulan |
| notes | varchar |  | YES |  | Catatan |
| created_at | datetime |  | NO |  | Waktu dibuat |

---

## 13. BUDGET_STAGE_SNAPSHOTS
**Deskripsi:** Snapshot per tahap (approval per tahap), menunjuk versi tertentu.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| snapshot_id | int | PK | NO |  | ID snapshot |
| proposal_id | int | FK | NO | BUDGET_PROPOSALS.proposal_id | Proposal |
| stage_id | int | FK | NO | REF_BUDGET_STAGES.stage_id | Tahap |
| based_on_version_id | int | FK | NO | PROPOSAL_VERSIONS.version_id | Versi acuan |
| stage_status | varchar |  | NO |  | `DRAFT/SUBMITTED/IN_REVIEW/APPROVED/REJECTED/LOCKED` |
| locked_by_user_id | int | FK | YES | USERS.user_id | Pengunci |
| locked_at | datetime |  | YES |  | Waktu lock |
| created_at | datetime |  | NO |  | Waktu dibuat |

---

## 14. WORKFLOW_STAGE_STATUS
**Deskripsi:** Master status valid per tahap (opsional).

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| stage_status_id | int | PK | NO |  | ID |
| stage_id | int | FK | NO | REF_BUDGET_STAGES.stage_id | Tahap |
| status_code | varchar |  | NO |  | Kode status |
| description | varchar |  | YES |  | Deskripsi |

---

## 15. WORKFLOW_ACTIONS
**Deskripsi:** Log aksi workflow pada snapshot.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| action_id | int | PK | NO |  | ID aksi |
| snapshot_id | int | FK | NO | BUDGET_STAGE_SNAPSHOTS.snapshot_id | Snapshot |
| performed_by_user_id | int | FK | NO | USERS.user_id | Pelaku |
| resulting_stage_status_id | int | FK | YES | WORKFLOW_STAGE_STATUS.stage_status_id | Status hasil |
| action_type | varchar |  | NO |  | `SUBMIT/VALIDATE/APPROVE/REJECT/REQUEST_REVISION/LOCK/UNLOCK` |
| remarks | varchar |  | YES |  | Catatan |
| performed_at | datetime |  | NO |  | Waktu aksi |

---

## 16. REF_VALIDATION_RULES
**Deskripsi:** Master rule validasi otomatis.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| rule_id | int | PK | NO |  | ID rule |
| rule_code | varchar |  | NO |  | Kode rule |
| rule_name | varchar |  | NO |  | Nama rule |
| severity | varchar |  | NO |  | `INFO/WARN/ERROR` |
| description | varchar |  | YES |  | Deskripsi |
| is_active | bit |  | NO |  | Aktif |

---

## 17. VALIDATION_RESULTS
**Deskripsi:** Hasil eksekusi rule validasi pada snapshot.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| validation_result_id | int | PK | NO |  | ID |
| snapshot_id | int | FK | NO | BUDGET_STAGE_SNAPSHOTS.snapshot_id | Snapshot |
| rule_id | int | FK | NO | REF_VALIDATION_RULES.rule_id | Rule |
| executed_by_user_id | int | FK | YES | USERS.user_id | Eksekutor |
| result | varchar |  | NO |  | `PASS/FAIL` |
| message | varchar |  | YES |  | Pesan |
| validated_at | datetime |  | NO |  | Waktu validasi |

---

## 18. MEETING_MINUTES (BA)
**Deskripsi:** Berita Acara per siklus dan per tahap.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| ba_id | int | PK | NO |  | ID BA |
| cycle_id | int | FK | NO | BUDGET_CYCLES.cycle_id | Siklus |
| stage_id | int | FK | NO | REF_BUDGET_STAGES.stage_id | Tahap BA |
| ba_number | varchar |  | NO |  | Nomor BA |
| meeting_date | date |  | NO |  | Tanggal rapat |
| status | varchar |  | NO |  | `DRAFT/FINAL` |
| created_by_user_id | int | FK | NO | USERS.user_id | Pembuat |
| finalized_by_user_id | int | FK | YES | USERS.user_id | Finalizer |
| created_at | datetime |  | NO |  | Dibuat |
| finalized_at | datetime |  | YES |  | Difinalkan |

---

## 19. MEETING_MINUTE_ITEMS
**Deskripsi:** Item BA yang mereferensikan proposal.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| ba_item_id | int | PK | NO |  | ID item |
| ba_id | int | FK | NO | MEETING_MINUTES.ba_id | BA |
| proposal_id | int | FK | NO | BUDGET_PROPOSALS.proposal_id | Proposal |
| discussion_summary | varchar |  | YES |  | Ringkasan pembahasan |
| decision_summary | varchar |  | YES |  | Ringkasan keputusan |

---

## 20. DOCUMENT_ATTACHMENTS
**Deskripsi:** Lampiran BA + metadata + TTE.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| attachment_id | int | PK | NO |  | ID attachment |
| ba_id | int | FK | NO | MEETING_MINUTES.ba_id | BA |
| uploaded_by_user_id | int | FK | NO | USERS.user_id | Pengunggah |
| file_name | varchar |  | NO |  | Nama file |
| file_type | varchar |  | NO |  | `PDF/XLSX/DOCX/OTHER` |
| storage_uri | varchar |  | NO |  | Lokasi file |
| sha256 | varchar |  | YES |  | Hash |
| file_size_bytes | bigint |  | YES |  | Ukuran |
| is_signed_electronically | bit |  | NO |  | Ada TTE |
| signed_by | varchar |  | YES |  | Penanda tangan |
| signed_at | datetime |  | YES |  | Waktu tanda tangan |
| uploaded_at | datetime |  | NO |  | Waktu upload |

---

## 21. REPORT_TEMPLATES
**Deskripsi:** Template output.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| template_id | int | PK | NO |  | ID |
| template_code | varchar |  | NO |  | Kode |
| template_name | varchar |  | NO |  | Nama |
| output_format | varchar |  | NO |  | `XLSX/PDF/CSV` |
| is_active | bit |  | NO |  | Aktif |

---

## 22. GENERATED_REPORTS
**Deskripsi:** Hasil generate laporan.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| report_id | int | PK | NO |  | ID |
| template_id | int | FK | NO | REPORT_TEMPLATES.template_id | Template |
| cycle_id | int | FK | NO | BUDGET_CYCLES.cycle_id | Siklus |
| stage_id | int | FK | YES | REF_BUDGET_STAGES.stage_id | Tahap |
| generated_by_user_id | int | FK | NO | USERS.user_id | Generator |
| parameters_json | nvarchar(max) |  | YES |  | Parameter |
| storage_uri | varchar |  | NO |  | Lokasi file |
| generated_at | datetime |  | NO |  | Waktu |

---

## 23. INTEGRATION_SOURCES
**Deskripsi:** Master sumber integrasi.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| source_id | int | PK | NO |  | ID |
| source_code | varchar |  | NO |  | `ISS_PM/DMFAS/SLDK_SPAN/CDS` |
| source_name | varchar |  | NO |  | Nama |
| auth_type | varchar |  | NO |  | `API_KEY/OAUTH/TOKEN` |
| is_active | bit |  | NO |  | Aktif |

---

## 24. INTEGRATION_JOBS
**Deskripsi:** Job sinkronisasi.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| job_id | int | PK | NO |  | ID job |
| source_id | int | FK | NO | INTEGRATION_SOURCES.source_id | Sumber |
| job_type | varchar |  | NO |  | `SYNC_REGISTER/SYNC_CURRENCY/SYNC_OTHER` |
| status | varchar |  | NO |  | `QUEUED/RUNNING/SUCCESS/FAILED/RETRYING` |
| started_at | datetime |  | YES |  | Mulai |
| finished_at | datetime |  | YES |  | Selesai |

---

## 25. INTEGRATION_JOB_LOGS
**Deskripsi:** Log job integrasi.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| job_log_id | int | PK | NO |  | ID log |
| job_id | int | FK | NO | INTEGRATION_JOBS.job_id | Job |
| level | varchar |  | NO |  | `INFO/WARN/ERROR` |
| message | nvarchar(max) |  | NO |  | Pesan |
| created_at | datetime |  | NO |  | Waktu |

---

## 26. AUDIT_LOGS
**Deskripsi:** Audit trail.

| Kolom | Tipe | Key | Null | Referensi | Deskripsi |
|---|---|---|---|---|---|
| audit_id | bigint | PK | NO |  | ID audit |
| user_id | int | FK | NO | USERS.user_id | Pelaku |
| entity_type | varchar |  | NO |  | Nama entitas |
| entity_id | int |  | NO |  | ID entitas |
| action | varchar |  | NO |  | Aksi |
| before_data | nvarchar(max) |  | YES |  | Data sebelum |
| after_data | nvarchar(max) |  | YES |  | Data sesudah |
| ip_address | varchar |  | YES |  | IP |
| created_at | datetime |  | NO |  | Waktu |

---

# Constraints & Index (ringkas)
- `REGISTERS.register_no` unik  
- `PROPOSAL_VERSIONS (proposal_id, version_no)` unik  
- `PROPOSAL_MONTHLY_LINES (version_id, year_month)` unik  
- `BUDGET_STAGE_SNAPSHOTS (proposal_id, stage_id)` unik (jika 1 snapshot aktif per tahap)
````
