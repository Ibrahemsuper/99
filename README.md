<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quran Learning App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #fdf6e3;
      color: #333;
      margin: 0;
      padding: 0;
    }
    header {
      padding: 1rem;
      background-color: #008080;
      color: white;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
    }
    input, button {
      padding: 0.5rem;
      font-size: 1rem;
      margin: 0.2rem;
    }
    .ayah-card {
      border-bottom: 1px solid #ccc;
      padding: 0.5rem;
    }
    .dark-mode {
      background-color: #121212;
      color: #f5f5f5;
    }
  </style>
</head>
<body>
  <header>
    <h1>Quran Learning App</h1>
    <div>
      <input type="text" id="search-input" placeholder="ابحث عن كلمة أو اسم السورة">
      <button id="search-btn">بحث</button>
      <button id="toggle-theme">تغيير الوضع</button>
    </div>
  </header>

  <main>
    <div id="surah-list"></div>
  </main>

  <script src="script.js"></script>
</body>
</html>
