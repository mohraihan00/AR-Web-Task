# AR Web Task - Pendekatan yang Digunakan

## Ringkasan
Proyek ini awalnya mencoba pendekatan **WebXR immersive-ar** (kamera + hit-test + penempatan objek 3D).
Namun pada perangkat yang tidak mendukung ARCore/WebXR, kamera AR tidak bisa berjalan stabil (layar hitam/putih).

Karena target pengujian dialihkan ke laptop dan device non-WebXR, implementasi dirombak menjadi:

- **Webcam Overlay AR (tanpa marker)** menggunakan `getUserMedia` + `three.js`
- Model 3D (`.glb`) dirender di atas feed kamera secara real-time
- User tetap mendapat interaksi seperti aplikasi AR: geser, zoom, rotate

## Kenapa Memakai Pendekatan Ini
Pendekatan ini dipilih agar:

1. Tetap bisa jalan di laptop biasa (tanpa ARCore/WebXR)
2. Tetap memakai model 3D asli (`GLB`) di atas kamera
3. Tetap interaktif untuk demo tugas
4. Tetap mudah di-deploy di GitHub Pages

## Arsitektur Teknis Saat Ini
Implementasi utama ada di `index.html`:

1. **Background kamera**
- Browser meminta izin kamera via `navigator.mediaDevices.getUserMedia`
- Stream kamera ditampilkan ke elemen `<video>` full screen

2. **Layer 3D transparan**
- `three.js` dirender pada canvas transparan di atas video
- Scene berisi camera virtual + light + model GLB

3. **Model 3D**
- Dimuat dengan `THREE.GLTFLoader`
- Sumber model: URL raw GitHub

4. **Interaksi user**
- Drag kiri: geser model
- Drag kanan: rotate model
- Scroll mouse: zoom in/out
- Touch 1 jari: geser
- Touch 2 jari pinch: zoom
- Touch 2 jari twist: rotate
- Tombol Reset: kembalikan transform model ke default

## Catatan Penting
Implementasi ini adalah **AR-style overlay**, bukan world-tracking AR penuh.
Artinya:

- Tidak ada deteksi lantai/meja (hit-test)
- Model tidak "menempel" ke dunia nyata
- Model ditampilkan sebagai overlay 3D di atas feed kamera

Ini adalah kompromi terbaik saat device tidak mendukung WebXR/ARCore.

## Cara Menjalankan
1. Deploy ke GitHub Pages (sudah dilakukan)
2. Buka URL project via HTTPS
3. Klik tombol **Start Kamera**
4. Izinkan akses kamera
5. Interaksikan model (drag/zoom/rotate)

## Pengembangan Lanjutan (Opsional)
Jika nanti tersedia device yang support WebXR/ARCore:

1. Tambahkan mode switch:
   - Mode A: WebXR (true AR)
   - Mode B: Webcam overlay (fallback)
2. Aktifkan hit-test untuk peletakan objek di permukaan nyata
3. Tambahkan UI transform (slider posisi/rotasi/skala)
