<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Khidmah Media Store</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-slate-900 text-white min-h-screen flex items-center justify-center p-4">

  <main class="w-full max-w-md">

    <!-- 1. LOGIN VIEW -->
    <section id="viewLogin" class="space-y-5">
      <div class="text-center space-y-2">
        <!-- LOGO KHIDMAH MEDIA STORE -->
        <div class="w-32 h-32 mx-auto flex items-center justify-center">
          <img 
            src="https://lh3.googleusercontent.com/d/14X6YZs3II1C1B96O6JXY37AmqHt1aS6X" 
            alt="Logo Khidmah Media Store" 
            class="w-full h-full object-contain filter drop-shadow-xl rounded-2xl"
          >
        </div>
        <h1 class="text-xl font-black text-amber-400 tracking-wider">KHIDMAH MEDIA STORE</h1>
        <p class="text-xs opacity-70">Sistem Presensi Online Enterprise</p>
      </div>

      <form id="formLogin" onsubmit="handleLogin(event)" class="bg-slate-800/80 backdrop-blur-md p-6 rounded-3xl space-y-4 shadow-2xl border border-slate-700">
        <div>
          <label class="block text-[11px] opacity-70 mb-1">Username / Email</label>
          <input type="text" id="loginUser" required class="w-full bg-black/40 border border-slate-700 rounded-xl px-4 py-3 text-xs focus:outline-none focus:border-amber-400">
        </div>
        <div>
          <label class="block text-[11px] opacity-70 mb-1">Password</label>
          <input type="password" id="loginPass" required class="w-full bg-black/40 border border-slate-700 rounded-xl px-4 py-3 text-xs focus:outline-none focus:border-amber-400">
        </div>
        
        <button type="submit" id="btnLogin" class="w-full bg-amber-400 hover:bg-amber-500 text-black font-bold py-3.5 rounded-xl shadow-lg text-xs transition">
          MASUK APLIKASI
        </button>
      </form>
    </section>


    <!-- 2. DASHBOARD VIEW (Diberi class 'hidden' di awal agar tersembunyi) -->
    <section id="viewDashboard" class="hidden space-y-5">
      <div class="bg-slate-800/80 backdrop-blur-md p-6 rounded-3xl border border-slate-700 text-center space-y-4 shadow-2xl">
        <div class="w-20 h-20 mx-auto flex items-center justify-center">
          <img src="https://lh3.googleusercontent.com/d/14X6YZs3II1C1B96O6JXY37AmqHt1aS6X" class="w-full h-full object-contain">
        </div>
        <div>
          <h2 class="text-lg font-bold text-amber-400" id="welcomeUser">Selamat Datang!</h2>
          <p class="text-xs text-slate-300">Sistem Presensi Khidmah Media Store</p>
        </div>

        <div class="pt-4 space-y-2">
          <button onclick="alert('Fitur Absen Masuk')" class="w-full bg-emerald-500 hover:bg-emerald-600 text-white font-bold py-3 rounded-xl text-xs transition">
            ABSEN MASUK
          </button>
          <button onclick="handleLogout()" class="w-full bg-rose-500/20 hover:bg-rose-500/30 text-rose-300 font-bold py-3 rounded-xl text-xs transition border border-rose-500/30">
            LOGOUT / KELUAR
          </button>
        </div>
      </div>
    </section>

  </main>

  <script>
    const GAS_URL = "https://script.google.com/macros/s/AKfycbyqryrO_dISgWh7DAIjlOVAeqR4EwrIMJzzWUVNM425Kr2vsvfIi_WFO_NhtKfOWTDKUw/exec";

    async function handleLogin(e) {
      e.preventDefault();
      
      const btn = document.getElementById("btnLogin");
      const user = document.getElementById("loginUser").value;
      const pass = document.getElementById("loginPass").value;

      btn.disabled = true;
      btn.innerText = "PROSES...";

      try {
        const response = await fetch(GAS_URL, {
          method: "POST",
          headers: { "Content-Type": "text/plain;charset=utf-8" },
          body: JSON.stringify({
            action: "login",
            username: user,
            password: pass
          })
        });

        const result = await response.json();

        if (result.status === "success") {
          // --- KODE PEMINDAH HALAMAN LOG IN KE DASHBOARD ---
          document.getElementById("viewLogin").classList.add("hidden");
          document.getElementById("viewDashboard").classList.remove("hidden");
          document.getElementById("welcomeUser").innerText = "Selamat Datang, " + user + "!";
        } else {
          alert("Gagal: " + result.message);
        }
      } catch (err) {
        alert("Terjadi kesalahan koneksi!");
        console.error(err);
      } finally {
        btn.disabled = false;
        btn.innerText = "MASUK APLIKASI";
      }
    }

    function handleLogout() {
      // Kembalikan ke tampilan Login
      document.getElementById("viewDashboard").classList.add("hidden");
      document.getElementById("viewLogin").classList.remove("hidden");
      document.getElementById("loginPass").value = "";
    }
  </script>
</body>
</html>
