<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>The Cozy Bar 🍹</title>
  <style>
    body { font-family: Arial; background-color:#1c1c1c; color:#fff; margin:0; }
    header { background:#333; padding:10px; text-align:center; }
    nav a { color:#ffd700; margin:0 10px; cursor:pointer; text-decoration:none; font-weight:bold; }
    section { padding:20px; display:none; }
    section.active { display:block; }
    button { background:#ffd700; color:#000; padding:10px 20px; border:none; cursor:pointer; }
    button:hover { background:#ffcc00; }
    input { display:block; margin:10px 0; padding:8px; width:100%; max-width:300px; }
    .menu-item { margin-bottom:15px; }
  </style>
</head>
<body>

<header>
  <h1>The Cozy Bar 🍹</h1>
  <nav>
    <a onclick="showSection('home')">Home</a>
    <a onclick="showSection('menu')">Menu</a>
    <a onclick="showSection('contact')">Contact</a>
    <a onclick="showSection('about')">About</a>
  </nav>
</header>

<!-- Home Section -->
<section id="home" class="active">
  <h2>Welcome!</h2>
  <p>Experience the best cocktails and cozy vibes in town!</p>
  <img src="https://via.placeholder.com/800x400?text=Bar+Image" alt="Bar Image" style="width:100%; max-height:400px; object-fit:cover;">
</section>

<!-- Menu Section -->
<section id="menu">
  <h2>Our Cocktail Menu 🍸</h2>
  <div id="drinkList"></div>
  <button id="randomDrinkBtn">Pick a Random Drink 🍹</button>
  <p id="drinkResult"></p>
</section>

<!-- Contact Section -->
<section id="contact">
  <h2>Contact / Table Reservation 📩</h2>
  <form id="reservationForm">
    <label>Name:</label>
    <input type="text" id="name" required>
    <label>Email:</label>
    <input type="email" id="email" required>
    <label>Date:</label>
    <input type="date" id="date" required>
    <label>Time:</label>
    <input type="time" id="time" required>
    <button type="submit">Reserve Table</button>
  </form>
  <p id="formMsg"></p>
</section>

<!-- About Section -->
<section id="about">
  <h2>About The Cozy Bar</h2>
  <p>We are a modern bar offering the finest cocktails, live music, and a cozy atmosphere for everyone.</p>
</section>

<script>
  // Navigation function
  function showSection(id){
    document.querySelectorAll('section').forEach(sec=>sec.classList.remove('active'));
    document.getElementById(id).classList.add('active');
  }

  // Drinks data
  const drinks = [
    {name:"Mojito", desc:"Mint, lime, sugar & rum", price:"$8"},
    {name:"Martini", desc:"Gin and vermouth", price:"$10"},
    {name:"Old Fashioned", desc:"Bourbon, sugar, bitters", price:"$9"},
    {name:"Margarita", desc:"Tequila, lime, triple sec", price:"$8"}
  ];

  // Display drinks
  const drinkListDiv = document.getElementById("drinkList");
  drinks.forEach(drink=>{
    const div = document.createElement("div");
    div.className = "menu-item";
    div.innerHTML = `<h3>${drink.name} - ${drink.price}</h3><p>${drink.desc}</p>`;
    drinkListDiv.appendChild(div);
  });

  // Random Drink Picker
  const randomBtn = document.getElementById("randomDrinkBtn");
  const drinkResult = document.getElementById("drinkResult");
  randomBtn.addEventListener("click", ()=>{
    const randomDrink = drinks[Math.floor(Math.random()*drinks.length)];
    drinkResult.textContent = `Try: ${randomDrink.name} 🍹`;
  });

  // Reservation form
  const form = document.getElementById("reservationForm");
  const formMsg = document.getElementById("formMsg");
  form.addEventListener("submit",(e)=>{
    e.preventDefault();
    formMsg.textContent = `Thank you ${document.getElementById("name").value}, your table is reserved!`;
    form.reset();
  });
</script>

</body>
</html>
