<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Cloud Gaming Shop - Mua các gói chơi game đám mây giá rẻ, chất lượng cao.">
    <meta name="keywords" content="cloud gaming, game online, mua gói game">
    <meta name="author" content="Cloud Gaming Team">
    <title>Cloud Gaming Shop</title>
    <link rel="icon" href="assets/favicon.ico" type="image/x-icon">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/tsparticles@2.9.3/tsparticles.bundle.min.js"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #0f0f0f; color: #fff; line-height: 1.6; overflow-x: hidden; }
        #tsparticles { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1; }
        #loading { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #0f0f0f; display: flex; justify-content: center; align-items: center; z-index: 999; transition: opacity 0.5s; }
        #loading.hidden { opacity: 0; pointer-events: none; }
        nav { background: linear-gradient(90deg, #4CAF50, #1e7e34); padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100; box-shadow: 0 2px 10px rgba(0,0,0,0.5); }
        nav .logo { font-size: 1.5em; font-weight: 600; }
        nav .search-container { display: flex; align-items: center; }
        nav input[type="text"] { padding: 8px; width: 200px; border-radius: 5px; border: none; background: #2a2a2a; color: #fff; }
        nav ul { list-style: none; display: flex; align-items: center; gap: 15px; }
        nav ul li a { color: #fff; text-decoration: none; font-size: 1em; }
        nav ul li a:hover { color: #d4e157; }
        nav .cart-count { background: #FF5722; padding: 2px 8px; border-radius: 50%; cursor: pointer; }
        .container { max-width: 1000px; margin: 0 auto; padding: 20px; text-align: center; }
        h1 { font-size: 2em; color: #4CAF50; margin-bottom: 15px; }
        .products { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; margin-top: 20px; }
        .product { background: #1a1a1a; padding: 15px; border-radius: 10px; transition: transform 0.3s; }
        .product:hover { transform: translateY(-5px); box-shadow: 0 5px 15px rgba(0,0,0,0.5); }
        .product img { width: 100%; height: 120px; object-fit: cover; border-radius: 5px; }
        .product h3 { color: #4CAF50; margin: 10px 0; }
        .product p, .product .note, .product .duration, .product .countdown { font-size: 0.9em; color: #bbb; }
        .product .buttons { display: flex; gap: 10px; margin-top: 10px; }
        .product button { padding: 8px; border: none; color: #fff; border-radius: 5px; cursor: pointer; flex: 1; }
        .product button.detail-btn { background: #0288d1; }
        .product button.cart-btn { background: #f9a825; }
        .product button.buy-btn { background: #4CAF50; }
        .product button:disabled { background: #555; cursor: not-allowed; opacity: 0.6; }
        .product button:hover:not(:disabled) { opacity: 0.9; }
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 200; justify-content: center; align-items: center; }
        .modal-content { background: #1a1a1a; padding: 15px; border-radius: 10px; max-width: 400px; width: 90%; }
        .modal-content h2 { color: #4CAF50; margin-bottom: 10px; }
        .modal-content button { background: #4CAF50; padding: 10px; width: 100%; margin-top: 10px; border: none; color: #fff; border-radius: 5px; }
        .login-form, .recharge-form, .transfer-form, .profile-form, .product-form { display: flex; flex-direction: column; gap: 10px; }
        .login-form input, .recharge-form input, .recharge-form select, .transfer-form input, .transfer-form select, .profile-form input, .product-form input, .product-form select { padding: 10px; background: #2a2a2a; border: 1px solid #4CAF50; color: #fff; border-radius: 5px; }
        #scrollTop, #chatNow { position: fixed; bottom: 20px; padding: 10px; background: #4CAF50; border: none; color: #fff; border-radius: 50%; cursor: pointer; }
        #scrollTop { right: 20px; display: none; }
        #chatNow { left: 20px; border-radius: 25px; padding: 10px 20px; background: #0288d1; }
        footer { text-align: center; padding: 15px; background: #1a1a1a; margin-top: 20px; color: #888; }
        #notification { display: none; position: fixed; top: 80px; right: 20px; background: #4CAF50; color: #fff; padding: 10px 20px; border-radius: 5px; z-index: 1000; }
        .admin-controls, .user-actions { margin: 10px 0; display: flex; gap: 10px; }
        .admin-controls input, .user-actions input { flex: 1; padding: 8px; background: #2a2a2a; border: 1px solid #4CAF50; color: #fff; }
        .admin-controls button, .user-actions button { padding: 8px 15px; }
        .user-item { margin: 10px 0; padding: 10px; background: #252525; border-radius: 5px; }
        .history-item { margin: 10px 0; padding: 10px; background: #252525; border-radius: 5px; }
        @media (max-width: 600px) {
            nav { flex-direction: column; gap: 10px; }
            nav .search-container { width: 100%; }
            nav input[type="text"] { width: 100%; }
            .products { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>
    <div id="loading"><h2 style="color: #4CAF50;">Đang tải...</h2></div>
    <div id="tsparticles"></div>
    <nav>
        <div class="logo">Cloud Gaming</div>
        <div class="search-container">
            <input type="text" id="searchInput" placeholder="Tìm kiếm gói..." aria-label="Tìm kiếm gói">
        </div>
        <ul id="navLinks"></ul>
    </nav>
    <div id="notification" role="alert"></div>
    <div class="container" id="home">
        <h1>Cửa hàng Cloud Gaming</h1>
        <div class="products" id="products"></div>
    </div>
    <footer id="contact">
        <p>© 2025 Cloud Gaming Shop | <span id="currentDate"></span> | support@cloudgaming.vn | 0909 123 456</p>
    </footer>
    <button id="scrollTop" aria-label="Cuộn lên đầu">↑</button>
    <button id="chatNow" aria-label="Chat hỗ trợ">Chat</button>

    <!-- Modals -->
    <div class="modal" id="detailModal" role="dialog" aria-labelledby="modalTitle">
        <div class="modal-content">
            <h2 id="modalTitle"></h2>
            <p id="modalText"></p>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
        </div>
    </div>
    <div class="modal" id="cartModal" role="dialog" aria-labelledby="cartTitle">
        <div class="modal-content">
            <h2 id="cartTitle">Giỏ hàng</h2>
            <ul id="cartItems"></ul>
            <p id="cartTotal">Tổng: 0 VNĐ</p>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
            <button id="checkoutBtn">Thanh toán</button>
        </div>
    </div>
    <div class="modal" id="loginModal" role="dialog" aria-labelledby="loginTitle">
        <div class="modal-content">
            <h2 id="loginTitle">Đăng Nhập</h2>
            <form id="loginForm" class="login-form">
                <input type="email" id="loginEmail" placeholder="Email" required aria-label="Email">
                <input type="password" id="loginPassword" placeholder="Mật khẩu" required aria-label="Mật khẩu">
                <button type="submit">Đăng Nhập</button>
            </form>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
        </div>
    </div>
    <div class="modal" id="registerModal" role="dialog" aria-labelledby="registerTitle">
        <div class="modal-content">
            <h2 id="registerTitle">Đăng Ký</h2>
            <form id="registerForm" class="login-form">
                <input type="text" id="registerUsername" placeholder="Tên người dùng" required aria-label="Tên người dùng">
                <input type="email" id="registerEmail" placeholder="Email" required aria-label="Email">
                <input type="password" id="registerPassword" placeholder="Mật khẩu" minlength="6" required aria-label="Mật khẩu">
                <input type="password" id="registerConfirmPassword" placeholder="Xác nhận mật khẩu" required aria-label="Xác nhận mật khẩu">
                <button type="submit">Đăng Ký</button>
            </form>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
        </div>
    </div>
    <div class="modal" id="rechargeModal" role="dialog" aria-labelledby="rechargeTitle">
        <div class="modal-content">
            <h2 id="rechargeTitle">Nạp Tiền</h2>
            <form id="rechargeForm" class="recharge-form">
                <select id="rechargeMethod" required aria-label="Phương thức nạp tiền">
                    <option value="">Phương thức</option>
                    <option value="card">Thẻ cào</option>
                    <option value="bank">Ngân hàng</option>
                </select>
                <div id="cardFields" style="display:none;">
                    <select id="cardProvider" aria-label="Nhà mạng">
                        <option value="viettel">Viettel</option>
                        <option value="mobi">Mobi</option>
                        <option value="vina">Vina</option>
                    </select>
                    <input type="text" id="cardNumber" placeholder="Số seri" aria-label="Số seri">
                    <input type="text" id="cardCode" placeholder="Mã thẻ" aria-label="Mã thẻ">
                </div>
                <div id="bankFields" style="display:none;">
                    <select id="bankProvider" aria-label="Ngân hàng">
                        <option value="abc">ABC Bank</option>
                        <option value="vietinbank">VietinBank</option>
                    </select>
                    <input type="number" id="bankAmount" placeholder="Số tiền (VNĐ)" aria-label="Số tiền">
                </div>
                <button type="submit">Nạp</button>
            </form>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
        </div>
    </div>
    <div class="modal" id="chatModal" role="dialog" aria-labelledby="chatTitle">
        <div class="modal-content">
            <h2 id="chatTitle">Liên hệ</h2>
            <p>Chat: support@cloudgaming.vn<br>Gọi: 0909 123 456</p>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
        </div>
    </div>
    <div class="modal" id="checkoutModal" role="dialog" aria-labelledby="checkoutTitle">
        <div class="modal-content">
            <h2 id="checkoutTitle">Xác nhận thanh toán</h2>
            <p id="checkoutText"></p>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
            <button id="confirmCheckout">Xác nhận</button>
        </div>
    </div>
    <div class="modal" id="adminModal" role="dialog" aria-labelledby="adminTitle">
        <div class="modal-content">
            <h2 id="adminTitle">Quản lý Người Chơi</h2>
            <div class="admin-controls">
                <input type="text" id="adminSearch" placeholder="Tìm kiếm (ID/Email/Tên)..." aria-label="Tìm kiếm người dùng">
                <button id="refreshUsers">Làm mới</button>
            </div>
            <div id="adminUserList"></div>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
        </div>
    </div>
    <div class="modal" id="transferModal" role="dialog" aria-labelledby="transferTitle">
        <div class="modal-content">
            <h2 id="transferTitle">Tặng Số Dư</h2>
            <form id="transferForm" class="transfer-form">
                <select id="transferRecipient" required aria-label="Người nhận">
                    <option value="">Chọn người nhận</option>
                </select>
                <input type="number" id="transferAmount" placeholder="Số tiền (VNĐ)" min="1000" required aria-label="Số tiền">
                <button type="submit">Tặng</button>
            </form>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
        </div>
    </div>
    <div class="modal" id="profileModal" role="dialog" aria-labelledby="profileTitle">
        <div class="modal-content">
            <h2 id="profileTitle">Hồ sơ</h2>
            <form id="profileForm" class="profile-form">
                <input type="text" id="profileUsername" placeholder="Tên người dùng" required aria-label="Tên người dùng">
                <input type="password" id="profilePassword" placeholder="Mật khẩu mới (tùy chọn)" aria-label="Mật khẩu mới">
                <button type="submit">Cập nhật</button>
            </form>
            <button id="syncBalance" style="background: #0288d1;">Đồng bộ số dư</button>
            <div id="purchaseHistory">
                <h3>Lịch sử mua hàng</h3>
                <div id="historyList"></div>
            </div>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
        </div>
    </div>
    <div class="modal" id="productModal" role="dialog" aria-labelledby="productTitle">
        <div class="modal-content">
            <h2 id="productTitle">Quản lý Sản phẩm</h2>
            <form id="productForm" class="product-form">
                <input type="text" id="productName" placeholder="Tên gói" required aria-label="Tên gói">
                <input type="number" id="productPrice" placeholder="Giá (VNĐ)" required aria-label="Giá">
                <input type="text" id="productDuration" placeholder="Thời hạn (e.g., 1 week)" required aria-label="Thời hạn">
                <input type="text" id="productNote" placeholder="Ghi chú" required aria-label="Ghi chú">
                <input type="text" id="productImage" placeholder="URL ảnh" required aria-label="URL ảnh">
                <textarea id="productDetails" placeholder="Chi tiết" required aria-label="Chi tiết"></textarea>
                <button type="submit">Thêm/Cập nhật</button>
            </form>
            <div id="productList"></div>
            <button class="close-modal" aria-label="Đóng">Đóng</button>
        </div>
    </div>

    <script>
        // Configuration
        const config = {
            products: [
                { id: '1000', name: 'Gói Bình Dân', price: 1000, duration: '1 day', note: 'Android/iOS', image: 'https://via.placeholder.com/250x120/FF5722/FFFFFF?text=Gói+Bình+Dân', details: 'Gói rẻ, trải nghiệm cơ bản.' },
                { id: 'basic', name: 'Gói Cơ Bản', price: 120000, duration: '1 week', note: 'Android/iOS', image: 'https://via.placeholder.com/250x120/4CAF50/FFFFFF?text=Gói+Cơ+Bản', details: '1 tuần chơi, game cơ bản, tốc độ ổn định.' },
                { id: 'advanced', name: 'Gói Cao Cấp', price: 230000, duration: '2 months', note: 'Android/iOS', image: 'https://via.placeholder.com/250x120/0288D1/FFFFFF?text=Gói+Cao+Cấp', details: '2 tháng chơi, game đồ họa cao, ưu tiên băng thông.' },
                { id: 'vip', name: 'Gói VIP', price: 530000, duration: '1 month', note: 'Android/iOS', image: 'https://via.placeholder.com/250x120/FF5722/FFFFFF?text=Gói+VIP', details: 'Không giới hạn, tất cả game, hỗ trợ 24/7.' }
            ],
            telegram: { adminLink: 'https://t.me/limorecloudgame' }
        };

        let currentUser = null;
        let cartItems = [];
        let cartCount = 0;
        let redirectToRecharge = false;

        // Initialize
        window.onload = () => {
            setTimeout(() => document.getElementById('loading').classList.add('hidden'), 1000);
            initParticles();
            updateDate();
            startCountdowns();
            loadCart();
            renderProducts();
            renderNav();
            initializeUsers();
            setupEventListeners();
        };

        function initParticles() {
            tsParticles.load('tsparticles', {
                particles: {
                    number: { value: 30 },
                    size: { value: 2 },
                    opacity: { value: 0.3, animation: { enable: true, speed: 1 } },
                    move: { enable: true, speed: 1, direction: 'top' }
                }
            });
        }

        function updateDate() {
            document.getElementById('currentDate').textContent = new Date().toLocaleDateString('vi-VN', { day: '2-digit', month: '2-digit', year: 'numeric' });
        }

        function getDurationMs(duration) {
            if (duration.includes('week')) return parseInt(duration) * 7 * 24 * 60 * 60 * 1000;
            if (duration.includes('month')) return parseInt(duration) * 30 * 24 * 60 * 60 * 1000;
            if (duration.includes('day')) return parseInt(duration) * 24 * 60 * 60 * 1000;
            return 0;
        }

        function startCountdowns() {
            setInterval(() => {
                const now = new Date().getTime();
                const purchases = currentUser ? currentUser.purchases : [];
                document.querySelectorAll('.countdown').forEach(el => {
                    const productId = el.id.replace('countdown-', '');
                    const purchase = purchases.find(p => p.items.some(i => i.id === productId));
                    if (!purchase) {
                        el.textContent = 'Chưa kích hoạt';
                        return;
                    }
                    const product = config.products.find(p => p.id === productId);
                    const purchaseTime = new Date(purchase.date).getTime();
                    const durationMs = getDurationMs(product.duration);
                    const timeLeft = purchaseTime + durationMs - now;
                    if (timeLeft < 0) el.textContent = 'Đã hết hạn!';
                    else {
                        const days = Math.floor(timeLeft / (1000*60*60*24));
                        const hours = Math.floor((timeLeft % (1000*60*60*24)) / (1000*60*60));
                        const minutes = Math.floor((timeLeft % (1000*60*60)) / (1000*60));
                        const seconds = Math.floor((timeLeft % (1000*60)) / 1000);
                        el.textContent = `Còn: ${days}d ${hours}h ${minutes}m ${seconds}s`;
                    }
                });
            }, 1000);
        }

        function initializeUsers() {
            let users = JSON.parse(localStorage.getItem('users') || '[]');
            // Ensure admin account exists with new credentials
            if (!users.some(u => u.email === 'dolce14@gmail.com')) {
                users.push({
                    id: 'ADM001',
                    username: 'dolce14',
                    email: 'dolce14@gmail.com',
                    password: btoa('dolce14'),
                    balance: 1000000, // 1,000,000 VNĐ
                    role: 'admin',
                    purchases: []
                });
            } else {
                // Update existing admin account if it exists
                const adminIndex = users.findIndex(u => u.email === 'dolce14@gmail.com');
                users[adminIndex] = {
                    id: 'ADM001',
                    username: 'dolce14',
                    email: 'dolce14@gmail.com',
                    password: btoa('dolce14'),
                    balance: 1000000, // Ensure balance is set to 1,000,000 VNĐ
                    role: 'admin',
                    purchases: users[adminIndex].purchases || []
                };
            }
            users.forEach((u, i) => {
                if (!u.id) u.id = `USR${String(i + 1).padStart(3, '0')}`;
                if (!u.purchases) u.purchases = [];
            });
            localStorage.setItem('users', JSON.stringify(users));
            localStorage.setItem('products', JSON.stringify(config.products));
            // Mock shared storage for balance sync
            if (!localStorage.getItem('sharedBalances')) {
                localStorage.setItem('sharedBalances', JSON.stringify(users.map(u => ({ id: u.id, balance: u.balance }))));
            } else {
                const sharedBalances = JSON.parse(localStorage.getItem('sharedBalances') || '[]');
                const adminBalanceIndex = sharedBalances.findIndex(b => b.id === 'ADM001');
                if (adminBalanceIndex >= 0) {
                    sharedBalances[adminBalanceIndex].balance = 1000000;
                } else {
                    sharedBalances.push({ id: 'ADM001', balance: 1000000 });
                }
                localStorage.setItem('sharedBalances', JSON.stringify(sharedBalances));
            }
        }

        function showNotification(message, type = 'success') {
            const notification = document.getElementById('notification');
            notification.textContent = message;
            notification.style.background = type === 'success' ? '#4CAF50' : '#d32f2f';
            notification.style.display = 'block';
            setTimeout(() => notification.style.display = 'none', 3000);
        }

        function renderProducts() {
            const products = JSON.parse(localStorage.getItem('products') || JSON.stringify(config.products));
            const grid = document.getElementById('products');
            grid.innerHTML = products.map(p => `
                <div class="product" data-name="${p.name}">
                    <img src="${p.image}" alt="${p.name}">
                    <h3>${p.name}</h3>
                    <p>${p.price.toLocaleString('vi-VN')} VNĐ</p>
                    <div class="duration">Thời hạn: ${p.duration}</div>
                    <div class="note">Lưu ý: ${p.note}</div>
                    <div class="countdown" id="countdown-${p.id}"></div>
                    <div class="buttons">
                        <button class="detail-btn" data-id="${p.id}" aria-label="Xem chi tiết ${p.name}">Chi tiết</button>
                        <button class="cart-btn" data-id="${p.id}" aria-label="Thêm ${p.name} vào giỏ">Thêm giỏ</button>
                        <button class="buy-btn" data-id="${p.id}" ${currentUser && currentUser.balance < p.price ? 'disabled' : ''} aria-label="Mua ngay ${p.name}">Mua ngay</button>
                    </div>
                </div>
            `).join('');
        }

        function renderNav() {
            const navLinks = document.getElementById('navLinks');
            if (currentUser) {
                navLinks.innerHTML = `
                    <li><a href="#home" aria-label="Trang chủ">Trang chủ</a></li>
                    <li><a href="#products" aria-label="Sản phẩm">Sản phẩm</a></li>
                    <li><a href="#contact" aria-label="Liên hệ">Liên hệ</a></li>
                    <li><a href="#" id="rechargeLink" aria-label="Nạp tiền">Nạp Tiền</a></li>
                    ${currentUser.role === 'admin' ? '<li><a href="#" id="transferLink" aria-label="Tặng số dư">Tặng Số Dư</a></li>' : ''}
                    ${currentUser.role === 'admin' ? '<li><a href="#" id="adminLink" aria-label="Quản lý">Quản lý</a></li>' : ''}
                    <li><a href="#" id="productLink" aria-label="Quản lý sản phẩm">Quản lý SP</a></li>
                    <li><a href="#" id="profileLink" aria-label="Hồ sơ"><i class="fas fa-user"></i> ${currentUser.username} (ID: ${currentUser.id}) - ${currentUser.balance.toLocaleString('vi-VN')} VNĐ</a></li>
                    <li><a href="#" id="logoutLink" aria-label="Đăng xuất">Đăng Xuất</a></li>
                    <li><span class="cart-count" id="cartCount" aria-label="Giỏ hàng">${cartCount}</span></li>
                `;
            } else {
                navLinks.innerHTML = `
                    <li><a href="#home" aria-label="Trang chủ">Trang chủ</a></li>
                    <li><a href="#products" aria-label="Sản phẩm">Sản phẩm</a></li>
                    <li><a href="#contact" aria-label="Liên hệ">Liên hệ</a></li>
                    <li><a href="#" id="rechargeLink" aria-label="Nạp tiền">Nạp Tiền</a></li>
                    <li><a href="#" id="loginLink" aria-label="Đăng nhập">Đăng Nhập</a></li>
                    <li><a href="#" id="registerLink" aria-label="Đăng ký">Đăng Ký</a></li>
                    <li><span class="cart-count" id="cartCount" aria-label="Giỏ hàng">0</span></li>
                `;
            }
        }

        async function processRecharge(e) {
            e.preventDefault();
            const method = document.getElementById('rechargeMethod').value;
            if (!currentUser) return showNotification('Vui lòng đăng nhập!', 'error'), showLogin();
            try {
                let details = {};
                if (method === 'card') {
                    const provider = document.getElementById('cardProvider').value;
                    const cardNumber = document.getElementById('cardNumber').value;
                    const cardCode = document.getElementById('cardCode').value;
                    if (!/^\d{10,15}$/.test(cardNumber)) return showNotification('Số seri phải từ 10-15 chữ số!', 'error');
                    if (!/^\d{5,15}$/.test(cardCode)) return showNotification('Mã thẻ phải từ 5-15 chữ số!', 'error');
                    details = { provider, serial: cardNumber, code: cardCode, username: currentUser.username, email: currentUser.email, id: currentUser.id };
                } else if (method === 'bank') {
                    const provider = document.getElementById('bankProvider').value;
                    const amount = parseInt(document.getElementById('bankAmount').value);
                    if (amount < 10000) return showNotification('Số tiền tối thiểu là 10.000 VNĐ!', 'error');
                    details = { provider, amount, username: currentUser.username, email: currentUser.email, id: currentUser.id };
                }
                await sendTelegramMessage('recharge', details);
                showNotification('Đang chuyển đến Telegram để xác nhận nạp tiền…');
                closeModal('rechargeModal');
                setTimeout(() => window.open(config.telegram.adminLink, '_blank'), 1000);
            } catch (error) {
                showNotification('Không gửi được yêu cầu, kiểm tra kết nối mạng!', 'error');
            }
        }

        function setupEventListeners() {
            document.getElementById('searchInput').addEventListener('keyup', searchPackages);
            document.getElementById('scrollTop').addEventListener('click', scrollToTop);
            document.getElementById('chatNow').addEventListener('click', showChat);
            document.getElementById('loginForm').addEventListener('submit', processLogin);
            document.getElementById('registerForm').addEventListener('submit', processRegister);
            document.getElementById('rechargeForm').addEventListener('submit', processRecharge);
            document.getElementById('rechargeMethod').addEventListener('change', toggleRechargeFields);
            document.getElementById('transferForm').addEventListener('submit', processTransfer);
            document.getElementById('profileForm').addEventListener('submit', processProfileUpdate);
            document.getElementById('productForm').addEventListener('submit', processProductUpdate);
            document.getElementById('products').addEventListener('click', handleProductActions);
            document.getElementById('cartModal').addEventListener('click', handleCartActions);
            document.getElementById('adminModal').addEventListener('click', handleAdminActions);
            document.getElementById('adminSearch').addEventListener('keyup', searchUsers);
            document.getElementById('refreshUsers').addEventListener('click', showAdminPanel);
            document.getElementById('navLinks').addEventListener('click', handleNavActions);
            document.querySelectorAll('.close-modal').forEach(btn => btn.addEventListener('click', () => closeModal(btn.closest('.modal').id)));
            window.addEventListener('scroll', () => {
                document.getElementById('scrollTop').style.display = (document.body.scrollTop > 100 || document.documentElement.scrollTop > 100) ? 'block' : 'none';
            });
            document.getElementById('syncBalance').addEventListener('click', syncBalance);
            // Thêm xử lý phím Escape để đóng modal
            document.addEventListener('keydown', (e) => {
                if (e.key === 'Escape') {
                    document.querySelectorAll('.modal').forEach(modal => modal.style.display = 'none');
                }
            });
        }

        function handleNavActions(e) {
            e.preventDefault();
            const target = e.target.closest('a, span');
            if (!target) return;
            if (target.id === 'rechargeLink') showRecharge();
            else if (target.id === 'loginLink') showLogin();
            else if (target.id === 'registerLink') showRegister();
            else if (target.id === 'transferLink') showTransfer();
            else if (target.id === 'adminLink') showAdminPanel();
            else if (target.id === 'profileLink') showProfile();
            else if (target.id === 'logoutLink') logout();
            else if (target.id === 'productLink') showProductManagement();
            else if (target.id === 'cartCount') showCart();
        }

        function handleProductActions(e) {
            const btn = e.target.closest('button');
            if (!btn) return;
            const id = btn.dataset.id;
            const product = JSON.parse(localStorage.getItem('products')).find(p => p.id === id);
            if (btn.classList.contains('detail-btn')) showDetails(product);
            else if (btn.classList.contains('cart-btn')) addToCart(product);
            else if (btn.classList.contains('buy-btn')) buyProduct(product);
        }

        function handleCartActions(e) {
            const btn = e.target.closest('button');
            if (!btn) return;
            if (btn.id === 'checkoutBtn') checkout();
            else if (btn.dataset.index) removeFromCart(parseInt(btn.dataset.index));
        }

        function handleAdminActions(e) {
            const btn = e.target.closest('button');
            if (!btn) return;
            const index = btn.dataset.index;
            if (btn.classList.contains('update-balance')) updateBalance(index);
            else if (btn.classList.contains('delete-user')) deleteUser(index);
        }

        function showDetails(product) {
            const modal = document.getElementById('detailModal');
            document.getElementById('modalTitle').textContent = `Chi tiết: ${product.name}`;
            document.getElementById('modalText').textContent = `${product.details}\nThời hạn: ${product.duration}\nLưu ý: ${product.note}`;
            modal.style.display = 'flex';
            modal.querySelector('.modal-content').focus();
        }

        function addToCart(product) {
            if (!currentUser) return showNotification('Vui lòng đăng nhập!', 'error'), showLogin();
            cartItems.push({ id: product.id, name: product.name, price: product.price });
            cartCount++;
            document.getElementById('cartCount').textContent = cartCount;
            showNotification(`${product.name} đã thêm vào giỏ!`);
            updateCart();
            saveCart();
        }

        function removeFromCart(index) {
            cartItems.splice(index, 1);
            cartCount--;
            document.getElementById('cartCount').textContent = cartCount;
            updateCart();
            saveCart();
        }

        function updateCart() {
            const cartList = document.getElementById('cartItems');
            const cartTotal = document.getElementById('cartTotal');
            cartList.innerHTML = cartItems.length === 0 ? '<li>Giỏ hàng trống!</li>' : cartItems.map((item, index) => `
                <li>${item.name} - ${item.price.toLocaleString('vi-VN')} VNĐ <button data-index="${index}" aria-label="Xóa ${item.name}">Xóa</button></li>
            `).join('');
            cartTotal.textContent = `Tổng: ${cartItems.reduce((sum, item) => sum + item.price, 0).toLocaleString('vi-VN')} VNĐ`;
        }

        function saveCart() {
            if (currentUser) {
                localStorage.setItem(`cart_${currentUser.id}`, JSON.stringify(cartItems));
                localStorage.setItem(`cartCount_${currentUser.id}`, cartCount);
            }
        }

        function loadCart() {
            if (currentUser) {
                const savedItems = localStorage.getItem(`cart_${currentUser.id}`);
                const savedCount = localStorage.getItem(`cartCount_${currentUser.id}`);
                if (savedItems && savedCount) {
                    cartItems = JSON.parse(savedItems);
                    cartCount = parseInt(savedCount);
                    document.getElementById('cartCount').textContent = cartCount;
                    updateCart();
                }
            }
        }

        function showCart() {
            if (!currentUser) return showNotification('Vui lòng đăng nhập!', 'error'), showLogin();
            document.getElementById('cartModal').style.display = 'flex';
            updateCart();
        }

        function checkout() {
            if (!currentUser) return showNotification('Vui lòng đăng nhập!', 'error'), showLogin();
            const total = cartItems.reduce((sum, item) => sum + item.price, 0);
            if (total === 0) return showNotification('Giỏ hàng trống!', 'error');
            if (currentUser.balance < total) return showNotification('Số dư không đủ!', 'error');
            const modal = document.getElementById('checkoutModal');
            document.getElementById('checkoutText').textContent = `Tổng: ${total.toLocaleString('vi-VN')} VNĐ\nSố dư: ${currentUser.balance.toLocaleString('vi-VN')} VNĐ\nXác nhận thanh toán qua Telegram?`;
            modal.style.display = 'flex';
            document.getElementById('confirmCheckout').onclick = () => confirmCheckout(total);
        }

        function confirmCheckout(total) {
            currentUser.balance -= total;
            const users = JSON.parse(localStorage.getItem('users'));
            const userIndex = users.findIndex(u => u.id === currentUser.id);
            users[userIndex].balance = currentUser.balance;
            users[userIndex].purchases.push({
                items: [...cartItems],
                total,
                date: new Date().toLocaleString('vi-VN')
            });
            localStorage.setItem('users', JSON.stringify(users));
            updateSharedBalance(currentUser.id, currentUser.balance);
            sendTelegramMessage('purchase', {
                username: currentUser.username,
                id: currentUser.id,
                items: cartItems.map(i => i.name).join(', '),
                total
            });
            cartItems = [];
            cartCount = 0;
            saveCart();
            updateCart();
            renderNav();
            showNotification('Đang chuyển đến Telegram để xác nhận thanh toán…');
            setTimeout(() => window.open(config.telegram.adminLink, '_blank'), 1000);
            closeModal('checkoutModal');
        }

        function buyProduct(product) {
            if (!currentUser) return showNotification('Vui lòng đăng nhập!', 'error'), showLogin();
            if (currentUser.balance < product.price) return showNotification('Số dư không đủ!', 'error');
            const users = JSON.parse(localStorage.getItem('users'));
            const userIndex = users.findIndex(u => u.id === currentUser.id);
            currentUser.balance -= product.price;
            users[userIndex].balance = currentUser.balance;
            users[userIndex].purchases.push({
                items: [{ id: product.id, name: product.name, price: product.price }],
                total: product.price,
                date: new Date().toLocaleString('vi-VN')
            });
            localStorage.setItem('users', JSON.stringify(users));
            updateSharedBalance(currentUser.id, currentUser.balance);
            sendTelegramMessage('purchase', {
                username: currentUser.username,
                id: currentUser.id,
                items: product.name,
                total: product.price
            });
            showNotification('Đang chuyển đến Telegram để xác nhận…');
            setTimeout(() => window.open(config.telegram.adminLink, '_blank'), 1000);
            renderNav();
            startCountdowns();
        }

        function searchPackages() {
            const input = document.getElementById('searchInput').value.toLowerCase();
            document.querySelectorAll('.product').forEach(product => {
                product.style.display = product.getAttribute('data-name').toLowerCase().includes(input) ? 'block' : 'none';
            });
        }

        function showChat() {
            document.getElementById('chatModal').style.display = 'flex';
        }

        function closeModal(modalId) {
            document.getElementById(modalId).style.display = 'none';
        }

        function showLogin() {
            document.getElementById('loginModal').style.display = 'flex';
        }

        function showRegister() {
            document.getElementById('registerModal').style.display = 'flex';
        }

        function showRecharge() {
            if (!currentUser) return showNotification('Vui lòng đăng nhập!', 'error'), redirectToRecharge = true, showLogin();
            document.getElementById('rechargeModal').style.display = 'flex';
        }

        function toggleRechargeFields() {
            const method = document.getElementById('rechargeMethod').value;
            document.getElementById('cardFields').style.display = method === 'card' ? 'block' : 'none';
            document.getElementById('bankFields').style.display = method === 'bank' ? 'block' : 'none';
        }

        function processLogin(e) {
            e.preventDefault();
            const email = document.getElementById('loginEmail').value.trim();
            const password = btoa(document.getElementById('loginPassword').value);
            const users = JSON.parse(localStorage.getItem('users') || '[]');
            const user = users.find(u => u.email === email && u.password === password);
            if (user) {
                currentUser = user;
                showNotification('Đăng nhập thành công!');
                closeModal('loginModal');
                renderNav();
                loadCart();
                startCountdowns();
                if (redirectToRecharge) {
                    redirectToRecharge = false;
                    showRecharge();
                }
            } else {
                showNotification('Email hoặc mật khẩu sai!', 'error');
            }
        }

        function processRegister(e) {
            e.preventDefault();
            const username = document.getElementById('registerUsername').value.trim();
            const email = document.getElementById('registerEmail').value.trim();
            const password = document.getElementById('registerPassword').value;
            const confirmPassword = document.getElementById('registerConfirmPassword').value;
            if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) return showNotification('Email không hợp lệ!', 'error');
            if (password !== confirmPassword) return showNotification('Mật khẩu không khớp!', 'error');
            if (password.length < 6) return showNotification('Mật khẩu phải từ 6 ký tự!', 'error');
            const users = JSON.parse(localStorage.getItem('users') || '[]');
            if (users.some(u => u.email === email)) return showNotification('Email đã đăng ký!', 'error');
            const newUser = {
                id: `USR${String(users.length + 1).padStart(3, '0')}`,
                username,
                email,
                password: btoa(password),
                balance: 0,
                role: 'user',
                purchases: []
            };
            users.push(newUser);
            localStorage.setItem('users', JSON.stringify(users));
            updateSharedBalance(newUser.id, 0);
            currentUser = newUser;
            sendTelegramMessage('register', { username, email, id: newUser.id });
            showNotification('Đăng ký thành công!');
            closeModal('registerModal');
            renderNav();
            loadCart();
        }

        async function sendTelegramMessage(eventType, details) {
            let message = '';
            switch (eventType) {
                case 'recharge':
                    if (details.serial) {
                        message = `Yêu cầu nạp tiền:\nNhà mạng: ${details.provider.toUpperCase()}\nSố seri: ${details.serial}\nMã thẻ: ${details.code}\nNgười dùng: ${details.username} (${details.email})\nID: ${details.id}\nThời gian: ${new Date().toLocaleString('vi-VN')}`;
                    } else {
                        message = `Yêu cầu nạp tiền qua ngân hàng:\nNgân hàng: ${details.provider}\nSố tiền: ${details.amount.toLocaleString('vi-VN')} VNĐ\nNgười dùng: ${details.username} (${details.email})\nID: ${details.id}\nThời gian: ${new Date().toLocaleString('vi-VN')}`;
                    }
                    break;
                case 'register':
                    message = `Người dùng mới:\nID: ${details.id}\nTên: ${details.username}\nEmail: ${details.email}\nThời gian: ${new Date().toLocaleString('vi-VN')}`;
                    break;
                case 'balance_update':
                    message = `Cập nhật số dư:\nID: ${details.id}\nTên: ${details.username}\nEmail: ${details.email}\nSố dư mới: ${details.newBalance.toLocaleString('vi-VN')} VNĐ\nThời gian: ${new Date().toLocaleString('vi-VN')}`;
                    break;
                case 'transfer':
                    message = `Chuyển số dư:\nTừ: ${details.fromUsername} (ID: ${details.fromId})\nĐến: ${details.toUsername} (ID: ${details.toId})\nSố tiền: ${details.amount.toLocaleString('vi-VN')} VNĐ\nThời gian: ${new Date().toLocaleString('vi-VN')}`;
                    break;
                case 'purchase':
                    message = `Mua hàng:\nNgười dùng: ${details.username} (ID: ${details.id})\nSản phẩm: ${details.items}\nTổng: ${details.total.toLocaleString('vi-VN')} VNĐ\nThời gian: ${new Date().toLocaleString('vi-VN')}`;
                    break;
                case 'product_update':
                    message = `Cập nhật sản phẩm:\nID: ${details.id}\nTên: ${details.name}\nGiá: ${details.price.toLocaleString('vi-VN')} VNĐ\nThời hạn: ${details.duration}\nThời gian: ${new Date().toLocaleString('vi-VN')}`;
                    break;
            }
            console.log('Sending to Telegram:', message);
        }

        function updateSharedBalance(userId, balance) {
            const sharedBalances = JSON.parse(localStorage.getItem('sharedBalances') || '[]');
            const index = sharedBalances.findIndex(b => b.id === userId);
            if (index >= 0) {
                sharedBalances[index].balance = balance;
            } else {
                sharedBalances.push({ id: userId, balance });
            }
            localStorage.setItem('sharedBalances', JSON.stringify(sharedBalances));
        }

        function syncBalance() {
            if (!currentUser) return showNotification('Vui lòng đăng nhập!', 'error'), showLogin();
            const sharedBalances = JSON.parse(localStorage.getItem('sharedBalances') || '[]');
            const shared = sharedBalances.find(b => b.id === currentUser.id);
            if (shared) {
                const users = JSON.parse(localStorage.getItem('users') || '[]');
                const userIndex = users.findIndex(u => u.id === currentUser.id);
                users[userIndex].balance = shared.balance;
                currentUser.balance = shared.balance;
                localStorage.setItem('users', JSON.stringify(users));
                showNotification('Đã đồng bộ số dư!');
                renderNav();
                showProfile();
            } else {
                showNotification('Không tìm thấy dữ liệu số dư!', 'error');
            }
        }

        function logout() {
            currentUser = null;
            cartItems = [];
            cartCount = 0;
            saveCart();
            renderNav();
            showNotification('Đăng xuất thành công!');
        }

        function showAdminPanel() {
            if (!currentUser || currentUser.role !== 'admin') return showNotification('Chỉ admin truy cập được!', 'error');
            const modal = document.getElementById('adminModal');
            displayUserList(JSON.parse(localStorage.getItem('users') || '[]'));
            modal.style.display = 'flex';
        }

        function displayUserList(users) {
            const userList = document.getElementById('adminUserList');
            userList.innerHTML = users.length === 0 ? '<p>Không có người dùng.</p>' : users.map((user, index) => `
                <div class="user-item">
                    <p>ID: ${user.id} | ${user.username} (${user.email})<br>
                    Số dư: ${user.balance.toLocaleString('vi-VN')} VNĐ | Vai trò: ${user.role}</p>
                    ${user.role !== 'admin' ? `
                        <div class="user-actions">
                            <input type="number" class="balance-input" data-index="${index}" placeholder="Số dư mới" aria-label="Số dư mới cho ${user.username}">
                            <button class="update-balance" data-index="${index}" aria-label="Cập nhật số dư">Cập nhật</button>
                            <button class="delete-user" data-index="${index}" aria-label="Xóa người dùng">Xóa</button>
                        </div>
                    ` : ''}
                </div>
            `).join('');
        }

        function updateBalance(index) {
            if (!confirm('Xác nhận cập nhật số dư?')) return;
            const users = JSON.parse(localStorage.getItem('users') || '[]');
            const newBalance = parseInt(document.querySelector(`.balance-input[data-index="${index}"]`).value);
            if (isNaN(newBalance) || newBalance < 0) return showNotification('Số dư không hợp lệ!', 'error');
            users[index].balance = newBalance;
            localStorage.setItem('users', JSON.stringify(users));
            updateSharedBalance(users[index].id, newBalance);
            if (currentUser.email === users[index].email) currentUser.balance = newBalance;
            showNotification(`Đã cập nhật số dư cho ${users[index].username}!`);
            sendTelegramMessage('balance_update', {
                id: users[index].id,
                username: users[index].username,
                email: users[index].email,
                newBalance
            });
            showAdminPanel();
            renderNav();
        }

        function deleteUser(index) {
            if (!confirm('Xác nhận xóa người dùng?')) return;
            const users = JSON.parse(localStorage.getItem('users') || '[]');
            if (users[index].email === currentUser.email) return showNotification('Không thể xóa chính mình!', 'error');
            const userId = users[index].id;
            users.splice(index, 1);
            localStorage.setItem('users', JSON.stringify(users));
            const sharedBalances = JSON.parse(localStorage.getItem('sharedBalances') || '[]');
            const balanceIndex = sharedBalances.findIndex(b => b.id === userId);
            if (balanceIndex >= 0) sharedBalances.splice(balanceIndex, 1);
            localStorage.setItem('sharedBalances', JSON.stringify(sharedBalances));
            showNotification('Xóa thành công!');
            showAdminPanel();
        }

        function searchUsers() {
            const searchTerm = document.getElementById('adminSearch').value.toLowerCase();
            const users = JSON.parse(localStorage.getItem('users') || '[]');
            const filteredUsers = users.filter(user =>
                user.id.toLowerCase().includes(searchTerm) ||
                user.username.toLowerCase().includes(searchTerm) ||
                user.email.toLowerCase().includes(searchTerm)
            );
            displayUserList(filteredUsers);
        }

        function showTransfer() {
            if (!currentUser || currentUser.role !== 'admin') return showNotification('Chỉ admin có thể tặng số dư!', 'error');
            const select = document.getElementById('transferRecipient');
            const users = JSON.parse(localStorage.getItem('users') || '[]');
            select.innerHTML = '<option value="">Chọn người nhận</option>' + users
                .filter(u => u.email !== currentUser.email)
                .map(u => `<option value="${u.id}">${u.username} (ID: ${u.id})</option>`)
                .join('');
            document.getElementById('transferModal').style.display = 'flex';
        }

        function processTransfer(e) {
            e.preventDefault();
            const recipientId = document.getElementById('transferRecipient').value;
            const amount = parseInt(document.getElementById('transferAmount').value);
            const users = JSON.parse(localStorage.getItem('users') || '[]');
            const recipient = users.find(u => u.id === recipientId);
            if (!recipientId) return showNotification('Vui lòng chọn người nhận!', 'error');
            if (isNaN(amount) || amount < 1000) return showNotification('Số tiền phải từ 1.000 VNĐ trở lên!', 'error');
            recipient.balance += amount;
            localStorage.setItem('users', JSON.stringify(users));
            updateSharedBalance(recipient.id, recipient.balance);
            showNotification(`Đã tặng ${amount.toLocaleString('vi-VN')} VNĐ cho ${recipient.username}!`);
            sendTelegramMessage('transfer', {
                fromUsername: currentUser.username,
                fromId: currentUser.id,
                toUsername: recipient.username,
                toId: recipient.id,
                amount
            });
            closeModal('transferModal');
            renderNav();
            if (currentUser.role === 'admin') showAdminPanel();
        }

        function showProfile() {
            if (!currentUser) return showNotification('Vui lòng đăng nhập!', 'error'), showLogin();
            document.getElementById('profileUsername').value = currentUser.username;
            document.getElementById('profilePassword').value = '';
            const historyList = document.getElementById('historyList');
            historyList.innerHTML = currentUser.purchases.length === 0 ? '<p>Chưa có giao dịch nào.</p>' : currentUser.purchases.map(p => `
                <div class="history-item">
                    <p>Ngày: ${p.date}<br>Sản phẩm: ${p.items.map(i => i.name).join(', ')}<br>Tổng: ${p.total.toLocaleString('vi-VN')} VNĐ</p>
                </div>
            `).join('');
            document.getElementById('profileModal').style.display = 'flex';
        }

        function processProfileUpdate(e) {
            e.preventDefault();
            const username = document.getElementById('profileUsername').value.trim();
            const password = document.getElementById('profilePassword').value;
            if (!username) return showNotification('Tên người dùng không được để trống!', 'error');
            const users = JSON.parse(localStorage.getItem('users') || '[]');
            const userIndex = users.findIndex(u => u.id === currentUser.id);
            users[userIndex].username = username;
            if (password) users[userIndex].password = btoa(password);
            localStorage.setItem('users', JSON.stringify(users));
            currentUser.username = username;
            showNotification('Cập nhật hồ sơ thành công!');
            closeModal('profileModal');
            renderNav();
        }

        function showProductManagement() {
            if (!currentUser || currentUser.role !== 'admin') return showNotification('Chỉ admin truy cập được!', 'error');
            const products = JSON.parse(localStorage.getItem('products') || '[]');
            const productList = document.getElementById('productList');
            productList.innerHTML = products.map(p => `
                <div class="user-item">
                    <p>${p.name} - ${p.price.toLocaleString('vi-VN')} VNĐ<br>Thời hạn: ${p.duration}<br>Lưu ý: ${p.note}</p>
                    <button class="edit-product" data-id="${p.id}" aria-label="Sửa sản phẩm">Sửa</button>
                    <button class="delete-product" data-id="${p.id}" aria-label="Xóa sản phẩm">Xóa</button>
                </div>
            `).join('');
            document.getElementById('productForm').reset();
            document.getElementById('productModal').style.display = 'flex';
            document.getElementById('productList').onclick = handleProductManagementActions;
        }

        function handleProductManagementActions(e) {
            const btn = e.target.closest('button');
            if (!btn) return;
            const id = btn.dataset.id;
            if (btn.classList.contains('edit-product')) {
                const products = JSON.parse(localStorage.getItem('products') || '[]');
                const product = products.find(p => p.id === id);
                document.getElementById('productName').value = product.name;
                document.getElementById('productPrice').value = product.price;
                document.getElementById('productDuration').value = product.duration;
                document.getElementById('productNote').value = product.note;
                document.getElementById('productImage').value = product.image;
                document.getElementById('productDetails').value = product.details;
                document.getElementById('productForm').dataset.id = id;
            } else if (btn.classList.contains('delete-product')) {
                if (!confirm('Xác nhận xóa sản phẩm?')) return;
                let products = JSON.parse(localStorage.getItem('products') || '[]');
                products = products.filter(p => p.id !== id);
                localStorage.setItem('products', JSON.stringify(products));
                showNotification('Xóa sản phẩm thành công!');
                showProductManagement();
                renderProducts();
            }
        }

        function processProductUpdate(e) {
            e.preventDefault();
            const name = document.getElementById('productName').value.trim();
            const price = parseInt(document.getElementById('productPrice').value);
            const duration = document.getElementById('productDuration').value.trim();
            const note = document.getElementById('productNote').value.trim();
            const image = document.getElementById('productImage').value.trim();
            const details = document.getElementById('productDetails').value.trim();
            if (!name || price <= 0 || !duration || !note || !image || !details) return showNotification('Vui lòng điền đầy đủ thông tin!', 'error');
            let products = JSON.parse(localStorage.getItem('products') || '[]');
            const existingId = document.getElementById('productForm').dataset.id;
            if (existingId) {
                const index = products.findIndex(p => p.id === existingId);
                products[index] = { id: existingId, name, price, duration, note, image, details };
            } else {
                products.push({ id: `prod${Date.now()}`, name, price, duration, note, image, details });
            }
            localStorage.setItem('products', JSON.stringify(products));
            sendTelegramMessage('product_update', { id: existingId || products[products.length - 1].id, name, price, duration });
            showNotification('Cập nhật sản phẩm thành công!');
            document.getElementById('productForm').reset();
            delete document.getElementById('productForm').dataset.id;
            showProductManagement();
            renderProducts();
        }

        function scrollToTop() {
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }
    </script>
</body>
</html>
