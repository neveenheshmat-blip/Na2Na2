
    <header class="bg-brand-yellow py-8 px-4 text-center shadow-md mb-8">
        <h1 class="brand-font text-5xl text-red-700 italic uppercase">Na2Na2</h1>
        <p class="font-bold text-red-600 mt-2">📍 Gardenia Mall | ⏰ 12 PM - 02 AM</p>
    </header>

    <div class="container mx-auto px-4 flex flex-col lg:flex-row gap-8">
        <div class="flex-1">
            <h2 class="brand-font text-2xl text-orange-600 border-b-4 border-brand-yellow inline-block mb-6 uppercase">The Menu</h2>
            <div id="menu-container" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
        </div>

        <!-- Sidebar Cart -->
        <div class="w-full lg:w-96">
            <div class="bg-white p-6 rounded-3xl shadow-2xl border-4 border-brand-yellow sticky-cart">
                <h3 class="brand-font text-2xl mb-4 text-center text-red-600">Your Order 🛒</h3>
                <div id="cart-list" class="space-y-3 mb-6 min-h-[100px] border-b pb-4">
                    <p class="text-gray-400 text-center py-4">Your cart is empty</p>
                </div>
                <div class="flex justify-between font-bold text-xl mb-6">
                    <span>Total:</span>
                    <span id="cart-total" class="text-red-600">0 EGP</span>
                </div>
                <div class="space-y-4">
                    <input type="text" id="cust-name" placeholder="Your Name" class="w-full p-3 bg-gray-50 rounded-xl border">
                    <input type="text" id="cust-phone" placeholder="Phone Number" class="w-full p-3 bg-gray-50 rounded-xl border">
                    <textarea id="cust-address" placeholder="Delivery Address (Gardenia City...)" class="w-full p-3 bg-gray-50 rounded-xl border h-20"></textarea>
                    <div class="p-3 bg-green-50 text-green-700 text-sm rounded-lg font-bold text-center">Payment: Cash on Delivery 💵</div>
                    <button onclick="placeOrder()" class="w-full bg-brand-red text-white brand-font py-4 rounded-2xl text-xl hover:scale-105 transition-transform">ORDER VIA WHATSAPP</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const items = [
            {name: 'The Buff Pop', price: 175, cat: 'Loaded'},
            {name: 'The Italiano Crunch', price: 150, cat: 'Loaded'},
            {name: 'The Mexi Heat', price: 170, cat: 'Loaded'},
            {name: 'Classic Hotdog Smash', price: 160, cat: 'Loaded'},
            {name: 'The Masri Bite', price: 170, cat: 'Loaded'},
            {name: 'Long Fries (Classic)', price: 120, cat: 'Potata'},
            {name: 'Long Fries (Dirty)', price: 145, cat: 'Potata'},
            {name: 'Chicken Wings (Classic)', price: 120, cat: 'Wings'},
            {name: 'Chicken Wings (Dirty)', price: 145, cat: 'Wings'},
            {name: 'Popcorn Chicken (Classic)', price: 100, cat: 'Wings'},
            {name: 'Popcorn Chicken (Dirty)', price: 125, cat: 'Wings'},
            {name: 'Corn Dog Stick', price: 110, cat: 'Hotdog'},
            {name: 'Cheese Dog', price: 140, cat: 'Hotdog'},
            {name: 'Waffle Fries (Classic)', price: 60, cat: 'Fries'},
            {name: 'Curly Fries (Classic)', price: 60, cat: 'Fries'},
            {name: 'Lemon Mojito (Classic)', price: 100, cat: 'Drinks'},
            {name: 'Mango Mojito (Boba)', price: 150, cat: 'Drinks'},
            {name: 'Classic Pancake', price: 100, cat: 'Sweets'},
            {name: 'Stuffed Ball Pancake', price: 145, cat: 'Sweets'},
            {name: 'Crazy Finger Waffle', price: 120, cat: 'Sweets'}
        ];

        let cart = [];
        const menuContainer = document.getElementById('menu-container');

        items.forEach(item => {
            const div = document.createElement('div');
            div.className = "bg-white p-4 rounded-2xl shadow-sm border border-orange-100 flex justify-between items-center";
            div.innerHTML = `<div><h4 class="font-bold">${item.name}</h4><p class="text-red-600 font-bold">${item.price} EGP</p></div>
                             <button onclick="addToCart('${item.name}', ${item.price})" class="bg-brand-yellow font-bold px-4 py-2 rounded-full">+</button>`;
            menuContainer.appendChild(div);
        });

        function addToCart(name, price) {
            cart.push({name, price});
            renderCart();
        }

        function renderCart() {
            const list = document.getElementById('cart-list');
            const totalSpan = document.getElementById('cart-total');
            list.innerHTML = '';
            let total = 0;
            cart.forEach((item, index) => {
                total += item.price;
                list.innerHTML += `<div class="flex justify-between text-sm"><span>${item.name}</span><span class="font-bold">${item.price} EGP</span></div>`;
            });
            totalSpan.innerText = total + " EGP";
        }

        function placeOrder() {
            const name = document.getElementById('cust-name').value;
            const phone = document.getElementById('cust-phone').value;
            const address = document.getElementById('cust-address').value;
            if (cart.length === 0 || !name || !phone || !address) return alert("Please fill all details!");

            let orderText = cart.map(i => `- ${i.name} (${i.price} EGP)`).join('%0A');
            let total = cart.reduce((s, i) => s + i.price, 0);
            let message = `*NEW ORDER: Na2Na2*%0A*Name:* ${name}%0A*Phone:* ${phone}%0A*Address:* ${address}%0A%0A*Items:*%0A${orderText}%0A%0A*TOTAL:* ${total} EGP`;
            
            window.open(`https://wa.me/201223469554?text=${message}`, '_blank');
        }
    </script>
</body>
</html>