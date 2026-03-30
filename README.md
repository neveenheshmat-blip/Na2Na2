<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Na2Na2 - Order Online</title>
    <!-- Standard CSS Link (Faster for Production) -->
    <link href="https://cdnjs.cloudflare.com/ajax/libs/tailwindcss/2.2.19/tailwind.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Bungee&family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Poppins', sans-serif; background-color: #fdfaf0; }
        .brand-font { font-family: 'Bungee', cursive; }
        .bg-brand-yellow { background-color: #ffcc00; }
        .bg-brand-red { background-color: #e63946; }
        .text-brand-red { color: #e63946; }
        .sticky-cart { position: sticky; top: 1rem; }
        .card-item { border: 1px solid #fed7aa; transition: transform 0.2s; }
        .card-item:active { transform: scale(0.95); }
    </style>
</head>
<body class="pb-20">

    <!-- Header -->
    <header class="bg-brand-yellow py-6 px-4 text-center shadow-lg mb-6">
        <h1 class="brand-font text-4xl md:text-6xl text-brand-red italic uppercase tracking-wider">Na2Na2</h1>
        <p class="font-bold text-red-700 mt-2 text-sm md:text-base">📍 Gardenia Mall | ⏰ 12 PM - 02 AM</p>
    </header>

    <div class="container mx-auto px-4 flex flex-col lg:flex-row gap-6">
        
        <!-- Menu Side -->
        <div class="flex-1">
            <h2 class="brand-font text-2xl text-orange-600 border-b-4 border-brand-yellow inline-block mb-6 uppercase">Menu Items</h2>
            <div id="menu-container" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <!-- Items are loaded here by JavaScript below -->
            </div>
        </div>

        <!-- Checkout Side -->
        <div class="w-full lg:w-96">
            <div class="bg-white p-6 rounded-3xl shadow-xl border-4 border-brand-yellow sticky-cart">
                <h3 class="brand-font text-xl mb-4 text-center text-brand-red">Your Order 🛒</h3>
                
                <div id="cart-list" class="space-y-3 mb-6 min-h-[80px] border-b pb-4 overflow-y-auto max-h-60">
                    <p class="text-gray-400 text-center py-4">Your cart is empty</p>
                </div>

                <div class="flex justify-between font-bold text-xl mb-6">
                    <span>Total:</span>
                    <span id="cart-total" class="text-brand-red">0 EGP</span>
                </div>

                <div class="space-y-4">
                    <input type="text" id="cust-name" placeholder="Full Name" class="w-full p-3 bg-gray-50 rounded-xl border focus:ring-2 focus:ring-yellow-400 outline-none">
                    <input type="tel" id="cust-phone" placeholder="WhatsApp Phone" class="w-full p-3 bg-gray-50 rounded-xl border focus:ring-2 focus:ring-yellow-400 outline-none">
                    <textarea id="cust-address" placeholder="Delivery Address (Floor/Flat/Building)" class="w-full p-3 bg-gray-50 rounded-xl border h-20 focus:ring-2 focus:ring-yellow-400 outline-none"></textarea>
                    
                    <div class="p-3 bg-green-50 text-green-700 text-xs rounded-lg font-bold text-center border border-green-200">
                        Payment: Cash on Delivery (COD) 💵
                    </div>

                    <button onclick="placeOrder()" class="w-full bg-brand-red text-white brand-font py-4 rounded-2xl text-lg shadow-lg hover:bg-red-700 active:scale-95 transition-all">
                        SEND TO WHATSAPP
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const items = [
            { name: "The Buff Pop", price: 175 },
            { name: "The Italiano Crunch", price: 150 },
            { name: "The Mexi Heat", price: 170 },
            { name: "Classic Hotdog Smash", price: 160 },
            { name: "The Masri Bite", price: 170 },
            { name: "Long Fries (Classic)", price: 120 },
            { name: "Long Fries (Dirty)", price: 145 },
            { name: "Loly Fries (Classic)", price: 70 },
            { name: "Chicken Wings (Classic)", price: 120 },
            { name: "Popcorn Chicken (Classic)", price: 100 },
            { name: "Corn Dog Stick", price: 110 },
            { name: "Cheese Dog", price: 140 },
            { name: "Waffle Fries (Classic)", price: 60 },
            { name: "Curly Fries (Classic)", price: 60 },
            { name: "Lemon Mojito", price: 100 },
            { name: "Boba Mango Mojito", price: 150 },
            { name: "Classic Pancake", price: 100 },
            { name: "Stuffed Ball Pancake", price: 145 },
            { name: "Waffle Sticks", price: 70 },
            { name: "Crazy Finger Waffle", price: 120 }
        ];

        let cart = [];
        const menuDiv = document.getElementById('menu-container');

        // Display Menu
        items.forEach(item => {
            const el = document.createElement('div');
            el.className = "bg-white p-4 rounded-2xl shadow-sm card-item flex justify-between items-center";
            el.innerHTML = `
                <div>
                    <h4 class="font-bold text-gray-800 text-sm md:text-base">${item.name}</h4>
                    <p class="text-brand-red font-bold">${item.price} EGP</p>
                </div>
                <button onclick="addToCart('${item.name}', ${item.price})" class="bg-brand-yellow font-bold px-5 py-2 rounded-full text-lg hover:bg-yellow-500">+</button>
            `;
            menuDiv.appendChild(el);
        });

        function addToCart(name, price) {
            cart.push({name, price});
            updateCart();
        }

        function updateCart() {
            const list = document.getElementById('cart-list');
            const totalDisplay = document.getElementById('cart-total');
            list.innerHTML = '';
            let total = 0;

            if (cart.length === 0) {
                list.innerHTML = '<p class="text-gray-400 text-center py-4 text-sm">Your cart is empty</p>';
                totalDisplay.innerText = "0 EGP";
                return;
            }

            cart.forEach((item, index) => {
                total += item.price;
                list.innerHTML += `
                    <div class="flex justify-between items-center text-sm border-b border-gray-100 pb-2">
                        <span class="text-gray-700">${item.name}</span>
                        <div class="flex items-center gap-3">
                            <span class="font-bold">${item.price} EGP</span>
                            <button onclick="remove(${index})" class="text-red-400 font-bold text-lg">×</button>
                        </div>
                    </div>`;
            });
            totalDisplay.innerText = total + " EGP";
        }

        function remove(idx) {
            cart.splice(idx, 1);
            updateCart();
        }

        function placeOrder() {
            const name = document.getElementById('cust-name').value;
            const phone = document.getElementById('cust-phone').value;
            const addr = document.getElementById('cust-address').value;

            if (cart.length === 0) return alert("Add items to your cart first!");
            if (!name || !phone || !addr) return alert("Please enter your name, phone, and address.");

            let itemsTxt = cart.map(i => `- ${i.name} (${i.price} EGP)`).join('%0A');
            let total = cart.reduce((s, i) => s + i.price, 0);

            let msg = `*--- NEW ORDER: Na2Na2 ---*%0A`;
            msg += `*Name:* ${name}%0A`;
            msg += `*Phone:* ${phone}%0A`;
            msg += `*Address:* ${addr}%0A%0A`;
            msg += `*Items:*%0A${itemsTxt}%0A%0A`;
            msg += `*TOTAL:* ${total} EGP%0A`;
            msg += `*Payment:* Cash on Delivery 💵`;

            window.open(`https://wa.me/201223469554?text=${msg}`, '_blank');
        }
    </script>
</body>
</html>