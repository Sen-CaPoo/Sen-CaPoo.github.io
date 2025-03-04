<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GitHub 專業介紹</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">
  <style>
    /* 重設與基本樣式 */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      font-family: 'Roboto', sans-serif;
      background: linear-gradient(135deg, #0d47a1, #1976d2);
      color: #ffffff;
      line-height: 1.6;
      overflow-x: hidden;
    }
    header {
      padding: 50px 20px;
      text-align: center;
    }
    header h1 {
      font-size: 3rem;
      margin-bottom: 10px;
    }
    header p {
      font-size: 1.2rem;
    }
    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
    }
    .section {
      padding: 50px 20px;
    }
    .section-title {
      text-align: center;
      margin-bottom: 40px;
      font-size: 2.5rem;
    }
    .cards {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-around;
      gap: 20px;
    }
    .card {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 10px;
      padding: 20px;
      width: 300px;
      transition: transform 0.3s ease, background 0.3s ease;
      backdrop-filter: blur(5px);
    }
    .card:hover {
      transform: translateY(-10px);
      background: rgba(255, 255, 255, 0.2);
    }
    .card h3 {
      margin-bottom: 15px;
      font-size: 1.8rem;
    }
    .card p {
      font-size: 1rem;
      line-height: 1.5;
    }
    footer {
      text-align: center;
      padding: 20px;
      background: rgba(0, 0, 0, 0.2);
      margin-top: 50px;
    }
    @media (max-width: 768px) {
      .cards {
        flex-direction: column;
        align-items: center;
      }
      .card {
        width: 80%;
      }
    }
  </style>
</head>
<body>
  <header>
    <h1>GitHub</h1>
    <p>專業、創新的開發者協作平台</p>
  </header>
  <div class="container">
    <section class="section">
      <h2 class="section-title">為何選擇 GitHub？</h2>
      <div class="cards">
        <div class="card">
          <h3>版本控制</h3>
          <p>以 Git 為核心，提供強大的版本管理與協作功能，讓每次提交都有跡可循。</p>
        </div>
        <div class="card">
          <h3>協作無縫</h3>
          <p>全球開發者齊聚一堂，共同打造高品質軟體，促進知識共享與交流。</p>
        </div>
        <div class="card">
          <h3>社群支持</h3>
          <p>豐富的社群資源與技術討論，讓您在解決問題的路上不再孤單。</p>
        </div>
      </div>
    </section>
    <section class="section">
      <h2 class="section-title">無限創新</h2>
      <p style="text-align:center; font-size:1.2rem; max-width:800px; margin:0 auto;">
        GitHub 不僅是代碼的儲存庫，更是開發者思想的交流平臺。從初學者到專業人士，皆能在這裡找到最適合自己的開發方式與創新靈感。
      </p>
    </section>
  </div>
  <footer>
    <p>&copy; 2025 GitHub 專業介紹. All rights reserved.</p>
  </footer>
</body>
</html>
