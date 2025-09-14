<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>ShopMate</title>
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
    }

    header {
      background: #333;
      color: white;
      padding: 1em;
      text-align: center;
    }

    nav a {
      color: white;
      margin: 0 10px;
      text-decoration: none;
      font-weight: bold;
    }

    main {
      padding: 20px;
    }

    #products, #cart-items {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
    }

    .product, .cart-item {
      border: 1px solid #ccc;
      padding: 10px;
      width: 250px;
      text-align: center;
    }

    .product img, .cart-item img {
      max-width: 100%;
      height: auto;
    }

    button {
      background-color: #28a745;
      color: white;
      border: none;
      padding: 10px;
      cursor: pointer;
      margin-top: 10px;
    }

    button:hover {
      background-color: #218838;
    }

    form input, form button {
      display: block;
      margin: 10px 0;
      padding: 10px;
      width: 100%;
    }

    @media (max-width: 600px) {
      #products, #cart-items {
        flex-direction: column;
        align-items: center;
      }
    }
  </style>
</head>
<body>

  <header>
    <nav>
      <a href="#" onclick="showSection('home')">Home</a>
      <a href="#" onclick="showSection('cart')">Cart</a>
      <a href="#" onclick="showSection('checkout')">Checkout</a>
    </nav>
  </header>

  <main>
    <!-- Home Section -->
    <section id="home">
      <h1>Welcome to ShopMate</h1>
      <div id="products"></div>
    </section>

    <!-- Cart Section -->
    <section id="cart" style="display: none;">
      <h1>Your Cart</h1>
      <div id="cart-items"></div>
      <h2 id="total-price">Total: $0.00</h2>
    </section>

    <!-- Checkout Section -->
    <section id="checkout" style="display: none;">
      <h1>Checkout</h1>
      <form id="checkout-form">
        <input type="text" placeholder="Full Name" required />
        <input type="email" placeholder="Email Address" required />
        <input type="text" placeholder="Address" required />
        <button type="submit">Place Order</button>
      </form>
      <p id="confirmation" style="color: green;"></p>
    </section>
  </main>

  <script>
    const products = [
      { id: 1, name: "Shoes", price: 49.99, image: "https://via.placeholder.com/250x150?text=Shoes" },
      { id: 2, name: "Watch", price: 89.99, image: "https://via.placeholder.com/250x150?text=Watch" },
      { id: 3, name: "Bag", price: 59.99, image: "https://via.placeholder.com/250x150?text=Bag" }
    ];

    function showSection(section) {
      document.getElementById("home").style.display = "none";
      document.getElementById("cart").style.display = "none";
      document.getElementById("checkout").style.display = "none";
      document.getElementById(section).style.display = "block";

      if (section === "cart") loadCart();
    }

    function loadProducts() {
      const container = document.getElementById("products");
      products.forEach(product => {
        const div = document.createElement("div");
        div.className = "product";
        div.innerHTML = `
          <img src="${product.image}" alt="${product.name}">
          <h2>${product.name}</h2>
          <p>$${product.price.toFixed(2)}</p>
          <button onclick="addToCart(${product.id})">Add to Cart</button>
        `;
        container.appendChild(div);
      });
    }

    function addToCart(productId) {
      let cart = JSON.parse(localStorage.getItem("cart")) || [];
      cart.push(productId);
      localStorage.setItem("cart", JSON.stringify(cart));
      alert("Added to cart!");
    }

    function loadCart() {
      const cartItems = JSON.parse(localStorage.getItem("cart")) || [];
      const cartSection = document.getElementById("cart-items");
      cartSection.innerHTML = "";

      let total = 0;

      cartItems.forEach((productId, index) => {
        const product = products.find(p => p.id === productId);
        total += product.price;

        const item = document.createElement("div");
        item.className = "cart-item";
        item.innerHTML = `
          <img src="${product.image}" alt="${product.name}">
          <h2>${product.name}</h2>
          <p>Price: $${product.price.toFixed(2)}</p>
          <button onclick="removeFromCart(${index})">Remove</button>
        `;
        cartSection.appendChild(item);
      });

      document.getElementById("total-price").innerText = `Total: $${total.toFixed(2)}`;
    }

    function removeFromCart(index) {
      let cart = JSON.parse(localStorage.getItem("cart")) || [];
      cart.splice(index, 1);
      localStorage.setItem("cart", JSON.stringify(cart));
      loadCart();
    }

    document.getElementById("checkout-form").addEventListener("submit", function (e) {
      e.preventDefault();
      localStorage.removeItem("cart");
      document.getElementById("confirmation").innerText = "✅ Order placed successfully!";
      this.reset();
      showSection("home");
    });

    document.addEventListener("DOMContentLoaded", () => {
      loadProducts();
    });
  </script>

</body>
</html>
