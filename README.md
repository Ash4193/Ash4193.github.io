<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prasamsa - Hindu Ethnic & Flashy Outfits</title>
    <style>
        body { 
            font-family: Arial, sans-serif; 
            margin: 0; 
            padding: 0; 
            background-color: #F5F5DC; /* Cream for luxury */
        }
        header { 
            background: linear-gradient(135deg, #D4AF37, #FF9933); /* Gold to saffron gradient for luxury + religious */
            color: #FFF; 
            padding: 1rem; 
            text-align: center; 
            box-shadow: 0 4px 8px rgba(0,0,0,0.2); /* Cool shadow for depth */
        }
        .logo { 
            font-family: 'Book Antiqua', serif; /* Luxurious serif for Prasamsa */
            font-size: 2.5rem; 
            font-weight: bold; 
            margin-bottom: 0.5rem; 
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5); /* Cool metallic effect */
        }
        .slogan { 
            font-family: 'Monotype Corsiva', cursive; /* Flowing cursive for slogan */
            font-size: 1.4rem; 
            font-style: italic; 
        }
        .container { 
            max-width: 1200px; 
            margin: 0 auto; 
            padding: 2rem; 
        }
        .filters { 
            margin-bottom: 1rem; 
        }
        .filters select { 
            padding: 0.5rem; 
            border: 1px solid #D4AF37; /* Gold border for luxury */
            background-color: #FFF; 
            color: #800000; /* Maroon text for religious tone */
        }
        .products { 
            display: grid; 
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); 
            gap: 1rem; 
        }
        .product { 
            background: #FFF; 
            padding: 1rem; 
            border-radius: 8px; 
            box-shadow: 0 0 10px rgba(0,0,0,0.1); 
            text-align: center; 
            border: 1px solid #FF9933; /* Saffron border for religious flair */
        }
        .product img { 
            max-width: 100%; 
            height: 200px; 
            object-fit: cover; 
            border-radius: 4px; 
        }
        button { 
            background-color: #800000; /* Maroon for cool, religious depth */
            color: #FFF; 
            border: none; 
            padding: 0.5rem 1rem; 
            cursor: pointer; 
            border-radius: 4px; 
            transition: background-color 0.3s; /* Cool hover effect */
        }
        button:hover { 
            background-color: #D4AF37; /* Gold hover for luxury */
        }
        .cart { 
            position: fixed; 
            top: 10px; 
            right: 10px; 
            background: #FFF; 
            padding: 1rem; 
            border-radius: 8px; 
            box-shadow: 0 0 10px rgba(0,0,0,0.1); 
            max-width: 300px; 
            border: 2px solid #FF9933; /* Saffron accent */
        }
        .admin { 
            margin-top: 2rem; 
            background: #F5F5DC; /* Cream background for luxury */
            padding: 1rem; 
            border-radius: 8px; 
            border: 1px solid #D4AF37; /* Gold border */
        }
        input, select, textarea { 
            margin: 0.5rem; 
            padding: 0.5rem; 
            width: calc(100% - 1rem); 
            border: 1px solid #800000; /* Maroon border */
            background-color: #FFF; 
        }
    </style>
</head>
<body>
    <header>
        <div class="logo">Prasamsa</div>
        <div class="slogan">Dedicated To Who You Are</div>
        <p>Hindu Ethnic & Flashy Outfits - Global Fusion Fashion</p>
    </header>
    <div class="container">
        <div class="filters">
            <label for="category">Filter by Category:</label>
            <select id="category" onchange="filterProducts()">
                <option value="all">All</option>
                <option value="sarees">Sarees & Lehengas</option>
                <option value="kurtas">Kurtas & Sherwanis</option>
                <option value="accessories">Accessories</option>
            </select>
        </div>
        <h2>Featured Products</h2>
        <div class="products" id="products">
            <!-- Products will be dynamically added here -->
        </div>
        
        <div class="cart" id="cart">
            <h3>Cart</h3>
            <ul id="cart-items"></ul>
            <p>Total: ₹<span id="total">0</span> ($<span id="total-usd">0</span>)</p>
            <button onclick="checkout()">Checkout</button>
        </div>
        
        <div class="admin">
            <h3>Admin Panel (Your Full Control)</h3>
            <input type="text" id="product-name" placeholder="Product Name (e.g., Neon Om Kurta)">
            <input type="number" id="product-price" placeholder="Price (INR)">
            <input type="text" id="product-image" placeholder="Image URL">
            <select id="product-category">
                <option value="sarees">Sarees & Lehengas</option>
                <option value="kurtas">Kurtas & Sherwanis</option>
                <option value="accessories">Accessories</option>
            </select>
            <textarea id="product-description" placeholder="Description (e.g., Inspired by Lord Shiva, with flashy embroidery)"></textarea>
            <button onclick="addProduct()">Add Product</button>
        </div>
    </div>

    <script>
        let products = [
            { name: "Neon-Embroidered Silk Saree", price: 2500, category: "sarees", image: "https://via.placeholder.com/250x200?text=Saree", description: "Traditional silk saree with Om motifs in neon threads—perfect for Diwali celebrations." },
            { name: "Flashy Kurta with Graffiti Om", price: 1500, category: "kurtas", image: "https://via.placeholder.com/250x200?text=Kurta", description: "Modern kurta blending Hindu symbols with urban graffiti for a cool, fusion look." },
            { name: "Embroidered Dupatta with Lotus", price: 800, category: "accessories", image: "https://via.placeholder.com/250x200?text=Dupatta", description: "Light dupatta featuring lotus flowers, symbolizing purity in Hindu culture." }
        ];
        let cart = [];
        let total = 0;

        function renderProducts(filtered = products) {
            const container = document.getElementById('products');
            container.innerHTML = '';
            filtered.forEach((product, index) => {
                container.innerHTML += `
                    <div class="product">
                        <img src="${product.image}" alt="${product.name}">
                        <h3>${product.name}</h3>
                        <p>₹${product.price} (~$${Math.round(product.price / 83)})</p>
                        <p>${product.description}</p>
                        <button onclick="addToCart(${index})">Add to Cart</button>
                    </div>
                `;
            });
        }

        function filterProducts() {
            const category = document.getElementById('category').value;
            const filtered = category === 'all' ? products : products.filter(p => p.category === category);
            renderProducts(filtered);
        }

        function addToCart(index) {
            cart.push(products[index]);
            total += products[index].price;
            updateCart();
        }

        function updateCart() {
            const cartItems = document.getElementById('cart-items');
            cartItems.innerHTML = '';
            cart.forEach(item => {
                cartItems.innerHTML += `<li>${item.name} - ₹${item.price}</li>`;
            });
            document.getElementById('total').textContent = total;
            document.getElementById('total-usd').textContent = Math.round(total / 83);
        }

        function addProduct() {
            const name = document.getElementById('product-name').value;
            const price = parseFloat(document.getElementById('product-price').value);
            const image = document.getElementById('product-image').value;
            const category = document.getElementById('product-category').value;
            const description = document.getElementById('product-description').value;
            if (name && price && image && description) {
                products.push({ name, price, category, image, description });
                renderProducts();
                document.getElementById('product-name').value = '';
                document.getElementById('product-price').value = '';
                document.getElementById('product-image').value = '';
                document.getElementById('product-description').value = '';
            }
        }

        function checkout() {
            alert('Proceed to checkout! In a real site, integrate with Razorpay (for INR) or Stripe (global). Total: ₹' + total);
        }

        renderProducts();
    </script>
</body>
</html>
