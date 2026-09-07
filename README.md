# JajanFlow — Weekly Money Planner

Website budgeting uang jajan mingguan dengan login dan penyimpanan data online menggunakan Supabase.

## Cara menjalankan

1. Buat project di [Supabase](https://supabase.com).
2. Buka **SQL Editor**, lalu jalankan SQL berikut:

```sql
create table public.user_states (
	user_id uuid primary key references auth.users(id) on delete cascade,
	state jsonb not null default '{}'::jsonb,
	updated_at timestamptz not null default now()
);

alter table public.user_states enable row level security;

create policy "Users can read their own state"
on public.user_states for select
using (auth.uid() = user_id);

create policy "Users can insert their own state"
on public.user_states for insert
with check (auth.uid() = user_id);

create policy "Users can update their own state"
on public.user_states for update
using (auth.uid() = user_id)
with check (auth.uid() = user_id);
```

3. Di **Authentication > Providers > Email**, matikan **Confirm email** agar username/password dapat langsung dipakai.
4. Salin **Project URL** dan **anon public key** dari Supabase ke `SUPABASE_URL` dan `SUPABASE_ANON_KEY` di bagian konfigurasi `index.html`.
5. Publikasikan `index.html` ke hosting statis seperti Netlify, Vercel, atau GitHub Pages.

Password tidak disimpan oleh aplikasi; Supabase Auth yang menangani hash dan sesi login. Username diubah menjadi alamat internal agar dapat memakai provider email/password Supabase.

## Default

- Budget: Rp200.000/minggu
- Kebutuhan: 50% = Rp100.000
- Tabungan: 20% = Rp40.000
- Transportasi/bensin: 15% = Rp30.000
- Jajan/makan/minum: 15% = Rp30.000

## Fitur

- Dashboard sisa budget
- Progress pemakaian tiap kategori
- Catat pemasukan/pengeluaran
- Target tabungan
- Pengingat menabung mingguan
- Pengaturan budget & persentase
- Insight sederhana
- Export data JSON
- Responsive untuk HP dan desktop

## Oprek

Semua tampilan, warna, teks, dan logika ada di `index.html`, jadi mudah diedit.
