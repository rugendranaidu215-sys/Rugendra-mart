<!DOCTYPE html>
<html lang="en" class="overflow-x-hidden max-w-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RUGENDRA MART | Premium Online Food, Meats & Fresh Shopping</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        emeraldDark: '#064e3b',
                        emeraldBrand: '#047857',
                        goldBrand: '#d97706',
                        goldLight: '#f59e0b',
                        creamBg: '#fefbf6',
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Supabase JS SDK -->
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
    <style>
        body { background-color: #fefbf6; font-family: 'Inter', sans-serif; overflow-x: hidden; }
        .custom-scrollbar::-webkit-scrollbar { width: 6px; height: 6px; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #d97706; border-radius: 4px; }
    </style>
</head>
<body class="bg-creamBg text-gray-800 antialiased overflow-x-hidden max-w-full">

    <!-- SPLASH SCREEN (2 Seconds Luxury Brand Greeting) -->
    <div id="splash-screen" class="fixed inset-0 z-50 bg-emeraldDark flex flex-col items-center justify-center text-white transition-opacity duration-700">
        <div class="w-24 h-24 rounded-3xl bg-gradient-to-tr from-goldBrand to-goldLight flex items-center justify-center shadow-2xl mb-4 border-2 border-white/20 animate-pulse">
            <i class="fa-solid fa-basket-shopping text-4xl text-white"></i>
        </div>
        <h1 class="text-3xl font-extrabold tracking-wider">RUGENDRA <span class="text-goldLight">MART</span></h1>
        <p class="text-xs text-emerald-200 mt-1 uppercase tracking-widest font-semibold">100% Cloud Synced Fresh Delivery</p>
    </div>

    <!-- Top Announcement Bar -->
    <div class="bg-emeraldDark text-white text-xs sm:text-sm py-2 px-4 text-center font-medium flex justify-between items-center z-40 relative">
        <div class="hidden sm:block">🌿 Supabase Live Cloud Engine Connected</div>
        <div>⚡ Use code <span class="text-goldLight font-bold underline">RUGENDRA30</span> for ₹30 cashback + FREE Delivery!</div>
        <div id="top-user-greet" class="text-goldLight font-semibold">Welcome Guest</div>
    </div>

    <!-- Main Sticky Header -->
    <header class="sticky top-0 bg-white shadow-md z-30 border-b border-emerald-800/10">
        <div class="max-w-7xl mx-auto px-4 py-3 flex items-center justify-between gap-2 sm:gap-4">
            
            <!-- Left: Menu Trigger & Logo -->
            <div class="flex items-center gap-3">
                <button onclick="openSideMenu()" type="button" class="cursor-pointer bg-emeraldDark text-white px-3 py-2 rounded-lg text-sm font-semibold flex items-center gap-2 hover:bg-emerald-800 transition shadow-sm">
                    <i class="fa-solid fa-bars"></i>
                    <span class="hidden sm:inline">Menu</span>
                </button>
                <a href="#" onclick="switchSection('home')" class="flex items-center gap-2 cursor-pointer">
                    <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-emeraldDark to-goldBrand flex items-center text-white justify-center shadow-md">
                        <i class="fa-solid fa-basket-shopping text-xl"></i>
                    </div>
                    <div>
                        <h1 class="text-lg sm:text-xl font-extrabold text-emeraldDark tracking-tight leading-none">RUGENDRA</h1>
                        <span class="text-xs font-bold text-goldBrand tracking-widest uppercase">MART</span>
                    </div>
                </a>
            </div>

            <!-- Search Bar -->
            <div class="flex-1 max-w-xl relative hidden sm:block">
                <div class="relative flex items-center">
                    <span class="absolute left-3 text-gray-400"><i class="fa-solid fa-magnifying-glass"></i></span>
                    <input type="text" id="main-search-input" oninput="handleLiveSearch(this.value)" placeholder="Search fresh chicken, mutton, biryani, fruits..." class="w-full pl-10 pr-20 py-2 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:outline-none focus:border-goldBrand focus:bg-white transition shadow-inner">
                    <button onclick="executeSearch()" type="button" class="absolute right-1.5 bg-emeraldDark text-white px-3 py-1 rounded-lg text-xs font-semibold hover:bg-emerald-800 transition">Search</button>
                </div>
                <div id="search-suggestions-box" class="absolute top-full left-0 right-0 bg-white shadow-xl rounded-xl mt-1 border border-gray-100 hidden z-50 max-h-64 overflow-y-auto custom-scrollbar"></div>
            </div>

            <!-- Right Actions: Wishlist, Cart, Account -->
            <div class="flex items-center gap-2 sm:gap-4">
                <button onclick="switchSection('wishlist')" type="button" class="relative p-2 text-gray-700 hover:text-goldBrand transition cursor-pointer" title="Wishlist">
                    <i class="fa-regular fa-heart text-xl"></i>
                    <span id="wishlist-badge" class="absolute -top-1 -right-1 bg-goldBrand text-white text-[10px] font-bold w-4 h-4 rounded-full flex items-center justify-center shadow">0</span>
                </button>
                
                <button onclick="openCartDrawer()" type="button" class="relative bg-emeraldDark text-white px-3 sm:px-4 py-2 rounded-xl flex items-center gap-2 hover:bg-emerald-800 transition shadow cursor-pointer">
                    <i class="fa-solid fa-cart-shopping"></i>
                    <span class="text-xs font-bold hidden sm:inline" id="cart-total-price">₹0</span>
                    <span id="cart-count-badge" class="bg-goldBrand text-white text-[10px] font-bold px-1.5 py-0.5 rounded-full">0</span>
                </button>

                <button onclick="openAccountModal()" type="button" class="flex items-center gap-1.5 px-3 py-2 rounded-xl border border-gray-200 hover:border-goldBrand text-xs font-semibold bg-gray-50 transition cursor-pointer">
                    <i class="fa-regular fa-user text-goldBrand text-base"></i>
                    <span class="hidden lg:inline" id="header-account-name">Account</span>
                </button>
            </div>
        </div>
    </header>

    <!-- SIDE MENU DRAWER -->
    <div id="side-menu-drawer" class="fixed inset-0 z-50 bg-black/60 backdrop-blur-sm hidden transition-opacity">
        <div class="absolute top-0 left-0 bottom-0 w-80 bg-white shadow-2xl flex flex-col transform transition-transform">
            <div class="p-4 bg-emeraldDark text-white flex justify-between items-center">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-basket-shopping text-goldLight text-xl"></i>
                    <h3 class="font-bold text-lg">RUGENDRA MART</h3>
                </div>
                <button onclick="closeSideMenu()" type="button" class="text-white hover:text-goldLight text-xl"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div class="flex-1 overflow-y-auto p-4 space-y-4 custom-scrollbar">
                <div class="space-y-1">
                    <p class="text-xs font-bold text-gray-400 uppercase tracking-wider mb-2">Navigation</p>
                    <a href="#" onclick="switchSection('home'); closeSideMenu();" class="flex items-center gap-3 p-2.5 rounded-xl hover:bg-gray-100 text-sm font-medium text-gray-700"><i class="fa-solid fa-house text-emeraldDark w-5"></i> Home Dashboard</a>
                    <a href="#" onclick="switchSection('referral'); closeSideMenu();" class="flex items-center gap-3 p-2.5 rounded-xl hover:bg-goldBrand/10 text-sm font-medium text-goldBrand"><i class="fa-solid fa-gift w-5"></i> Refer & Earn ₹30</a>
                    <a href="#" onclick="switchSection('orders'); closeSideMenu();" class="flex items-center gap-3 p-2.5 rounded-xl hover:bg-gray-100 text-sm font-medium text-gray-700"><i class="fa-solid fa-box-archive w-5"></i> Live Orders & Tracking</a>
                    <a href="#" onclick="switchSection('vendor'); closeSideMenu();" class="flex items-center gap-3 p-2.5 rounded-xl hover:bg-emerald-50 text-sm font-bold text-emeraldDark"><i class="fa-solid fa-store w-5"></i> Sell on Rugendra Mart</a>
                </div>
                <hr class="border-gray-100">
                <div class="space-y-1">
                    <p class="text-xs font-bold text-gray-400 uppercase tracking-wider mb-2">Categories</p>
                    <div id="side-menu-categories" class="space-y-1"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- MAIN APP CONTAINER -->
    <main id="main-content-area" class="max-w-7xl mx-auto px-4 py-6 pb-24">
        
        <!-- SECTION: HOME -->
        <div id="section-home" class="space-y-8">
            <div class="relative rounded-3xl overflow-hidden bg-gradient-to-r from-emeraldDark to-emeraldBrand text-white shadow-xl p-6 sm:p-12 flex flex-col md:flex-row items-center justify-between gap-6">
                <div class="space-y-4 max-w-xl text-center md:text-left">
                    <span class="bg-goldBrand text-white text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wider shadow">⚡ Cloud Synced Fresh Delivery</span>
                    <h2 class="text-3xl sm:text-5xl font-extrabold tracking-tight leading-tight">Fresh Farm Meats, Juices & Hot Foods</h2>
                    <p class="text-emerald-100 text-sm sm:text-base">Order tender chicken, fresh mutton, farm fish, delicious biryanis instantly with real-time tracking.</p>
                </div>
            </div>

            <!-- Categories Grid -->
            <div class="space-y-4">
                <h3 class="text-xl font-extrabold text-emeraldDark">Shop By Categories</h3>
                <div id="home-categories-grid" class="grid grid-cols-2 sm:grid-cols-4 md:grid-cols-8 gap-4"></div>
            </div>

            <!-- Products Grid -->
            <div class="space-y-4">
                <h3 class="text-xl font-extrabold text-emeraldDark" id="product-section-title">Explore All Fresh Products</h3>
                <div id="products-grid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4"></div>
            </div>
        </div>

        <!-- SECTION: VENDOR PORTAL (Sell on Rugendra Mart) -->
        <div id="section-vendor" class="space-y-6 hidden">
            <div class="bg-white p-6 sm:p-8 rounded-3xl shadow-xl border border-gray-100 max-w-2xl mx-auto space-y-6">
                <div class="text-center space-y-2">
                    <div class="w-14 h-14 bg-emerald-50 text-emeraldDark rounded-2xl flex items-center justify-center text-2xl mx-auto shadow"><i class="fa-solid fa-store"></i></div>
                    <h2 class="text-2xl font-extrabold text-emeraldDark">Sell on Rugendra Mart</h2>
                    <p class="text-xs text-gray-500">List your fresh products, foods, or meats. Platform Commission: <span class="font-bold text-goldBrand">17%</span></p>
                </div>
                <div class="space-y-4">
                    <div>
                        <label class="text-xs font-bold text-gray-600">Store / Business Name</label>
                        <input type="text" id="vendor-store-name" placeholder="e.g. Sri Balaji Fresh Meats" class="w-full p-3 bg-gray-50 border rounded-xl text-sm mt-1 focus:outline-none focus:border-goldBrand">
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <div>
                            <label class="text-xs font-bold text-gray-600">Product Name</label>
                            <input type="text" id="vendor-prod-name" placeholder="e.g. Country Chicken" class="w-full p-3 bg-gray-50 border rounded-xl text-sm mt-1 focus:outline-none focus:border-goldBrand">
                        </div>
                        <div>
                            <label class="text-xs font-bold text-gray-600">Select Category</label>
                            <select id="vendor-prod-cat" class="w-full p-3 bg-gray-50 border rounded-xl text-sm mt-1 focus:outline-none focus:border-goldBrand">
                                <option value="Fresh Chicken">Fresh Chicken</option>
                                <option value="Fresh Mutton">Fresh Mutton</option>
                                <option value="Fresh Fish & Seafood">Fresh Fish & Seafood</option>
                                <option value="Foods & Restaurants">Foods & Restaurants</option>
                                <option value="Fresh Juices">Fresh Juices</option>
                                <option value="Bakery Items">Bakery Items</option>
                            </select>
                        </div>
                    </div>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <div>
                            <label class="text-xs font-bold text-gray-600">Your Selling Price (₹)</label>
                            <input type="number" id="vendor-prod-price" oninput="calculateCommission()" placeholder="500" class="w-full p-3 bg-gray-50 border rounded-xl text-sm mt-1 focus:outline-none focus:border-goldBrand">
                        </div>
                        <div>
                            <label class="text-xs font-bold text-gray-600">Net Payout (After 17% Commission)</label>
                            <input type="text" id="vendor-net-payout" readonly value="₹0" class="w-full p-3 bg-emerald-50 border border-emerald-200 rounded-xl text-sm mt-1 font-bold text-emeraldDark">
                        </div>
                    </div>
                    <div>
                        <label class="text-xs font-bold text-gray-600">Product Image (Select from Gallery)</label>
                        <input type="file" id="vendor-prod-img" accept="image/*" class="w-full p-2 bg-gray-50 border rounded-xl text-xs mt-1">
                    </div>
                    <button onclick="publishVendorProduct()" type="button" class="w-full bg-emeraldDark text-white font-bold py-3 rounded-xl shadow-lg hover:bg-emerald-800 transition text-sm cursor-pointer">Publish Item to Marketplace</button>
                </div>
            </div>
        </div>

        <!-- SECTION: REFER & EARN -->
        <div id="section-referral" class="space-y-6 hidden">
            <div class="bg-gradient-to-r from-emeraldDark to-emeraldBrand rounded-3xl p-6 sm:p-10 text-white shadow-2xl text-center space-y-4">
                <div class="w-16 h-16 bg-goldBrand rounded-full flex items-center justify-center mx-auto text-3xl shadow-lg"><i class="fa-solid fa-gift"></i></div>
                <h2 class="text-3xl font-extrabold">Invite Friends & Earn ₹30</h2>
                <p class="text-emerald-100 max-w-lg mx-auto text-sm">Share your unique referral link. When your friend completes an order, ₹30 is credited instantly!</p>
                <button onclick="shareOnWhatsApp()" type="button" class="bg-emerald-600 hover:bg-emerald-500 text-white font-bold px-8 py-3 rounded-xl shadow-lg inline-flex items-center gap-2 text-sm transition cursor-pointer">
                    <i class="fa-brands fa-whatsapp text-lg"></i> Share via WhatsApp
                </button>
            </div>
        </div>

        <!-- SECTION: LIVE ORDERS & TRACKING -->
        <div id="section-orders" class="space-y-6 hidden">
            <h2 class="text-2xl font-extrabold text-emeraldDark">Live Orders & Real-Time Tracking</h2>
            <div id="orders-list-container" class="space-y-4"></div>
        </div>

        <!-- SECTION: WISHLIST -->
        <div id="section-wishlist" class="space-y-6 hidden">
            <h2 class="text-2xl font-extrabold text-emeraldDark">My Saved Wishlist</h2>
            <div id="wishlist-grid" class="grid grid-cols-2 sm:grid-cols-4 gap-4"></div>
        </div>
    </main>

    <!-- SHOPPING CART DRAWER -->
    <div id="cart-drawer" class="fixed inset-0 z-50 bg-black/50 backdrop-blur-sm hidden justify-end">
        <div class="w-full max-w-md bg-white h-full flex flex-col shadow-2xl">
            <div class="p-4 bg-emeraldDark text-white flex justify-between items-center">
                <h3 class="font-bold text-lg"><i class="fa-solid fa-cart-shopping mr-2"></i> Your Shopping Cart</h3>
                <button onclick="closeCartDrawer()" type="button" class="text-white hover:text-goldLight text-xl"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div id="cart-items-container" class="flex-1 overflow-y-auto p-4 space-y-3 custom-scrollbar"></div>
            <div class="p-4 border-t bg-gray-50 space-y-3">
                <div class="flex justify-between text-sm font-extrabold text-emeraldDark"><span>Final Total</span><span id="cart-final-total">₹0</span></div>
                <button onclick="proceedToCheckout()" type="button" class="w-full bg-goldBrand text-white font-bold py-3 rounded-xl shadow-lg hover:bg-amber-600 transition text-sm cursor-pointer">Proceed to Secure Checkout</button>
            </div>
        </div>
    </div>

    <!-- ACCOUNT MODAL -->
    <div id="account-modal" class="fixed inset-0 z-50 bg-black/60 backdrop-blur-sm hidden items-center justify-center p-4">
        <div class="bg-white rounded-3xl max-w-md w-full p-6 space-y-4 shadow-2xl relative">
            <button onclick="closeAccountModal()" type="button" class="absolute top-3 right-3 text-gray-400"><i class="fa-solid fa-xmark text-lg"></i></button>
            <div class="text-center space-y-1">
                <h3 class="text-xl font-extrabold text-emeraldDark">Supabase Cloud Account</h3>
                <p class="text-xs text-gray-500">Sign in for secure cloud data persistence</p>
            </div>
            <div id="account-auth-form" class="space-y-3 pt-2">
                <input type="email" id="auth-email" placeholder="name@example.com" class="w-full p-2.5 bg-gray-50 border rounded-xl text-sm">
                <input type="password" id="auth-password" placeholder="••••••••" class="w-full p-2.5 bg-gray-50 border rounded-xl text-sm">
                <div class="flex gap-2 pt-2">
                    <button onclick="supabaseSignIn()" type="button" class="flex-1 bg-emeraldDark text-white font-bold py-2.5 rounded-xl text-xs">Sign In</button>
                    <button onclick="supabaseSignUp()" type="button" class="flex-1 bg-goldBrand text-white font-bold py-2.5 rounded-xl text-xs">Register</button>
                </div>
            </div>
            <div id="account-logged-in-view" class="hidden space-y-4 text-center">
                <p class="text-xs font-bold text-emeraldDark" id="user-display-email"></p>
                <button onclick="supabaseSignOut()" type="button" class="w-full bg-red-50 text-red-600 font-bold py-2 rounded-xl text-xs">Sign Out</button>
            </div>
        </div>
    </div>

    <!-- MOBILE STICKY NAVIGATION -->
    <nav class="fixed bottom-0 left-0 right-0 bg-white border-t py-2 px-4 flex justify-around items-center z-30 sm:hidden shadow-lg">
        <button onclick="switchSection('home')" type="button" class="flex flex-col items-center text-emeraldDark text-[10px] font-bold"><i class="fa-solid fa-house text-lg"><span>Home</span></i></button>
        <button onclick="switchSection('vendor')" type="button" class="flex flex-col items-center text-gray-500 hover:text-emeraldDark text-[10px] font-bold"><i class="fa-solid fa-store text-lg"><span>Sell</span></i></button>
        <button onclick="switchSection('referral')" type="button" class="flex flex-col items-center text-goldBrand text-[10px] font-bold"><i class="fa-solid fa-gift text-lg"><span>₹30 Earn</span></i></button>
        <button onclick="switchSection('orders')" type="button" class="flex flex-col items-center text-gray-500 hover:text-emeraldDark text-[10px] font-bold"><i class="fa-solid fa-box-archive text-lg"><span>Orders</span></i></button>
        <button onclick="openCartDrawer()" type="button" class="flex flex-col items-center text-gray-500 hover:text-emeraldDark text-[10px] font-bold relative"><i class="fa-solid fa-cart-shopping text-lg"><span>Cart</span></i></button>
    </nav>

    <!-- JavaScript App Engine -->
    <script>
        const SUPABASE_URL = 'https://nacbyqzxtykznmqafyeo.supabase.co';
        const SUPABASE_ANON_KEY = 'Sb_publishable_Lq1yazrKPuX-xN9bYBFOkA_uBb28Mib';
        const supabaseClient = supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

        let PRODUCTS_DB = [
            { id: 'prod-chicken-1', name: 'Fresh Chicken Curry Cut', cat: 'Fresh Chicken', price: 220, mrp: 260, weight: '500g', img: 'https://images.unsplash.com/photo-1604503468506-a8da13d82791?auto=format&fit=crop&w=500&q=80' },
            { id: 'prod-mutton-1', name: 'Fresh Mutton Curry Cut', cat: 'Fresh Mutton', price: 650, mrp: 720, weight: '500g', img: 'https://images.unsplash.com/photo-1603048588665-791ca8aea617?auto=format&fit=crop&w=500&q=80' },
            { id: 'prod-food-1', name: 'Hyderabadi Chicken Dum Biryani', cat: 'Foods & Restaurants', price: 220, mrp: 280, weight: '1 Plate', img: 'https://images.unsplash.com/photo-1563379091339-03b21ab4a4f8?auto=format&fit=crop&w=500&q=80' },
            { id: 'prod-juice-1', name: 'Fresh Pomegranate Juice', cat: 'Fresh Juices', price: 110, mrp: 140, weight: '300ml', img: 'https://images.unsplash.com/photo-1613478223719-2ab802602423?auto=format&fit=crop&w=500&q=80' }
        ];

        const CATEGORIES_LIST = [
            { name: 'Fresh Chicken', icon: 'fa-drumstick-bite' },
            { name: 'Fresh Mutton', icon: 'fa-utensils' },
            { name: 'Fresh Fish & Seafood', icon: 'fa-fish' },
            { name: 'Foods & Restaurants', icon: 'fa-burger' },
            { name: 'Fresh Juices', icon: 'fa-glass-water' },
            { name: 'Bakery Items', icon: 'fa-cake-candles' }
        ];

        let cart = [];
        let wishlist = [];
        let currentUser = null;
        let activeOrders = [];

        window.onload = async function() {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                if(splash) { splash.style.opacity = '0'; setTimeout(() => splash.remove(), 700); }
            }, 2000);

            renderCategories();
            renderProducts(PRODUCTS_DB);
            renderSideMenuCategories();
            updateCartUI();
            checkSupabaseSession();
            loadLiveOrders();
        };

        function renderCategories() {
            document.getElementById('home-categories-grid').innerHTML = CATEGORIES_LIST.map(cat => `
                <div onclick="filterCategory('${cat.name}')" class="bg-white p-4 rounded-2xl border border-gray-100 shadow-sm hover:border-goldBrand text-center cursor-pointer transition flex flex-col items-center gap-2">
                    <div class="w-12 h-12 rounded-xl bg-amber-50 text-goldBrand flex items-center justify-center text-xl"><i class="fa-solid ${cat.icon}"></i></div>
                    <span class="text-xs font-bold text-gray-700">${cat.name}</span>
                </div>
            `).join('');
        }

        function renderSideMenuCategories() {
            document.getElementById('side-menu-categories').innerHTML = CATEGORIES_LIST.map(cat => `
                <a href="#" onclick="filterCategory('${cat.name}'); closeSideMenu();" class="flex items-center gap-3 p-2.5 rounded-xl hover:bg-gray-100 text-sm font-medium text-gray-700"><i class="fa-solid ${cat.icon} text-goldBrand w-5"></i> ${cat.name}</a>
            `).join('');
        }

        function renderProducts(items) {
            document.getElementById('products-grid').innerHTML = items.map(p => `
                <div class="bg-white rounded-2xl border border-gray-100 shadow-sm overflow-hidden flex flex-col justify-between">
                    <img src="${p.img}" class="w-full h-36 object-cover">
                    <div class="p-3 space-y-2">
                        <h4 class="font-bold text-xs text-gray-800 line-clamp-1">${p.name}</h4>
                        <div class="flex items-center justify-between">
                            <span class="font-extrabold text-sm text-emeraldDark">₹${p.price}</span>
                            <button onclick="addToCart('${p.id}')" type="button" class="bg-emeraldDark text-white px-3 py-1.5 rounded-xl text-xs font-bold">Add</button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function filterCategory(catName) {
            const filtered = PRODUCTS_DB.filter(p => p.cat === catName);
            renderProducts(filtered);
            switchSection('home');
        }

        function addToCart(productId) {
            const product = PRODUCTS_DB.find(p => p.id === productId);
            if(!product) return;
            const existing = cart.find(item => item.id === productId);
            if(existing) existing.qty += 1;
            else cart.push({ ...product, qty: 1 });
            updateCartUI();
        }

        function updateCartUI() {
            const totalCount = cart.reduce((sum, item) => sum + item.qty, 0);
            const subtotal = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
            document.getElementById('cart-count-badge').innerText = totalCount;
            document.getElementById('cart-total-price').innerText = `₹${subtotal}`;
            document.getElementById('cart-final-total').innerText = `₹${subtotal > 0 ? subtotal + 30 : 0}`;

            document.getElementById('cart-items-container').innerHTML = cart.length === 0 ? 
                `<div class="text-center py-12 text-gray-400 text-sm">Cart is empty.</div>` :
                cart.map(item => `
                    <div class="flex items-center justify-between bg-gray-50 p-3 rounded-2xl">
                        <span class="text-xs font-bold">${item.name} (x${item.qty})</span>
                        <span class="text-xs font-extrabold text-emeraldDark">₹${item.price * item.qty}</span>
                    </div>
                `).join('');
        }

        async function proceedToCheckout() {
            if(cart.length === 0) { alert('Cart is empty!'); return; }
            const orderRef = 'RUG-' + Math.floor(100000 + Math.random() * 900000);
            const newOrder = {
                id: orderRef,
                items: [...cart],
                date: new Date().toLocaleDateString(),
                status: 'Order Confirmed & Butchery Prep',
                total: cart.reduce((sum, i) => sum + (i.price * i.qty), 0) + 30
            };
            activeOrders.unshift(newOrder);
            cart = [];
            updateCartUI();
            closeCartDrawer();
            switchSection('orders');
            renderOrders();

            if(currentUser) {
                await supabaseClient.from('orders').insert([{
                    user_id: currentUser.id,
                    order_ref: orderRef,
                    items: newOrder.items,
                    total_amount: newOrder.total,
                    status: newOrder.status
                }]);
            }
        }

        function renderOrders() {
            document.getElementById('orders-list-container').innerHTML = activeOrders.map(o => `
                <div class="bg-white p-6 rounded-2xl border shadow-sm space-y-4">
                    <div class="flex justify-between items-center border-b pb-3">
                        <span class="text-xs font-bold text-goldBrand">Order #${o.id}</span>
                        <span class="bg-emerald-100 text-emerald-800 text-xs font-bold px-3 py-1 rounded-full">${o.status}</span>
                    </div>
                    <div class="flex justify-between items-center">
                        <span class="text-sm font-extrabold text-emeraldDark">Total: ₹${o.total}</span>
                        <a href="https://api.whatsapp.com/send?phone=919999999999&text=Hello%20Rugendra%20Mart,%20status%20for%20order%20%23${o.id}" target="_blank" class="bg-emerald-600 text-white px-4 py-2 rounded-xl text-xs font-bold inline-flex items-center gap-1.5"><i class="fa-brands fa-whatsapp"></i> Track via WhatsApp</a>
                    </div>
                </div>
            `).join('');
        }

        async function loadLiveOrders() {
            activeOrders = [{
                id: 'RUG-592014',
                items: [{ name: 'Fresh Chicken Curry Cut', price: 220, qty: 1 }],
                date: new Date().toLocaleDateString(),
                status: 'Out for Delivery',
                total: 250
            }];
            renderOrders();
        }

        // Vendor Portal Calculations
        function calculateCommission() {
            const price = parseFloat(document.getElementById('vendor-prod-price').value) || 0;
            const commission = price * 0.17;
            const payout = price - commission;
            document.getElementById('vendor-net-payout').value = `₹${payout.toFixed(2)} (Commission: ₹${commission.toFixed(2)})`;
        }

        function publishVendorProduct() {
            const name = document.getElementById('vendor-prod-name').value;
            const price = parseFloat(document.getElementById('vendor-prod-price').value);
            const cat = document.getElementById('vendor-prod-cat').value;
            if(!name || !price) { alert('Please enter product details'); return; }

            const newProd = {
                id: 'prod-vendor-' + Date.now(),
                name: name,
                cat: cat,
                price: price,
                mrp: Math.round(price * 1.2),
                weight: '1 Unit',
                img: 'https://images.unsplash.com/photo-1546069901-ba9599a7e63c?auto=format&fit=crop&w=500&q=80'
            };
            PRODUCTS_DB.unshift(newProd);
            renderProducts(PRODUCTS_DB);
            alert('Product published successfully to Rugendra Mart!');
            switchSection('home');
        }

        async function checkSupabaseSession() {
            const { data: { session } } = await supabaseClient.auth.getSession();
            if(session) {
                currentUser = session.user;
                document.getElementById('top-user-greet').innerText = currentUser.email.split('@')[0];
                document.getElementById('header-account-name').innerText = currentUser.email.split('@')[0];
                document.getElementById('account-auth-form').classList.add('hidden');
                document.getElementById('account-logged-in-view').classList.remove('hidden');
                document.getElementById('user-display-email').innerText = currentUser.email;
            }
        }

        async function supabaseSignIn() {
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-password').value;
            const { error } = await supabaseClient.auth.signInWithPassword({ email, password: pass });
            if(error) { alert(error.message); return; }
            alert('Signed in!'); location.reload();
        }

        async function supabaseSignUp() {
            const email = document.getElementById('auth-email').value;
            const pass = document.getElementById('auth-password').value;
            const { error } = await supabaseClient.auth.signUp({ email, password: pass });
            if(error) { alert(error.message); return; }
            alert('Registered successfully!');
        }

        async function supabaseSignOut() {
            await supabaseClient.auth.signOut();
            location.reload();
        }

        function switchSection(id) {
            ['home', 'vendor', 'referral', 'orders', 'wishlist'].forEach(s => document.getElementById(`section-${s}`).classList.add('hidden'));
            document.getElementById(`section-${id}`).classList.remove('hidden');
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function openSideMenu() { document.getElementById('side-menu-drawer').classList.remove('hidden'); }
        function closeSideMenu() { document.getElementById('side-menu-drawer').classList.add('hidden'); }
        function openCartDrawer() { document.getElementById('cart-drawer').classList.remove('hidden'); document.getElementById('cart-drawer').classList.add('flex'); }
        function closeCartDrawer() { document.getElementById('cart-drawer').classList.add('hidden'); document.getElementById('cart-drawer').classList.remove('flex'); }
        function openAccountModal() { document.getElementById('account-modal').classList.remove('hidden'); document.getElementById('account-modal').classList.add('flex'); }
        function closeAccountModal() { document.getElementById('account-modal').classList.add('hidden'); document.getElementById('account-modal').classList.remove('flex'); }
        function shareOnWhatsApp() { window.open(`https://api.whatsapp.com/send?text=Check%20out%20Rugendra%20Mart%20for%20fresh%20meats%20and%20food!`, '_blank'); }
    </script>
</body>
</html>
