<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Kuota Murah — QRIS Neon</title>
<style>
  :root{
    --accent:#00ff88;
    --muted:#94a3b8;
    --bg:#0b1320;
    --card:#0f1724;
  }

  body{
    margin:0;font-family:'Share Tech Mono', monospace;background:var(--bg);color:#e6eef6;
    display:flex;align-items:center;justify-content:center;min-height:100vh;padding:20px;
  }

  .wrap{
    max-width:700px;width:100%;background:var(--card);padding:24px;border-radius:20px;
    display:flex;flex-direction:column;gap:20px;position:relative;
    box-shadow:0 0 40px rgba(0,255,136,0.3);
  }

  header h1{
    margin:0;font-size:22px;color:var(--accent);text-shadow:0 0 6px var(--accent);
  }
  p.lead{
    margin:4px 0 0;color:var(--muted);font-size:13px;
  }

  /* Testimoni */
  .testimoni-bar{
    background:#101a2a;padding:15px 0;border-radius:12px;overflow:hidden;position:relative;
    box-shadow:0 0 15px rgba(0,255,136,0.3);
  }
  .testimoni-track{ 
    display:flex;gap:500px;position:absolute;white-space:nowrap;will-change:transform;
  }
  .testimoni{
    font-size:10px;color:#00ff88;background:#0b1320;padding:2px 10px;border-radius:8px;white-space:nowrap;
    text-shadow:0 0 4px #00ff88;box-shadow:0 0 10px rgba(0,255,136,0.3);
  }

  .packages{display:grid;gap:14px;}
  .pkg{
    background:var(--card);padding:16px;border-radius:12px;display:flex;flex-direction:column;gap:8px;
    transition:.3s;cursor:pointer;
    box-shadow:0 0 10px rgba(0,255,136,0.2);
  }
  .pkg:hover{transform:scale(1.05);box-shadow:0 0 20px rgba(0,255,136,0.6);}
  .pkg .name{font-weight:700;font-size:16px;color:var(--accent);text-shadow:0 0 4px var(--accent);}
  .pkg .desc{font-size:13px;color:var(--muted)}
  .price .big{font-size:18px;font-weight:800;color:var(--accent);margin-top:4px;text-shadow:0 0 6px var(--accent);}
  .pkg button{
    background:transparent;border:2px solid var(--accent);color:var(--accent);
    padding:8px;border-radius:8px;cursor:pointer;font-weight:700;transition:.3s;text-shadow:0 0 2px var(--accent);
  }
  .pkg button:hover{background:var(--accent);color:#001b10;box-shadow:0 0 12px var(--accent);}

  aside{
    background:var(--card);padding:18px;border-radius:12px;display:flex;flex-direction:column;gap:12px;
    box-shadow:0 0 20px rgba(0,255,136,0.2);
  }
  .total-row{display:flex;justify-content:space-between;align-items:center;margin-top:8px}
  .pay-btn{
    background:var(--accent);color:#001b10;padding:12px;border:none;border-radius:10px;font-weight:800;cursor:pointer;
    transition:.3s;box-shadow:0 0 12px var(--accent);
  }
  .pay-btn:hover{box-shadow:0 0 20px var(--accent);}
  .pay-btn:disabled{opacity:.6;cursor:not-allowed;box-shadow:none;}

  .provider-group{display:flex;flex-wrap:wrap;gap:12px}
  .provider-option{display:flex;align-items:center;gap:6px;background:var(--bg);padding:6px 10px;
                   border-radius:8px;cursor:pointer;font-size:13px;color:#00ff88;transition:.2s;}
  .provider-option:hover{box-shadow:0 0 10px rgba(0,255,136,0.3);}
  .provider-option input{accent-color:var(--accent);}
  .error-text{color:red;font-size:12px;display:none;margin-top:-6px}

  /* Modal */
  .modal-backdrop{
    position:fixed;inset:0;background:rgba(0,0,0,0.7);display:none;
    align-items:center;justify-content:center;z-index:9999;backdrop-filter:blur(4px)
  }
  .modal{
    background:rgba(15,23,36,0.95);border:1px solid rgba(0,255,136,0.4);
    padding:24px;border-radius:20px;max-width:420px;width:90%;text-align:center;
    box-shadow:0 0 30px rgba(0,255,136,0.6);backdrop-filter:blur(12px);animation:fadeIn .4s ease;
  }
  .modal h3{margin-top:0;color:var(--accent);font-size:22px;text-shadow:0 0 6px var(--accent);}
  .modal img{
    width:220px;height:220px;object-fit:contain;border-radius:16px;background:#fff;
    padding:12px;box-shadow:0 0 20px rgba(0,255,136,0.5);margin:14px 0;
  }
  .modal .amount{font-size:20px;font-weight:700;color:var(--accent);margin-bottom:12px;text-shadow:0 0 6px var(--accent);}
  .close{
    background:rgba(0,255,136,0.2);border:0;color:#001b10;width:32px;height:32px;
    border-radius:50%;cursor:pointer;float:right;display:flex;align-items:center;justify-content:center;
    font-weight:bold;
  }
  .confirm-btn{
    margin-top:16px;background:var(--accent);color:#001b10;font-weight:700;
    border:none;padding:10px 16px;border-radius:8px;cursor:pointer;transition:.3s;
    box-shadow:0 0 12px var(--accent);
  }
  .confirm-btn:hover{box-shadow:0 0 20px var(--accent);}
  #statusText{margin-top:10px;font-size:14px;display:none;color:var(--accent);font-weight:600;text-shadow:0 0 4px var(--accent);}

  .loading-box{
    background:rgba(15,23,36,0.9);padding:30px 40px;border-radius:16px;
    display:flex;flex-direction:column;align-items:center;gap:16px;
    box-shadow:0 0 30px rgba(0,255,136,0.5);backdrop-filter:blur(12px)
  }
  .spinner{
    width:40px;height:40px;border:4px solid rgba(0,255,136,0.2);
    border-top-color:var(--accent);border-radius:50%;animation:spin 1s linear infinite;
  }
  @keyframes spin{to{transform:rotate(360deg)}}
  @keyframes fadeIn{from{opacity:0;transform:scale(.9)}to{opacity:1;transform:scale(1)}}
</style>
</head>
<body>
<main class="wrap">
  <div class="testimoni-bar"><div class="testimoni-track" id="testimoniTrack"></div></div>

  <header>
    <h1>Kuota Murah — QRIS</h1>
    <p class="lead">Pilih paket, bayar via QRIS, & kuota dikirim ke nomor HP Anda.</p>
  </header>

  <section class="packages">
    <div class="pkg"><div class="name">20GB</div><div class="desc">Berlaku 30 hari</div><div class="price"><div class="big">Rp 5.000</div></div><button onclick="choosePackage(5000,'20GB')">Beli</button></div>
    <div class="pkg"><div class="name">50GB</div><div class="desc">Berlaku 30 hari</div><div class="price"><div class="big">Rp 10.000</div></div><button onclick="choosePackage(10000,'50GB')">Beli</button></div>
    <div class="pkg"><div class="name">100GB</div><div class="desc">Berlaku 30 hari</div><div class="price"><div class="big">Rp 15.000</div></div><button onclick="choosePackage(15000,'100GB')">Beli</button></div>
  </section>

  <aside>
    <div class="total-row"><div>Paket</div><div id="paketName">-</div></div>
    <div class="total-row"><div>Total</div><div id="totalText">Rp 0</div></div>

    <label>Pilih Provider:</label>
    <div class="provider-group" id="providerGroup">
      <label class="provider-option"><input type="radio" name="provider" value="Telkomsel">Telkomsel</label>
      <label class="provider-option"><input type="radio" name="provider" value="Indosat">Indosat</label>
      <label class="provider-option"><input type="radio" name="provider" value="XL">XL</label>
      <label class="provider-option"><input type="radio" name="provider" value="Tri">Tri</label>
      <label class="provider-option"><input type="radio" name="provider" value="Smartfren">Smartfren</label>
    </div>

    <input id="phone" type="tel" placeholder="Nomor HP" style="padding:10px;border-radius:8px;border:2px solid #ccc;margin-top:8px;outline:none"/>
    <div class="error-text" id="phoneError">Nomor HP tidak valid</div>

    <button class="pay-btn" id="payBtn" onclick="openQris()" disabled>Bayar via QRIS</button>
  </aside>

  <!-- Modal QRIS -->
  <div class="modal-backdrop" id="modalBackdrop">
    <div class="loading-box" id="loadingBox"><div class="spinner"></div><div>Sedang menyiapkan QRIS...</div></div>
    <div class="modal" id="qrisModal" style="display:none">
      <button class="close" onclick="closeModal()">✕</button>
      <h3>Scan QRIS untuk Membayar</h3>
      <img src="https://i.imgur.com/LlLIpQn.jpeg" alt="QRIS"/>
      <div class="amount" id="modalAmount">Rp 0</div>
      <p style="color:var(--muted);font-size:13px;margin-top:6px">Gunakan aplikasi e-wallet / mobile banking untuk scan kode QR ini.</p>
      <button class="confirm-btn" onclick="confirmPayment()">Saya Sudah Membayar</button>
      <div id="statusText">✅ Menunggu verifikasi...</div>
    </div>
  </div>

  <!-- Popup Konfirmasi -->
  <div class="modal-backdrop" id="confirmBackdrop">
    <div class="modal">
      <h3>Konfirmasi Pembayaran</h3>
      <p style="color:var(--muted);font-size:14px;margin:12px 0">
        Apakah Anda yakin sudah melakukan pembayaran?
      </p>
      <div style="display:flex;gap:12px;justify-content:center;margin-top:16px">
        <button class="confirm-btn" onclick="proceedConfirm()">Ya</button>
        <button class="close" style="width:auto;height:auto;padding:6px 12px;font-weight:600"
                onclick="document.getElementById('confirmBackdrop').style.display='none'">Batal</button>
      </div>
    </div>
  </div>

  <audio id="soundLoading" src="https://www.soundjay.com/buttons/beep-07a.mp3" preload="auto"></audio>
  <audio id="soundSuccess" src="https://www.soundjay.com/buttons/button-3.mp3" preload="auto"></audio>
</main>

<script>
let currentTotal=0;
let purchasedNumbers = JSON.parse(localStorage.getItem("purchasedNumbers")) || [];
function savePurchasedNumbers(){localStorage.setItem("purchasedNumbers", JSON.stringify(purchasedNumbers));}
function formatRupiah(n){return 'Rp '+n.toLocaleString('id-ID');}
function choosePackage(price,name){currentTotal=price;document.getElementById('totalText').textContent=formatRupiah(price);document.getElementById('paketName').textContent=name;document.getElementById('payBtn').disabled=false;}

const providerPrefixes={
  "Telkomsel":["0811","0812","0813","0821","0822","0823","0851","0852","0853"],
  "Indosat":["0814","0815","0816","0855","0856","0857","0858"],
  "XL":["0817","0818","0819","0859","0877","0878"],
  "Tri":["0895","0896","0897","0898","0899"],
  "Smartfren":["0881","0882","0883","0884","0885","0886","0887","0888","0889"]
};

function isValidPhone(num,provider){
  if(!/^08\d{8,11}$/.test(num)) return false;
  return providerPrefixes[provider]?.some(prefix=>num.startsWith(prefix));
}

const phoneInput=document.getElementById('phone');
const phoneError=document.getElementById('phoneError');
phoneInput.addEventListener("input",()=>{phoneInput.style.borderColor="#ccc";phoneError.style.display="none";});

document.getElementById("providerGroup").addEventListener("change",e=>{
  let prov=e.target.value;
  if(providerPrefixes[prov]){
    phoneInput.placeholder="Contoh: "+providerPrefixes[prov][0]+"xxxxxxx";
  }
});

function openQris(){
  const phone=phoneInput.value.trim();
  const providerEl=document.querySelector('input[name="provider"]:checked');
  if(!providerEl){alert("Pilih provider terlebih dahulu");return;}
  const provider=providerEl.value;
  if(!phone){phoneError.textContent="Masukkan nomor HP";phoneError.style.display="block";phoneInput.style.borderColor="red";return;}
  if(!isValidPhone(phone,provider)){phoneError.textContent="Nomor tidak sesuai dengan provider "+provider;phoneError.style.display="block";phoneInput.style.borderColor="red";return;}
  if(purchasedNumbers.includes(phone)){alert("Nomor ini sudah pernah digunakan untuk pembelian.");return;}

  const loadingAudio = document.getElementById('soundLoading');
  loadingAudio.loop = true;
  loadingAudio.play();

  document.getElementById('modalBackdrop').style.display='flex';
  document.getElementById('loadingBox').style.display='flex';
  document.getElementById('qrisModal').style.display='none';

  setTimeout(()=>{
    document.getElementById('loadingBox').style.display='none';
    document.getElementById('qrisModal').style.display='block';
    document.getElementById('modalAmount').textContent=formatRupiah(currentTotal);
    loadingAudio.pause();
    loadingAudio.currentTime=0;
    document.getElementById('soundSuccess').play();
  },2000);
}
function closeModal(){document.getElementById('modalBackdrop').style.display='none';}

// buka popup konfirmasi
function confirmPayment(){
  document.getElementById("confirmBackdrop").style.display="flex";
}

// setelah klik YA di popup
function proceedConfirm(){
  document.getElementById("confirmBackdrop").style.display="none";
  const phone=phoneInput.value.trim();
  if(phone && !purchasedNumbers.includes(phone)){purchasedNumbers.push(phone);savePurchasedNumbers();}

  const statusText=document.getElementById('statusText');
  statusText.style.display='block';
  statusText.style.color='var(--accent)';
  statusText.textContent="✅ Menunggu verifikasi...";

  // setelah 5 detik berubah jadi gagal
  setTimeout(()=>{
    statusText.textContent="❌ Proses gagal, pembayaran belum diselesaikan";
    statusText.style.color="red";
    statusText.style.textShadow="0 0 4px red";
  },3000);
}

/* Testimoni */
const paketList=["20GB","50GB","100GB"];
function randomPhone(){let prefix=["0812","0813","0821","0857","0858","0878","0838"];let mid=Math.floor(100+Math.random()*900);return prefix[Math.floor(Math.random()*prefix.length)]+mid+"******";}
function addTestimoni(){let track=document.getElementById("testimoniTrack");let paket=paketList[Math.floor(Math.random()*paketList.length)];let nomor=randomPhone();let div=document.createElement("div");div.className="testimoni";div.textContent=`${paket} Ke ${nomor} berhasil ✅`;track.appendChild(div);if(track.children.length>20){track.removeChild(track.children[0]);}}
for(let i=0;i<8;i++) addTestimoni();

let scrollPos = 0;
function scrollTestimoni(){
  const track = document.getElementById("testimoniTrack");
  scrollPos -= 1;
  if(Math.abs(scrollPos) >= track.scrollWidth/2){scrollPos=0;}
  track.style.transform = `translateX(${scrollPos}px)`;
  requestAnimationFrame(scrollTestimoni);
}
requestAnimationFrame(scrollTestimoni);
setInterval(addTestimoni,4000);
</script>
</body>
</html>
