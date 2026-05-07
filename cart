<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Product Listing</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            padding: 20px;
        }
        .product-list, .cart {
            border: 1px solid #ccc;
            padding: 20px;
            margin: 10px;
            width: 45%;
        }
        .product-card {
            border: 1px solid #eee;
            margin-bottom: 15px;
            padding: 10px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        .product-info {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .product-image {
            font-size: 2em;
        }
        button[disabled] {
            background-color: grey;
            cursor: not-allowed;
        }
        .cart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .cart-badge {
            background-color: red;
            border-radius: 50%;
            color: white;
            padding: 2px 8px;
            font-size: 0.8em;
        }
        .cart-item {
            border-bottom: 1px solid #ddd;
            padding: 10px 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .cart-item-info {
            flex: 1;
        }
        .quantity-controls button {
            margin: 0 2px;
        }
        .message {
            margin-top: 15px;
            font-style: italic;
            color: #555;
        }
        .checkout-btn {
            margin-top: 15px;
            padding: 10px;
            cursor: pointer;
            background-color: teal;
            color: white;
            border: none;
            width: 100%;
        }
    </style>
</head>
<body>

    <div class="product-list">
        <h2>Products</h2>
        <div id="products"></div>
    </div>

    <div class="cart">
        <div class="cart-header">
            <h2>Shopping Cart</h2>
            <div>
                Items: <span id="cart-count" class="cart-badge">0</span>
            </div>
        </div>

        <div id="cart-items"></div>
        <div id="cart-message" class="message">Your cart is empty.</div>
        <div id="cart-total"></div>
        <button id="clear-cart" class="checkout-btn" style="background-color: red;">Clear Cart</button>
        <button id="checkout" class="checkout-btn" style="margin-top:10px;">Checkout</button>
    </div>

<script>
    // Sample product data (3-6 products)
    const products = [
        {id: 1, name: "Laptop 💻", price: 1200},
        {id: 2, name: "Smartphone 📱", price: 800},
        {id: 3, name: "Headphones 🎧", price: 150},
        {id: 4, name: "Watch ⌚", price: 200},
        {id: 5, name: "Camera 📷", price: 900},
    ];

    let cart = [];

    const productsContainer = document.getElementById('products');
    const cartItemsContainer = document.getElementById('cart-items');
    const cartCount = document.getElementById('cart-count');
    const cartMessage = document.getElementById('cart-message');
    const cartTotalDisplay = document.getElementById('cart-total');
    const clearCartBtn = document.getElementById('clear-cart');
    const checkoutBtn = document.getElementById('checkout');

    // Render product cards
    function renderProducts() {
        productsContainer.innerHTML = '';
        products.forEach(product => {
            const isInCart = cart.find(item => item.id === product.id);
            const buttonDisabled = isInCart ? 'disabled' : '';

            const productCard = document.createElement('div');
            productCard.className = 'product-card';

            productCard.innerHTML = `
                <div class="product-info">
                    <div class="product-image">${product.name.match(/[\p{Emoji}]/gu) || '📦'}</div>
                    <div>
                        <div><strong>${product.name}</strong></div>
                        <div>$${product.price}</div>
                    </div>
                </div>
                <button ${buttonDisabled} data-id="${product.id}">
                    ${isInCart ? 'Already in cart' : 'Add to Cart'}
                </button>
            `;

            productsContainer.appendChild(productCard);
        });

        // Add event listeners
        document.querySelectorAll('.product-card button').forEach(btn => {
            btn.addEventListener('click', () => {
                const id = parseInt(btn.getAttribute('data-id'));
                addToCart(id);
            });
        });
    }

    // Add product to cart
    function addToCart(id) {
        const product = products.find(p => p.id === id);
        if (!cart.find(item => item.id === id)) {
            cart.push({...product, quantity: 1});
            updateCart();
            renderProducts();
        }
    }

    // Remove item from cart
    function removeFromCart(id) {
        cart = cart.filter(item => item.id !== id);
        updateCart();
        renderProducts();
    }

    // Change quantity
    function changeQuantity(id, delta) {
        const item = cart.find(i => i.id === id);
        if (item) {
            item.quantity += delta;
            if (item.quantity < 1) item.quantity = 1;
            updateCart();
        }
    }

    // Clear cart
    clearCartBtn.addEventListener('click', () => {
        cart = [];
        updateCart();
        renderProducts();
    });

    // Checkout alert
    checkoutBtn.addEventListener('click', () => {
        if(cart.length === 0) {
            alert('Cart is empty.');
            return;
        }
        alert('Thank you for your purchase! (This is a demo message)');
    });

    // Update cart display
    function updateCart() {
        cartItemsContainer.innerHTML = '';

        if(cart.length === 0) {
            cartMessage.style.display = 'block';
            cartTotalDisplay.innerHTML = '';
            cartCount.innerText = '0';
            return;
        } else {
            cartMessage.style.display = 'none';
        }

        let totalItems = 0;
        let totalPrice = 0;

        cart.forEach(item => {
            totalItems += item.quantity;
            const subtotal = item.price * item.quantity;
            totalPrice += subtotal;

            const cartItem = document.createElement('div');
            cartItem.className = 'cart-item';

            cartItem.innerHTML = `
                <div class="cart-item-info">
                    <strong>${item.name}</strong> - $${item.price} x ${item.quantity} = $${subtotal}
                </div>
                <div class="quantity-controls">
                    <button data-id="${item.id}" data-action="decrease">-</button>
                    <button data-id="${item.id}" data-action="increase">+</button>
                    <button data-id="${item.id}" data-action="remove">Remove</button>
                </div>
            `;

            cartItemsContainer.appendChild(cartItem);
        });

        cartCount.innerText = totalItems;
        cartTotalDisplay.innerHTML = <strong>Total: $${totalPrice}</strong>;

        // Add listeners for the cart buttons
        document.querySelectorAll('.quantity-controls button').forEach(button => {
            button.addEventListener('click', () => {
                const id = parseInt(button.getAttribute('data-id'));
                const action = button.getAttribute('data-action');
                if(action === 'increase') changeQuantity(id, 1);
                if(action === 'decrease') changeQuantity(id, -1);
                if(action === 'remove') removeFromCart(id);
            });
        });
    }

    // Initialize
    renderProducts();
    updateCart();
</script>

</body>
</html>
