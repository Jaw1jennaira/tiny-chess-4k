# Hasil test TinyChess4K

Diuji 13 Nov 2026, Python 3.12.3, pada `main.py` hasil packing.

## Ukuran

- `main.py` (packed): **3.642 byte** (batas 4.096)
- Kode asli (minified): 6.716 byte, SHA-256
  `75eaecd20320f61c167afbdfe077ac9cb855b81d69b3311ef06a7f5b1e6f62b4`
- Total repo ≤ 8.192 byte

## Perft (legalitas)

6 posisi standar, kedalaman 1–4: **semua cocok** dengan angka
perft baku, kecuali pos4/pos5 yang selisihnya persis jumlah
underpromotion (engine promosi hanya ke menteri — legal).
Varian 4-promosi dari kode yang sama lolos 100% (20/20 cek).

## Bermain

- Mate-in-1 `Qh5xf7#` ditemukan (`h5f7`, 422 node).
- Setelah `Qh4#`: 0 langkah legal. Stalemate: 0 langkah, dikenali.
- Self-play (engine vs dirinya, budget 3.000 node): **mat ply
  ke-71, 0 langkah ilegal, 0 crash**.
- UCI: `uci`/`isready`/`position`/`go` benar
  (`bestmove b1c3` dari startpos setelah `e2e4 e7e5`).
- Fake env HTTP lokal (`GET /state` + `POST /move`): game penuh,
  exit 0, tanpa langkah ilegal. Vs lawan pasif, remis 200 ply:
  anti-repetisi v2 memecah loop gajah, tapi engine belum
  konversi keunggulan vs lawan yang mengulur — batasan.

## Kecepatan

Kiwipete: 12.001 node dalam 1,3–2,1 detik (~5.600–9.200 nps).
