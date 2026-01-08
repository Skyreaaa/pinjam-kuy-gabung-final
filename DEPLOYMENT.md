# 🚀 Panduan Deployment PinjamKuy

Aplikasi **PinjamKuy** adalah sistem peminjaman buku berbasis web dengan fitur QR Code, notifikasi real-time, dan manajemen denda.

## 📦 Teknologi

- **Frontend**: React + TypeScript
- **Backend**: Node.js + Express
- **Database**: MySQL
- **Storage**: Cloudinary (untuk upload gambar)
- **Real-time**: Socket.IO

## 🌐 Deployment

### Option 1: Railway (Recommended - All in One)

**Railway** menyediakan database MySQL gratis dan hosting backend/frontend.

#### A. Deploy Database + Backend ke Railway

1. **Buat akun di Railway**: https://railway.app
2. **New Project** → **Deploy from GitHub repo**
3. **Connect repository** ini
4. **Add MySQL Database**:
   - Di dashboard Railway, klik **New** → **Database** → **MySQL**
   - Railway akan generate credentials otomatis
5. **Setup Backend Service**:
   - Klik **New** → **GitHub Repo** → pilih repo ini
   - Set **Root Directory**: `be-pinjam-rev-main`
   - Set **Start Command**: `npm start`
   - Add **Environment Variables** (ambil dari `.env.example`):
     ```
     NODE_ENV=production
     DB_HOST=<dari MySQL service>
     DB_PORT=3306
     DB_USER=<dari MySQL service>
     DB_PASSWORD=<dari MySQL service>
     DB_DATABASE=<dari MySQL service>
     JWT_SECRET=<generate random string>
     SESSION_SECRET=<generate random string>
     CLOUDINARY_CLOUD_NAME=<dari cloudinary.com>
     CLOUDINARY_API_KEY=<dari cloudinary.com>
     CLOUDINARY_API_SECRET=<dari cloudinary.com>
     CORS_ORIGINS=<URL frontend nanti>
     ```
6. **Import Database Schema**:
   - Connect ke MySQL Railway menggunakan MySQL Workbench atau command line
   - Import file: `be-pinjam-rev-main/sql/pinjam-kuy-clean.sql`
   - Atau jalankan migrations di folder `be-pinjam-rev-main/sql/migrations/`

#### B. Deploy Frontend ke Vercel

1. **Buat akun di Vercel**: https://vercel.com
2. **Import Git Repository** → pilih repo ini
3. **Configure Project**:
   - Framework Preset: `Create React App`
   - Build Command: `npm run build`
   - Output Directory: `build`
4. **Environment Variables**:
   ```
   REACT_APP_API_BASE_URL=<URL backend dari Railway>/api
   ```
   Contoh: `https://your-backend-url.railway.app/api`
5. **Deploy** → tunggu build selesai
6. **Update CORS di Backend**:
   - Kembali ke Railway backend settings
   - Update environment variable `CORS_ORIGINS` dengan URL Vercel Anda
   - Contoh: `https://your-app.vercel.app`

---

### Option 2: Render (Alternative)

#### Backend + Database ke Render

1. **Buat akun di Render**: https://render.com
2. **New** → **PostgreSQL** (free tier) atau gunakan external MySQL
3. **New** → **Web Service** → connect GitHub repo
4. Set **Root Directory**: `be-pinjam-rev-main`
5. Build Command: `npm install`
6. Start Command: `npm start`
7. Add Environment Variables (sama seperti di Railway)
8. Deploy dan dapatkan URL backend

#### Frontend ke Netlify

1. **Buat akun di Netlify**: https://netlify.com
2. **Add new site** → **Import from Git**
3. Build command: `npm run build`
4. Publish directory: `build`
5. Add Environment Variable:
   ```
   REACT_APP_API_BASE_URL=<URL backend dari Render>/api
   ```

---

## 🔐 Generate Secrets untuk Production

Gunakan command ini untuk generate JWT_SECRET dan SESSION_SECRET yang kuat:

```bash
# Linux/Mac/Git Bash
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"

# PowerShell
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
```

---

## ✅ Checklist Deployment

- [ ] Database di-setup dan schema sudah diimport
- [ ] Backend deployed dan running (cek health endpoint: `/api/health` jika ada)
- [ ] Environment variables backend sudah diisi semua
- [ ] Frontend deployed
- [ ] `REACT_APP_API_BASE_URL` di frontend sudah diset ke backend URL
- [ ] CORS di backend sudah diset ke frontend URL
- [ ] Cloudinary credentials valid
- [ ] Test login user
- [ ] Test upload gambar
- [ ] Test fitur peminjaman dan pengembalian

---

## 🔧 Troubleshooting

### CORS Error
- Pastikan `CORS_ORIGINS` di backend environment sudah berisi URL frontend
- Format: `https://your-app.vercel.app` (tanpa trailing slash)

### Database Connection Error
- Cek credentials database (host, user, password, database name)
- Pastikan database sudah diimport schema-nya

### Upload Image Gagal
- Cek Cloudinary credentials
- Cek quota Cloudinary (free tier ada limit)

### JWT Error / Login Gagal
- Pastikan `JWT_SECRET` sudah diset di backend environment

---

## 📝 Environment Variables Summary

### Backend (.env)
```env
NODE_ENV=production
DB_HOST=
DB_PORT=3306
DB_USER=
DB_PASSWORD=
DB_DATABASE=
JWT_SECRET=
SESSION_SECRET=
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
CORS_ORIGINS=
```

### Frontend (.env)
```env
REACT_APP_API_BASE_URL=https://your-backend-url.com/api
```

---

## 📞 Support

Jika ada masalah, cek:
- Logs di Railway/Render dashboard
- Browser console untuk error frontend
- Network tab untuk API call issues

Good luck! 🎉
