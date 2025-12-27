<!DOCTYPE html>
<html lang="ur">
<head>
<meta charset="UTF-8">
<title>Mayo Stores</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{
    margin:0;
    font-family:"Noto Nastaliq Urdu",Arial;
    background:#f4f4f4;
    direction:rtl;
}
header{
    background:#111;
    color:#fff;
    padding:15px;
    text-align:center;
    font-size:26px;
}
.products{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(230px,1fr));
    gap:20px;
    padding:20px;
}
.card, .admin{
    background:#fff;
    padding:15px;
    border-radius:15px;
    box-shadow:0 10px 20px rgba(0,0,0,0.2);
    text-align:center;
}
.price{color:green;font-size:18px;font-weight:bold}
button{
    padding:10px;
    border:none;
    border-radius:8px;
    cursor:pointer;
    background:#27ae60;
    color:#fff;
    width:100%;
    margin-top:8px;
}
input{
    width:100%;
    padding:8px;
    margin:5px 0;
}
.admin-panel{display:none;padding:20px}
.whatsapp{
    position:fixed;bottom:20px;right:20px;
    width:60px;height:60px;
    background:#25D366;color:#fff;
    border-radius:50%;
    display:flex;align-items:center;justify-content:center;
    font-size:30px;text-decoration:none;
}
</style>

<script>
let products = [
 {id:1,name:"ہینڈ فری",price:1500},
 {id:2,name:"ایئر بڈز",price:3200},
 {id:3,name:"واچ الٹرا",price:5999},
 {id:4,name:"گیمنگ زیرو",price:4500},
 {id:5,name:"ایئر بڈز پرو",price:3999},
];

if(localStorage.prices){
    let saved = JSON.parse(localStorage.prices);
    products.forEach(p=>{
        if(saved[p.id]) p.price = saved[p.id];
    });
}

function showProducts(){
    let html="";
    products.forEach(p=>{
        html+=`
        <div class="card">
        <h3>${p.name}</h3>
        <div class="price">${p.price} روپے</div>
        <button onclick="order('${p.name}','${p.price}')">واٹس ایپ پر آرڈر</button>
        </div>`;
    });
    document.getElementById("products").innerHTML=html;
}

function order(name,price){
    let msg=`السلام علیکم Mayo Stores
میں یہ پروڈکٹ خریدنا چاہتا ہوں:
${name}
قیمت: ${price} روپے`;
    window.open("https://wa.me/923194898637?text="+encodeURIComponent(msg));
}

function login(){
    let u=document.getElementById("user").value;
    let p=document.getElementById("pass").value;
    if(u==="admin" && p==="1234"){
        document.getElementById("login").style.display="none";
        document.getElementById("panel").style.display="block";
        loadAdmin();
    } else alert("غلط لاگ ان");
}

function loadAdmin(){
    let html="";
    products.forEach(p=>{
        html+=`
        <div class="admin">
        <b>${p.name}</b>
        <input type="number" id="p${p.id}" value="${p.price}">
        <button onclick="save(${p.id})">محفوظ کریں</button>
        </div>`;
    });
    document.getElementById("adminProducts").innerHTML=html;
}

function save(id){
    let val=document.getElementById("p"+id).value;
    let saved = localStorage.prices ? JSON.parse(localStorage.prices) : {};
    saved[id]=val;
    localStorage.prices = JSON.stringify(saved);
    alert("قیمت اپڈیٹ ہو گئی");
    location.reload();
}

window.onload=function(){
    if(location.hash==="#admin"){
        document.getElementById("login").style.display="block";
    } else showProducts();
}
</script>
</head>

<body>

<header>مایو اسٹورز</header>

<div id="products" class="products"></div>

<!-- ADMIN LOGIN -->
<div id="login" class="admin-panel">
    <div class="admin">
        <h3>Admin Login</h3>
        <input id="user" placeholder="Username">
        <input id="pass" type="password" placeholder="Password">
        <button onclick="login()">Login</button>
    </div>
</div>

<!-- ADMIN PANEL -->
<div id="panel" class="admin-panel">
    <h3>قیمت تبدیل کریں</h3>
    <div id="adminProducts"></div>
</div>

<a class="whatsapp" href="https://wa.me/923194898637">💬</a>

</body>
</html>
