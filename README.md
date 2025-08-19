# Aura-farming-
Selling quality branded products on a cheap price
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Online Store</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    header { display: flex; justify-content: space-between; align-items: center; }
    .product { border: 1px solid #ccc; padding: 15px; margin: 15px; display: inline-block; width: 220px; }
    .product img { width: 100%; height: 150px; object-fit: cover; }
    .cart { margin-top: 20px; padding: 15px; border: 2px solid #333; }
    button { background: #28a745; color: white; padding: 7px 12px; border: none; cursor: pointer; border-radius: 5px; }
    button:hover { background: #218838; }
    #loginBox { border: 1px solid #333; padding: 10px; margin-bottom: 20px; }
  </style>
</head>
<body>
  <header>
    <h1>🛒 My Online Store</h1>
    <div id="loginBox">
      <input type="email" id="email" placeholder="Enter your email">
      <button onclick="login()">Login</button>
      <p id="user"></p>
    </div>
  </header>

  <!-- Products Section -->
  <div class="product">
    <img src="product1.jpg" alt="Product 1">
    <h3>Product 1</h3>
    <p>Location: Fiji</p>
    <p>$10</p>
    <button onclick="addToCart('Product 1', 10)">Add to Cart</button>
  </div>

  <div class="product">
    <img src="product2.jpg" alt="Product 2">
    <h3>Product 2</h3>
    <p>Location: USA</p>
    <p>$15</p>
    <button onclick="addToCart('Product 2', 15)">Add to Cart</button>
  </div>

  <div class="product">
    <img src="product3.jpg" alt="Product 3">
    <h3>Product 3</h3>
    <p>Location: Australia</p>
    <p>$20</p>
    <button onclick="addToCart('Product 3', 20)">Add to Cart</button>
  </div>

  <!-- Cart Section -->
  <div class="cart">
    <h2>Your Cart</h2>
    <ul id="cartItems"></ul>
    <p><strong>Total: $<span id="total">0</span></strong></p>
    
    <!-- PayPal Checkout Button -->
    <div id="paypal-button-container"></div>
  </div>

  <!-- PayPal SDK -->
  <script src="https://www.paypal.com/sdk/js?client-id=YOUR_CLIENT_ID&currency=USD"></script>

  <script>
    let cart = [];
    let total = 0;
    let userEmail = "";

    function login() {
      userEmail = document.getElementById("email").value;
      document.getElementById("user").textContent = "Logged in as: " + userEmail;
    }

    function addToCart(product, price) {
      cart.push({ product, price });
      total += price;
      updateCart();
    }

    function updateCart() {
      let cartList = document.getElementById("cartItems");
      cartList.innerHTML = "";
      cart.forEach(item => {
        let li = document.createElement("li");
        li.textContent = item.product + " - $" + item.price;
        cartList.appendChild(li);
      });
      document.getElementById("total").textContent = total;

      // Update PayPal Button
      renderPayPal();
    }

    function renderPayPal() {
      paypal.Buttons({
        createOrder: function(data, actions) {
          return actions.order.create({
            purchase_units: [{
              amount: { value: total.toString() }
            }]
          });
        },
        onApprove: function(data, actions) {
          return actions.order.capture().then(function(details) {
            alert("✅ Transaction completed by " + details.payer.name.given_name);
          });
        }
      }).render('#paypal-button-container');
    }
  </script>
</body>
</html>
