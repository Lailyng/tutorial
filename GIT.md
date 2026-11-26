# Tutorial Git & GitHub via SSH

Panduan singkat untuk membuat SSH key, menghubungkan komputer lokal ke GitHub menggunakan SSH, menyalin (*clone*) repository, serta mengunggah (*push*) folder/project baru dari lokal ke GitHub.

---

## 1. Membuat SSH Key & Menambahkan ke GitHub

Jika belum memiliki SSH key di komputer ini, ikuti langkah berikut:

### Langkah 1: Generate SSH Key
Buka terminal (PowerShell atau Git Bash), lalu jalankan:

```bash
ssh-keygen -t ed25519 -C "email-github@example.com"
```

- Saat muncul `Enter file in which to save the key`:
  - Tekan **Enter** untuk memakai lokasi default (`~/.ssh/id_ed25519`), **atau**
  - Ketik nama file khusus jika memakai lebih dari satu akun, misalnya `~/.ssh/id_ed25519_lailyng`.
- Saat muncul `Enter passphrase`: tekan **Enter** dua kali jika tidak ingin memakai password tambahan (atau isi passphrase jika ingin lebih aman).

Perintah ini akan menghasilkan 2 file di folder `~/.ssh/`:
- `id_ed25519` → **Private key** (rahasia, jangan dibagikan ke siapa pun).
- `id_ed25519.pub` → **Public key** (yang akan ditambahkan ke GitHub).

### Langkah 2: Salin Public Key (`.pub`)
Tampilkan atau salin isi file `.pub`:

- **PowerShell (langsung copy ke clipboard):**
  ```powershell
  Get-Content ~/.ssh/id_ed25519.pub | Set-Clipboard
  ```
- **Git Bash / Terminal biasa:**
  ```bash
  cat ~/.ssh/id_ed25519.pub
  ```
  *(Salin seluruh teks yang muncul mulai dari `ssh-ed25519 ...`)*

### Langkah 3: Tambahkan ke Akun GitHub
1. Buka [GitHub SSH Keys Settings](https://github.com/settings/keys).
2. Klik **New SSH key**.
3. Isi **Title** (misalnya `Laptop Windows`).
4. Biarkan **Key type** pada `Authentication Key`.
5. Tempel (*paste*) public key ke kolom **Key**.
6. Klik **Add SSH key**.

### Langkah 4: Atur `~/.ssh/config` (Jika Menggunakan Nama File Khusus)
Jika saat generate kamu memakai nama file selain `id_ed25519` (misalnya `id_ed25519_lailyng`), buat atau edit file `~/.ssh/config` lalu tambahkan:

```sshconfig
Host Lailyng github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_lailyng
    IdentitiesOnly yes
```

---

## 2. Cek Koneksi SSH ke GitHub

Pastikan koneksi SSH ke GitHub sudah aktif dengan menjalankan:

```bash
ssh -T git@github.com
```

Atau menggunakan alias host:

```bash
ssh -T Lailyng
```

Jika berhasil, akan muncul pesan:

```text
Hi Lailyng! You've successfully authenticated, but GitHub does not provide shell access.
```

> **Opsional (sekali saja):** Pastikan nama dan email Git sudah diatur agar riwayat commit tercatat dengan benar:
>
> ```bash
> git config --global user.name "Nama Kamu"
> git config --global user.email "email-github@example.com"
> ```

---

## 3. Copy (Clone) Repository dari GitHub ke Lokal

Jika repository sudah ada di GitHub dan ingin disalin ke komputer lokal:

1. Buka folder tujuan di terminal.
2. Jalankan perintah `git clone` dengan URL SSH:

```bash
git clone git@github.com:Lailyng/nama-repo.git
```

3. Masuk ke folder repository yang baru di-clone:

```bash
cd nama-repo
```

---

## 4. Membuat Folder Baru di Lokal & Push ke GitHub

Jika kamu membuat folder/project baru di lokal dan ingin mengunggahnya ke GitHub:

### Langkah 1: Buat Repository Kosong di GitHub
1. Buka [GitHub](https://github.com/new) dan buat repository baru.
2. Isi **Repository name** (misalnya `project-baru`).
3. **Jangan** centang *Add a README*, *.gitignore*, atau *license* agar repository benar-benar kosong.
4. Klik **Create repository**, lalu salin URL SSH-nya (`git@github.com:Lailyng/project-baru.git`).

### Langkah 2: Inisialisasi Git di Folder Lokal
```bash
# 1. Buat folder baru dan masuk ke dalamnya
mkdir project-baru
cd project-baru

# 2. Inisialisasi repository Git lokal
git init

# 3. Buat file pertama (contoh)
echo "# Project Baru" > README.md

# 4. Tambahkan semua file ke staging area
git add .

# 5. Simpan perubahan (commit)
git commit -m "Initial commit"
```

### Langkah 3: Hubungkan ke GitHub & Push
```bash
# 6. Pastikan nama branch utama adalah main
git branch -M main

# 7. Hubungkan folder lokal ke repository GitHub
git remote add origin git@github.com:Lailyng/project-baru.git

# 8. Upload (push) ke GitHub
git push -u origin main
```

> **Tips:** Jika ingin mengecek apakah remote sudah terhubung dengan benar, jalankan `git remote -v`.

---

## 5. Push Perubahan Selanjutnya

Setelah setup awal selesai, setiap kali ada perubahan file di lokal cukup jalankan 3 perintah berikut:

```bash
git add .
git commit -m "Deskripsi perubahan"
git push
```

Untuk menarik (*pull*) perubahan terbaru dari GitHub ke lokal:

```bash
git pull
```
