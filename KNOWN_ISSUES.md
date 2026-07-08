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

**Belum dipastikan.** Dugaan:

1. **Platform Overload** - Vercel sedang mengalami antrean panjang
2. **Token/Permission Issue** - Token CLI atau permission project bermasalah
3. **Configuration Error** - `vercel.json` atau environment variables issue
4. **Platform Bug** - Bug sementara di Vercel infrastructure

### Hypothesis

- **Confidence:** <80%
- **Evidence:** Deployment sebelumnya (12h) berhasil, tidak ada error eksplisit, build log kosong
- **Likelihood:** Platform overload > Permission issue > Config error > Platform bug

### Recovery Attempts

| Attempt | Time | Action | Result | Notes |
|---------|------|--------|--------|-------|
| 1 | 8 Juli 00:36 | Monitor 30s | No change | Status tetap QUEUED |
| 2 | 8 Juli 00:37 | Monitor 60s | No change | Status tetap QUEUED |
| 3 | 8 Juli 00:39 | Monitor 120s | No change | Status tetap QUEUED |
| 4 | 8 Juli 00:45 | `vercel inspect` | Confirmed QUEUED | Build log kosong |

**Total Attempts:** 4 (monitoring only, no active recovery yet)

### Result

**BLOCKED** - Deployment stuck di QUEUED >4 jam, tidak ada progress.

### Lessons Learned

1. Deployment method MANUAL_CLI berarti tidak ada auto-retry dari GitHub webhooks
2. Project Vercel dimiliki team `tajmincmp`, bukan `dianaTJM` - permission tied to specific user
3. Deployment stuck >30 menit mengindikasikan issue serius di platform atau configuration
4. Build log kosong (0ms) menandakan deployment tidak pernah mulai build - kemungkinan antrean atau permission issue

### Next Action

**Menunggu keputusan user** untuk salah satu:

1. **Cancel & Redeploy** - Hapus deployment stuck, buat baru (recommended)
2. **Force Redeploy** - Coba deploy ulang dengan `--force` flag
3. **Manual Investigation** - Akses Vercel Dashboard, cek token & permission
4. **Wait Longer** - Monitor setiap 30 menit, escalate jika >6 jam

**Confidence:** <80% - Human intervention required (Rule IX)

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
