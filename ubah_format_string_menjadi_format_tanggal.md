## Mengubah tanggal format string contoh "12 Januari 2020" menjadi "2020-01-12"
1. Buat kolom baru dengan format date di tabel yang dituju
2. Gunakan fungsi replace mengubah format penamaan bulan Bahasa Indonesia menjadi bulan Bahasa Inggris sebagai berikut
```sql
update nama_tabel set kolom2 = STR_TO_DATE(replace( replace( replace( replace( replace( replace( replace( replace( replace(kolom1, "Januari", "January"), "Februari", "February"), "Maret", "March"), "Mei", "May"), "Juni", "June"), "Juli", "July"), "Agustus", "August"), "Oktober", "October"), "Desember", "December"), "%d %M %Y");
```
