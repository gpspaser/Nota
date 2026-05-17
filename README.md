<html lang='id'>
<head>
    <meta charset='utf-8'/>
    <meta content='width=device-width,initial-scale=1.0' name='viewport'/>
    <title>Flash Sale Kuliner - Dengan Form Pelanggan</title>
    
    <link href='https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&amp;display=swap' rel='stylesheet'/>
    
    <style>
        :root {
            --primary-green: #00b159;
            --light-green: #e8f5e9;
            --accent-pink: #ff2c55;
            --accent-orange: #ff5722;
            --bg-light: #f4f4f4;
            --text-dark: #333333;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Poppins', sans-serif;
        }

        body {
            background-color: var(--bg-light);
            color: var(--text-dark);
            max-width: 480px;
            margin: 0 auto;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            background: #fff;
            min-height: 100vh;
            position: relative;
        }

        /* Header */
        header {
            background: linear-gradient(135deg, var(--primary-green), #00c853);
            color: white;
            padding: 15px;
            position: sticky;
            top: 0;
            z-index: 99;
        }
        
        .header-top {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 10px;
        }

        .logo-flash {
            font-size: 22px;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .logo-flash span { color: #ffd600; }

        .search-bar {
            background: rgba(255, 255, 255, 0.2);
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 14px;
            color: #fff;
        }

        .banner-promo {
            background: linear-gradient(to right, #4a00e0, #8e2de2);
            margin: 10px;
            padding: 12px;
            border-radius: 8px;
            color: white;
            font-size: 13px;
            font-weight: 600;
            text-align: center;
            border-left: 5px solid var(--accent-pink);
        }

        /* Tabs */
        .time-slots { display: flex; background: #fff; border-bottom: 1px solid #eee; }
        .slot { flex: 1; text-align: center; padding: 10px 0; font-size: 14px; color: #666; }
        .slot.active { color: var(--accent-orange); font-weight: bold; border-bottom: 3px solid var(--accent-orange); background: #fff5f2; }
        .filter-tabs { display: flex; gap: 8px; padding: 10px; overflow-x: auto; background: #fff; }
        .tab { background: #f0f0f0; padding: 6px 15px; border-radius: 15px; font-size: 12px; white-space: nowrap; color: #555; }
        .tab.mall { border: 1px solid var(--accent-pink); color: var(--accent-pink); background: #fff0f3; font-weight: bold; }

        /* Menu List */
        .menu-container { padding: 10px; margin-bottom: 80px; }
        .menu-card { display: flex; background: white; border-radius: 8px; padding: 10px; margin-bottom: 12px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); gap: 12px; }
        .menu-img-wrapper { position: relative; width: 100px; height: 100px; flex-shrink: 0; }
        .menu-img { width: 100%; height: 100%; object-fit: cover; border-radius: 6px; }
        .badge-diskon { position: absolute; top: 0; left: 0; background: var(--accent-pink); color: white; font-size: 10px; font-weight: bold; padding: 2px 6px; border-top-left-radius: 6px; border-bottom-right-radius: 6px; }
        .menu-info { flex-grow: 1; display: flex; flex-direction: column; justify-content: space-between; }
        .menu-title { font-size: 14px; font-weight: 600; color: #222; line-height: 1.3; }
        .tags { display: flex; gap: 5px; margin-top: 4px; }
        .tag-beli-lokal { background: var(--accent-pink); color: white; font-size: 10px; padding: 1px 4px; border-radius: 3px; font-weight: bold; }
        .tag-gratis-ongkir { color: var(--primary-green); font-size: 11px; font-weight: 600; }

        .stock-bar-container { background: #eee; border-radius: 10px; height: 8px; margin: 8px 0; position: relative; overflow: hidden; }
        .stock-bar { background: linear-gradient(to right, var(--accent-orange), #ff9800); height: 100%; border-radius: 10px; }
        .stock-text { font-size: 9px; position: absolute; right: 5px; top: -2px; color: #fff; font-weight: bold; }

        .price-action { display: flex; justify-content: space-between; align-items: center; }
        .price-now { color: var(--accent-pink); font-size: 16px; font-weight: 700; }
        .price-old { color: #999; font-size: 11px; text-decoration: line-through; }

        /* Tombol Tambah Keranjang */
        .btn-add-cart {
            background: var(--accent-pink);
            color: white;
            border: none;
            padding: 6px 14px;
            border-radius: 4px;
            font-weight: bold;
            font-size: 13px;
            cursor: pointer;
        }

        /* Floating Cart Button */
        .floating-cart {
            position: fixed;
            bottom: 20px;
            right: calc(50% - 220px);
            background: var(--primary-green);
            color: white;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            cursor: pointer;
            z-index: 100;
        }
        @media (max-width: 480px) {
            .floating-cart { right: 20px; }
        }

        .cart-badge {
            position: absolute;
            top: -5px;
            right: -5px;
            background: var(--accent-pink);
            color: white;
            font-size: 12px;
            font-weight: bold;
            padding: 3px 7px;
            border-radius: 50%;
            border: 2px solid white;
        }

        /* Modal Keranjang Belanja */
        .cart-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1000;
            justify-content: center;
            align-items: flex-end;
        }

        .modal-content {
            background: white;
            width: 100%;
            max-width: 480px;
            border-top-left-radius: 20px;
            border-top-right-radius: 20px;
            padding: 20px;
            max-height: 85vh;
            overflow-y: auto;
            animation: slideUp 0.3s ease-out;
        }

        @keyframes slideUp {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #eee;
            padding-bottom: 10px;
            margin-bottom: 15px;
        }

        .close-modal {
            font-size: 24px;
            cursor: pointer;
            color: #999;
        }

        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid #f9f9f9;
        }

        .cart-item-info h4 { font-size: 14px; font-weight: 600; }
        .cart-item-info span { color: var(--accent-pink); font-size: 13px; font-weight: bold; }

        .quantity-control {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .btn-qty {
            background: #eee;
            border: none;
            width: 25px;
            height: 25px;
            border-radius: 4px;
            font-weight: bold;
            cursor: pointer;
        }

        /* Style Form Pengiriman */
        .form-pengiriman {
            background: #fdfdfd;
            border: 1px solid #eef0f2;
            border-radius: 8px;
            padding: 15px;
            margin-top: 15px;
        }

        .form-pengiriman h4 {
            font-size: 14px;
            margin-bottom: 10px;
            color: #444;
            border-left: 3px solid var(--primary-green);
            padding-left: 8px;
        }

        .form-group {
            margin-bottom: 10px;
        }

        .form-group label {
            display: block;
            font-size: 12px;
            font-weight: 600;
            margin-bottom: 4px;
            color: #666;
        }

        .form-group input, .form-group textarea {
            width: 100%;
            padding: 8px 12px;
            border: 1px solid #ccc;
            border-radius: 6px;
            font-size: 13px;
            outline: none;
        }

        .form-group input:focus, .form-group textarea:focus {
            border-color: var(--primary-green);
        }

        .total-box {
            display: flex;
            justify-content: space-between;
            font-weight: bold;
            font-size: 16px;
            margin: 15px 0;
            padding-top: 10px;
            border-top: 2px dashed #eee;
        }

        .btn-checkout {
            background: var(--primary-green);
            color: white;
            border: none;
            width: 100%;
            padding: 12px;
            border-radius: 8px;
            font-size: 15px;
            font-weight: bold;
            cursor: pointer;
            text-align: center;
            display: block;
            text-decoration: none;
        }
    </style>
</head>
<body>

    <header>
        <div class='header-top'>
            <div class='logo-flash'>F<span>⚡</span>ash Sale Makanan</div>
        </div>
        <div class='search-bar'>🔍 Cari kuliner favoritmu...</div>
    </header>

    <div class='banner-promo'>🔥 Diskon s.d 50% Masuk Keranjang Langsung Hitung Otomatis!</div>

    <div class='time-slots'>
        <div class='slot active'><strong>13:00</strong><br/><span style='font-size:11px'>Sekarang</span></div>
        <div class='slot'><strong>19:00</strong><br/><span style='font-size:11px'>Berikutnya</span></div>
    </div>
    <div class='filter-tabs'>
        <div class='tab active'>Semua Menu</div>
        <div class='tab mall'>Flash Sale 🔥</div>
    </div>

    <div class='menu-container'>

        <div class='menu-card' data-id='1' data-nama='Healthy Salad Bowl' data-harga='35000'>
            <div class='menu-img-wrapper'>
                <span class='badge-diskon'>50%</span>
                <img class='menu-img' src='https://images.unsplash.com/photo-1546069901-ba9599a7e63c?w=500' alt='Salad Ayam Sehat'/>
            </div>
            <div class='menu-info'>
                <div>
                    <div class='tags'><span class='tag-beli-lokal'>Beli Lokal</span><span class='tag-gratis-ongkir'>🚚 Gratis Ongkir</span></div>
                    <h3 class='menu-title'>Healthy Salad Bowl + Grilled Chicken</h3>
                </div>
                <div>
                    <div class='stock-bar-container'><div class='stock-bar' style='width: 85%;'></div><span class='stock-text'>85% Terjual</span></div>
                    <div class='price-action'>
                        <div><span class='price-now'>Rp35.000</span><br/><span class='price-old'>Rp70.000</span></div>
                        <button class='btn-add-cart' onclick='tambahKeKeranjang(this)'>+ Beli</button>
                    </div>
                </div>
            </div>
        </div>

        <div class='menu-card' data-id='2' data-nama='Premium Beef Burger' data-harga='24000'>
            <div class='menu-img-wrapper'>
                <span class='badge-diskon'>40%</span>
                <img class='menu-img' src='https://images.unsplash.com/photo-1568901346375-23c9450c58cd?w=500' alt='Burger Premium'/>
            </div>
            <div class='menu-info'>
                <div>
                    <div class='tags'><span class='tag-beli-lokal'>Beli Lokal</span><span class='tag-gratis-ongkir'>🚚 Gratis Ongkir</span></div>
                    <h3 class='menu-title'>Premium Beef Burger Krispi Lumer</h3>
                </div>
                <div>
                    <div class='stock-bar-container'><div class='stock-bar' style='width: 92%;'></div><span class='stock-text'>92% Terjual</span></div>
                    <div class='price-action'>
                        <div><span class='price-now'>Rp24.000</span><br/><span class='price-old'>Rp40.000</span></div>
                        <button class='btn-add-cart' onclick='tambahKeKeranjang(this)'>+ Beli</button>
                    </div>
                </div>
            </div>
        </div>

        <div class='menu-card' data-id='3' data-nama='Pizza Mini Mozzarella' data-harga='42000'>
            <div class='menu-img-wrapper'>
                <span class='badge-diskon'>30%</span>
                <img class='menu-img' src='https://images.unsplash.com/photo-1513104890138-7c749659a591?w=500' alt='Pizza Mini'/>
            </div>
            <div class='menu-info'>
                <div>
                    <div class='tags'><span class='tag-beli-lokal'>Beli Lokal</span><span class='tag-gratis-ongkir'>🚚 Gratis Ongkir</span></div>
                    <h3 class='menu-title'>Pizza Mini Mozzarella Spesial Molor</h3>
                </div>
                <div>
                    <div class='stock-bar-container'><div class='stock-bar' style='width: 60%;'></div><span class='stock-text'>60% Terjual</span></div>
                    <div class='price-action'>
                        <div><span class='price-now'>Rp42.000</span><br/><span class='price-old'>Rp60.000</span></div>
                        <button class='btn-add-cart' onclick='tambahKeKeranjang(this)'>+ Beli</button>
                    </div>
                </div>
            </div>
        </div>

    </div>

    <div class='floating-cart' onclick='bukaKeranjang()'>
        🛒
        <span class='cart-badge' id='cartCount'>0</span>
    </div>

    <div class='cart-modal' id='cartModal' onclick='tutupKeranjangLuar(event)'>
        <div class='modal-content'>
            <div class='modal-header'>
                <h3>Keranjang Belanja</h3>
                <span class='close-modal' onclick='tutupKeranjang()'>&times;</span>
            </div>
            
            <div id='cartItemsList'>
                <p style="text-align:center; color:#999; padding: 20px 0;">Keranjang masih kosong.</p>
            </div>

            <div class="form-pengiriman">
                <h4>Form Data Pelanggan</h4>
                <div class="form-group">
                    <label>Nama Lengkap *</label>
                    <input type="text" id="custNama" placeholder="Contoh: Budi Santoso" required="required"/>
                </div>
                <div class="form-group">
                    <label>No. WhatsApp Aktif *</label>
                    <input type="tel" id="custWa" placeholder="Contoh: 081234567xxx" required="required"/>
                </div>
                <div class="form-group">
                    <label>Alamat Pengiriman Lengkap *</label>
                    <textarea id="custAlamat" rows="3" placeholder="Nama jalan, nomor rumah, RT/RW, kelurahan..." required="required"></textarea>
                </div>
            </div>

            <div class='total-box'>
                <span>Total Bayar:</span>
                <span id='totalHargaHtml'>Rp0</span>
            </div>

            <button class='btn-checkout' onclick='prosesCheckout()'>Checkout via WhatsApp (WA)</button>
        </div>
    </div>

    <script>
        let keranjang = [];

        function tambahKeKeranjang(button) {
            const card = button.closest('.menu-card');
            const id = card.getAttribute('data-id');
            const nama = card.getAttribute('data-nama');
            const harga = parseInt(card.getAttribute('data-harga'));

            const itemAda = keranjang.find(item => item.id === id);

            if (itemAda) {
                itemAda.qty += 1;
            } else {
                keranjang.push({ id, nama, harga, qty: 1 });
            }

            perbaruiUIKeranjang();
        }

        function ubahJumlahItem(id, jumlah) {
            const item = keranjang.find(item => item.id === id);
            if (item) {
                item.qty += jumlah;
                if (item.qty <= 0) {
                    keranjang = keranjang.filter(item => item.id !== id);
                }
            }
            perbaruiUIKeranjang();
        }

        function perbaruiUIKeranjang() {
            let totalItem = 0;
            let totalHarga = 0;
            const listContainer = document.getElementById('cartItemsList');

            if (keranjang.length === 0) {
                listContainer.innerHTML = `<p style="text-align:center; color:#999; padding: 20px 0;">Keranjang masih kosong.</p>`;
                document.getElementById('cartCount').innerText = '0';
                document.getElementById('totalHargaHtml').innerText = 'Rp0';
                return;
            }

            listContainer.innerHTML = '';

            keranjang.forEach(item => {
                totalItem += item.qty;
                totalHarga += (item.harga * item.qty);

                listContainer.innerHTML += `
                    <div class="cart-item">
                        <div class="cart-item-info">
                            <h4>${item.nama}</h4>
                            <span>Rp${(item.harga * item.qty).toLocaleString('id-ID')}</span>
                        </div>
                        <div class="quantity-control">
                            <button class="btn-qty" onclick="ubahJumlahItem('${item.id}', -1)">-</button>
                            <span>${item.qty}</span>
                            <button class="btn-qty" onclick="ubahJumlahItem('${item.id}', 1)">+</button>
                        </div>
                    </div>
                `;
            });

            document.getElementById('cartCount').innerText = totalItem;
            document.getElementById('totalHargaHtml').innerText = 'Rp' + totalHarga.toLocaleString('id-ID');
        }

        function bukaKeranjang() {
            document.getElementById('cartModal').style.display = 'flex';
        }

        function tutupKeranjang() {
            document.getElementById('cartModal').style.display = 'none';
        }

        function tutupKeranjangLuar(e) {
            if (e.target.id === 'cartModal') tutupKeranjang();
        }

        function prosesCheckout() {
            if (keranjang.length === 0) {
                alert('Keranjang kamu masih kosong, pilih menu dulu ya!');
                return;
            }

            // Validasi Input Form
            const nama = document.getElementById('custNama').value.trim();
            const wa = document.getElementById('custWa').value.trim();
            const alamat = document.getElementById('custAlamat').value.trim();

            if (!nama || !wa || !alamat) {
                alert('Mohon lengkapi data Nama, No. WhatsApp, dan Alamat Pengiriman terlebih dahulu!');
                return;
            }

            let nomorWA = "623137527300"; // Nomor WA Admin
            
            // Format Pesanan teks untuk WA
            let pesan = "*PESANAN BARU - FLASH SALE KULINER*\n";
            pesan += "---------------------------------------\n";
            pesan += `👤 *Nama:* ${nama}\n`;
            pesan += `📱 *WhatsApp:* ${wa}\n`;
            pesan += `📍 *Alamat:* ${alamat}\n`;
            pesan += "---------------------------------------\n\n";
            pesan += "📦 *Daftar Belanjaan:*\n";
            
            let totalHarga = 0;
            keranjang.forEach((item, index) => {
                let subtotal = item.harga * item.qty;
                totalHarga += subtotal;
                pesan += `${index + 1}. ${item.nama} (x${item.qty}) = Rp${subtotal.toLocaleString('id-ID')}\n`;
            });

            pesan += "\n---------------------------------------\n";
            pesan += `💰 *Total Pembayaran: Rp${totalHarga.toLocaleString('id-ID')}*\n`;
            pesan += "---------------------------------------\n\n";
            pesan += "Mohon segera diproses dan dihitung ongkirnya ya Min! 🙏";

            let urlWA = `https://wa.me/${nomorWA}?text=${encodeURIComponent(pesan)}`;
            
            window.open(urlWA, '_blank');
        }
    </script>

</body>
</html>
