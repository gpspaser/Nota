<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Nota Digital</title>
<style>
:root{
  --pink:#FF2D55;
  --dark:#1C1C1E;
  --gray:#F2F2F7;
  --border:#E5E5EA;
}

*{margin:0;padding:0;box-sizing:border-box;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,sans-serif}

body{background:var(--gray);padding:16px}

.container{max-width:600px;margin:0 auto}

.card{background:#fff;border-radius:16px;padding:20px;margin-bottom:16px;box-shadow:0 2px 8px rgba(0,0,0,0.06)}

h3{text-align:center;margin-bottom:20px;color:var(--dark);font-size:20px}

.form-group{margin-bottom:16px}
.form-group label{display:block;font-size:13px;font-weight:600;color:#666;margin-bottom:6px}
.form-group input{width:100%;padding:12px;border:1px solid var(--border);border-radius:10px;font-size:15px;outline:none}
.form-group input:focus{border-color:var(--pink)}

.item-belanja{display:flex;gap:8px;margin-bottom:8px}
.item-belanja input{flex:1}
.item-belanja input[type="number"]{max-width:90px}

.btn-tambah{width:100%;padding:10px;background:transparent;border:2px dashed var(--pink);color:var(--pink);border-radius:10px;font-weight:600;cursor:pointer;margin-top:8px}
.btn-tambah:hover{background:#FFF5F7}

.btn-hapus{background:#FF3B30;color:#fff;border:none;width:32px;height:32px;border-radius:8px;font-size:18px;cursor:pointer}

.total-box{background:linear-gradient(135deg,#FFF5F7,#FFE8ED);border:2px solid var(--pink);border-radius:12px;padding:16px;margin:20px 0}
.total-row{display:flex;justify-content:space-between;align-items:center}
.total-label{font-size:14px;font-weight:600;color:#666}
.total-amount{font-size:24px;font-weight:700;color:var(--pink)}

.btn-kirim{width:100%;padding:14px;background:var(--pink);color:#fff;border:none;border-radius:12px;font-size:16px;font-weight:700;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:8px}
.btn-kirim:active{transform:scale(0.98)}

.header-icon{font-size:24px}
</style>
</head>
<body>

<div class="container">
  <div class="card">
    <h3><span class="header-icon">📝</span> Nota Digital</h3>

    <div class="form-group">
      <label>🏪 Nama Toko</label>
      <input type="text" id="nota-toko" placeholder="Toko Kurir Tiktok" value="Toko Kurir Tiktok"/>
    </div>

    <div class="form-group">
      <label>👤 Nama Pelanggan</label>
      <input type="text" id="nota-pelanggan" placeholder="Nama customer"/>
    </div>

    <div class="form-group">
      <label>📱 No. WhatsApp Pelanggan</label>
      <input type="tel" id="nota-wa" placeholder="081234567890"/>
    </div>

    <div class="form-group">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
        <label style="margin:0">🛒 Item Produk</label>
        <button onclick="hapusSemua()" style="background:#FF3B30;color:#fff;border:none;padding:6px 10px;border-radius:6px;font-size:11px;font-weight:600;cursor:pointer">🗑️ Hapus Semua</button>
      </div>
      <div id="listNota"></div>
      <button class="btn-tambah" onclick="tambahItem()">+ Tambah Produk</button>
    </div>

    <div class="total-box">
      <div class="total-row">
        <span class="total-label">TOTAL TAGIHAN</span>
        <span class="total-amount" id="totalBelanja">Rp 0</span>
      </div>
      <div style="font-size:11px;color:#999;margin-top:4px;text-align:right">
        <span id="jumlahItem">0</span> item
      </div>
    </div>

    <button class="btn-kirim" onclick="kirimWA()">
      📱 Kirim Nota ke WhatsApp
    </button>
  </div>
</div>

<script>
window.onload = function(){
  tambahItem();
}

function tambahItem(){
  const div = document.createElement('div');
  div.className = 'item-belanja';
  div.innerHTML = `
    <input type="text" placeholder="Nama produk" oninput="hitungTotal()"/>
    <input type="number" placeholder="Harga" oninput="hitungTotal()"/>
    <input type="number" placeholder="Qty" value="1" oninput="hitungTotal()"/>
    <button class="btn-hapus" onclick="this.parentElement.remove();hitungTotal()">×</button>
  `;
  document.getElementById('listNota').appendChild(div);
  hitungTotal();
}

function hapusSemua(){
  if(confirm('Hapus semua item?')){
    document.getElementById('listNota').innerHTML = '';
    tambahItem();
  }
}

function hitungTotal(){
  const items = document.querySelectorAll('#listNota.item-belanja');
  let total = 0, jmlItem = 0;

  items.forEach(i=>{
    const inputs = i.querySelectorAll('input');
    const produk = inputs[0].value.trim();
    const harga = parseFloat(inputs[1].value)||0;
    const qty = parseFloat(inputs[2].value)||0;
    if(produk && harga>0 && qty>0){
      total += harga * qty;
      jmlItem += qty;
    }
  });

  document.getElementById('totalBelanja').textContent = 'Rp ' + total.toLocaleString('id-ID');
  document.getElementById('jumlahItem').textContent = jmlItem;
}

function kirimWA(){
  const toko = document.getElementById('nota-toko').value.trim() || 'Toko';
  const pelanggan = document.getElementById('nota-pelanggan').value.trim();
  const wa = document.getElementById('nota-wa').value.trim();

  if(!pelanggan ||!wa){
    alert('Isi Nama & No WA Pelanggan dulu!');
    return;
  }

  const items = document.querySelectorAll('#listNota.item-belanja');
  let total = 0, listText = '', adaItem = false;

  items.forEach(i=>{
    const inputs = i.querySelectorAll('input');
    const produk = inputs[0].value.trim();
    const harga = parseFloat(inputs[1].value)||0;
    const qty = parseFloat(inputs[2].value)||0;
    if(produk && harga>0 && qty>0){
      adaItem = true;
      const subtotal = harga * qty;
      total += subtotal;
      listText += `• ${produk}%0A`;
      listText += ` ${qty} x Rp ${harga.toLocaleString('id-ID')} = Rp ${subtotal.toLocaleString('id-ID')}%0A`;
    }
  });

  if(!adaItem){
    alert('Tambah minimal 1 produk dengan harga & qty yang benar!');
    return;
  }

  const tanggal = new Date().toLocaleString('id-ID',{dateStyle:'medium',timeStyle:'short'});

  let pesan = `*🧾 NOTA DIGITAL*%0A`;
  pesan += `🏪 ${toko}%0A`;
  pesan += `📅 ${tanggal}%0A`;
  pesan += `━━━━━━━━━━━━━━%0A`;
  pesan += `👤 Kepada: ${pelanggan}%0A`;
  pesan += `━━━━━━━━━━━━━━%0A`;
  pesan += `*📦 RINCIAN BELANJA:*%0A`;
  pesan += `${listText}`;
  pesan += `━━━━━━━━━━━━━━%0A`;
  pesan += `*💰 TOTAL TAGIHAN: Rp ${total.toLocaleString('id-ID')}*%0A`;
  pesan += `━━━━━━━━━━━━━━%0A`;
  pesan += `Mohon segera lakukan pembayaran.%0A`;
  pesan += `Terima kasih 🙏%0A`;

  // Kirim ke WA pelanggan
  const waClean = wa.replace(/[^0-9]/g,'');
  const waFinal = waClean.startsWith('0')? '62' + waClean.substring(1) : waClean;

  window.open(`https://wa.me/${waFinal}?text=${pesan}`,'_blank');
}
</script>

</body>
</html>
