# TinyChess4K

Chess engine Python ≤ 4 KB untuk bounty make.fun
"Build the strongest chess engine in 4 KB of Python"
(`tiny-chess-01`, $500, deadline 24 Nov 2026 17:00 UTC).
Nama repo yang diusulkan: **`tiny-chess-4k`**

## File

`main.py` (3.633 byte) + `README.md` + `LICENSE` (MIT) + `TESTS.md`.
Total repo ≤ 8.192 byte, tak ada file > 4.096 byte.

## Menjalankan

Mode make.fun (dipakai grader):

```sh
MAKEFUN_ENV_URL=<url env> python3 main.py
```

Loop `POST {url}/game {}` untuk game, kirim langkah via
`POST {url}/move` (`{"game": n, "move": "e2e4"}`). Tanpa env var, engine
masuk mode UCI (`uci`, `isready`, `position ...`, `go`, `quit`).
Python 3.13, standard library saja. Tanpa API key / model call.

## Cara kerja

- Papan 0x88. Aturan lengkap: rokade, en passant (termasuk pin),
  promosi (selalu ke menteri — legal, satu-satunya penyederhanaan),
  skak/mat/stalemate.
- Negamax alpha-beta + iterative deepening (1–7) + quiescence
  khusus tangkapan; urutan langkah MVV-LVA.
- Evaluasi: material + posisi (sentralisasi, pion maju, pion
  bebas, pasangan gajah +30, benteng baris ke-7, raja sembunyi di
  middlegame / aktif di endgame).
- Deterministik: budget tetap **60.000 node/langkah** (bukan jam),
  tanpa randomness. ~1–2 detik/langkah; ~5.600–9.200 node/detik.
- Anti-repetisi di root: posisi yang pernah muncul tak dipilih
  lagi bila ada alternatif.

## Tentang `main.py` terkompresi

Kode asli terbaca (6.671 byte) tak muat di batas 4.096, jadi
`main.py` berisi payload **zlib + base85** + unpacker satu baris
— cara ini diizinkan eksplisit oleh aturan bounty (payload
terkompresi beserta unpacker-nya ikut dihitung). Total 3.633 byte.

Lihat kode aslinya:

```sh
python3 -c "import zlib,base64;print(zlib.decompress(base64.b85decode(open('main.py').read().split(\"'\",2)[1])).decode())"
```

SHA-256 kode asli: lihat `TESTS.md`. Kode terbaca disimpan di
workspace agen untuk perawatan lanjutan.

## Batasan

- Promosi hanya ke menteri; underpromotion tak pernah dipilih.
- Quiescence hanya memeriksa tangkapan (bukan evasi skak).
- Protokol HTTP env ditulis dari brief publik bounty; detail
  field JSON pastinya hanya ada di kit resmi make.fun — sebelum
  submit, cocokkan dengan `RULES.txt` + `starter/main.py` kit.
