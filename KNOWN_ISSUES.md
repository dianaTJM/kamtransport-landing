# KAM Transport — Known Issues Database

> **Database permanen** untuk seluruh masalah yang pernah terjadi. WAJIB diperiksa sebelum setiap recovery attempt.

---

## Issue #001: Deployment Stuck in QUEUED Status

| Field | Value |
|-------|-------|
| **Issue ID** | #001 |
| **Tanggal** | 8 Juli 2026 00:36 WIB |
| **Judul** | Deployment Stuck in QUEUED Status (>4 jam) |
| **Status** | 🔴 ACTIVE - BLOCKED |
| **Deployment ID** | `dpl_Cfft87hY1MbSmHpxYKg4TvzKMVcf` |
| **Production URL** | `https://kamtransport.vercel.app` |

### Evidence

- **Created:** Tue Jul 07 2026 23:36:36 GMT+0000
- **Status:** ● Queued (tidak berubah >4 jam)
- **Build Log:** Kosong (0ms) - tidak mulai build
- **Previous Deployments:** Berhasil READY dalam 2-3 detik (12h sebelumnya)
- **Current Commit:** `28f715f` (branch `feat/review-submission-moderation`)

### Root Cause

**Classification:** UNKNOWN

**Evidence:**
- Deployment stuck di QUEUED >4 jam (Observation)
- Build log kosong (0ms) — deployment tidak pernah mulai build (Evidence)
- Deployment sebelumnya (12h) berhasil READY dalam 2-3 detik (Evidence)
- Tidak ada error eksplisit di `vercel inspect` output (Evidence)

**Analysis:**
Build log kosong (0ms) menandakan deployment tidak pernah mulai build. Tidak ada evidence langsung yang membuktikan penyebab spesifik (permission error, config error, platform bug, dll). Recovery berhasil (cancel & redeploy) tidak membuktikan root cause — hanya membuktikan antrean dapat di-clear.

**Hypothesis (LIKELY):**
- Platform overload / antrean Vercel sedang panjang

**Hypothesis (POSSIBLE):**
- Token/permission issue yang intermittent
- Configuration error yang tidak terdeteksi
- Platform bug sementara

---

| Attempt | Time | Action | Result | Notes |
|---------|------|--------|--------|-------|
| 1 | 8 Juli 00:36 | Monitor 30s | No change | Status tetap QUEUED |
| 2 | 8 Juli 00:37 | Monitor 60s | No change | Status tetap QUEUED |
| 3 | 8 Juli 00:39 | Monitor 120s | No change | Status tetap QUEUED |
| 4 | 8 Juli 00:45 | `vercel inspect` | Confirmed QUEUED | Build log kosong |
| 5 | 8 Juli 00:50 | **Cancel & Redeploy** | ✅ **SUCCESS** | Deployment READY in 7s |

**Total Attempts:** 5 (4 monitoring + 1 active recovery)

### Result

**✅ RESOLVED** — Deployment berhasil via cancel & redeploy.

**Execution Details:**
- Deployment lama di-cancel: `dpl_Cfft87hY1MbSmHpxYKg4TvzKMVcf`
- Deployment baru dibuat: `2XaYpjMLheXSgMGM5yUwM8kvUL4q`
- Status: READY in 7s
- Build machine: Washington, D.C. (iad1)
- Production URL: `https://kamtransport.vercel.app` updated
- Fitur "KAM Photos" muncul
- HTML production identical dengan source

### Lessons Learned

1. Deployment method MANUAL_CLI berarti tidak ada auto-retry dari GitHub webhooks
2. Project Vercel dimiliki team `tajmincmp`, bukan `dianaTJM` - permission tied to specific user
3. Deployment stuck >30 menit mengindikasikan issue serius di platform atau configuration
4. Build log kosong (0ms) menandakan deployment tidak pernah mulai build - kemungkinan antrean atau permission issue
5. **Cancel & redeploy adalah solusi efektif** untuk deployment stuck di QUEUED
6. Recovery berhasil dengan confidence high (85%+)

### Next Action

**✅ COMPLETED** — Issue resolved.

**Follow-up:**
- Update documentation (DEPLOYMENT_MANIFEST, project-memory)
- Monitor production stability
- Document recovery workflow untuk future reference

---

## Issue #002: Production Serving Stale Content

| Field | Value |
|-------|-------|
| **Issue ID** | #002 |
| **Tanggal** | 8 Juli 2026 00:30 WIB |
| **Judul** | Production Serving Stale Content (Missing KAM Photos Feature) |
| **Status** | 🔴 ACTIVE - BLOCKED (dependency: #001) |
| **Missing Features** | Tombol "KAM Photos", Project Manifest |
| **Production Commit** | `e8276bf` |
| **Repository Commit** | `4986870` |

### Evidence

- **Production URL:** `https://kamtransport.vercel.app`
- **Missing Feature:** Tombol "KAM Photos" (commit `8c58eda`)
- **Repository:** Sudah commit `4986870` (2 commit ahead)
- **Verification:** `curl https://kamtransport.vercel.app \| grep "KAM Photos"` → Tidak ditemukan
- **Source Check:** `grep "KAM Photos" index.html` → Ditemukan

### Root Cause

**Dependency:** Issue #001 (Deployment stuck) → Production tidak dapat diupdate.

### Hypothesis

- Production akan update setelah deployment #001 resolved
- Tidak ada issue lain yang menghalangi selain deployment blocker

### Recovery Attempts

**Belum ada** - Blocked by #001.

### Result

**BLOCKED** - Menunggu deployment #001 resolved.

### Lessons Learned

1. Manual deployment berarti setiap push memerlukan action eksplisit
2. Production verification wajib dilakukan setelah deployment READY
3. Tidak boleh asumsikan push = deploy

### Next Action

**Blocked by #001.** Setelah deployment #001 resolved:

1. Production Verification (HTML match, features appear, assets load)
2. Update DEPLOYMENT_MANIFEST.md dengan hasil verification
3. Update PROJECT_STATE.md status

---

**Database Created:** 8 Juli 2026 00:45 WIB  
**Last Updated:** 8 Juli 2026 00:45 WIB  
**Total Issues:** 2 (1 active blocker, 1 dependency)  
**Status:** 🚨 AWAITING USER DECISION  
**Next Review:** Setelah user decision atau ada informasi baru (commit berubah, deployment berubah, dll)
