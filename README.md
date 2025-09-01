<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Adin Store — Mobile Legends Diamonds</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(to bottom, #111827, #1f2937);
      color: white;
    }
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px;
    }
    h1 {
      font-size: 22px;
      font-weight: bold;
    }
    .cart {
      display: flex;
      align-items: center;
      cursor: pointer;
    }
    .cart span {
      margin-left: 5px;
      font-weight: bold;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 20px;
      padding: 20px;
    }
    .card {
      background: #374151;
      padding: 15px;
      border-radius: 15px;
      text-align: center;
      transition: 0.3s;
    }
    .card:hover {
      background: #4b5563;
    }
    .card h2 {
      font-size: 18px;
      margin-bottom: 10px;
    }
    .card p {
      font-size: 16px;
      margin-bottom: 15px;
    }
    .btn {
      display: block;
      width: 100%;
      padding: 10px;
      background: #2563eb;
      border: none;
      border-radius: 10px;
      color: white;
      font-weight: bold;
      cursor: pointer;
      transition: 0.3s;
    }
    .btn:hover {
      background: #1d4ed8;
    }
    /* Checkout Modal */
    .modal {
      display: none;
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      background: rgba(0,0,0,0.8);
      justify-content: center;
      align-items: center;
      z-index: 999;
    }
    .modal-content {
      background: #374151;
      padding: 20px;
      border-radius: 15px;
      width: 90%;
      max-width: 400px;
    }
    .modal input, .modal select {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border-radius: 8px;
      border: none;
      background: #1f2937;
      color: white;
    }
    .qr-box {
      text-align: center;
    }
    .qr-box img {
      background: white;
      padding: 10px;
      border-radius: 10px;
    }
  </style>
</head>
<body>

  <header>
    <h1>Adin Store — MLBB Diamonds</h1>
    <div class="cart" onclick="openCheckout()">
      🛒 <span id="cartCount">0</span>
    </div>
  </header>

  <div class="grid" id="productGrid"></div>

  <!-- Modal Checkout -->
  <div class="modal" id="checkoutModal">
    <div class="modal-content" id="checkoutContent">
      <h2>Checkout</h2>
      <label>User ID</label>
      <input type="text" id="userId" placeholder="Masukkan User ID MLBB">
      <label>Server ID</label>
      <input type="text" id="serverId" placeholder="Masukkan Server ID MLBB">
      <label>Metode Pembayaran</label>
      <select id="paymentMethod">
        <option value="qris">QRIS</option>
      </select>
      <h3>Ringkasan Pesanan:</h3>
      <ul id="orderSummary"></ul>
      <button class="btn" onclick="handlePayment()">Bayar Sekarang</button>
      <button class="btn" style="margin-top:10px;background:#6b7280" onclick="closeCheckout()">Batal</button>
    </div>
  </div>

  <!-- Modal QRIS -->
  <div class="modal" id="qrisModal">
    <div class="modal-content qr-box">
      <h2>Scan QRIS untuk Membayar</h2>
      <img id="qrisCode" src="" alt="QRIS Code" width="200">
      <p>Silakan scan QRIS ini dengan aplikasi e-wallet pilihanmu.</p>
      <button class="btn" onclick="confirmPayment()">Saya Sudah Bayar</button>
    </div>
  </div>

  <script>
    const products = [
      { id: 1, name: "86 Diamonds", price: 24999 },
      { id: 2, name: "172 Diamonds", price: 47999 },
      { id: 3, name: "257 Diamonds", price: 71999 },
      { id: 4, name: "344 Diamonds", price: 95999 },
      { id: 5, name: "429 Diamonds", price: 119999 },
      { id: 6, name: "514 Diamonds", price: 143999 },
      { id: 7, name: "600 Diamonds", price: 167999 },
      { id: 8, name: "706 Diamonds", price: 191999 },
      { id: 9, name: "878 Diamonds", price: 239999 },
      { id: 10, name: "1412 Diamonds", price: 383999 },
      { id: 11, name: "2195 Diamonds", price: 575999 },
      { id: 12, name: "3688 Diamonds", price: 959999 },
      { id: 13, name: "5532 Diamonds", price: 1439999 },
      { id: 14, name: "9288 Diamonds", price: 2399999 },
    ];

    const cart = [];
    const productGrid = document.getElementById("productGrid");
    const cartCount = document.getElementById("cartCount");
    const orderSummary = document.getElementById("orderSummary");

    // Render produk
    products.forEach(p => {
      const div = document.createElement("div");
      div.className = "card";
      div.innerHTML = `
        <h2>${p.name}</h2>
        <p>Rp ${p.price.toLocaleString()}</p>
        <button class="btn" onclick="addToCart(${p.id})">Tambah ke Keranjang</button>
      `;
      productGrid.appendChild(div);
    });

    function addToCart(id) {
      const product = products.find(p => p.id === id);
      cart.push(product);
      cartCount.textContent = cart.length;
      alert(`${product.name} ditambahkan ke keranjang!`);
    }

    function openCheckout() {
      if (cart.length === 0) {
        alert("Keranjang masih kosong!");
        return;
      }
      document.getElementById("checkoutModal").style.display = "flex";
      orderSummary.innerHTML = cart.map(item => `<li>${item.name} - Rp ${item.price.toLocaleString()}</li>`).join('');
    }

    function closeCheckout() {
      document.getElementById("checkoutModal").style.display = "none";
    }

    function handlePayment() {
      const userId = document.getElementById("userId").value;
      const serverId = document.getElementById("serverId").value;
      if (!userId || !serverId) {
        alert("Isi User ID dan Server ID terlebih dahulu!");
        return;
      }
      document.getElementById("checkoutModal").style.display = "none";
      document.getElementById("qrisModal").style.display = "flex";
      document.getElementById("qrisCode").src = `https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=AdinStore-${Date.now()}`;
    }

    function confirmPayment() {
      alert("Pembayaran berhasil! Diamond akan segera diproses ✅");
      cart.length = 0;
      cartCount.textContent = 0;
      document.getElementById("qrisModal").style.display = "none";
    }
  </script>

</body>
</html>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: linear-gradient(to bottom, #111827, #1f2937);
  color: white;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
}
.logo {
  height: 40px;
}
.banner {
  text-align: center;
}
.banner-img {
  max-width: 100%;
  border-radius: 15px;
}
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 20px;
  padding: 20px;
}
.card {
  background: #374151;
  padding: 15px;
  border-radius: 15px;
  text-align: center;
  transition: 0.3s;
}
.card:hover {
  background: #4b5563;
}
.card h2 {
  font-size: 18px;
  margin-bottom: 10px;
}
.card p {
  font-size: 16px;
  margin-bottom: 15px;
}
.btn {
  display: block;
  width: 100%;
  padding: 10px;
  background: #2563eb;
  border: none;
  border-radius: 10px;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s;
}
.btn:hover {
  background: #1d4ed8;
}
.btn.cancel {
  margin-top: 10px;
  background: #6b7280;
}
.modal {
  display: none;
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  background: rgba(0,0,0,0.8);
  justify-content: center;
  align-items: center;
  z-index: 999;
}
.modal-content {
  background: #374151;
  padding: 20px;
  border-radius: 15px;
  width: 90%;
  max-width: 400px;
}
.modal input, .modal select {
  width: 100%;
  padding: 10px;
  margin: 8px 0;
  border-radius: 8px;
  border: none;
  background: #1f2937;
  color: white;
}
.qr-box {
  text-align: center;
}
.qr-box img {
  background: white;
  padding: 10px;
  border-radius: 10px;
}const products = [
  { id: 1, name: "86 Diamonds", price: 24999 },
  { id: 2, name: "172 Diamonds", price: 47999 },
  { id: 3, name: "257 Diamonds", price: 71999 },
  { id: 4, name: "344 Diamonds", price: 95999 },
  { id: 5, name: "429 Diamonds", price: 119999 },
  { id: 6, name: "514 Diamonds", price: 143999 },
  { id: 7, name: "600 Diamonds", price: 167999 },
  { id: 8, name: "706 Diamonds", price: 191999 },
  { id: 9, name: "878 Diamonds", price: 239999 },
  { id: 10, name: "1412 Diamonds", price: 383999 },
  { id: 11, name: "2195 Diamonds", price: 575999 },
  { id: 12, name: "3688 Diamonds", price: 959999 },
  { id: 13, name: "5532 Diamonds", price: 1439999 },
  { id: 14, name: "9288 Diamonds", price: 2399999 },
];

const cart = [];
const productGrid = document.getElementById("productGrid");
const cartCount = document.getElementById("cartCount");
const orderSummary = document.getElementById("orderSummary");

// Render produk
products.forEach(p => {
  const div = document.createElement("div");
  div.className = "card";
  div.innerHTML = `
    <h2>${p.name}</h2>
    <p>Rp ${p.price.toLocaleString()}</p>
    <button class="btn" onclick="addToCart(${p.id})">Tambah ke Keranjang</button>
  `;
  productGrid.appendChild(div);
});

function addToCart(id) {
  const product = products.find(p => p.id === id);
  cart.push(product);
  cartCount.textContent = cart.length;
  alert(`${product.name} ditambahkan ke keranjang!`);
}

function openCheckout() {
  if (cart.length === 0) {
    alert("Keranjang masih kosong!");
    return;
  }
  document.getElementById("checkoutModal").style.display = "flex";
  orderSummary.innerHTML = cart.map(item => `<li>${item.name} - Rp ${item.price.toLocaleString()}</li>`).join('');
}

function closeCheckout() {
  document.getElementById("checkoutModal").style.display = "none";
}

function handlePayment() {
  const userId = document.getElementById("userId").value;
  const serverId = document.getElementById("serverId").value;
  if (!userId || !serverId) {
    alert("Isi User ID dan Server ID terlebih dahulu!");
    return;
  }
  document.getElementById("checkoutModal").style.display = "none";
  document.getElementById("qrisModal").style.display = "flex";
}

function confirmPayment() {
  alert("Pembayaran berhasil! Diamond akan segera diproses ✅");
  cart.length = 0;
  cartCount.textContent = 0;
  document.getElementById("qrisModal").style.display = "none";
}
