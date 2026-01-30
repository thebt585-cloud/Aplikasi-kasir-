# Aplikasi-kasir-
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kasir Mobile Pro</title>

<style>
:root{
  --bg:#f1f5f9;--card:#fff;--text:#000;
  --primary:#22c55e;--danger:#ef4444;--blue:#3b82f6;
}
.dark{
  --bg:#020617;--card:#0f172a;--text:#fff;
}
*{box-sizing:border-box;font-family:Arial}
body{margin:0;background:var(--bg);color:var(--text)}
.app{max-width:420px;margin:auto;min-height:100vh;display:flex;flex-direction:column}
header{
  background:var(--primary);color:#fff;
  padding:14px;text-align:center;font-size:18px;font-weight:bold;
}
.card{
  background:var(--card);
  margin:10px;padding:12px;border-radius:12px;
  box-shadow:0 3px 8px rgba(0,0,0,.15)
}
button{
  border:none;border-radius:8px;padding:10px;width:100%;
  font-size:15px;margin-top:6px
}
.green{background:var(--primary);color:#fff}
.red{background:var(--danger);color:#fff}
.blue{background:var(--blue);color:#fff}
.small{padding:5px 10px;width:auto}
.row{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.qty{display:flex;gap:5px}
input,select{
  width:100%;padding:10px;border-radius:8px;
  border:1px solid #ccc;margin-top:6px
}
.hide{display:none}
</style>
</head>

<body>
<div class="app">

<header>
Kasir Mobile Pro
<button class="small" onclick="toggleDark()">🌙</button>
</header>

<!-- LOGIN -->
<div class="card" id="login">
<h3>🔐 Login Kasir</h3>
<input id="user" placeholder="Username">
<input id="pass" type="password" placeholder="Password">
<button class="green" onclick="login()">Masuk</button>
</div>

<!-- APP -->
<div id="main" class="hide">

<div class="card">
<h3>🍽️ Produk</h3>
<div class="row"><span>Bakso Mercon Ceker</span><button class="small green" onclick="add('Es Teh',5000)">+</button></div>
<div class="row"><span>Bakso Kuah</span><button class="small green" onclick="add('Nasi Goreng',5000)">+</button></div>
<div class="row"><span>Cilok Bumbu</span><button class="small green" onclick="add('Ayam Goreng',5000)">+</button></div>
<div class="row"><span>Cilok Kuah</span><button class="small green" onclick="add('Ayam Goreng',5000)">+</button></div>
<div class="row"><span>Seblak Ceker</span><button class="small green" onclick="add('Ayam Goreng',5000)">+</button></div>
<div class="row"><span>Gorengan</span><button class="small green" onclick="add('Ayam Goreng',5000)">+</button></div>
</div>
<div class="card"
<h3>🛒 Keranjang</h3>
<div id="cart"></div>
<b id="total">Total: Rp 0</b>

<select id="payMethod">
<option>QRIS</option>
<option>DANA</option>
<option>OVO</option>
</select>

<button class="blue" onclick="pay()">Bayar</button>
<button class="red" onclick="clearCart()">Hapus Keranjang</button>
</div>

<div class="card">
<h3>📊 Riwayat Transaksi</h3>
<div id="history"></div>
</div>

</div>
</div>

<script>
let cart=[],history=JSON.parse(localStorage.getItem("history"))||[];

function login(){
  if(user.value&&pass.value){
    loginBox.classList.add("hide");
    main.classList.remove("hide");
    renderHistory();
  }else alert("Login gagal");
}
const loginBox=document.getElementById("login");
const main=document.getElementById("main");

function toggleDark(){
  document.body.classList.toggle("dark");
}

function add(name,price){
  let i=cart.find(x=>x.name==name);
  i?i.qty++:cart.push({name,price,qty:1});
  renderCart();
}

function renderCart(){
  cartBox.innerHTML="";
  let t=0;
  cart.forEach((i,idx)=>{
    t+=i.price*i.qty;
    cartBox.innerHTML+=`
    <div class="row">
      ${i.name}
      <div class="qty">
        <button onclick="qty(${idx},-1)">−</button>
        ${i.qty}
        <button onclick="qty(${idx},1)">+</button>
      </div>
    </div>`;
  });
  total.innerText="Total: Rp "+t.toLocaleString();
}

function qty(i,n){
  cart[i].qty+=n;
  if(cart[i].qty<=0) cart.splice(i,1);
  renderCart();
}

function clearCart(){
  cart=[];renderCart();
}

function pay(){
  if(!cart.length)return alert("Keranjang kosong");
  const data={
    date:new Date().toLocaleString(),
    total:total.innerText,
    method:payMethod.value
  };
  history.push(data);
  localStorage.setItem("history",JSON.stringify(history));
  printStruk(data);
  cart=[];renderCart();renderHistory();
}

function renderHistory(){
  historyBox.innerHTML="";
  history.forEach(h=>{
    historyBox.innerHTML+=`
    <div>${h.date}<br>${h.total} (${h.method})</div><hr>`;
  });
}

function printStruk(d){
  const w=window.open("","");
  w.document.write(`<pre>
=== STRUK ===
${d.date}
${d.total}
Bayar: ${d.method}
Terima kasih 🙏
</pre>`);
  w.print();
}

const cartBox=document.getElementById("cart");
const historyBox=document.getElementById("history");
</script>
</body>
</html>
