# KAM Transport — Project Manifest

> **Sumber kebenaran tunggal** untuk project ini. Wajib dibaca sebelum bekerja.

## 📋 Project Identity

| Field | Value |
|-------|-------|
| **Project Name** | KAM Transport |
| **Client** | Karya Muda Abadi Transport |
| **Repository Owner** | dianaTJM |
| **Repository Name** | kamtransport-landing |
| **Remote Origin** | `https://github.com/dianaTJM/kamtransport-landing.git` |
| **Default Branch** | `main` |
| **Production Branch** | `feat/review-submission-moderation` |
| **Workspace Path** | `/workspaces/kamtransport-landing` |
| **Entry Point** | `index.html` |
| **Framework** | Static HTML/CSS/JS (no framework) |
| **Build Method** | None (static deployment) |

## 🚀 Deployment Configuration

| Field | Value |
|-------|-------|
| **Deployment Provider** | Vercel |
| **Deployment Method** | Vercel CLI (`vercel --prod`) — **BUKAN GitHub webhook** |
| **Project ID** | `prj_TF7m9opX73tfTBhjaaZ0feWiEA3A` |
| **Team/Organization** | `team_AtMVCnSlsjbkk8PvUpah0XCv` (tajmincmp) |
| **Deployment Owner** | tajin-9233 (via CLI) |
| **Production URL** | `https://kamtransport.vercel.app` |
| **Preview URL Pattern** | `https://kamtransport-<random>-tajmincmp.vercel.app` |
| **Root Directory** | `null` (root repo) |
| **Build Command** | `null` (default) |
| **Output Directory** | `null` (default) |
| **Environment Variables** | None configured |
| **Git Link** | ❌ **Tidak terhubung** (no linked repository) |

## ✅ Verification Method

1. **Production HTML** harus identik dengan `index.html` di commit terbaru branch production.
2. **Asset URLs** harus match dengan struktur repo.
3. **Config hash** di `js/config.js` harus sama.
4. **Visual inspection** via browser atau screenshot.

## 🔄 Recovery Procedure

Jika production tidak match dengan repository:

1. **Pastikan branch production sedang aktif:**
   ```bash
   git checkout feat/review-submission-moderation
   git pull origin feat/review-submission-moderation
   ```

2. **Deploy manual via Vercel CLI:**
   ```bash
   vercel --prod
   ```

3. **Verifikasi deployment:**
   - Cek deployment ID di output
   - Tunggu hingga status READY
   - Akses `https://kamtransport.vercel.app`
   - Bandingkan HTML production dengan source

4. **Jika masih gagal:**
   - Cek Vercel project connection
   - Pastikan token Vercel valid
   - Coba redeploy dengan `vercel --prod --force`

## 📝 Deployment Notes

- Deployment dilakukan **manual via Vercel CLI**, bukan otomatis dari GitHub.
- Setiap push ke branch production **TIDAK otomatis deploy**.
- Harus deploy manual setelah push.
- Project Vercel dimiliki oleh team `tajmincmp`, bukan `dianaTJM`.
- Domain production `kamtransport.vercel.app` terhubung ke project ini.

## 🎓 Lessons Learned

### Insiden Deployment (Juli 2026)

**Masalah:** Perubahan di repository tidak muncul di production.

**Akar Penyebab:**
- Deployment dilakukan via CLI, bukan webhook GitHub.
- Commit terbaru (`8c58eda`) belum pernah di-deploy.
- Deployment terakhir menggunakan commit `e8276bf` (satu commit sebelumnya).

**Dampak:** Fitur tombol "KAM Photos" tidak tampil di production selama >24 jam.

**Solusi:** Deploy manual via `vercel --prod`.

**Pencegahan:**
1. **WAJIB** baca Project Manifest sebelum bekerja.
2. **WAJIB** verifikasi deployment method di Manifest.
3. **WAJIB** lakukan Production Verification setelah push.
4. **JANGAN** asumsikan push = deploy.
5. **JANGAN** asumsikan GitHub webhook aktif.

### SOP Baru

1. **Pre-Work:** Baca Project Manifest → verifikasi deployment method.
2. **Post-Push:** Langsung deploy manual jika method = CLI.
3. **Post-Deploy:** Production Verification (bandingkan HTML).
4. **Final Check:** Hanya READY jika production match source.

---

**Manifest ini dibuat:** 7 Juli 2026  
**Terakhir diverifikasi:** 8 Juli 2026 00:15 WIB  
**Status:** ✅ ACTIVE  
**Deployment saat ini:** ❌ BLOCKED (lihat DEPLOYMENT_MANIFEST.md)
