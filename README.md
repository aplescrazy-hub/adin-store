import { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { ShoppingCart } from "lucide-react"; import { motion } from "framer-motion";

export default function AdinStore() { const [cart, setCart] = useState([]); const [showCheckout, setShowCheckout] = useState(false); const [userId, setUserId] = useState(""); const [serverId, setServerId] = useState(""); const [paymentMethod, setPaymentMethod] = useState("qris"); const [showQr, setShowQr] = useState(false);

const products = [ { id: 1, name: "86 Diamonds", price: 24999 }, { id: 2, name: "172 Diamonds", price: 47999 }, { id: 3, name: "257 Diamonds", price: 71999 }, { id: 4, name: "344 Diamonds", price: 95999 }, { id: 5, name: "429 Diamonds", price: 119999 }, { id: 6, name: "514 Diamonds", price: 143999 }, { id: 7, name: "600 Diamonds", price: 167999 }, { id: 8, name: "706 Diamonds", price: 191999 }, { id: 9, name: "878 Diamonds", price: 239999 }, { id: 10, name: "1412 Diamonds", price: 383999 }, { id: 11, name: "2195 Diamonds", price: 575999 }, { id: 12, name: "3688 Diamonds", price: 959999 }, { id: 13, name: "5532 Diamonds", price: 1439999 }, { id: 14, name: "9288 Diamonds", price: 2399999 }, ];

const addToCart = (product) => { setCart([...cart, product]); };

const handleCheckout = () => { if (!userId || !serverId) { alert("Mohon isi User ID dan Server ID terlebih dahulu!"); return; } if (paymentMethod === "qris") { setShowQr(true); return; } };

const confirmPayment = () => { alert(Pembayaran berhasil!\nUser ID: ${userId}\nServer ID: ${serverId}\nMetode: ${paymentMethod.toUpperCase()}\nTotal Item: ${cart.length}); setCart([]); setUserId(""); setServerId(""); setPaymentMethod("qris"); setShowCheckout(false); setShowQr(false); };

return ( <div className="min-h-screen bg-gradient-to-b from-gray-900 to-gray-800 text-white p-6"> <header className="flex justify-between items-center mb-10"> <h1 className="text-3xl font-bold">Adin Store — Mobile Legends Diamonds</h1> <div className="flex items-center space-x-2 cursor-pointer" onClick={() => setShowCheckout(!showCheckout)}> <ShoppingCart /> <span>{cart.length}</span> </div> </header>

{!showCheckout ? (
    <motion.div
      className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{ duration: 0.5 }}
    >
      {products.map((product) => (
        <Card
          key={product.id}
          className="bg-gray-700 text-white rounded-2xl shadow-lg hover:shadow-xl transition"
        >
          <CardContent className="p-4 flex flex-col items-center">
            <h2 className="text-xl font-semibold mb-2">{product.name}</h2>
            <p className="text-lg mb-4">Rp {product.price.toLocaleString()}</p>
            <Button onClick={() => addToCart(product)} className="w-full">
              Tambah ke Keranjang
            </Button>
          </CardContent>
        </Card>
      ))}
    </motion.div>
  ) : !showQr ? (
    <motion.div
      className="bg-gray-700 p-6 rounded-2xl shadow-xl max-w-lg mx-auto"
      initial={{ y: 50, opacity: 0 }}
      animate={{ y: 0, opacity: 1 }}
      transition={{ duration: 0.5 }}
    >
      <h2 className="text-2xl font-bold mb-4">Checkout</h2>
      <div className="mb-4">
        <label className="block mb-1">User ID</label>
        <input
          type="text"
          value={userId}
          onChange={(e) => setUserId(e.target.value)}
          className="w-full p-2 rounded bg-gray-800 border border-gray-600"
          placeholder="Masukkan User ID MLBB"
        />
      </div>
      <div className="mb-4">
        <label className="block mb-1">Server ID</label>
        <input
          type="text"
          value={serverId}
          onChange={(e) => setServerId(e.target.value)}
          className="w-full p-2 rounded bg-gray-800 border border-gray-600"
          placeholder="Masukkan Server ID MLBB"
        />
      </div>
      <div className="mb-4">
        <label className="block mb-1">Metode Pembayaran</label>
        <select
          value={paymentMethod}
          onChange={(e) => setPaymentMethod(e.target.value)}
          className="w-full p-2 rounded bg-gray-800 border border-gray-600"
        >
          <option value="qris">QRIS</option>
        </select>
      </div>
      <div className="mb-6">
        <h3 className="font-semibold mb-2">Ringkasan Pesanan:</h3>
        {cart.length > 0 ? (
          <ul className="mb-2 list-disc list-inside">
            {cart.map((item, index) => (
              <li key={index}>{item.name} - Rp {item.price.toLocaleString()}</li>
            ))}
          </ul>
        ) : (
          <p>Keranjang masih kosong</p>
        )}
      </div>
      <Button onClick={handleCheckout} className="w-full">
        Bayar Sekarang
      </Button>
    </motion.div>
  ) : (
    <motion.div
      className="bg-gray-700 p-6 rounded-2xl shadow-xl max-w-md mx-auto text-center"
      initial={{ y: 50, opacity: 0 }}
      animate={{ y: 0, opacity: 1 }}
      transition={{ duration: 0.5 }}
    >
      <h2 className="text-2xl font-bold mb-4">Scan QRIS untuk Membayar</h2>
      <img
        src="https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=AdinStore-Pembayaran"
        alt="QRIS Code"
        className="mx-auto mb-4 rounded bg-white p-2"
      />
      <p className="mb-4">Silakan scan QRIS ini dengan aplikasi e-wallet pilihanmu.</p>
      <Button onClick={confirmPayment} className="w-full">
        Saya Sudah Bayar
      </Button>
    </motion.div>
  )}
</div>

); }

