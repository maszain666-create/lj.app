<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>LanggengJaya Anugrah Nusantara</title>
  <meta name="theme-color" content="#0a0a0a" />
  <meta name="mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <meta name="apple-mobile-web-app-title" content="LanggengJaya" />
  
  <!-- PWA Manifest via Data URI -->
  <link rel="manifest" href='data:application/manifest+json,{"name":"LanggengJaya Anugrah Nusantara","short_name":"LanggengJaya","start_url":".","display":"standalone","background_color":"#0a0a0a","theme_color":"#eab308","icons":[{"src":"https://cdn-icons-png.flaticon.com/512/3135/3135715.png","sizes":"512x512","type":"image/png"}]}' />

  <!-- Tailwind CSS & React CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/react@18/umd/react.production.min.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js" crossorigin></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

  <style>
    body {
      background-color: #0a0a0a;
      color: #f3f4f6;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
      user-select: none;
      -webkit-tap-highlight-color: transparent;
    }
    .animate-fade-in { animation: fadeIn 0.4s cubic-bezier(0.4, 0, 0.2, 1); }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(8px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  </style>
</head>
<body>
  <div id="root"></div>

  <script type="text/babel">
    const { useState, useEffect, useMemo } = React;

    const HARGA_SATUAN = 500; // 1300 pcs x 500 = 650.000

    const formatRupiah = (angka) => {
      return new Intl.NumberFormat('id-ID', {
        style: 'currency',
        currency: 'IDR',
        minimumFractionDigits: 0
      }).format(angka || 0);
    };

    const formatTanggal = (date) => {
      const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
      return date.toLocaleDateString('id-ID', options);
    };

    const formatWaktu = (date) => {
      return date.toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit', second: '2-digit' });
    };

    const LiveClock = ({ type }) => {
      const [time, setTime] = useState(new Date());
      useEffect(() => {
        const timer = setInterval(() => setTime(new Date()), 1000);
        return () => clearInterval(timer);
      }, []);
      return <>{type === 'time' ? formatWaktu(time) : formatTanggal(time)}</>;
    };

    const Icons = {
      Dashboard: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"></path></svg>,
      Input: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"></path></svg>,
      Recap: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M9 17v-2m3 2v-4m3 4v-6m2 10H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"></path></svg>,
      Finance: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>,
      AI: () => <svg className="w-6 h-6 text-yellow-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>,
      Download: () => <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path></svg>
    };

    function LanggengJayaApp() {
      const [activeTab, setActiveTab] = useState('dashboard');
      const [deferredPrompt, setDeferredPrompt] = useState(null);

      // Handle PWA Installation Event
      useEffect(() => {
        window.addEventListener('beforeinstallprompt', (e) => {
          e.preventDefault();
          setDeferredPrompt(e);
        });
      }, []);

      const handleInstallClick = () => {
        if (deferredPrompt) {
          deferredPrompt.prompt();
          deferredPrompt.userChoice.then(() => setDeferredPrompt(null));
        } else {
          alert('Untuk menginstal aplikasi di HP/Laptop:\n1. Buka menu browser (titik 3 di kanan atas)\n2. Pilih "Tambahkan ke Layar Utama" / "Install App"');
        }
      };

      // State penyimpanan lokal permanen
      const [records, setRecords] = useState(() => {
        const saved = localStorage.getItem('langgengJayaData');
        return saved ? JSON.parse(saved) : [];
      });

      useEffect(() => {
        localStorage.setItem('langgengJayaData', JSON.stringify(records));
      }, [records]);

      const analytics = useMemo(() => {
        const today = new Date();
        const currentMonth = today.getMonth();
        const currentYear = today.getFullYear();
        const todayString = today.toLocaleDateString('id-ID');

        let totalTahuTerjual = 0;
        let totalSotongTerjual = 0;
        let totalPendapatanKotor = 0;
        
        let pendapatanKotorHariIni = 0;
        let pengeluaranHariIni = 0;
        let pendapatanBersihHariIni = 0;
        let bagiDuaHariIni = 0;
        let bagiDuaBulanIni = 0;
        let totalBarangHariIni = 0;

        records.forEach(record => {
          const recordDate = new Date(record.timestamp);
          const isToday = recordDate.toLocaleDateString('id-ID') === todayString;
          const isThisMonth = recordDate.getMonth() === currentMonth && recordDate.getFullYear() === currentYear;

          const kotor = record.keuangan.pendapatanKotor !== undefined ? record.keuangan.pendapatanKotor : (record.keuangan.pendapatan || 0);
          const pengeluaran = record.keuangan.totalPengeluaran || 0;
          const bersih = record.keuangan.pendapatanBersih !== undefined ? record.keuangan.pendapatanBersih : kotor;
          const bagiDua = record.keuangan.bagiDua !== undefined ? record.keuangan.bagiDua : (bersih / 2);

          totalTahuTerjual += record.tahu.terjual;
          totalSotongTerjual += record.sotong.terjual;
          totalPendapatanKotor += kotor;

          if (isToday) {
            pendapatanKotorHariIni += kotor;
            pengeluaranHariIni += pengeluaran;
            pendapatanBersihHariIni += bersih;
            bagiDuaHariIni += bagiDua;
            totalBarangHariIni += (record.tahu.terjual + record.sotong.terjual);
          }

          if (isThisMonth) {
            bagiDuaBulanIni += bagiDua;
          }
        });

        return {
          totalTahuTerjual,
          totalSotongTerjual,
          totalPendapatanKotor,
          pendapatanKotorHariIni,
          pengeluaranHariIni,
          pendapatanBersihHariIni,
          bagiDuaHariIni,
          bagiDuaBulanIni,
          totalBarangHariIni
        };
      }, [records]);

      const renderDashboard = () => (
        <div className="space-y-6 animate-fade-in">
          <div className="flex flex-col sm:flex-row sm:items-center justify-between gap-4 mb-6">
            <div className="flex items-center space-x-3">
              <Icons.AI />
              <h2 className="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-yellow-300 to-amber-500">
                Dashboard AI LanggengJaya
              </h2>
            </div>
            
            <button 
              onClick={handleInstallClick}
              className="flex items-center justify-center space-x-2 bg-gradient-to-r from-yellow-500 to-amber-600 text-black px-4 py-2.5 rounded-xl font-bold text-sm shadow-lg shadow-yellow-500/20 hover:scale-105 active:scale-95 transition-all"
            >
              <Icons.Download />
              <span>Instal Aplikasi di HP</span>
            </button>
          </div>

          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
            <div className="bg-white/5 backdrop-blur-lg border border-yellow-500/20 p-5 rounded-2xl shadow-xl">
              <p className="text-gray-400 text-xs uppercase tracking-wider mb-1">Pendapatan Hari Ini (Kotor)</p>
              <p className="text-2xl sm:text-3xl font-bold text-white">{formatRupiah(analytics.pendapatanKotorHariIni)}</p>
              <div className="mt-3 text-xs text-green-400 bg-green-400/10 px-2.5 py-1 rounded-full w-max font-medium">
                Murni Tanpa Potongan
              </div>
            </div>

            <div className="bg-white/5 backdrop-blur-lg border border-yellow-500/20 p-5 rounded-2xl shadow-xl">
              <p className="text-gray-400 text-xs uppercase tracking-wider mb-1">Operasional Hari Ini</p>
              <p className="text-2xl sm:text-3xl font-bold text-red-400">- {formatRupiah(analytics.pengeluaranHariIni)}</p>
              <div className="mt-3 text-xs text-red-400 bg-red-400/10 px-2.5 py-1 rounded-full w-max font-medium">
                Total Biaya Pengeluaran
              </div>
            </div>

            <div className="bg-white/5 backdrop-blur-lg border border-yellow-500/20 p-5 rounded-2xl shadow-xl">
              <p className="text-gray-400 text-xs uppercase tracking-wider mb-1">Bagi Hasil Anda (Bulan Ini)</p>
              <p className="text-2xl sm:text-3xl font-bold text-yellow-400">{formatRupiah(analytics.bagiDuaBulanIni)}</p>
              <div className="mt-3 text-xs text-yellow-400 bg-yellow-400/10 px-2.5 py-1 rounded-full w-max font-medium">
                Bersih Setelah Operasional
              </div>
            </div>

            <div className="bg-white/5 backdrop-blur-lg border border-yellow-500/20 p-5 rounded-2xl shadow-xl">
              <p className="text-gray-400 text-xs uppercase tracking-wider mb-1">Total Terjual (Hari Ini)</p>
              <p className="text-2xl sm:text-3xl font-bold text-white">{analytics.totalBarangHariIni} <span className="text-base font-normal text-gray-500">pcs</span></p>
              <div className="mt-3 text-xs text-blue-400 bg-blue-400/10 px-2.5 py-1 rounded-full w-max font-medium">
                Tahu Bulat + Sotong
              </div>
            </div>
          </div>

          <div className="bg-gradient-to-br from-gray-900 to-gray-800 border border-gray-700 p-6 rounded-2xl shadow-xl">
            <h3 className="text-lg font-semibold text-yellow-400 mb-3 flex items-center gap-2">
              <Icons.AI />
              <span>Ringkasan Integritas AI System</span>
            </h3>
            <p className="text-gray-300 text-sm leading-relaxed">
              Sistem telah mengkalkulasi seluruh data secara presisi. Total pendapatan bagi hasil Anda bulan ini adalah <strong className="text-yellow-400">{formatRupiah(analytics.bagiDuaBulanIni)}</strong> yang bersumber dari pembagian 50:50 setelah dipotong seluruh biaya operasional. Seluruh rekaman tersimpan secara permanen untuk menjaga akuntabilitas keuangan LanggengJaya Anugrah Nusantara.
            </p>
          </div>
        </div>
      );

      const InputForm = () => {
        const [tahuMasuk, setTahuMasuk] = useState('');
        const [tahuSisa, setTahuSisa] = useState('');
        const [sotongMasuk, setSotongMasuk] = useState('');
        const [sotongSisa, setSotongSisa] = useState('');
        const [pengeluaranList, setPengeluaranList] = useState([{ id: Date.now(), deskripsi: '', nominal: '' }]);

        const tahuTerjual = Math.max(0, (Number(tahuMasuk) || 0) - (Number(tahuSisa) || 0));
        const sotongTerjual = Math.max(0, (Number(sotongMasuk) || 0) - (Number(sotongSisa) || 0));
        
        const totalTerjualHariIni = tahuTerjual + sotongTerjual;
        const pendapatanKotor = totalTerjualHariIni * HARGA_SATUAN;
        const totalPengeluaran = pengeluaranList.reduce((sum, item) => sum + (Number(item.nominal) || 0), 0);
        const pendapatanBersih = Math.max(0, pendapatanKotor - totalPengeluaran);
        const bagiDua = pendapatanBersih / 2;

        const handleAddPengeluaran = () => {
          setPengeluaranList([...pengeluaranList, { id: Date.now(), deskripsi: '', nominal: '' }]);
        };

        const handlePengeluaranChange = (id, field, value) => {
          setPengeluaranList(pengeluaranList.map(item => item.id === id ? { ...item, [field]: value } : item));
        };

        const handleRemovePengeluaran = (id) => {
          setPengeluaranList(pengeluaranList.filter(item => item.id !== id));
        };

        const handleSimpan = (e) => {
          e.preventDefault();
          if (!tahuMasuk && !sotongMasuk) return;

          const newRecord = {
            id: Date.now().toString(),
            timestamp: new Date().toISOString(),
            tahu: {
              masuk: Number(tahuMasuk) || 0,
              sisa: Number(tahuSisa) || 0,
              terjual: tahuTerjual
            },
            sotong: {
              masuk: Number(sotongMasuk) || 0,
              sisa: Number(sotongSisa) || 0,
              terjual: sotongTerjual
            },
            keuangan: {
              pendapatanKotor: pendapatanKotor,
              totalPengeluaran: totalPengeluaran,
              pengeluaranDetail: pengeluaranList.filter(p => p.deskripsi || p.nominal),
              pendapatanBersih: pendapatanBersih,
              bagiDua: bagiDua
            }
          };

          setRecords([newRecord, ...records]);
          
          setTahuMasuk(''); setTahuSisa('');
          setSotongMasuk(''); setSotongSisa('');
          setPengeluaranList([{ id: Date.now(), deskripsi: '', nominal: '' }]);
          setActiveTab('recap');
        };

        return (
          <div className="animate-fade-in max-w-4xl mx-auto space-y-6">
            <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center bg-white/5 p-4 rounded-xl border border-white/10 gap-3">
              <div>
                <h2 className="text-xl font-bold text-white">Input Penjualan & Operasional</h2>
                <p className="text-gray-400 text-xs">Otomatis menghitung barang terjual, pengeluaran & bagi hasil.</p>
              </div>
              <div className="text-left sm:text-right">
                <p className="text-yellow-400 font-medium text-xs"><LiveClock type="date" /></p>
                <p className="text-white text-lg font-mono font-bold"><LiveClock type="time" /></p>
              </div>
            </div>

            <form onSubmit={handleSimpan} className="space-y-6">
              <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                {/* Tahu Bulat */}
                <div className="bg-gradient-to-br from-gray-900 to-gray-800 border border-gray-700 p-5 rounded-2xl shadow-xl space-y-4">
                  <h3 className="text-lg font-bold text-amber-400 border-b border-gray-700 pb-2">TAHU BULAT</h3>
                  <div>
                    <label className="block text-xs text-gray-400 mb-1">Barang Masuk (pcs)</label>
                    <input type="number" min="0" required value={tahuMasuk} onChange={e => setTahuMasuk(e.target.value)}
                      className="w-full bg-black/60 border border-gray-600 text-white rounded-xl px-4 py-3 text-base focus:outline-none focus:border-amber-500"
                      placeholder="Contoh: 1000" />
                  </div>
                  <div>
                    <label className="block text-xs text-gray-400 mb-1">Sisa Fisik (pcs)</label>
                    <input type="number" min="0" required value={tahuSisa} onChange={e => setTahuSisa(e.target.value)}
                      className="w-full bg-black/60 border border-gray-600 text-white rounded-xl px-4 py-3 text-base focus:outline-none focus:border-amber-500"
                      placeholder="Contoh: 200" />
                  </div>
                  <div className="bg-amber-500/10 border border-amber-500/20 p-3 rounded-xl">
                    <p className="text-xs text-amber-200/70">Otomatis Terjual</p>
                    <p className="text-2xl font-bold text-amber-400">{tahuTerjual} pcs</p>
                  </div>
                </div>

                {/* Sotong */}
                <div className="bg-gradient-to-br from-gray-900 to-gray-800 border border-gray-700 p-5 rounded-2xl shadow-xl space-y-4">
                  <h3 className="text-lg font-bold text-amber-400 border-b border-gray-700 pb-2">SOTONG</h3>
                  <div>
                    <label className="block text-xs text-gray-400 mb-1">Barang Masuk (pcs)</label>
                    <input type="number" min="0" required value={sotongMasuk} onChange={e => setSotongMasuk(e.target.value)}
                      className="w-full bg-black/60 border border-gray-600 text-white rounded-xl px-4 py-3 text-base focus:outline-none focus:border-amber-500"
                      placeholder="Contoh: 500" />
                  </div>
                  <div>
                    <label className="block text-xs text-gray-400 mb-1">Sisa Fisik (pcs)</label>
                    <input type="number" min="0" required value={sotongSisa} onChange={e => setSotongSisa(e.target.value)}
                      className="w-full bg-black/60 border border-gray-600 text-white rounded-xl px-4 py-3 text-base focus:outline-none focus:border-amber-500"
                      placeholder="Contoh: 100" />
                  </div>
                  <div className="bg-amber-500/10 border border-amber-500/20 p-3 rounded-xl">
                    <p className="text-xs text-amber-200/70">Otomatis Terjual</p>
                    <p className="text-2xl font-bold text-amber-400">{sotongTerjual} pcs</p>
                  </div>
                </div>

                {/* Pengeluaran Operasional */}
                <div className="bg-gradient-to-br from-red-950/40 to-gray-900 border border-red-900/50 p-5 rounded-2xl shadow-xl md:col-span-2 space-y-4">
                  <div className="flex justify-between items-center border-b border-red-900/50 pb-2">
                    <h3 className="text-lg font-bold text-red-400">PENGELUARAN / OPERASIONAL</h3>
                    <button type="button" onClick={handleAddPengeluaran} className="text-xs bg-red-500/20 text-red-300 hover:bg-red-500/30 px-3 py-2 rounded-lg font-medium transition-all">
                      + Tambah Baris
                    </button>
                  </div>
                  <div className="space-y-3">
                    {pengeluaranList.map((item) => (
                      <div key={item.id} className="flex flex-col sm:flex-row gap-2 items-stretch sm:items-center">
                        <input type="text" value={item.deskripsi} onChange={e => handlePengeluaranChange(item.id, 'deskripsi', e.target.value)} 
                          placeholder="Deskripsi (Misal: Plastik, Bensin, Minyak)" 
                          className="flex-1 bg-black/60 border border-gray-700 text-white rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:border-red-500" />
                        <div className="flex gap-2">
                          <input type="number" min="0" value={item.nominal} onChange={e => handlePengeluaranChange(item.id, 'nominal', e.target.value)} 
                            placeholder="Nominal (Rp)" 
                            className="w-full sm:w-44 bg-black/60 border border-gray-700 text-white rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:border-red-500" />
                          <button type="button" onClick={() => handleRemovePengeluaran(item.id)} className="text-red-400 p-2.5 bg-red-500/10 rounded-xl hover:bg-red-500/20">
                            ✕
                          </button>
                        </div>
                      </div>
                    ))}
                  </div>
                </div>
              </div>

              {/* Summary Bottom Box */}
              <div className="bg-yellow-500/10 border border-yellow-500/30 p-5 rounded-2xl flex flex-col md:flex-row justify-between items-stretch md:items-center gap-4">
                <div className="space-y-1.5 text-sm">
                  <div className="flex justify-between gap-6 border-b border-white/10 pb-1">
                    <span className="text-gray-400">Pendapatan Kotor:</span>
                    <span className="text-white font-bold">{formatRupiah(pendapatanKotor)}</span>
                  </div>
                  <div className="flex justify-between gap-6 border-b border-white/10 pb-1">
                    <span className="text-gray-400">Total Operasional:</span>
                    <span className="text-red-400 font-bold">- {formatRupiah(totalPengeluaran)}</span>
                  </div>
                  <div className="flex justify-between gap-6 pt-1">
                    <span className="text-gray-300 font-medium">Bagi Hasil Anda (Bersih 50%):</span>
                    <span className="text-yellow-400 font-bold text-xl">{formatRupiah(bagiDua)}</span>
                  </div>
                </div>
                <button type="submit" className="bg-gradient-to-r from-yellow-500 to-amber-600 text-black font-bold py-3.5 px-8 rounded-xl shadow-lg shadow-yellow-500/20 hover:scale-105 active:scale-95 transition-all text-center">
                  Simpan Permanen
                </button>
              </div>
            </form>
          </div>
        );
      };

      const RecapTab = () => (
        <div className="animate-fade-in space-y-6">
          <div>
            <h2 className="text-xl font-bold text-white">Ringkasan Data Terpadu</h2>
            <p className="text-gray-400 text-xs">Seluruh riwayat transaksi tersimpan permanen dan tidak dapat diubah (Read-Only).</p>
          </div>

          <div className="bg-gray-900 border border-gray-800 rounded-2xl shadow-2xl overflow-hidden">
            <div className="overflow-x-auto">
              <table className="w-full text-left border-collapse min-w-max">
                <thead>
                  <tr className="bg-black/60 text-yellow-500 text-xs uppercase tracking-wider">
                    <th className="p-3 border-b border-gray-800">Waktu & Tanggal</th>
                    <th className="p-3 border-b border-gray-800 text-center" colSpan="3">Tahu Bulat (pcs)</th>
                    <th className="p-3 border-b border-gray-800 text-center" colSpan="3">Sotong (pcs)</th>
                    <th className="p-3 border-b border-gray-800 text-center" colSpan="3">Keuangan</th>
                  </tr>
                  <tr className="bg-gray-800/40 text-gray-400 text-[11px]">
                    <th className="p-2 border-b border-gray-800"></th>
                    <th className="p-2 border-b border-gray-800 text-center">Masuk</th>
                    <th className="p-2 border-b border-gray-800 text-center">Sisa</th>
                    <th className="p-2 border-b border-gray-800 text-center text-green-400">Terjual</th>
                    <th className="p-2 border-b border-gray-800 text-center">Masuk</th>
                    <th className="p-2 border-b border-gray-800 text-center">Sisa</th>
                    <th className="p-2 border-b border-gray-800 text-center text-green-400">Terjual</th>
                    <th className="p-2 border-b border-gray-800 text-right">Kotor</th>
                    <th className="p-2 border-b border-gray-800 text-right text-red-400">Operasional</th>
                    <th className="p-2 border-b border-gray-800 text-right text-yellow-400">Bagi 2</th>
                  </tr>
                </thead>
                <tbody className="text-gray-300 text-xs">
                  {records.length === 0 ? (
                    <tr>
                      <td colSpan="10" className="p-8 text-center text-gray-500 italic">Belum ada data penjualan yang tersimpan.</td>
                    </tr>
                  ) : (
                    records.map((record) => {
                      const kotor = record.keuangan.pendapatanKotor !== undefined ? record.keuangan.pendapatanKotor : (record.keuangan.pendapatan || 0);
                      const pengeluaran = record.keuangan.totalPengeluaran || 0;
                      const bagiDua = record.keuangan.bagiDua !== undefined ? record.keuangan.bagiDua : (kotor / 2);
                      
                      return (
                        <tr key={record.id} className="border-b border-gray-800/60 hover:bg-white/5 transition-colors">
                          <td className="p-3 whitespace-nowrap">
                            <div className="font-medium text-white">{formatTanggal(new Date(record.timestamp))}</div>
                            <div className="text-[10px] text-gray-500">{formatWaktu(new Date(record.timestamp))}</div>
                          </td>
                          <td className="p-3 text-center">{record.tahu.masuk}</td>
                          <td className="p-3 text-center">{record.tahu.sisa}</td>
                          <td className="p-3 text-center font-bold text-green-400">{record.tahu.terjual}</td>
                          <td className="p-3 text-center">{record.sotong.masuk}</td>
                          <td className="p-3 text-center">{record.sotong.sisa}</td>
                          <td className="p-3 text-center font-bold text-green-400">{record.sotong.terjual}</td>
                          <td className="p-3 text-right font-medium">{formatRupiah(kotor)}</td>
                          <td className="p-3 text-right text-red-400">{formatRupiah(pengeluaran)}</td>
                          <td className="p-3 text-right font-bold text-yellow-400">{formatRupiah(bagiDua)}</td>
                        </tr>
                      )
                    })
                  )}
                </tbody>
              </table>
            </div>
          </div>
        </div>
      );

      const FinanceTab = () => (
        <div className="animate-fade-in space-y-6">
          <div>
            <h2 className="text-xl font-bold text-white">Laporan Keuangan & Bagi Hasil</h2>
            <p className="text-gray-400 text-xs">Sistem pembagian 50:50 dipotong operasional secara otomatis & transparan.</p>
          </div>

          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div className="bg-gradient-to-br from-blue-950/40 to-gray-900 border border-blue-500/20 p-6 rounded-2xl space-y-4">
              <h3 className="text-sm text-blue-300 font-semibold uppercase tracking-wider">Performa Hari Ini</h3>
              <div className="space-y-3">
                <div className="flex justify-between items-center border-b border-blue-500/20 pb-2">
                  <div>
                    <p className="text-xs text-gray-400">Pendapatan Kotor</p>
                    <p className="text-[10px] text-blue-400">({analytics.totalBarangHariIni} pcs x Rp {HARGA_SATUAN})</p>
                  </div>
                  <p className="text-xl font-bold text-white">{formatRupiah(analytics.pendapatanKotorHariIni)}</p>
                </div>
                <div className="flex justify-between items-center border-b border-blue-500/20 pb-2">
                  <p className="text-xs text-red-300">Pengeluaran Operasional</p>
                  <p className="text-lg font-bold text-red-400">- {formatRupiah(analytics.pengeluaranHariIni)}</p>
                </div>
                <div className="pt-2">
                  <p className="text-xs text-gray-400 mb-1">Hak Bagi Hasil Anda Hari Ini (Bersih)</p>
                  <p className="text-3xl font-bold text-green-400">{formatRupiah(analytics.bagiDuaHariIni)}</p>
                </div>
              </div>
            </div>

            <div className="bg-gradient-to-br from-amber-950/40 to-gray-900 border border-amber-500/20 p-6 rounded-2xl space-y-4">
              <h3 className="text-sm text-amber-400 font-semibold uppercase tracking-wider">Akumulasi Bulan Ini</h3>
              <div className="space-y-4">
                <div>
                  <p className="text-xs text-gray-400 mb-1">Estimasi Total Pendapatan Bersih</p>
                  <p className="text-2xl font-bold text-gray-300">{formatRupiah(analytics.bagiDuaBulanIni * 2)}</p>
                </div>
                <div className="pt-3 border-t border-amber-500/20">
                  <p className="text-xs text-gray-400 mb-1">Total Hak Bagi Hasil Anda (1 Bulan)</p>
                  <p className="text-4xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-yellow-300 to-amber-500">
                    {formatRupiah(analytics.bagiDuaBulanIni)}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      );

      return (
        <div className="min-h-screen bg-[#0a0a0a] text-gray-100 flex flex-col">
          <nav className="sticky top-0 z-50 bg-black/80 backdrop-blur-xl border-b border-white/10">
            <div className="max-w-7xl mx-auto px-4">
              <div className="flex justify-between items-center h-16">
                <div className="flex items-center gap-3">
                  <div className="w-9 h-9 bg-gradient-to-tr from-yellow-400 to-amber-600 rounded-xl flex items-center justify-center font-bold text-black text-lg shadow-md shadow-yellow-500/20">
                    LJ
                  </div>
                  <div>
                    <h1 className="text-base font-bold text-white leading-tight">LanggengJaya</h1>
                    <p className="text-[10px] text-yellow-500 uppercase tracking-widest font-semibold">Anugrah Nusantara</p>
                  </div>
                </div>
                
                <div className="hidden md:flex space-x-1">
                  {[
                    { id: 'dashboard', label: 'Dashboard AI', icon: Icons.Dashboard },
                    { id: 'input', label: 'Input Data', icon: Icons.Input },
                    { id: 'recap', label: 'Rekapitulasi', icon: Icons.Recap },
                    { id: 'finance', label: 'Keuangan', icon: Icons.Finance }
                  ].map((tab) => (
                    <button
                      key={tab.id}
                      onClick={() => setActiveTab(tab.id)}
                      className={`flex items-center space-x-2 px-4 py-2 rounded-full text-xs font-medium transition-all ${
                        activeTab === tab.id 
                          ? 'bg-yellow-500/10 text-yellow-400 border border-yellow-500/30' 
                          : 'text-gray-400 hover:text-white'
                      }`}
                    >
                      <tab.icon />
                      <span>{tab.label}</span>
                    </button>
                  ))}
                </div>

                <div className="md:hidden flex flex-col items-end">
                   <span className="text-[11px] text-yellow-500 font-mono font-semibold"><LiveClock type="time" /></span>
                </div>
              </div>
            </div>

            {/* Mobile Bottom Tabs */}
            <div className="md:hidden flex bg-black/90 border-t border-white/5 no-scrollbar">
              {[
                { id: 'dashboard', label: 'Dashboard', icon: Icons.Dashboard },
                { id: 'input', label: 'Input', icon: Icons.Input },
                { id: 'recap', label: 'Rekap', icon: Icons.Recap },
                { id: 'finance', label: 'Keuangan', icon: Icons.Finance }
              ].map((tab) => (
                <button
                  key={tab.id}
                  onClick={() => setActiveTab(tab.id)}
                  className={`flex-1 flex flex-col items-center justify-center py-2.5 text-[11px] gap-1 border-b-2 transition-all ${
                    activeTab === tab.id 
                      ? 'border-yellow-500 text-yellow-400 bg-yellow-500/5' 
                      : 'border-transparent text-gray-500'
                  }`}
                >
                  <tab.icon />
                  <span>{tab.label}</span>
                </button>
              ))}
            </div>
          </nav>

          <main className="max-w-7xl mx-auto px-4 py-6 flex-1 w-full">
            {activeTab === 'dashboard' && renderDashboard()}
            {activeTab === 'input' && <InputForm />}
            {activeTab === 'recap' && <RecapTab />}
            {activeTab === 'finance' && <FinanceTab />}
          </main>
        </div>
      );
    }

    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<LanggengJayaApp />);
  </script>
</body>
</html>
