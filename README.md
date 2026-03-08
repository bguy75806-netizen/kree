# kree
Chat.app
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kree - Chat App</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Arial, sans-serif;
      background: #f5f5f5;
      height: 100vh;
      overflow: hidden;
    }

    .app-container {
      display: flex;
      flex-direction: column;
      height: 100vh;
      max-width: 600px;
      margin: 0 auto;
      background: white;
      position: relative;
    }

    /* ============= SCREEN MANAGEMENT ============= */
    .screen {
      display: none;
      flex: 1;
      overflow-y: auto;
      padding-bottom: 0;
    }

    .screen.active {
      display: flex;
      flex-direction: column;
    }

    /* ============= LOGIN SCREEN ============= */
    .login-screen {
      justify-content: center;
      align-items: center;
      padding: 20px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    }

    .login-container {
      background: white;
      padding: 40px 25px;
      border-radius: 15px;
      width: 100%;
      max-width: 350px;
      box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
    }

    .login-container h1 {
      text-align: center;
      color: #333;
      margin-bottom: 10px;
      font-size: 28px;
    }

    .login-container p {
      text-align: center;
      color: #999;
      margin-bottom: 30px;
      font-size: 14px;
    }

    .form-group {
      margin-bottom: 18px;
    }

    .form-group label {
      display: block;
      margin-bottom: 8px;
      color: #333;
      font-weight: 600;
      font-size: 14px;
    }

    .form-group input {
      width: 100%;
      padding: 12px 14px;
      border: 1.5px solid #ddd;
      border-radius: 8px;
      font-size: 14px;
      transition: border-color 0.3s;
    }

    .form-group input:focus {
      outline: none;
      border-color: #667eea;
      box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
    }

    .btn-login {
      width: 100%;
      padding: 13px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 15px;
      font-weight: 700;
      cursor: pointer;
      margin-top: 10px;
      transition: transform 0.2s;
    }

    .btn-login:active {
      transform: scale(0.98);
    }

    .toggle-form {
      text-align: center;
      margin-top: 18px;
      color: #999;
      font-size: 14px;
    }

    .toggle-form button {
      background: none;
      border: none;
      color: #667eea;
      font-weight: 700;
      cursor: pointer;
      text-decoration: underline;
    }

    /* ============= HEADER WITH PROFILE & SEARCH ============= */
    .header {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      padding: 12px 15px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      z-index: 10;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    }

    .header-left {
      display: flex;
      align-items: center;
      gap: 10px;
      flex: 1;
    }

    .profile-circle {
      width: 40px;
      height: 40px;
      background: rgba(255, 255, 255, 0.3);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 20px;
      cursor: pointer;
      transition: background 0.3s;
      border: 2px solid white;
    }

    .profile-circle:active {
      background: rgba(255, 255, 255, 0.5);
    }

    .app-title {
      font-size: 16px;
      font-weight: 600;
    }

    .search-box {
      flex: 0.8;
      display: flex;
      align-items: center;
      background: rgba(255, 255, 255, 0.2);
      border-radius: 20px;
      padding: 6px 12px;
      gap: 8px;
    }

    .search-box input {
      background: none;
      border: none;
      color: white;
      font-size: 12px;
      width: 100%;
      outline: none;
    }

    .search-box input::placeholder {
      color: rgba(255, 255, 255, 0.7);
    }

    .search-icon {
      font-size: 14px;
      cursor: pointer;
    }

    /* ============= HOME SCREEN ============= */
    .home-screen {
      flex-direction: column;
    }

    .home-content {
      flex: 1;
      overflow-y: auto;
      padding-bottom: 70px;
    }

    /* ============= CHAT TAB ============= */
    .chat-container {
      display: flex;
      flex-direction: column;
      height: 100%;
    }

    .chat-list {
      flex: 1;
      overflow-y: auto;
      padding: 10px 0;
    }

    .chat-item {
      padding: 12px 15px;
      border-bottom: 1px solid #f0f0f0;
      display: flex;
      gap: 12px;
      align-items: center;
      cursor: pointer;
      transition: background 0.2s;
    }

    .chat-item:active {
      background: #f5f5f5;
    }

    .chat-avatar {
      width: 45px;
      height: 45px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 20px;
      flex-shrink: 0;
    }

    .chat-info {
      flex: 1;
    }

    .chat-name {
      font-size: 15px;
      font-weight: 600;
      color: #333;
    }

    .chat-preview {
      font-size: 13px;
      color: #999;
      margin-top: 3px;
    }

    .chat-time {
      font-size: 12px;
      color: #ccc;
    }

    .empty-state {
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100%;
      color: #999;
      text-align: center;
      padding: 40px;
      flex-direction: column;
    }

    .empty-state-icon {
      font-size: 64px;
      margin-bottom: 15px;
    }

    /* ============= CONTACTS TAB ============= */
    .contacts-container {
      padding: 15px;
      overflow-y: auto;
    }

    .contact-item {
      padding: 12px;
      background: #f9f9f9;
      border-radius: 10px;
      margin-bottom: 10px;
      display: flex;
      align-items: center;
      gap: 12px;
      cursor: pointer;
      transition: background 0.2s;
    }

    .contact-item:active {
      background: #f0f0f0;
    }

    .contact-avatar {
      width: 50px;
      height: 50px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 24px;
      flex-shrink: 0;
    }

    .contact-info {
      flex: 1;
    }

    .contact-name {
      font-size: 14px;
      font-weight: 600;
      color: #333;
    }

    .contact-status {
      font-size: 12px;
      color: #999;
      margin-top: 3px;
    }

    /* ============= FEED TAB ============= */
    .feed-container {
      padding: 15px;
      overflow-y: auto;
    }

    .feed-post {
      background: #fff;
      border-radius: 10px;
      padding: 15px;
      margin-bottom: 15px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
    }

    .post-header {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 12px;
    }

    .post-avatar {
      width: 40px;
      height: 40px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 18px;
    }

    .post-author {
      flex: 1;
    }

    .post-name {
      font-size: 14px;
      font-weight: 600;
      color: #333;
    }

    .post-time {
      font-size: 12px;
      color: #999;
    }

    .post-content {
      font-size: 14px;
      color: #333;
      line-height: 1.5;
      margin-bottom: 12px;
    }

    .post-actions {
      display: flex;
      gap: 15px;
      padding-top: 10px;
      border-top: 1px solid #f0f0f0;
    }

    .post-action {
      flex: 1;
      text-align: center;
      padding: 8px;
      cursor: pointer;
      color: #999;
      font-size: 13px;
      transition: color 0.2s;
    }

    .post-action:active {
      color: #667eea;
    }

    /* ============= LOVE TAB ============= */
    .love-container {
      padding: 15px;
      overflow-y: auto;
    }

    .love-card {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      border-radius: 15px;
      padding: 20px;
      margin-bottom: 15px;
      color: white;
      text-align: center;
      box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
    }

    .love-avatar {
      width: 80px;
      height: 80px;
      background: rgba(255, 255, 255, 0.2);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 40px;
      margin: 0 auto 15px;
      border: 3px solid white;
    }

    .love-name {
      font-size: 18px;
      font-weight: bold;
      margin-bottom: 5px;
    }

    .love-location {
      font-size: 13px;
      opacity: 0.9;
      margin-bottom: 15px;
    }

    .love-actions {
      display: flex;
      gap: 10px;
      justify-content: center;
    }

    .love-btn {
      width: 45px;
      height: 45px;
      border-radius: 50%;
      border: none;
      font-size: 20px;
      cursor: pointer;
      transition: transform 0.2s;
    }

    .love-btn:active {
      transform: scale(0.95);
    }

    .love-reject {
      background: rgba(255, 255, 255, 0.3);
      color: white;
    }

    .love-like {
      background: white;
      color: #ff6b6b;
      font-size: 24px;
    }

    /* ============= PROFILE TAB ============= */
    .profile-container {
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .profile-header {
      text-align: center;
      margin-bottom: 30px;
    }

    .profile-avatar {
      width: 120px;
      height: 120px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 50px;
      margin: 0 auto 15px;
      box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
    }

    .profile-name {
      font-size: 24px;
      font-weight: bold;
      color: #333;
      margin-bottom: 5px;
    }

    .profile-email {
      color: #999;
      font-size: 14px;
      margin-bottom: 20px;
    }

    .profile-section {
      width: 100%;
      background: #f9f9f9;
      border-radius: 10px;
      padding: 15px;
      margin-bottom: 15px;
    }

    .profile-section-title {
      font-size: 13px;
      font-weight: 700;
      color: #999;
      margin-bottom: 10px;
      text-transform: uppercase;
    }

    .profile-input {
      width: 100%;
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 8px;
      font-size: 14px;
      margin-bottom: 8px;
      font-family: inherit;
    }

    .profile-input:last-child {
      margin-bottom: 0;
    }

    .btn-logout {
      width: 100%;
      padding: 12px;
      background: #ff6b6b;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 15px;
      font-weight: 700;
      cursor: pointer;
      margin-top: 20px;
    }

    .btn-save-profile {
      width: 100%;
      padding: 12px;
      background: #667eea;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 15px;
      font-weight: 700;
      cursor: pointer;
      margin-top: 10px;
    }

    /* ============= BOTTOM NAVIGATION BAR ============= */
    .bottom-nav {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      background: white;
      border-top: 1px solid #e5e5ea;
      display: none;
      justify-content: space-around;
      padding: 8px 0;
      max-width: 600px;
      margin: 0 auto;
      z-index: 20;
      box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.05);
    }

    .bottom-nav.show {
      display: flex;
    }

    .nav-item {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 10px;
      cursor: pointer;
      border: none;
      background: none;
      color: #999;
      transition: color 0.3s;
      font-size: 12px;
    }

    .nav-item.active {
      color: #667eea;
    }

    .nav-icon {
      font-size: 24px;
      margin-bottom: 5px;
    }

    .nav-label {
      font-size: 11px;
      font-weight: 600;
    }

    /* ============= PROFILE MODAL ============= */
    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0, 0, 0, 0.5);
      z-index: 100;
      align-items: center;
      justify-content: center;
      max-width: 600px;
      margin: 0 auto;
    }

    .modal.show {
      display: flex;
    }

    .modal-content {
      background: white;
      border-radius: 15px;
      padding: 30px 25px;
      width: 90%;
      text-align: center;
      max-height: 80vh;
      overflow-y: auto;
    }

    .modal-close {
      position: absolute;
      top: 15px;
      right: 15px;
      background: none;
      border: none;
      font-size: 24px;
      cursor: pointer;
      color: #999;
    }

    .modal-avatar {
      width: 100px;
      height: 100px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 40px;
      margin: 0 auto 15px;
    }

    .modal-name {
      font-size: 20px;
      font-weight: bold;
      color: #333;
      margin-bottom: 5px;
    }

    .modal-bio {
      color: #666;
      font-size: 14px;
      margin-bottom: 15px;
    }

    /* ============= SCROLLBAR ============= */
    ::-webkit-scrollbar {
      width: 6px;
    }

    ::-webkit-scrollbar-track {
      background: transparent;
    }

    ::-webkit-scrollbar-thumb {
      background: #ccc;
      border-radius: 3px;
    }

    ::-webkit-scrollbar-thumb:hover {
      background: #999;
    }

    @media (max-width: 600px) {
      .app-container {
        max-width: 100%;
        height: 100vh;
      }
    }
  </style>
</head>
<body>
  <div class="app-container">
    <!-- ============= LOGIN SCREEN ============= -->
    <div id="loginScreen" class="screen active login-screen">
      <div class="login-container">
        <h1>💬 Kree</h1>
        <p>လူမှုကွန်ယက်ချိတ်ဆက်မှု</p>

        <div id="loginForm">
          <div class="form-group">
            <label>အီမေး</label>
            <input type="email" id="loginEmail" placeholder="example@gmail.com">
          </div>
          <div class="form-group">
            <label>စကားဝှက်</label>
            <input type="password" id="loginPassword" placeholder="••••••••">
          </div>
          <button class="btn-login" onclick="handleLogin()">လက်မှတ်ထိုးခြင်း</button>
          <div class="toggle-form">
            အကောင့်မရှိသေးလား?
            <button onclick="toggleForm()">စာရင်းပေးသွင်းခြင်း</button>
          </div>
        </div>

        <div id="signupForm" style="display: none;">
          <div class="form-group">
            <label>အမည်</label>
            <input type="text" id="signupName" placeholder="သင့်အမည်">
          </div>
          <div class="form-group">
            <label>အီမေး</label>
            <input type="email" id="signupEmail" placeholder="example@gmail.com">
          </div>
          <div class="form-group">
            <label>စကားဝှက်</label>
            <input type="password" id="signupPassword" placeholder="••••••••">
          </div>
          <div class="form-group">
            <label>အီလာမျ</label>
            <input type="text" id="signupBio" placeholder="သင်ကိုယ်သင်ကိုအကျဉ်းချုပ်ဖြန့်ဝေခြင်း">
          </div>
          <button class="btn-login" onclick="handleSignup()">စာရင်းပေးသွင်းခြင်း</button>
          <div class="toggle-form">
            ပြီးသားအကောင့်ရှိ?
            <button onclick="toggleForm()">လက်မှတ်ထိုးခြင်း</button>
          </div>
        </div>
      </div>
    </div>

    <!-- ============= HOME SCREEN ============= -->
    <div id="homeScreen" class="screen home-screen">
      <!-- HEADER WITH PROFILE & SEARCH -->
      <div class="header">
        <div class="header-left">
          <div class="profile-circle" onclick="openProfileModal()">👤</div>
          <div class="app-title">Kree</div>
        </div>
        <div class="search-box">
          <input type="text" placeholder="ရှာပါ" id="searchInput">
          <span class="search-icon">🔍</span>
        </div>
      </div>

      <div class="home-content">
        <!-- ============= CHAT TAB ============= -->
        <div id="chatTab" class="active">
          <div class="chat-list" id="chatList">
            <div class="chat-item">
              <div class="chat-avatar">👩</div>
              <div class="chat-info">
                <div class="chat-name">Sarah Johnson</div>
                <div class="chat-preview">အစ်ကိုဆိုတာ အမေးတွေ့ပါတယ်...</div>
              </div>
              <div class="chat-time">၂:၃၀</div>
            </div>
            <div class="chat-item">
              <div class="chat-avatar">👨</div>
              <div class="chat-info">
                <div class="chat-name">John Doe</div>
                <div class="chat-preview">ကျွန်တော်အုပ်စုတွေ့ရင်...</div>
              </div>
              <div class="chat-time">၁:၁၅</div>
            </div>
            <div class="chat-item">
              <div class="chat-avatar">👩</div>
              <div class="chat-info">
                <div class="chat-name">Emily Smith</div>
                <div class="chat-preview">ပြီးတော့ကျွန်တော်အုပ်...</div>
              </div>
              <div class="chat-time">မနက် ၁၀:၄၅</div>
            </div>
          </div>
        </div>

        <!-- ============= CONTACTS TAB ============= -->
        <div id="contactsTab" style="display: none;">
          <div class="contacts-container">
            <div class="contact-item">
              <div class="contact-avatar">👩</div>
              <div class="contact-info">
                <div class="contact-name">Sarah Johnson</div>
                <div class="contact-status">အွန်လိုင်း</div>
              </div>
            </div>
            <div class="contact-item">
              <div class="contact-avatar">👨</div>
              <div class="contact-info">
                <div class="contact-name">John Doe</div>
                <div class="contact-status">အွန်လိုင်း</div>
              </div>
            </div>
            <div class="contact-item">
              <div class="contact-avatar">👩</div>
              <div class="contact-info">
                <div class="contact-name">Emily Smith</div>
                <div class="contact-status">၁၅ မိနစ်လောက်အရင်</div>
              </div>
            </div>
          </div>
        </div>

        <!-- ============= FEED TAB ============= -->
        <div id="feedTab" style="display: none;">
          <div class="feed-container">
            <div class="feed-post">
              <div class="post-header">
                <div class="post-avatar">👩</div>
                <div class="post-author">
                  <div class="post-name">Sarah Johnson</div>
                  <div class="post-time">၂ ခြိုင်တစ်ခုအရင်</div>
                </div>
              </div>
              <div class="post-content">ခရီးလှည့်သည့်အတွက်ကောင်းမွန်တဲ့နေ့ ☀️✈️</div>
              <div class="post-actions">
                <div class="post-action">❤️ ၁၂</div>
                <div class="post-action">💬 ၅</div>
                <div class="post-action">↗️</div>
              </div>
            </div>

            <div class="feed-post">
              <div class="post-header">
                <div class="post-avatar">👨</div>
                <div class="post-author">
                  <div class="post-name">John Doe</div>
                  <div class="post-time">၅ ခြိုင်တစ်ခုအရင်</div>
                </div>
              </div>
              <div class="post-content">နတ်ဆုံးအီမေးတွေ့ပြီးအစ်ကို ❤️</div>
              <div class="post-actions">
                <div class="post-action">❤️ ២ ៨</div>
                <div class="post-action">💬 ១២</div>
                <div class="post-action">↗️</div>
              </div>
            </div>
          </div>
        </div>

        <!-- ============= LOVE TAB ============= -->
        <div id="loveTab" style="display: none;">
          <div class="love-container">
            <div class="love-card">
              <div class="love-avatar">👩</div>
              <div class="love-name">Sarah Johnson</div>
              <div class="love-location">ရန်ကုန်, မြန်မာ</div>
              <div class="love-actions">
                <button class="love-btn love-reject">✕</button>
                <button class="love-btn love-like">♥</button>
              </div>
            </div>

            <div class="love-card">
              <div class="love-avatar">👩</div>
              <div class="love-name">Emily Smith</div>
              <div class="love-location">မန္တလေး, မြန်မာ</div>
              <div class="love-actions">
                <button class="love-btn love-reject">✕</button>
                <button class="love-btn love-like">♥</button>
              </div>
            </div>
          </div>
        </div>

        <!-- ============= PROFILE TAB ============= -->
        <div id="profileTab" style="display: none;">
          <div class="profile-container">
            <div class="profile-header">
              <div class="profile-avatar">👤</div>
              <div class="profile-name" id="profileName">User Name</div>
              <div class="profile-email" id="profileEmail">user@example.com</div>
            </div>

            <div class="profile-section">
              <div class="profile-section-title">အ​ခြေခံ အချက်အလက်များ</div>
              <input type="text" class="profile-input" id="profileBio" placeholder="အီလာမျ">
              <input type="text" class="profile-input" id="profileLocation" placeholder="နေရာ">
              <input type="number" class="profile-input" id="profileAge" placeholder="အသက်">
            </div>

            <div class="profile-section">
              <div class="profile-section-title">စိတ်ဝင်စားလားဟု</div>
              <input type="text" class="profile-input" id="profileInterests" placeholder="ဥပမာ: ခရီးလှည့်သည်, ဂိမ်း, စာဆိုခြင်း">
            </div>

            <button class="btn-save-profile" onclick="saveProfile()">ကယ်သည် အချက်အလက်များ</button>
            <button class="btn-logout" onclick="logout()">ထွက်ခွါမည်</button>
          </div>
        </div>
      </div>
    </div>

    <!-- ============= PROFILE MODAL ============= -->
    <div id="profileModal" class="modal">
      <div class="modal-content">
        <button class="modal-close" onclick="closeProfileModal()">✕</button>
        <div class="modal-avatar">👤</div>
        <div class="modal-name" id="modalName">User Name</div>
        <div class="modal-bio" id="modalBio">Welcome to Kree</div>
        <div style="margin-top: 20px;">
          <button class="btn-save-profile" onclick="closeProfileModal()">အကျင့်အရာကိုပြင်ဆင်ပါ</button>
          <button class="btn-logout" style="margin-top: 10px;" onclick="closeProfileModal()">ပိတ်ပါ</button>
        </div>
      </div>
    </div>

    <!-- ============= BOTTOM NAVIGATION (5 TABS) ============= -->
    <div class="bottom-nav" id="bottomNav">
      <button class="nav-item active" onclick="switchTab('chat')">
        <div class="nav-icon">💬</div>
        <div class="nav-label">စာများ</div>
      </button>
      <button class="nav-item" onclick="switchTab('contacts')">
        <div class="nav-icon">👥</div>
        <div class="nav-label">အဆက်အသွယ်</div>
      </button>
      <button class="nav-item" onclick="switchTab('feed')">
        <div class="nav-icon">📰</div>
        <div class="nav-label">Feed</div>
      </button>
      <button class="nav-item" onclick="switchTab('love')">
        <div class="nav-icon">💕</div>
        <div class="nav-label">ချစ်မှု</div>
      </button>
      <button class="nav-item" onclick="switchTab('profile')">
        <div class="nav-icon">👤</div>
        <div class="nav-label">အကျင့်အရာ</div>
      </button>
    </div>
  </div>

  <script>
    const STORAGE_KEY = 'kree_app_data';

    // Initialize app data
    function initAppData() {
      if (!localStorage.getItem(STORAGE_KEY)) {
        localStorage.setItem(STORAGE_KEY, JSON.stringify({
          users: [
            { id: 1, email: 'test@gmail.com', password: 'test123', name: 'Test User', bio: 'Welcome to Kree', location: 'ရန်ကုန်', age: 25, interests: [] }
          ],
          currentUser: null
        }));
      }
    }

    initAppData();

    // Login Handler
    function handleLogin() {
      const email = document.getElementById('loginEmail').value.trim();
      const password = document.getElementById('loginPassword').value.trim();

      if (!email || !password) {
        alert('အီမေးနှင့် စကားဝှက်များထည့်သွင်းပါ');
        return;
      }

      const appData = JSON.parse(localStorage.getItem(STORAGE_KEY));
      const user = appData.users.find(u => u.email === email && u.password === password);

      if (user) {
        appData.currentUser = user;
        localStorage.setItem(STORAGE_KEY, JSON.stringify(appData));
        switchToHome();
      } else {
        alert('အီမေးသို့မဟုတ်စကားဝှက်မှားဒါ');
      }
    }

    // Signup Handler
    function handleSignup() {
      const name = document.getElementById('signupName').value.trim();
      const email = document.getElementById('signupEmail').value.trim();
      const password = document.getElementById('signupPassword').value.trim();
      const bio = document.getElementById('signupBio').value.trim();

      if (!name || !email || !password) {
        alert('အားလုံးအကွက်များထည့်သွင်းပါ');
        return;
      }

      const appData = JSON.parse(localStorage.getItem(STORAGE_KEY));
      if (appData.users.some(u => u.email === email)) {
        alert('အီမေးဆိုင်ရာအကောင့်ရှိပြီး');
        return;
      }

      const newUser = {
        id: appData.users.length + 1,
        email,
        password,
        name,
        bio,
        location: '',
        age: null,
        interests: []
      };

      appData.users.push(newUser);
      appData.currentUser = newUser;
      localStorage.setItem(STORAGE_KEY, JSON.stringify(appData));

      switchToHome();
    }

    // Toggle between login and signup
    function toggleForm() {
      const loginForm = document.getElementById('loginForm');
      const signupForm = document.getElementById('signupForm');

      if (loginForm.style.display === 'none') {
        loginForm.style.display = 'block';
        signupForm.style.display = 'none';
      } else {
        loginForm.style.display = 'none';
        signupForm.style.display = 'block';
      }
    }

    // Switch to home screen
    function switchToHome() {
      document.getElementById('loginScreen').classList.remove('active');
      document.getElementById('homeScreen').classList.add('active');
      document.getElementById('bottomNav').classList.add('show');
      loadUserProfile();
    }

    // Switch between tabs
    function switchTab(tab) {
      const tabs = {
        chat: document.getElementById('chatTab'),
        contacts: document.getElementById('contactsTab'),
        feed: document.getElementById('feedTab'),
        love: document.getElementById('loveTab'),
        profile: document.getElementById('profileTab')
      };

      const navItems = document.querySelectorAll('.nav-item');
      navItems.forEach(item => item.classList.remove('active'));

      Object.values(tabs).forEach(t => t.style.display = 'none');

      if (tabs[tab]) {
        tabs[tab].style.display = 'block';
      }

      event.target.closest('.nav-item').classList.add('active');
    }

    // Profile Modal
    function openProfileModal() {
      const appData = JSON.parse(localStorage.getItem(STORAGE_KEY));
      const user = appData.currentUser;

      document.getElementById('modalName').textContent = user.name;
      document.getElementById('modalBio').textContent = user.bio;
      document.getElementById('profileModal').classList.add('show');
    }

    function closeProfileModal() {
      document.getElementById('profileModal').classList.remove('show');
    }

    // Load user profile
    function loadUserProfile() {
      const appData = JSON.parse(localStorage.getItem(STORAGE_KEY));
      const user = appData.currentUser;

      document.getElementById('profileName').textContent = user.name;
      document.getElementById('profileEmail').textContent = user.email;
      document.getElementById('profileBio').value = user.bio || '';
      document.getElementById('profileLocation').value = user.location || '';
      document.getElementById('profileAge').value = user.age || '';
      document.getElementById('profileInterests').value = (user.interests || []).join(', ');
    }

    // Save profile
    function saveProfile() {
      const appData = JSON.parse(localStorage.getItem(STORAGE_KEY));
      const userIndex = appData.users.findIndex(u => u.id === appData.currentUser.id);

      if (userIndex !== -1) {
        appData.users[userIndex].bio = document.getElementById('profileBio').value;
        appData.users[userIndex].location = document.getElementById('profileLocation').value;
        appData.users[userIndex].age = document.getElementById('profileAge').value;
        appData.users[userIndex].interests = document.getElementById('profileInterests').value.split(',').map(i => i.trim());

        appData.currentUser = appData.users[userIndex];
        localStorage.setItem(STORAGE_KEY, JSON.stringify(appData));

        alert('အချက်အလက်များကို သိမ်းဆည်းလိုက်ပါတယ်');
      }
    }

    // Logout
    function logout() {
      if (confirm('ကုန်းခွါလုပ်တော့မလား?')) {
        const appData = JSON.parse(localStorage.getItem(STORAGE_KEY));
        appData.currentUser = null;
        localStorage.setItem(STORAGE_KEY, JSON.stringify(appData));

        document.getElementById('homeScreen').classList.remove('active');
        document.getElementById('loginScreen').classList.add('active');
        document.getElementById('bottomNav').classList.remove('show');

        document.getElementById('loginForm').style.display = 'block';
        document.getElementById('signupForm').style.display = 'none';
        document.getElementById('loginEmail').value = '';
        document.getElementById('loginPassword').value = '';
      }
    }
  </script>
</body>
</html>
