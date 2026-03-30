# Kamus Data (Data Dictionary)
## Aplikasi Integrated Support System (ISS) – Modul Penganggaran

**Tanggal:** 2026-03-30 09:27:13  
**Format:** dipisah per tabel (per entitas), masing-masing dalam bentuk tabel.

---

### Konvensi
- **PK**: Primary Key
- **FK**: Foreign Key
- **Nullable**: Y = boleh kosong

---

## ORGANIZATIONS
**Deskripsi:** Master instansi/unit pengguna (K/L, DJPPR, DJA).

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| org_id | int | Y | N | N |  | ID organisasi |
| org_type | varchar | N | N | N |  | Jenis organisasi: KL/DJPPR/DJA |
| org_code | varchar | N | N | Y |  | Kode organisasi |
| org_name | varchar | N | N | N |  | Nama organisasi |
| is_active | bit | N | N | N |  | Status aktif |
| created_at | datetime | N | N | N |  | Waktu dibuat |

---

## USERS
**Deskripsi:** Akun pengguna aplikasi.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| user_id | int | Y | N | N |  | ID user |
| org_id | int | N | Y | N | ORGANIZATIONS.org_id | Organisasi user |
| role_id | int | N | Y | N | ROLES.role_id | Role user |
| username | varchar | N | N | N |  | Username login |
| full_name | varchar | N | N | N |  | Nama lengkap |
| email | varchar | N | N | Y |  | Email |
| mfa_status | varchar | N | N | N |  | ENABLED/DISABLED |
| is_active | bit | N | N | N |  | Status aktif |
| last_login_at | datetime | N | N | Y |  | Login terakhir |
| created_at | datetime | N | N | N |  | Waktu dibuat |

---

## ROLES
**Deskripsi:** Role untuk RBAC.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| role_id | int | Y | N | N |  | ID role |
| role_name | varchar | N | N | N |  | Nama role |
| access_level | varchar | N | N | N |  | FULL/HIGH/STANDARD/LIMITED |
| description | varchar | N | N | Y |  | Deskripsi role |
| is_active | bit | N | N | N |  | Status aktif |

---

## PERMISSIONS
**Deskripsi:** Aksi yang bisa dilakukan per modul.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| permission_id | int | Y | N | N |  | ID permission |
| module_name | varchar | N | N | N |  | Nama modul |
| action | varchar | N | N | N |  | CREATE/EDIT/SUBMIT/VALIDATE/APPROVE/LOCK/DOWNLOAD/GENERATE/SYNC |
| description | varchar | N | N | Y |  | Deskripsi |

---

## ROLE_PERMISSIONS
**Deskripsi:** Penghubung many-to-many role ↔ permission.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| role_permission_id | int | Y | N | N |  | ID |
| role_id | int | N | Y | N | ROLES.role_id | Role |
| permission_id | int | N | Y | N | PERMISSIONS.permission_id | Permission |

---

## REF_CURRENCIES
**Deskripsi:** Referensi mata uang (CDS).

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| currency_code | varchar | Y | N | N |  | Kode mata uang (IDR, USD, dst.) |
| currency_name | varchar | N | N | N |  | Nama mata uang |
| is_active | bit | N | N | N |  | Status aktif |

---

## REGISTERS
**Deskripsi:** Register pinjaman/hibah.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| register_id | int | Y | N | N |  | ID register |
| org_id | int | N | Y | N | ORGANIZATIONS.org_id | Pemilik (K/L) |
| register_no | varchar | N | N | N |  | Nomor register (unik) |
| financing_type | varchar | N | N | N |  | LOAN/GRANT |
| project_name | varchar | N | N | N |  | Nama proyek |
| currency_code | varchar | N | Y | N | REF_CURRENCIES.currency_code | Mata uang register |
| plafond_amount | decimal | N | N | Y |  | Plafon/komitmen |
| effective_date | date | N | N | Y |  | Tanggal efektif |
| closing_date | date | N | N | Y |  | Tanggal penutupan |
| status | varchar | N | N | N |  | ACTIVE/CLOSED/CANCELLED |
| created_at | datetime | N | N | N |  | Waktu dibuat |
| updated_at | datetime | N | N | Y |  | Waktu update |

---

## BUDGET_CYCLES
**Deskripsi:** Siklus penganggaran.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| cycle_id | int | Y | N | N |  | ID cycle |
| fiscal_year | int | N | N | N |  | Tahun anggaran |
| cycle_type | varchar | N | N | N |  | MEDIUM_TERM/ANNUAL/MONTHLY |
| start_date | date | N | N | Y |  | Mulai |
| end_date | date | N | N | Y |  | Selesai |
| status | varchar | N | N | N |  | OPEN/LOCKED/CLOSED |
| created_at | datetime | N | N | N |  | Waktu dibuat |

---

## REF_BUDGET_STAGES
**Deskripsi:** Referensi tahap (TM/SBPI/SBPA/HLM/OUTLOOK).

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| stage_id | int | Y | N | N |  | ID stage |
| stage_code | varchar | N | N | N |  | TM/SBPI/SBPA/HLM/OUTLOOK |
| stage_name | varchar | N | N | N |  | Nama tahap |
| stage_order | int | N | N | N |  | Urutan tahap |
| is_active | bit | N | N | N |  | Status aktif |

---

## BUDGET_PROPOSALS
**Deskripsi:** Header usulan penganggaran.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| proposal_id | int | Y | N | N |  | ID proposal |
| register_id | int | N | Y | N | REGISTERS.register_id | Register basis |
| cycle_id | int | N | Y | N | BUDGET_CYCLES.cycle_id | Siklus |
| horizon | varchar | N | N | N |  | MEDIUM_TERM/MONTHLY |
| proposal_kind | varchar | N | N | N |  | WITHDRAWAL/RMP/WITHDRAWAL_AND_RMP |
| current_status | varchar | N | N | N |  | Status umum proposal |
| current_version_no | int | N | N | N |  | Versi terakhir |
| submitted_at | datetime | N | N | Y |  | Waktu submit |
| created_at | datetime | N | N | N |  | Waktu buat |
| updated_at | datetime | N | N | Y |  | Waktu update |

---

## PROPOSAL_VERSIONS
**Deskripsi:** Versioning usulan.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| version_id | int | Y | N | N |  | ID versi |
| proposal_id | int | N | Y | N | BUDGET_PROPOSALS.proposal_id | Proposal |
| version_no | int | N | N | N |  | Nomor versi |
| created_by_user_id | int | N | Y | N | USERS.user_id | Pembuat versi |
| change_reason | varchar | N | N | Y |  | Alasan perubahan |
| is_finalized | bit | N | N | N |  | Penanda versi final |
| created_at | datetime | N | N | N |  | Waktu dibuat |

---

## PROPOSAL_MONTHLY_LINES
**Deskripsi:** Rincian bulanan per versi (YYYY-MM).

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| line_id | int | Y | N | N |  | ID line |
| version_id | int | N | Y | N | PROPOSAL_VERSIONS.version_id | Versi |
| year_month | char(7) | N | N | N |  | Periode YYYY-MM |
| currency_code | varchar | N | Y | N | REF_CURRENCIES.currency_code | Mata uang |
| total_withdrawal_amount | decimal | N | N | N |  | Total penarikan |
| rmp_amount | decimal | N | N | N |  | RMP |
| notes | varchar | N | N | Y |  | Catatan |
| created_at | datetime | N | N | N |  | Waktu dibuat |

---

## BUDGET_STAGE_SNAPSHOTS
**Deskripsi:** Snapshot per tahap + status approval per tahap.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| snapshot_id | int | Y | N | N |  | ID snapshot |
| proposal_id | int | N | Y | N | BUDGET_PROPOSALS.proposal_id | Proposal |
| stage_id | int | N | Y | N | REF_BUDGET_STAGES.stage_id | Tahap |
| based_on_version_id | int | N | Y | N | PROPOSAL_VERSIONS.version_id | Versi acuan |
| stage_status | varchar | N | N | N |  | DRAFT/SUBMITTED/IN_REVIEW/APPROVED/REJECTED/LOCKED |
| locked_by_user_id | int | N | Y | Y | USERS.user_id | Pengunci |
| locked_at | datetime | N | N | Y |  | Waktu lock |
| created_at | datetime | N | N | N |  | Waktu dibuat |

---

## WORKFLOW_STAGE_STATUS
**Deskripsi:** Master status valid per tahap (opsional).

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| stage_status_id | int | Y | N | N |  | ID |
| stage_id | int | N | Y | N | REF_BUDGET_STAGES.stage_id | Tahap |
| status_code | varchar | N | N | N |  | Kode status |
| description | varchar | N | N | Y |  | Deskripsi |

---

## WORKFLOW_ACTIONS
**Deskripsi:** Log aksi workflow.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| action_id | int | Y | N | N |  | ID aksi |
| snapshot_id | int | N | Y | N | BUDGET_STAGE_SNAPSHOTS.snapshot_id | Snapshot |
| performed_by_user_id | int | N | Y | N | USERS.user_id | Pelaku |
| resulting_stage_status_id | int | N | Y | Y | WORKFLOW_STAGE_STATUS.stage_status_id | Status hasil |
| action_type | varchar | N | N | N |  | SUBMIT/VALIDATE/APPROVE/REJECT/REQUEST_REVISION/LOCK/UNLOCK |
| remarks | varchar | N | N | Y |  | Catatan |
| performed_at | datetime | N | N | N |  | Waktu aksi |

---

## REF_VALIDATION_RULES
**Deskripsi:** Master rule validasi otomatis.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| rule_id | int | Y | N | N |  | ID rule |
| rule_code | varchar | N | N | N |  | Kode rule |
| rule_name | varchar | N | N | N |  | Nama rule |
| severity | varchar | N | N | N |  | INFO/WARN/ERROR |
| description | varchar | N | N | Y |  | Deskripsi |
| is_active | bit | N | N | N |  | Aktif |

---

## VALIDATION_RESULTS
**Deskripsi:** Hasil validasi rule pada snapshot.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| validation_result_id | int | Y | N | N |  | ID |
| snapshot_id | int | N | Y | N | BUDGET_STAGE_SNAPSHOTS.snapshot_id | Snapshot |
| rule_id | int | N | Y | N | REF_VALIDATION_RULES.rule_id | Rule |
| executed_by_user_id | int | N | Y | Y | USERS.user_id | Eksekutor |
| result | varchar | N | N | N |  | PASS/FAIL |
| message | varchar | N | N | Y |  | Pesan |
| validated_at | datetime | N | N | N |  | Waktu validasi |

---

## MEETING_MINUTES
**Deskripsi:** Berita Acara (BA).

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| ba_id | int | Y | N | N |  | ID BA |
| cycle_id | int | N | Y | N | BUDGET_CYCLES.cycle_id | Siklus |
| stage_id | int | N | Y | N | REF_BUDGET_STAGES.stage_id | Tahap BA |
| ba_number | varchar | N | N | N |  | Nomor BA |
| meeting_date | date | N | N | N |  | Tanggal rapat |
| status | varchar | N | N | N |  | DRAFT/FINAL |
| created_by_user_id | int | N | Y | N | USERS.user_id | Pembuat |
| finalized_by_user_id | int | N | Y | Y | USERS.user_id | Finalizer |
| created_at | datetime | N | N | N |  | Dibuat |
| finalized_at | datetime | N | N | Y |  | Difinalkan |

---

## MEETING_MINUTE_ITEMS
**Deskripsi:** Item detail BA.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| ba_item_id | int | Y | N | N |  | ID item BA |
| ba_id | int | N | Y | N | MEETING_MINUTES.ba_id | BA |
| proposal_id | int | N | Y | N | BUDGET_PROPOSALS.proposal_id | Proposal |
| discussion_summary | varchar | N | N | Y |  | Ringkasan pembahasan |
| decision_summary | varchar | N | N | Y |  | Ringkasan keputusan |

---

## DOCUMENT_ATTACHMENTS
**Deskripsi:** Lampiran BA.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| attachment_id | int | Y | N | N |  | ID attachment |
| ba_id | int | N | Y | N | MEETING_MINUTES.ba_id | BA |
| uploaded_by_user_id | int | N | Y | N | USERS.user_id | Pengunggah |
| file_name | varchar | N | N | N |  | Nama file |
| file_type | varchar | N | N | N |  | PDF/XLSX/DOCX/OTHER |
| storage_uri | varchar | N | N | N |  | Lokasi file |
| sha256 | varchar | N | N | Y |  | Hash |
| file_size_bytes | bigint | N | N | Y |  | Ukuran |
| is_signed_electronically | bit | N | N | N |  | Ada TTE |
| signed_by | varchar | N | N | Y |  | Penanda tangan |
| signed_at | datetime | N | N | Y |  | Waktu tanda tangan |
| uploaded_at | datetime | N | N | N |  | Waktu upload |

---

## REPORT_TEMPLATES
**Deskripsi:** Template output.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| template_id | int | Y | N | N |  | ID template |
| template_code | varchar | N | N | N |  | Kode template |
| template_name | varchar | N | N | N |  | Nama template |
| output_format | varchar | N | N | N |  | XLSX/PDF/CSV |
| is_active | bit | N | N | N |  | Aktif |

---

## GENERATED_REPORTS
**Deskripsi:** Hasil generate laporan.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| report_id | int | Y | N | N |  | ID laporan |
| template_id | int | N | Y | N | REPORT_TEMPLATES.template_id | Template |
| cycle_id | int | N | Y | N | BUDGET_CYCLES.cycle_id | Siklus |
| stage_id | int | N | Y | Y | REF_BUDGET_STAGES.stage_id | Tahap (opsional) |
| generated_by_user_id | int | N | Y | N | USERS.user_id | Generator |
| parameters_json | nvarchar(max) | N | N | Y |  | Parameter (JSON) |
| storage_uri | varchar | N | N | N |  | Lokasi file |
| generated_at | datetime | N | N | N |  | Waktu generate |

---

## INTEGRATION_SOURCES
**Deskripsi:** Master sumber integrasi.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| source_id | int | Y | N | N |  | ID source |
| source_code | varchar | N | N | N |  | ISS_PM/DMFAS/SLDK_SPAN/CDS |
| source_name | varchar | N | N | N |  | Nama source |
| auth_type | varchar | N | N | N |  | API_KEY/OAUTH/TOKEN |
| is_active | bit | N | N | N |  | Aktif |

---

## INTEGRATION_JOBS
**Deskripsi:** Job sinkronisasi.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| job_id | int | Y | N | N |  | ID job |
| source_id | int | N | Y | N | INTEGRATION_SOURCES.source_id | Source |
| job_type | varchar | N | N | N |  | SYNC_REGISTER/SYNC_CURRENCY/SYNC_OTHER |
| status | varchar | N | N | N |  | QUEUED/RUNNING/SUCCESS/FAILED/RETRYING |
| started_at | datetime | N | N | Y |  | Mulai |
| finished_at | datetime | N | N | Y |  | Selesai |

---

## INTEGRATION_JOB_LOGS
**Deskripsi:** Log detail job.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| job_log_id | int | Y | N | N |  | ID log |
| job_id | int | N | Y | N | INTEGRATION_JOBS.job_id | Job |
| level | varchar | N | N | N |  | INFO/WARN/ERROR |
| message | nvarchar(max) | N | N | N |  | Pesan log |
| created_at | datetime | N | N | N |  | Waktu |

---

## AUDIT_LOGS
**Deskripsi:** Audit trail aktivitas.

| Column | Data Type | PK | FK | Nullable | Reference | Description |
|---|---|---:|---:|---:|---|---|
| audit_id | bigint | Y | N | N |  | ID audit |
| user_id | int | N | Y | N | USERS.user_id | Pelaku |
| entity_type | varchar | N | N | N |  | Nama entitas |
| entity_id | int | N | N | N |  | ID entitas |
| action | varchar | N | N | N |  | Aksi |
| before_data | nvarchar(max) | N | N | Y |  | Data sebelum (JSON) |
| after_data | nvarchar(max) | N | N | Y |  | Data sesudah (JSON) |
| ip_address | varchar | N | N | Y |  | IP |
| created_at | datetime | N | N | N |  | Waktu audit |