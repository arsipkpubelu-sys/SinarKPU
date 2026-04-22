[SINAR V3.1.html](https://github.com/user-attachments/files/26959997/SINAR.V3.1.html)
# SinarKPU
Sistem Informasi Penomoran Arsip Komisi Pemilihan Umum Kabupaten Belu
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SINAR | Sistem Informasi Penomoran dan Arsip KPU</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background-image: url('uploaded:KPU.jpg-f2efe4a2-b4ed-4d2b-a27f-d2ddd2260bc7');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
        }
        /* Overlay profesional untuk memastikan kontras teks */
        .bg-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(rgba(15, 23, 42, 0.75), rgba(15, 23, 42, 0.85));
            z-index: -1;
        }
        [x-cloak] { display: none !important; }
        
        .card-premium {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }

        .step-circle {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
    </style>
    
    <script>
        // Logika Utama (Didefinisikan sebelum Alpine dimuat)
        function kpuApp() {
            return {
                step: 1,
                pejabat: '',
                inputKlasifikasi: '',
                selectedNaskah: '',
                selectedSatker: '1',
                tanggal: new Date().toISOString().split('T')[0],
                perihal: '',
                linkDrive: '', 
                hasilFinal: '',

                dataNaskah: {
                    "Surat Perintah": "SPt", "Surat Tugas": "ST", 
                    "Lembar Disposisi": "LD", "Memorandum": "MM", "Surat Dinas": "SD", 
                    "Surat Undangan": "Und", "Surat Keterangan": "Kt",
                    "Surat Pengantar": "SR", "Pengumuman": "Pu", "Laporan": "LP"
                },

                dataKlasifikasi: {
                    "PP.01" :"Perencanaan Program dan Anggaran",
		"PP.01.1" : "Pemilu",
		"PP.01.2" : "Pemilihan",
		"PP.02" : "Penataan Organisasi",
		"PP.02.1" : "Pemilu",
		"PP.02.2" : "Pemilihan",
		"PP.03" : "Pendaftaran Pemantau dan Pemantauan",
		"PP.03.1" : "Pemilu",
		"PP.03.2" : "Pemilihan",
		"PP.04" : "Pembentukan Badan Penyelenggara",
		"PP.04.1" : "Pemilu",
		"PP.04.2" : "Pemilihan",
		"PP.05" : "Rapat Kerja dan Rapat Koordinasi di Setiap Tingkatan",
		"PP.05.1" : "Pemilu",
		"PP.05.2" : "Pemilihan",
		"PP.06" : "Sosialisasi, Bimbingan Teknis, Penyuluhan, Publikasi, dan Pendidikan Pemilih",
		"PP.06.1" : "Pemilu",
		"PP.06.2" : "Pemilihan",
		"PP.07" : "Pengelolaan Data dan Informasi",
		"PP.07.1" : "Pemilu",
		"PP.07.2" : "Pemilihan",
		"PP.08" : "Logistik Penyelenggaraan Pemilu",
		"PP.08.1" : "Pengelolaan Data dan Dokumentasi Kebutuhan Sarana Pemilu",
		"PP.08.2" : "Standar Barang/Jasa Sarana dan Prasarana Pemilu",
		"PP.08.3" : "Administrasi dan Alokasi Sarana dan Prasarana Pemilu",
		"PP.08.4" : "Distribusi Sarana dan Prasarana Pemilu(Angkutan Reguler/Non Reguler)",
		"PP.08.5" : "Penyimpanan Sarana Pemilu",
		"PP.09" : "Logistik Penyelenggaraan Pemilihan",
		"PP.09.1" : "Pengelolaan Data dan Dokumentasi Kebutuhan Sarana Pemilihan",
		"PP.09.2" : "Standar Barang/Jasa Sarana dan Prasarana Pemilihan",
		"PP.09.3" : "Administrasi dan Alokasi Sarana dan Prasarana Pemilihan",
		"PP.09.4" : "Distribusi Sarana dan Prasarana Pemilihan (Angkutan Reguler/Non Reguler)",
		"PP.09.5" : "Penyimpanan Sarana Pemilihan",
		"PL.01" : "Pelaksanaan Pemilu",
		"PL.01.1" : "Pendaftaran, Verifikasi dan Penetapan Peserta Pemilu",
		"PL.01.2" : "Pemutahiran Data Pemilih dan Penyusunan Daftar Pemilih",
		"PL.01.3" : "Penetapan Jumlah Kursi dan Penetapan Daerah Pemilihan",
		"PL.01.4" : "Pencalonan",
		"PL.01.5" : "Penetapan Calon Anggota DPR, DPD, DPRD dan Pasangan Calon Presiden dan Wakil Presiden serta Penetapan Nomor Urut",
		"PL.01.6" : "Kampanye",
		"PL.01.7" : "Dana Kampanye",
		"PL.01.8" : "Pemungutan, Penghitungan, dan Rekapitulasi Hasil Penghitungan Perolehan Suara",
		"PL.01.9" : "Penetapan Calon Terpilih",
		"PL.01.10" : "Sumpah/Janji Anggota DPR, DPD, DPRD,Presiden dan Wakil Presiden",
		"PL.02" : "Pelaksanaan Pemilihan",
		"PL.02.1" : "Pemutakhiran dan Penyusunan Daftar Pemilih",
		"PL.02.2" : "Pencalonan",
		"PL.02.3" : "Penetapan Pasangan Calon Gubernur dan Wakil Gubernur,Bupati dan Wakil Bupati,serta Walikota dan Wakil Walikota dan Penetapan Nomor Urut",
		"PL.02.4" : "Kampanye",
		"PL.02.5" : "Dana Kampanye",
		"PL.02.6" : "Pemungutan, Penghitungan Suara, dan Rekapitulasi Hasil Penghitungan Perolehan Suara",
		"PL.02.7" : "Penetapan Calon Terpilih",
		"PY.01" : "Penyelesaian Pemilu",
		"PY.01.1" : "Sengketa Pemilu",
		"PY.01.2" : "Penyusunan Laporan Keuangan Penyelenggaraan Pemilu",
		"PY.01.3" : "Pengelolaan Arsip Penyelenggaraan Pemilu",
		"PY.01.4" : "Penyusunan Dokumentasi Penyelenggaraan Pemilu",
		"PY.01.5" : "Evaluasi dan Pelaporan Penyelenggaraan Pemilu",
		"PY.02" : "Penyelesaian Pemilihan",
		"PY.02.1" : "Sengketa Pemilihan",
		"PY.02.2" : "Evaluasi dan Pelaporan Penyelenggaraan Pemilihan",
		"PR.01" : "Pokok-Pokok Kebijakan",
		"PR.01.1" : "Rencana Pembangunan Jangka Panjang (RPJP)",
		"PR.01.2" : "Rencana Pembangunan Jangka Menengah (RPJM)",
		"PR.01.3" : "Rencana Strategis (Renstra)",
		"PR.02" : "Rencana Kerja Tahunan",
		"PR.02.1" : "Naskah yang berhubungan dengan usulan unit kerja termasuk Kerangka Acuan Kerja (KAK)/Rencana Anggaran Biaya (RAB) dan Satuan Kerja beserta Data Pendukung",
		"PR.02.2" : "Usulan Kegiatan Lembaga",
		"PR.02.3" : "Rencana Kerja Tahunan (naskah yang berkaitan dengan penyusunan Rencana Kerja, sejak penyusunan Pagu indikatif sampai penyusunan Rencanan Kerja)",
		"PR.02.4" : "Rencana kerja berdasarkan Pagu Indikatif",
		"PR.03" : "Penetapan/Kontrak Kinerja Penetapan Kinerja (TAPKIN) dan Laporan Kinerja Instansi Pemerintah (LAKIP)",
		"PR.03.1" : "Lembaga/KPU",
		"PR.03.2" : "Pimpinan Tinggi Utama dan Madya",
		"PR.03.3" : "Pimpinan Tinggi Pratama",
		"PR.04" : "Laporan",
		"PR.04.1" : "Laporan berkala mencakup: a) Laporan Triwulan;b) Laporan Semester; dan c) Laporan Tahunan.",
		"PR.04.2" : "Laporan Insidental",
		"PR.05" : "Dokumen Rapat Dengar Pendapat",
		"PR.06" : "Evaluasi Program Meliputi Evaluasi Program Unit Kerja dan Evaluasi Program Lembaga.",
		"HK.01" : "Program Legislasi",
		"HK.02" : "Peraturan KPU Naskah Dinas yang berkenaan dengan penyusunan rancangan Peraturan KPU, meliputi pelaksanaan diskusi terarah (FGD), uji publik, permohonan konsultasi Peraturan KPU kepada DPR, proses konsinyering, harmonisasi, permohonan penetapan, dan pengundangan Peraturan KPU.",

		"PAW.01" : "Penggantian Antar Waktu dan Pengisian Anggota DPR, DPD, dan DPRD",
		"PAW.01.1" : "Penggantian Antar Waktu",
		"PAW 01.2" : "Pengisian Keanggotaan",
		"PR.01" : "Pokok-Pokok Kebijakan",
                "PW.01": "Rencana Pengawasan",
		"HK.03" : "Keputusan Meliputi Naskah Dinas yang berkaitan dengan Keputusan KPU, Keputusan KPU Provinsi, Keputusan KPU Kabupaten/Kota, dan Keputusan Sekretaris Jenderal KPU, Keputusan Sekretaris KPU Provinsi, dan Keputusan Sekretaris KPU Kabupaten/Kota, yang terkait dengan kegiatan Non Tahapan Pemilu dan Pemilihan, atau yang diatur lain dalam Kode Klasifikasi Arsip ini.",
		"HK.03.1" : "Keputusan KPU, Keputusan KPU Provinsi, dan Keputusan KPU Kabupaten/Kota",
		"HK.03.2" : "Keputusan Sekretaris Jenderal KPU,Keputusan Sekretaris KPU Provinsi, dan Keputusan Sekretaris KPU Kabupaten/Kota",
		"HK.04" : "Instruksi/Surat Edaran",
		"HK.04.1" : "Instruksi/Surat Edaran KPU, KPU Provinsi, dan KPU kabupaten Kota",
		"HK.04.2" : "Surat Edaran Sekretaris Jenderal KPU, Sekretaris KPU Provinsi, dan KPU Kabupaten/Kota.",
		"HK.05" : "Nota Kesepahaman/Memorandum of Understanding(MoU) berikut pelaksanaan kegiatannya termasuk rancangan awal sampai dengan rancangan akhir dan telaah hukum Nota Kesepahaman/Memorandum of Understanding (MoU) dan Perjanjian Kerja Sama.",
		"HK.05.1" : "Dalam Negeri",
		"HK.05.2" : "Luar Negeri",
		"HK.06" : "Sosialisasi/Penyuluhan Pembinaan Hukum Berkas yang berhubungan dengan kegiatan sosialisasi dan penyuluhan hukum.",
		"HK.07" : "Bantuan/Konsultasi Hukum/Advokasi Berkas tentang pemberian bantuan/konsultasi dan kasus pelanggaran administrasi/pelanggaran kode etik/sengketa hukum, dan kajian/telaah hukum",
		"HK.07.1" :"Pengadilan Negeri/Pengadilan Tinggi/ Mahkamah Agung (Pidana)",
		"HK.07.2" : "Pengadilan Negeri/Pengadilan Tinggi/ Mahkamah Agung (Perdata)",
		"HK.07.3" : "Pengadilan Tata Usaha Negara/Pengadilan Tinggi Tata Usaha Negara/Mahkamah Agung (Tata Usaha Negara)",
		"HK.07.4" : "Dewan Kehormatan Penyelenggara Pemilu (Kode Etik).",
		"HK.07.5" : "Mahkamah Konstitusi (hasil Pemilu/Pemilihan)",
		"HK.07.6" : "Badan Pengawas Pemilu (Pelanggaran Administrasi/Sengketa Pemilu/ Pemilihan",
		"HK.07.7" : "Pendampingan Kepolisian, Kejaksaan, Komisi Pemberantasan Korupsi",
		"ORT.01" : "Struktur Organisasi di Lingkungan Lembaga Negara dan Badan Pemerintah/Instansi",
		"ORT.01.1" : "Pembentukan (SOTK Tipologi KPU Provinsi)",
		"ORT.01.2" : "Pengubahan (SOTK Tipologi KPU Provinsi)",
		"ORT.02" : "Uraian Jabatan dan Tata Kerja",
		"ORT.03" : "Standar Kompetensi Jabatan Manajerial dan Fungsional",
		"ORT.04" : "Evaluasi Kelembagaan",
		"ORT.05" : "Peta Proses Bisnis",
		"ORT.06" : "Standar Operasional Prosedur yang bersifat nasional/regional/internasional termasuk rancangan awal sampai dengan rancangan akhir",
		"ORT.07" : "Reformasi Birokrasi Naskah yang berkaitan dengan kegiatan Pembentukan Pelaksaanaan dan evaluasi.",
		"ORT.08" : "Pelayanan Publik di Lingkungan Negara dan Badan Pemerintah/Instansi",
		"ORT.08.1" : "Pedoman Standar Pelayanan",
		"ORT.08.2" : "Hasil Evaluasi Pelayanan Publik",
		"ORT.09" : "Sertifikat Manajemen Mutu Organisasi (ISO)",
		"ORT.10" : "Analisis Jabatan dan Beban Kerja KPU, KPU Provinsi, dan KPU Kabupaten/Kota.",
		"ORT.10.1" : "Analisis jabatan (Anjab)",
		"ORT.10.2" : "Analisis Beban Kerja (ABK)",
		"ORT.10.3" : "Peta Jabatan",
		"TU.01" : "Administrasi Persuratan",
		"TU.01.1" : "Buku Agenda",
		"TU.01.2" : "Lembar Pengantar/Buku Ekspedisi",
		"TU.02" : "Penyimpanan dan Pemeliharaan Arsip Naskah yang berkaitan dengan daftar Arsip. pemeliharaan Arsip, dan ruang penyimpanan.",
		"TU.03" : "Persetujuan Jadwal Retensi Arsip (JRA) Naskah yang berkaitan dengan Penyusunan rancangan awal sampai dengan rancangan akhir.",
		"TU.04" : "Layanan Arsip Naskah yang berkaitan dengan peminjaman dan pengunaan Arsip.",
		"TU.05" : "Penyusutan Arsip",
		"TU.05.1" : "Pemindahan Arsip Inaktif",
		"TU.05.2" : "Pemusnahan Arsip yang tidak bernilai guna",
		"TU.05.3" : "Penyerahan Arsip Statis",
		"TU.06" : "Pembinaan Kearsipan",
		"TU.06.1" : "Apresiasi/Sosialisasi/Penyuluhan Kearsipan",

		"TU.06.2" : "Bimbingan Teknis",
		"TU.06.3" : "Supervisi dan Monitoring",
		"RT.01" : "Inventarisasi Aset",
		"RT.01.1" : "Administrasi Pengadaan Aset dan Persediaan Meliputi naskah pengadaan secara lelang dan pengadaan secara langsung.",
		"RT.01.2" : "Administrasi Pengelolaan Aset dan Persediaan Meliputi naskah: a) pengelolaan aset melalui aplikasi Sistem Informasi Manajemen Akutansi Barang Milik Negara; dan b) pengelolaan aset tanah melalui aplikasi Sistem Informasi Manajemen Tanah Pemerintah, dan Pengelolaan barang-barang habis pakai.",
		"RT.01.3" : "Administrasi Penghapusan Aset dan Persediaan Meliputi naskah: a) penghapusan melalui prosedur lelang; b) penghapusan melalui prosedur pemusnahan; c) penghapusan melalui prosedur hibah; dan d) penjualan barang habis pakai.",

		"RT.02" : "Perjalanan Dinas",
		"RT.02.1" : "Perjalanan Dinas Dalam Negeri",
		"RT.02.2" : "Perjalanan Dinas Luar Negeri",
		"RT.03" : "Pengurusan Kendaraan Dinas",
		"RT.03.1" : "Pengurusan surat-surat kendaraan dinas",
		"RT.03.2" : "Pemeliharaan dan Perbaikan",
		"RT.03.3" : "Peminjaman Kendaraan Dinas",
		"RT.03.4" : "Pengurusan kehilangan dan masalah kendaraan",
		"RT.04" : "Pemeliharaan Gedung dan Taman Meliputi Arsip administrasi pemeliharaan Gedung dan taman.",
		"RT.05" : "Telekomunikasi Administrasi penggunaan/langganan peralatan telekomunikasi meliputi telepon, radio, teleks, televisi kabel, dan internet.",
		"RT.06" : "Pengelolaan Jaringan Listrik, Air, Telepon dan Peralatan Kantor Lainnya.",
		"RT.06.1" : "Perbaikan/pemeliharaan",
		"RT.06.2" : "Pemasangan",
		"RT.06.3" : "Peminjaman peralatan kantor",
		"RT.07" : "Administrasi Penggunaan Fasilitas Kantor meliputi naskah-naskah permintaan dan penggunaan ruang, gedung, kendaraan, rumah dinas, dan fasilitas kantor lainnya",
		"RT.08" : "Administrasi Penyediaan Konsumsi dan Akomodasi di Lingkungan Kantor KPU",
		"RT.09" : "Ketertiban dan Keamanan",
		"RT.09.1" : "Pengamanan, penjagaan, dan pengawalan terhadap pejabat, kantor, dan rumah dinas meliputi naskah:a) daftar nama satuan pengamanan; b) daftar jaga/daftar piket;c) catatan gangguan pelanggaran/ catatan kejadian; d) surat ijin keluar masuk tamu; dan e) buku mutasi piket.",
		"RT.09.2" : "Laporan ketertiban dan keamanan meliputi naskah-naskah: a) kehilangan; b) kerusakan;c) kecelakaan; dan d) gangguan.",
		"RT.10" : "Administrasi Pengelolaan Parkir",
		"RT.11" : "Adiministrasi Pakaian Dinas Pegawai, Satuan Pengamanan (Satpam), Petugas Kebersihan dan Pegawai Lainnya",
		"PK.01" : "Persidangan Meliputi Naskah Dinas berupa Undangan Rapat Pleno, Risalah Rapat, Berita Acara Rapat Pleno,Transkrip Rekaman Rapat, dan Rekaman Hasil Rapat (Audio).",
		"PK.02" : "Keprotokolan",
		"PK.02.1" : "Upacara/Acara Kedinasan Naskah Dinas yang berkaitan dengan kegiatan Protokoler termasuk upacara bendera, upacara hari besar, upacara pelantikan dan upacara serah terima jabatan.",
		"PK.02.2" : "Kunjungan Naskah yang berkaitan dengan kegiatan kunjungan dinas dalam dan luar negeri dan kunjungan dari masyarakat",
		"HM.01" : "Dokumentasi/Liputan Kegiatan Dinas Pimpinan, Acara Kedinasan dan Peristiwa lain dalam berbagai media Kegiatan yang berhubungan dengan kehumasan, dalam berbagai media (kertas, foto, video, rekaman suara, dan multimedia)",
		"HM.02" : "Pengumpulan, Pengolahan, dan Penyajian Informasi Kelembagaan meliputi: a) kliping koran; b) bahan dokumentasi mencakup brosur, leaflet, dan poster; c) plakat; d) pengumuman; dan e) pemberitaan.",
		"HM.03" : "Hubungan KPU dengan Badan Pemerintahan/ Instansi",
		"HM.03.1" : "Hubungan dengan lembaga pemerintah",
		"HM.03.2" : "Hubungan dengan organisasi sosial dan lembaga swadaya masyarakat (LSM)",
		"HM.03.3" : "Hubungan dengan perusahaan",
		"HM.03.4" : "Hubungan dengan lembaga Pendidikan meliputi sekolah dan perguruan tinggi mencakup naskah: a) magang; b) Pendidikan Sistem Ganda (PSG); dan c) Praktek Kerja Lapangan (PKL).",
		"HM.03.5" : "Forum kehumasan a) badan koordinasi hubungan masyarakat; b) perhimpunan hubungan masyarakat; atau c) forum lain dalam kaitannya dengan kehumasan.",
		"HM.03.6" : "Hubungan dengan media massa yang meliputi naskah: a) siaran pers, konferensi pers, atau press release; b) kunjungan wartawan/peliputan; dan c) wawancara.",
		"HM.04" : "Master Publikasi Melalui Media Cetak dan Elektronik",
		"HM.05" : "Duplikasi Publikasi Melalui Media Cetak dan ELektronik",
		"HM.06" : "Pameran/Sayembara/Lomba, Festival, Pembuatan Spanduk, dan Iklan",
		"HM.07" : "Penghargaan/Tanda Kenang-Kenangan dan Administrasi Pemberian Penghargaan/Tanda Kenang-Kenangan kepada Masyarakat yang Memiliki Jasa Prestasi Besar",
		"HM.08" : "Ucapan Terima Kasih, Ucapan Selamat, Bela Sungkawa, dan Permohonan Maaf",
		"PUS.01" : "Penyimpanan Deposit Bahan Pustaka",
		"PUS.01.1" : "Bukti peneriman koleksi bahan pustaka deposit",
		"PUS.01.2" : "Administrasi pengolahan deposit bahan Pustaka",
		"PUS.02" : "Pengadaan dan Pengelolaan Bahan Pustaka",
		"PUS.02.1" : "Buku induk koleksi",
		"PUS.02.2" : "Daftar buku terseleksi",
		"PUS.02.3" : "Daftar buku dalam pemesanan",
		"PUS.02.4" : "Daftar buku dalam permintaan",
		"PUS.02.5" : "Daftar penerimaan bahan Pustaka hasil pembelian, hadiah deposit, hibah",
		"PUS.02.6" : "Daftar pengiriman bahan Pustaka surplus",
		"PUS.02.7" : "Lembar kerja pengolahan Buram dan Pengkatalogan (BP)",
		"PUS.02.8" : "Shelt List/Jajaran Kartu Utama (Master List)",
		"PUS.02.9" : "Daftar tambahan buku (Assesion List)",
		"PUS.03" : "Pembentukan Keanggotaan Perpustakaan",
		"PLB.01" : "Penelitian dan Pengembangan",
		"PLB.01.1" : "Jurnal Kepemiluan",
		"PLB.01.2" : "Buku Bunga Rampai Kepemiluan",
		"PLB.01.3" : "Riset Nasional Kepemiluan",
		"PLB.01.4" : "Hasil Kajian/Analisis Kepemiluan Pusat dan Daerah",
		"PLB.01.5" : "Hasil Seminar Nasional dan Internasional",
		"PLB.01.6" : "Izin Penelitian Kepemiluan",
		"PLB.02" : "Pendidikan dan Pelatihan (diklat)",
		"PLB.02.1" : "Perencanaan Diklat Naskah berkaitan dengan kegiatan analisis kebutuhan diklat dan rencana Penyelanggaraan diklat tahunan.",
		"PLB.02.2" : "Kelengkapan Diklat naskah yang berkaitan dengan pedoman kediklatan, kurikulum diklat, modul diklat, panduan fasilitator, saran/rekomendasi penyelengaraan diklat, dan notulen sosialisasi rapat Kebijakan diklat.",
		"PLB.02.3" : "Penyelenggaraan Diklat, yang meliputi: a. surat pemanggilan peserta; b. surat keputusan tim penyelenggara diklat;c. surat keputusan tim pengajar diklat; d. panduan diklat; e. laporan panitia penyelenggara diklat; f. sambutan pembukaan penyelenggara diklat; g. daftar peserta diklat; h. bahan ajar diklat; i. daftar hadir peserta diklat; j. daftar hadir widyaiswara; k. formulir evaluasi diklat; l. formulir evaluasi widyaiswara; m. hasil formulasi evaluasi peserta diklat; n. Sertifikat/Surat Tanda Tamat Pendidikan dan Pelatihan (STPP); o. sambutan penutup diklat; p. laporan penyelengaraan diklat; q. evaluasi penyelenggaraan diklat; dan r. evaluasi alumni pasca diklat.",
		"PLB.02.4" : "Registrasi Sertifikat/STTPL Peserta Diklat a. surat permohonan kode registrasi; b. buku registrasi; dan c. surat penyampaian kode registrasi.",
		"PLB.02.5" : "Akreditasi Lembaga Diklat a) surat permohonan akreditasi; b) berita acara rapat verifikasi; c) berita acara rapat tim penilai; d) surat keputusan penetapan akreditasi; e) sertifikat akreditasi; dan f) laporan akreditasi lembaga diklat.",
		"PLB.02.6" : "Sertifikasi Sumber Daya Manusia Kediklatan a. surat permohonan sertifikasi; b. laporan hasil verifikasi lapangan; c. berita acara rapat verifikasi; d. berita acara rapat tim penilai; e. surat keputusan penetapan sertifikasi; f. sertifikat sertifikasi; dan g. laporan sertifikasi individual.",
		"PLB.02.7" : "Sistem Informasi Diklat a. data lembaga diklat; b. data prasarana diklat; c. data sarana diklat; d. data pengelola diklat; e. data penyelenggara diklat; f. data widyaiswara; dan g. data program diklat.",
		"TIK.01" : "Rencana Strategis/Master Plan Pembangunan Sistem Informasi Manajemen (SIM)",
		"TIK.02" : "Dokumen arsitektur meliputi naskah-naskah: a) sistem informasi; b) sistem aplikasi; dan c) infrastruktur.",
		"TIK.03" : "Dokumentasi implementasi meliputi naskah-naskah: a) sistem informasi; b) sistem aplikasi; dan c) infrastruktur.",
		"TIK.04" : "Laporan Hasil Perekaman dan Pemutakhiran Data",
		"TIK.05" : "Migrasi sistem aplikasi, data, dan dokumen hosting meliputi naskah-naskah: a) perencanaan migrasi; b) pelaksanaan migrasi; c) berita acara kegiatan migrasi; d) daftar sistem aplikasi dan data yang dimigrasi; dan e) laporan hasil migrasi.",
		"TIK.06" : "Dokumen Hosting Naskah yang berkaitan dengan formulir permintaan hosting, laporan hasil uji kelayakan, dan laporan hasil pelaksanaan hosting.",
		"TIK.07" : "Layanan back up data digital",
                "PW.01.1": "Rencana Strategis Pengawasan",
                "PW.01.2": "Rencana Kerja Tahunan",
                "PW.01.3": "Rencana Kinerja Tahunan",
                "PW.01.4": "Penetapan Kinerja Tahunan",
                "PW.02": "Pelaksanaan Pengawasan",
                "PW.02.1": "Laporan hasil pemeriksaan non keuangan",
		"PW.02.2": "Laporan Perkembangan Penanganan Surat Pengaduan Masyarakat tentang Keuangan",
		"PW.02.3": "Laporan Pemutakhiran Data Tindak Lanjut Temuan yang Bermasalah",
		"PW.02.4": "Laporan Kegiatan Pendampingan Penyusunan Laporan Keuangan",
		"PW.02.5": "Good Corporate Governance (Dokumen Pakta Integritas)",
		"PW.02.6": "Kertas Kerja Audit",
		"PW.02.7": "Kertas Kerja Evaluasi",
		"PW.02.8": "Kertas Kerja Verifikasi",
		"PW.02.9": "Laporan Hasil Evaluasi",
		"PW.02.10": "Laporan Verfikasi",
		"PW.02.11": "Laporan Hasil Pemeriksaan atas Laporan Keuangan oleh Badan Pemeriksa Keuangan Republik Indonesia (BPK RI)",
		"PW.02.12": "Hasil Pengawasan dan Pelaksanaan Internal oleh Inspektorat",
		"PW.02.13": "Laporan Aparat Pemeriksa Fungsional: 1) Laporan Hasil Pengawasan (LHP) 2) Memorandum Hasil Pengawasan (MHP) 3) Tindak lanjut/Tanggapan LHP",
		"PW.03": "Evaluasi Laporan Akuntabilitas Kinerja Instansi Pemerintah",
		"PW.04": "Piagam Internal Audit Charter",
                "PBJ.01": "Layanan Pengadaan Barang/Jasa",
		"PBJ.01.1": "Telaah/Kajian Pengadaan Barang dan Jasa",
		"PBJ.01.2": "Penyusunan Kebijakan/Regulasi/Edaran Pengadaan Barang dan Jasa",
		"PBJ.01.3": "Rencana Umum Pengadaan",
		"PBJ.01.4": "Pembuatan Dokumen Surat Tugas Penunjukan Pokja Pemilihan",
		"PBJ.01.5": "Analisis/Survey Pasar Barang/Jasa Naskah yang berkaitan dengan survey, penyusunan kuisioner, pengumpulan, dan pengolahan data, dan Laporan Survey",
		"PBJ.01.6": "Penyusunan Dokumen Pengadaan barang dan Jasa Naskah yang berkaitan dengan dokumen persiapan pengadaan barang/jasa antara lain spesifikasi teknis/KAK, Harga Perkiraan Sendiri (HPS), rancangan kontrak, penetapan uang muka dan/atau jaminan, dan dokumen pemilihan penyedia.",
		"PBJ.01.7": "Dokumen Kontrak Pengadaan Barang dan Jasa",
		"PBJ.01.8": "Laporan Monitoring dan Evaluasi Pengadaan Barang dan Jasa",
		"PBJ.02": "Laporan Pengadaan Barang dan Jasa",
		"PBJ.03": "Bimbingan dan Pendampingan Pengadaan Barang dan Jasa",
		"PBJ.03.1": "Pendampingan Pengadaan barang dan Jasa",
		"PBJ.03.2": "Petunjuk Pelaksanaan Bimbingan Pengadaan Barang dan Jasa",
		"PBJ.03.3": "Pendampingan/Konsultasi Hukum Pengadaan Barang dan Jasa",
		"PBJ.03.4": "Sosialisasi/Bimbingan Teknis Pengadaan Barang dan Jasa",
                "LPSE.01": "Pengelolaan Layanan Pengadaan Secara Elektronik",
		"LPSE.01.1": "Permintaan Update Aplikasi SPSE",
		"LPSE.01.2": "Pemeliharaan Aplikasi SPSE",
		"LPSE.02": "Registrasi dan Verifikasi Akun pengguna LPSE dan Sistem Informasi Rencana Umum Pengadaan (SIRUP)",
		"LPSE.02.1": "Verifikasi Penyedia",
		"LPSE.02.2": "Permohonan Akun Pengguna LPSE dan SIRUP",
		"LPSE.03": "Layanan Helpdesk",
		"LPSE.03.1": "Pelayanan Layanan Pengadaan Secara Elektronik (LPSE)",
		"LPSE.03.2": "Penyusunan Laporan Sosialisasi Aplikasi dan Pedoman Rencana Umum Pengadaan",
		"LPSE.03.3": "Penyusunan laporan Monitoring dan Evaluasi Layanan Pengadaan Secara Elektronik",
		"LPSE.03.4": "Survey Layanan Pengadaan Secara Elektronik",
		"SDM.01": "Formasi Pegawai meliputi naskah: a) usulan dari unit kerja; b) usulan permintaan formasi kepada Menteri Pendayagunaan Aparatur Negara dan Reformasi Birokrasi dan Kepala Badan Kepegawaian Nasional(BKN); c) persetujuan Menteri Pendayagunaan Aparatur Negara dan Reformasi Birokrasi; d) penetapan formasi Aparatur Sipil Negara (ASN); dan e) penetapan formasi khusus.",
		"SDM.02": "Pengadaan Pegawai",
		"SDM.02.1": "Proses Penerimaan Pegawai Meliputi naskah: a) pengumuman; b) seleksi administrasi; c) pemanggilan peserta test; d) pelaksanaan ujian tertulis; e) keputusan hasil ujian; dan f) wawancara.",
		"SDM.02.2": "Penetapan Pengumuman Kelulusan",
		"SDM.02.3": "Berkas Lamaran yang Tidak Diterima",
		"SDM.02.4": "Pengadaan Pegawai non PNS /ASN: a) pengumuman; b) seleksi administrasi; c) pemanggilan peserta test; d) pelaksanaan ujian tertulis; e) keputusan hasil ujian; dan f) wawancara.",
		"SDM.02.5": "Penetapan Pengumuman Kelulusan",
		"SDM.02.6": "Seleksi Anggota KPU Meliputi naskah: a) panitia seleksi b) hasil seleksi c) penetapan; dan d) Penggantian Antar Waktu (PAW Anggota).",
		"SDM.02.7": "Seleksi Anggota KPU Provinsi dan KPU Kabupaten/Kota Meliputi naskah: a) panitia seleksi; b) hasil seleksi; dan c) Keputusan Penetapan.",
		"SDM.02.8": "Pengunduran diri Anggota KPU, KPU Provinsi, dan KPU kabupaten/ Kota dan Tindak Lanjut Putusan Dewan Kehormatan Penyelenggara Pemilu Meliputi naskah: a) berkas permohonan pengunduran diri; b) kategori pengunduran diri; dan c) Keputusan Pengunduran diri.",
		"SDM.02.9": "Pemberian Sanksi atas Pengawasan Internal",
		"SDM.03": "Pembinaan Karir Pegawai",
		"SDM.03.1": "Pendidikan dan Pelatihan/Kursus/Tugas Belajar/Ujian Dinas/Izin Belajar Pegawai Mencakup naskah: a) Surat Perintah/Surat Tugas (ST)/SK/Surat; dan b) Laporan Kegiatan Pengembangan Diri.",
		"SDM.03.2": "Ujian Kompetensi a) Assesmenttest pegawai; dan b) Pemetaan/talent mapping pegawai.",
		"SDM.03.3": "Penghargaan dan Tanda Jasa",
		"SDM.03.4": "Pembinaan Mental Kepegawaian",
		"SDM.04": "Kode Etik, Disiplin, Pemberhentian, dan Pensiun ASN",
		"SDM.04.1": "Disiplin Pegawai a) daftar hadir; dan b) rekapitulasi daftar hadir.",
		"SDM.04.2": "Berkas Hukuman Disiplin a) ringan; b) sedang; dan c) berat.",
		"SDM.04.3": "Pemberhentian Pegawai Tanpa Hak Pensiun",
		"SDM.04.4": "Pengaduan/Permasalahan Kepegawaian a) penyelesaian pengelolaan keberatan pegawai; dan b) laporan permasalahan pegawai.",
		"SDM.04.5": "Perselisihan/Sengketa Kepegawaian",
		"SDM.04.6": "Usul Pemberhentian dan Penetapan Pensiun Pegawai/Janda/Dudanya dan PNS yang tewas",
		"SDM.05": "Mutasi Pegawai",
		"SDM.05.1": "Alih Status, Pindah Instansi, Pindah Wilayah Kerja, Dipekerjakan, dan Mutasi Antar Unit",
		"SDM.05.2": "Badan Pertimbangan Jabatan dan Kepangkatan (Baperjakat)",
		"SDM.06": "Administrasi Pegawai",
		"SDM.06.1": "Surat Perintah Dinas/Surat Tugas",
		"SDM.06.2": "Cuti Besar",
		"SDM.06.3": "Cuti Sakit, Cuti Bersalin, dan Cuti Tahunan",
		"SDM.06.4": "Cuti Alasan Penting",
		"SDM.06.5": "Cuti Diluar Tanggungan Negara (CLTN)",
		"SDM.06.6": "Dokumentasi Identitas Pegawai meliputi: a) Usul Penetapan Kartu Pegawai (Karpeg)/Kartu PNS Elektronik (KPE)/Kartu Istri (Karis)/Kartu Suami(Karsu); dan b) Keterangan Penerimaan Pembayaran Penghasilan Pegawai (KP4).",
		"SDM.06.7": "Kesejahteraan Pegawai a) Layanan Pemeliharaan Kesehatan Pegawai; b) Layanan Asuransi Pegawai; c) Layanan Tabungan Perumahan; d) Layanan Bantuan Sosial; e) Layanan Olahraga dan Rekreasi; f) Layanan Beras/Pakaian Dinas; dan g) Layanan Pengurusan Jenazah.",
		"SDM.07": "Laporan Harta Kekayaan Aparatur Sipil Negara (LHKASN)",
		"SDM.08": "Laporan Harta Kekayaan Penyelenggara Negara (LHKPN)(Ketua KPU dan Anggota KPU)",
		"SDM.09": "Berkas Perseorangan ASN meliputi naskah 1) nota usul dan kelengkapan Penetapan Nomor Induk Pegawai (NIP); 2) nota usul kenaikan pangkat/golongan/jabatan; 3) Sasaran Kinerja Pegawai (SKP); 4) daftar usul Penetapan Angka Kredit (PAK); 5) pakta integritas pegawai; 6) Berita Acara dan Serah Terima Jabatan; 7) Surat Keputusan (SK) Usul Pengangkatan dan Pemberhentian dalam Jabatan Manajerial/Fungsional; 8) SK Usul Penetapan Perubahan Data Dasar/Status/Kedudukan Hukum Pegawai; 9) Surat Keputusan Pemberhentian Pegawai Tanpa Hak Pensiun; 10) Nota Usul Pengangkatan Calon Pegawai Negeri Sipil (CPNS) Menjadi PNS; 11) Surat Keputusan (SK) CPNS/PNS Kolektif/ASN; 12) Nota Persetujuan/Pertimbangan Kepala BKN; 13) Keputusan Pengangkatan Calon Aparatur Sipil Negara (CASN); 14) Hasil Pengujian Kesehatan; 15) SK Pengangkatan PNS/ASN; 16) Keputusan Peninjauan Masa Kerja; 17) Keputusan Kenaikan Pangkat; 18) Surat Pernyataan Melaksanakan Tugas/Menduduki Jabatan/Surat Pernyataan Pelantikan; 19) Keputusan Pengangkatan dalam atau Pemberhentian dari Jabatan Manajerial/Fungsional; 20) Keputusan Perpindahan Wilayah Kerja; 21) Keputusan Perpindahan Antar Instansi; 22) Berita Acara Pemeriksaan; 23) Keputusan Hukuman Jabatan/Hukuman Disiplin Aparatur Sipil Negara; 24) Keputusan Perbantuan/Dipekerjakan di Luar Instansi Induk; 25) Keputusan Penarikan Kembali dari Perbantuan/Dipekerjakan; 26) Keputusan Pemberian Uang Tunggu; 27) Keputusan Pembebasan dari Jabatan Organik; 28) Keputusan Pengalihan Pegawai Negeri Sipil; 29) Keputusan Pemberhentian sebagai Pegawai Negeri Sipil; 30) Keputusan Pemberhentian Sementara; 31) Surat Keterangan Pernyataan Pegawai Negeri Sipil Hilang; 32) Surat Keterangan Kembalinya Pegawai Negeri Sipil yang Dinyatakan Hilang; 33) Keputusan Pengangkatan/Pemberhentian sebagai Pejabat Negara; 34) Keputusan Penggantian Nama; 35) Surat Perbaikan Tanggal Tahun Kelahiran; 36) Akta Nikah/Cerai; 37) Akta Kelahiran; 38) Isian Formulir Pendataan Ulang PNS (PUPNS dan ASN); 39) Berita Acara Pengambilan Sumpah/Janji ASN dan Jabatan; 40) Surat Permohonan Menjadi Anggota Partai Politik; 41) Surat Keterangan Mutasi Keluarga; 42) Surat Keterangan Meninggal Dunia; 43) Surat Keterangan Peningkatan Pendidikan; 44) Penetapan Angka Kredit Jabatan Fungsional; 45) Surat Keterangan Hasil Penelitian Khusus; 46) Surat Pemberitahuan Kenaikan Gaji Berkala; 47) Surat Tugas/Izin Belajar Dalam/Luar Negeri, Surat Izin Bepergian Ke Luar Negeri; 48) Kartu Pendaftaran Ulang (Kardaf) Pegawai Negeri Sipil; 49) Ijazah/Sertifikat; 50) Keputusan Penempatan/Penarikan Pegawai; 51) Keputusan Pengangkatan pada Jabatan di Luar Instansi Induk; 52) Surat Pertimbangan Status Pegawai Negeri Sipil; 53) Keputusan Pengaktifan Kembali sebagai Pegawai Negeri Sipil; 54) Surat Pernyataan Pengunduran Diri dari Jabatan Organik; 55) Kenaikan Pangkat; 56) Surat Pemberitahuan Kenaikan Grade; 57) Hasil Ujian Dinas; dan 58) Keputusan Pensiun.",
		"SDM.10": "Berkas Perseorangan Anggota KPU, KPU Provinsi, dan KPU Kabupaten/Kota",
		"SDM.10.1": "Ketua dan Anggota KPU",
		"SDM.10.2": "Ketua dan Anggota KPU Provinsi",
		"SDM.10.3": "Ketua dan Anggota KPU Kabupaten/Kota",
		"KU.01": "Rencana Anggaran Pendapatan dan Belanja Negara(RAPBN)",
		"KU.01.1": "Penyusunan Rencana Anggaran Pendapatan dan Belanja Negara Meliputi naskah: a) dokumen Rencana Anggaran Kerja Instansi Pemerintahan (RAKIP) dan Dokumen Rencana Kerja Anggaran (RKA) KPU; dan b) dokumen Rancangan Satuan Kerja Instansi Pemerintah (RASKIP)/ Standar Biaya Khusus (SKB)",
		"KU.01.2": "Penyampaian Rancangan Anggaran Pendapatan dan Belanja Negara (RAPBN) kepada Dewan Perwakilan Rakyat (DPR) Meliputi naskah: a) Nota keuangan Pemerintahan dan rancangan Undang-undang terkait RAPBN; b) Nota Keuangan Pemerintah; dan c) Materi RAPBN dari Lembaga Negara dan Badan Pemerintah (LNBP)", 
		"KU.01.3": "Pembahasan RAPBN oleh Komisi DPR",
		"KU.01.4": "Nota Jawaban Pemerintah atas Pertanyaan DPR",
		"KU.01.5": "Undang-Undang terkait Anggaran Pendapatan dan Belanja Negara (APBN) dan Rencana Pembangunan Tahunan (RAPETA",
		"KU.02": "Penyusunan Anggaran Pendapatan dan Belanja Negara (APBN)",
		"KU.02.1": "Ketetapan Pagu Indikatif/Pagu Sementara",
		"KU.02.2": "Ketetapan Pagu Definitif",
		"KU.02.3": "Rencana Kerja Anggaran (RKA) Lembaga Negara dan Badan Pemerintahan (LNBP)",
		"KU.02.4": "Daftar Isian Pelaksanaan Anggaran (DIPA), Petunjuk Operasional Kegiatan (POK) dan Revisinya",
		"KU.02.5": "Ketentuan/Peraturan yang menyangkut Pelaksanaan Penata-usahaan dan Pertanggungjawaban Anggaran",
		"KU.03": "Pelaksanaan Anggaran",
		"KU.03.1": "Keputusan KPU terkait Penetapan: a) Kuasa Pengguna Anggaran/Barang; b) Keputusan Sekretaris Jenderal terkait Penetapan Pejabat Pembuat Komitmen (PPK), PjSPM, PBJ Pusat; dab c) keputusan Sekretaris Jenderal terkait Penetapan Kuasa Pengguna Anggaran(KPA)/ Barang Sekretariat",
		"KU.03.2": "Pendapatan Meliputi naskah: a) Surat Setoran Pajak (SSP); b) Surat Setoran Bukan Pajak (SSBP); c) Bukti Penerimaan Negara Bukan Pajak (PNBP); d) Penerimaan Sisa Anggaran Lebih dan Saldo Kas atau Surat Setoran Pengembalian Belanja (SSPB); e) Penatausahaan Barang Milik Negara (BMN)",	
		"KU.03.3": "Belanja Meliputi naskah: a) Surat Penyedia Dana (SPP-UP, SPPDU/TU, SPP-LS); b) Dokumen Belanja Pegawai (Daftar Gaji, Honor Kehormatan, Tunjangan, Uang Makan, Uang lembur, dan Perjalanan Dinas); c) Dokumen Belanja beserta Data pendukungnya; (1) Dokumen uang muka dan data pendukung; (2) Penagihan/Invoice, kuitansi pembayaran, Faktur Barang/Pajak, Perjanjian, SP KBAP, Surat Pernyataan; (3) Berita Acara Penyelesaian Pekerjaan/Serah terima; (4) Surat Permintaan Pembayaran (SPP), ST, Undangan, Daftar Hadir, dan Keputusan; dan (5) Berita Acara Serah Terima Barang; d) Laporan Pertanggungjawaban Anggaran yaitu: (1) Buku Kas Umum (BKU); (2) Buku Kas Pembantu (BKP); dan (3) Buku/Kartu Pengawasan Kredit Anggaran; e) Laporan Pertanggungjawaban (LPJ): (1) KPU; (2) KPU Provinsi; dan (3) KPU Kabupaten/Kota; f) Dokumen Akuntansi Keuangan: (1) Berita Acara Pemeriksaan Kas; (2)Kas/Register Penutupan Kas; (3) Arsip Data Komputer; dan (4) Berita Acara Rekonsiliasi Internal g) Verikasi anggaran (1) nota hasil verifikasi; dan (2) jawaban hasil verifikasi. h) Laporan keuangan tahunan (1) Laporan Realisasi Anggaran (LRA); (2) neraca; (3) catatan atas laporan keuangan; (4) Laporan Pembukan Equitas (LPE); dan (5) Laporan Operasional (LO)",
		"KU.03.4": "Reviuv Laporan Keuangan: a) Sistem Akuntansi Instansi (SAI); b) manual implemantasi SAI c) kebijakan akutansi d) arsip data komputer dan berita acara rekonsoliansi; dan e) laporan realisasi semesteran APBN",
		"KU.03.5": "Dana Hibah Dalam Negeri a) dokumen usulan anggaran; b) naskah perjanjian di bank daerah; c) RKA/RKB; d) pakta integritas; e) SPTJM; f) persetujuan nomor register; g) persetujuan pembukaan rekening; h) persetujuan revisi anggaran; i) pengesahan belanja; (1) SP2HL; (2) SPHL; (3) SP4HL;(4) SP3HL; dan (5) Bukti setoran pengembalian Hibah",
		"KU.03.6": "Pertanggung jawaban Keuangan Negara Dokumen Penyelesaian kerugian Negara a) Tuntutan Perbendaharaan; dan b) Tuntutan ganti rugi",
                },

                getKlasifikasiKet() {
                    if (!this.inputKlasifikasi) return null;
                    const code = this.inputKlasifikasi.toUpperCase().trim();
                    return this.dataKlasifikasi[code] || null;
                },

                filteredNaskah() {
                    if (this.pejabat === 'Berita Acara') return { "Berita Acara": "BA" };
                    if (this.pejabat === 'Nota Dinas') return { "Nota Dinas": "ND" };
                    return this.dataNaskah;
                },

                setStep1(pejabat) {
                    this.pejabat = pejabat;
                    if (pejabat === 'Berita Acara') this.selectedNaskah = 'BA';
                    else if (pejabat === 'Nota Dinas') this.selectedNaskah = 'ND';
                    else this.selectedNaskah = '';
                    this.step = 2;
                },

                generateNomor() {
                    const tahun = new Date(this.tanggal).getFullYear();
                    const storageKey = 'lastNoUrut_' + this.pejabat.replace(/\s+/g, '_');
                    let lastNumber = localStorage.getItem(storageKey) || 0;
                    const newNoUrut = parseInt(lastNumber) + 1;
                    
                    localStorage.setItem(storageKey, newNoUrut);
                    
                    let tambahanSekKab = (this.pejabat === 'Nota Dinas') ? "/Sek-Kab" : "";
                    const cleanCode = this.inputKlasifikasi.toUpperCase().trim();
                    
                    this.hasilFinal = `${newNoUrut}/${cleanCode}-${this.selectedNaskah}${tambahanSekKab}/5304/${this.selectedSatker}/${tahun}`;
                    this.simpanKeDatabase();
                    this.step = 6;
                },

                async simpanKeDatabase() {
                    const scriptURL = 'https://script.google.com/macros/s/AKfycbyVAbMy7Q1o2LvkMOLjawLU6HuXKqB2cahKQbggz1Ut7je592GdkP-pewUJE60fAxhS/exec';
                    const formData = new URLSearchParams();
                    formData.append('nomor', this.hasilFinal);
                    formData.append('pejabat', this.pejabat);
                    formData.append('klasifikasi', this.inputKlasifikasi);
                    formData.append('naskah', this.selectedNaskah);
                    formData.append('satker', this.selectedSatker);
                    formData.append('perihal', this.perihal);
                    formData.append('linkDrive', this.linkDrive);

                    try {
                        await fetch(scriptURL, { method: 'POST', body: formData, mode: 'no-cors' });
                    } catch (e) { 
                        console.error('Error pengarsipan!', e.message);
                    }
                },

                resetApp() {
                    this.step = 1;
                    this.inputKlasifikasi = '';
                    this.selectedNaskah = '';
                    this.perihal = '';
                    this.pejabat = '';
                    this.hasilFinal = '';
                    this.linkDrive = '';
                    window.scrollTo({ top: 0, behavior: 'smooth' });
                }
            };
        }
    </script>
    <script src="https://unpkg.com/alpinejs@3.x.x/dist/cdn.min.js"></script>
</head>
<body class="min-h-screen text-slate-800 antialiased">

    <div class="bg-overlay"></div>

    <!-- NAVBAR -->
    <nav class="bg-white/90 backdrop-blur-md shadow-lg sticky top-0 z-50 border-b-2 border-red-600">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-20 items-center">
                <div class="flex items-center space-x-4">
                    <div @click="resetApp()" class="cursor-pointer group">
                        <h2 class="text-xs font-black text-slate-400 uppercase tracking-[0.3em] leading-none mb-1">KOMISI PEMILIHAN UMUM</h2>
                        <h1 class="text-xl sm:text-2xl font-black text-slate-800 tracking-tighter uppercase leading-none">
                            KABUPATEN BELU<span class="text-red-600">.</span>
                        </h1>
                    </div>
                </div>
                <div class="flex items-center space-x-3">
                    <div class="hidden sm:flex flex-col text-right">
                        <span class="text-[9px] font-bold text-slate-400 uppercase tracking-widest leading-none">ID Satuan Kerja</span>
                        <span class="text-sm font-black text-red-600">5304</span>
                    </div>
                    <div class="h-8 w-[1px] bg-slate-200 hidden sm:block"></div>
                    <div class="bg-red-600 text-white px-3 py-1.5 rounded-lg text-xs font-black uppercase shadow-md shadow-red-200">SINAR</div>
                </div>
            </div>
        </div>
    </nav>

    <main class="container mx-auto px-4 py-12" x-data="kpuApp()">
        
        <div class="max-w-3xl mx-auto card-premium rounded-3xl shadow-2xl overflow-hidden border border-white">
            
            <!-- PROGRESS BAR -->
            <div class="bg-slate-50/80 border-b border-slate-100 p-6 flex justify-between items-center overflow-x-auto">
                <template x-for="n in 5">
                    <div class="flex items-center flex-1 last:flex-none">
                        <div class="flex flex-col items-center">
                            <div :class="step >= n ? 'bg-red-600 text-white scale-110 shadow-lg shadow-red-200' : 'bg-slate-200 text-slate-400'" 
                                 class="w-10 h-10 rounded-xl flex items-center justify-center font-black text-sm step-circle transition-all duration-300" 
                                 x-text="n"></div>
                        </div>
                        <div x-show="n < 5" class="flex-1 h-1 mx-4 rounded-full transition-all duration-500" :class="step > n ? 'bg-red-600' : 'bg-slate-200'"></div>
                    </div>
                </template>
            </div>

            <div class="p-8 sm:p-14">
                <!-- STEP 1: JENIS NASKAH -->
                <div x-show="step === 1" x-transition:enter="transition ease-out duration-300 transform opacity-0 translate-y-4" x-transition:enter-end="opacity-100 translate-y-0">
                    <div class="mb-10">
                        <h2 class="text-3xl font-black text-slate-800 tracking-tight leading-tight">Pilih Pejabat Penandatangan</h2>
                        <p class="text-slate-500 mt-2 text-sm font-medium">Tentukan jenis dokumen atau pejabat yang akan memproses penomoran.</p>
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <template x-for="item in ['Surat Keluar Sekretaris', 'Surat Keluar Ketua', 'Berita Acara', 'Nota Dinas']">
                            <button @click="setStep1(item)" 
                                    class="p-6 border-2 border-slate-100 rounded-2xl hover:border-red-600 hover:bg-white hover:shadow-xl transition-all duration-300 text-left group">
                                <p class="font-black text-slate-700 text-lg leading-snug group-hover:text-red-700" x-text="item"></p>
                                <p class="text-[10px] text-slate-400 uppercase font-bold mt-2 tracking-widest opacity-0 group-hover:opacity-100 transition-opacity italic">Pilih Sekarang &rarr;</p>
                            </button>
                        </template>
                    </div>
                </div>

                <!-- STEP 2: KLASIFIKASI -->
                <div x-show="step === 2" x-transition>
                    <div class="mb-10">
                        <h2 class="text-3xl font-black text-slate-800 tracking-tight">Kode Klasifikasi</h2>
                        <p class="text-slate-500 mt-2 text-sm font-medium">Gunakan kode klasifikasi arsip yang sesuai dengan perihal surat.</p>
                    </div>
                    <input type="text" x-model="inputKlasifikasi" list="klasifikasiList" 
                           class="w-full p-6 bg-slate-50 border-2 border-slate-200 rounded-2xl mb-4 uppercase font-black text-2xl outline-none focus:border-red-600 focus:bg-white shadow-inner" 
                           placeholder="CTH: PP.01">
                    <div class="p-5 bg-amber-50 rounded-2xl mb-10 border-2 border-amber-100">
                        <p class="text-xs font-black text-amber-700 uppercase tracking-widest mb-1">Deskripsi:</p>
                        <p class="text-base font-bold text-amber-900 italic leading-snug" x-text="getKlasifikasiKet() || 'Silakan masukkan kode yang valid...'"></p>
                    </div>
                    <div class="flex justify-between items-center pt-6 border-t border-slate-100">
                        <button @click="step = 1" class="text-slate-400 font-black uppercase text-xs tracking-widest hover:text-slate-600">Kembali</button>
                        <button @click="step = 3" :disabled="!getKlasifikasiKet()" class="bg-red-600 text-white px-10 py-4 rounded-2xl font-black shadow-lg shadow-red-200 disabled:opacity-30 hover:scale-105 active:scale-95 transition-all">LANJUT</button>
                    </div>
                </div>

                <!-- STEP 3: JENIS NASKAH DINAS -->
                <div x-show="step === 3" x-transition>
                    <div class="mb-10">
                        <h2 class="text-3xl font-black text-slate-800 tracking-tight">Singkatan Naskah</h2>
                        <p class="text-slate-500 mt-2 text-sm font-medium">Pilih singkatan jenis naskah dinas sesuai aturan persuratan KPU.</p>
                    </div>
                    <div class="relative">
                        <select x-model="selectedNaskah" class="w-full p-6 bg-slate-50 border-2 border-slate-200 rounded-2xl focus:border-red-600 outline-none font-black text-xl text-slate-700 appearance-none shadow-inner">
                            <option value="">-- PILIH JENIS NASKAH --</option>
                            <template x-for="(val, key) in filteredNaskah()">
                                <option :value="val" x-text="key"></option>
                            </template>
                        </select>
                        <div class="absolute right-6 top-1/2 -translate-y-1/2 pointer-events-none text-slate-400">
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M19 9l-7 7-7-7"/></svg>
                        </div>
                    </div>
                    <div class="mt-12 flex justify-between items-center pt-6 border-t border-slate-100">
                        <button @click="step = 2" class="text-slate-400 font-black uppercase text-xs tracking-widest hover:text-slate-600">Kembali</button>
                        <button @click="step = 4" :disabled="!selectedNaskah" class="bg-red-600 text-white px-10 py-4 rounded-2xl font-black shadow-lg shadow-red-200 disabled:opacity-30 hover:scale-105 active:scale-95 transition-all">LANJUT</button>
                    </div>
                </div>

                <!-- STEP 4: KONFIRMASI WILAYAH -->
                <div x-show="step === 4" x-transition>
                    <div class="mb-10">
                        <h2 class="text-3xl font-black text-slate-800 tracking-tight leading-none">Verifikasi Wilayah</h2>
                    </div>
                    <div class="p-10 border-4 border-double border-red-100 bg-red-50/50 rounded-[2rem] text-center shadow-inner relative overflow-hidden">
                        <div class="relative z-10">
                            <p class="text-xs text-red-600 font-black uppercase tracking-[0.4em] mb-4">SATUAN KERJA TERDAFTAR</p>
                            <p class="text-6xl font-black text-slate-800 uppercase italic tracking-tighter mb-8 leading-none">BELU</p>
                            <div class="inline-flex items-center gap-4 px-10 py-4 bg-red-600 text-white rounded-full font-mono font-black text-4xl tracking-widest shadow-xl">
                                5304
                            </div>
                        </div>
                    </div>
                    <div class="mt-12 flex justify-between items-center pt-6 border-t border-slate-100">
                        <button @click="step = 3" class="text-slate-400 font-black uppercase text-xs tracking-widest hover:text-slate-600">Kembali</button>
                        <button @click="step = 5" class="bg-red-600 text-white px-14 py-4 rounded-2xl font-black shadow-lg shadow-red-200 hover:scale-105 active:scale-95 transition-all uppercase tracking-widest">KONFIRMASI</button>
                    </div>
                </div>

                <!-- STEP 5: DETAIL AKHIR -->
                <div x-show="step === 5" x-transition>
                    <div class="mb-10">
                        <h2 class="text-3xl font-black text-slate-800 tracking-tight">Detail Dokumen & Arsip</h2>
                    </div>
                    <div class="space-y-6">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div>
                                <label class="block text-[10px] font-black text-slate-400 uppercase tracking-widest mb-2">Unit Pelaksana</label>
                                <select x-model="selectedSatker" class="w-full p-4 bg-slate-50 border-2 border-slate-200 rounded-xl outline-none focus:border-red-600 font-bold shadow-inner transition-all">
                                    <option value="1">Keuangan Umum dan Logistik</option>
                                    <option value="2">Teknis Penyelenggaran Pemilu, Parmas & Humas</option>
                                    <option value="3">Perencanaan Data dan Informasi</option>
                                    <option value="4">Hukum dan Sumber Daya Manusia</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-[10px] font-black text-slate-400 uppercase tracking-widest mb-2">Tanggal</label>
                                <input type="date" x-model="tanggal" class="w-full p-4 bg-slate-50 border-2 border-slate-200 rounded-xl outline-none focus:border-red-600 font-bold shadow-inner transition-all">
                            </div>
                        </div>
                        <div>
                            <label class="block text-[10px] font-black text-slate-400 uppercase tracking-widest mb-2">Perihal / Tentang</label>
                            <textarea x-model="perihal" rows="3" class="w-full p-5 bg-slate-50 border-2 border-slate-200 rounded-2xl font-bold outline-none focus:border-red-600 shadow-inner placeholder:text-slate-300 transition-all" placeholder="Tuliskan perihal surat secara singkat..."></textarea>
                        </div>
                        <div class="p-8 bg-slate-900 rounded-[2rem] border-4 border-slate-800 shadow-2xl relative">
                            <label class="block text-[11px] font-black text-rose-400 uppercase tracking-widest mb-4 flex items-center gap-2">
                                <span class="w-2 h-2 bg-rose-500 rounded-full animate-pulse"></span>
                                Link Drive Digital <span class="text-red-500 text-lg">*</span>
                            </label>
                            <input type="url" x-model="linkDrive" required class="w-full p-5 bg-slate-800 border-2 border-slate-700 rounded-2xl text-rose-400 font-mono text-sm outline-none focus:border-red-500 transition-all shadow-inner placeholder:text-slate-600" placeholder="https://drive.google.com/file/d/...">
                            <p class="text-[10px] text-slate-500 mt-4 italic font-medium">Wajib: Atur akses file ke "Siapa saja yang memiliki link"</p>
                        </div>
                    </div>
                    <div class="mt-12 flex flex-col sm:flex-row gap-5 justify-between items-center pt-8 border-t border-slate-100">
                        <button @click="step = 4" class="text-slate-400 font-black uppercase text-xs tracking-widest hover:text-slate-600 order-2 sm:order-1">Kembali</button>
                        <button @click="if(linkDrive && perihal) { generateNomor() } else { alert('Lengkapi perihal dan link drive!') }" 
                                class="w-full sm:w-auto bg-red-600 text-white px-16 py-6 rounded-[2rem] font-black shadow-2xl shadow-red-200 hover:scale-105 active:scale-95 transition-all order-1 sm:order-2 uppercase tracking-[0.25em]">
                            GENERATE NOMOR
                        </button>
                    </div>
                </div>

                <!-- STEP 6: HASIL AKHIR -->
                <div x-show="step === 6" x-transition class="text-center py-6">
                    <div class="mb-10">
                        <div class="w-24 h-24 bg-emerald-50 text-emerald-500 rounded-[2rem] flex items-center justify-center mx-auto mb-8 ring-8 ring-emerald-50/50 animate-bounce shadow-xl">
                            <svg class="w-12 h-12" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="4" d="M5 13l4 4L19 7"></path></svg>
                        </div>
                        <h2 class="text-4xl font-black text-slate-800 tracking-tighter leading-none mb-4">Nomor Terbit!</h2>
                        <p class="text-slate-500 font-bold">Data telah berhasil diarsipkan secara digital.</p>
                    </div>

                    <div class="bg-slate-50 border-4 border-double border-slate-200 rounded-[2.5rem] p-12 mb-10 group transition-all hover:bg-white hover:shadow-2xl relative">
                        <p class="text-[10px] font-black text-red-600 uppercase tracking-[0.5em] mb-8 bg-red-50 inline-block px-4 py-1.5 rounded-full">NOMOR SURAT RESMI</p>
                        <p class="text-3xl md:text-4xl font-black text-slate-900 break-all leading-tight font-mono tracking-tight mb-8" x-text="hasilFinal"></p>
                        
                        <button @click="navigator.clipboard.writeText(hasilFinal); alert('Nomor berhasil disalin!')" 
                                class="inline-flex items-center gap-3 bg-white border-2 border-slate-200 px-8 py-3 rounded-2xl font-black text-slate-600 hover:bg-slate-900 hover:text-white transition-all shadow-sm active:scale-95">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"/></svg>
                            SALIN NOMOR
                        </button>
                    </div>

                    <button @click="resetApp()" 
                            class="w-full sm:w-auto bg-slate-900 hover:bg-black text-white px-14 py-7 rounded-[2rem] font-black shadow-2xl transition-all transform hover:-translate-y-1 active:scale-95 flex items-center justify-center gap-5 mx-auto uppercase tracking-widest text-sm">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M12 4v16m8-8H4"></path></svg>
                        PENOMORAN BARU
                    </button>
                </div>
            </div>
        </div>

        <datalist id="klasifikasiList">
            <template x-for="(keterangan, kode) in dataKlasifikasi" :key="kode">
                <option :value="kode" x-text="keterangan"></option>
            </template>
        </datalist>
    </main>

    <footer class="mt-24 text-center pb-20 relative z-10">
        <p class="text-white/70 text-[11px] font-black uppercase tracking-[0.5em] mb-2">&copy; 2024 Sekretariat KPU Kabupaten Belu</p>
        <p class="text-white/40 text-[9px] italic font-medium tracking-widest uppercase">Sistem Informasi Penomoran dan Arsip Digital v3.1</p>
    </footer>

</body>
</html>
