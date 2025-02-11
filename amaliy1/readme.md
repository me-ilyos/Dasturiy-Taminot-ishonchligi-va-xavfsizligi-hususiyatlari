# Dasturiy ta'minot ishonchliligi va xavfsizligi

Assalomu alaykum, aziz o'quvchilar! Bugun biz veb-saytlarda tizimga kirish va ro'yxatdan o'tish jarayonlarini o'rganamiz. Biz oddiy tizimdan boshlaymiz.

## Register.html - Ro'yxatdan o'tish sahifasi

Birinchi navbatda register.html faylining to'liq kodini ko'rib chiqamiz:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Register</title>
    <style>
        .form-container {
            width: 300px;
            margin: 50px auto;
            padding: 20px;
            border: 1px solid #ccc;
        }
        .form-group { margin-bottom: 15px; }
        input {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
        }
        button {
            width: 100%;
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
        .link {
            text-align: center;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="form-container">
        <h2>Register</h2>
        <form id="registerForm">
            <div class="form-group">
                <label>Username:</label>
                <input type="text" id="username" required>
            </div>
            <div class="form-group">
                <label>Password:</label>
                <input type="password" id="password" required>
            </div>
            <button type="submit">Register</button>
        </form>
        <div class="link">
            <a href="login.html">Already have an account? Login</a>
        </div>
    </div>

    <script>
        console.log('Registration page loaded');

        function addLog(action, username, password, success, details = '') {
            const logEntry = {
                timestamp: new Date().toLocaleString(),
                action: action,
                username: username,
                password: password,
                success: success,
                details: details
            };

            let logs = [];
            const existingLogs = localStorage.getItem('logs');
            if (existingLogs) {
                logs = JSON.parse(existingLogs);
            }

            logs.push(logEntry);
            localStorage.setItem('logs', JSON.stringify(logs));

            console.log('------------------------');
            console.log('New Activity Log:');
            console.log('Time:', logEntry.timestamp);
            console.log('Action:', logEntry.action);
            console.log('Credentials used:');
            console.log('  Username:', logEntry.username);
            console.log('  Password:', logEntry.password);
            console.log('Success:', logEntry.success);
            console.log('Details:', logEntry.details);
            console.log('------------------------');
        }

        document.getElementById('registerForm').addEventListener('submit', function(e) {
            e.preventDefault();
            console.log('Registration attempt started');
            
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;

            console.log('Checking for existing username:', username);

            let users = [];
            const existingUsers = localStorage.getItem('users');
            if (existingUsers) {
                users = JSON.parse(existingUsers);
                console.log('Current registered users:', users);
            }

            if (users.some(u => u.username === username)) {
                console.log('Registration failed: Username already exists');
                addLog('REGISTER', username, password, false, 'Username already exists');
                alert('Username already exists!');
                return;
            }

            users.push({ username, password });
            localStorage.setItem('users', JSON.stringify(users));
            
            console.log('Registration successful for user:', username);
            addLog('REGISTER', username, password, true, 'New user registered');
            alert('Registration successful!');
            window.location.href = 'login.html';
        });
    </script>
</body>
</html>
```

### Register.html tushuntirilishi

HTML qismida biz forma yaratdik. Forma tarkibida:
- Username uchun oddiy matn maydoni
- Parol uchun maxfiy maydon (password type)
- Ro'yxatdan o'tish tugmasi
- Login sahifasiga o'tish havolasi

CSS qismida formani chiroyli ko'rinishga keltirdik:
- Forma markazda joylashgan (margin: 50px auto)
- Oq fon va chegaralar (border: 1px solid #ccc)
- Yashil tugma (background-color: #4CAF50)
- Maydonlar orasida bo'shliqlar (margin-bottom: 15px)

JavaScript qismida asosiy logika yozilgan:

addLog() funksiyasi har bir harakat uchun jurnal yozuvini saqlaydi. U quyidagi ma'lumotlarni saqlaydi:
- Harakat vaqti (timestamp)
- Harakat turi (action)
- Username
- Parol
- Muvaffaqiyatli/muvaffaqiyatsiz (success)
- Qo'shimcha ma'lumot (details)

Ro'yxatdan o'tish jarayoni:
1. Foydalanuvchi ma'lumotlarni kiritib, "Register" tugmasini bosadi
2. JavaScript forma yuborilishini ushlab qoladi (e.preventDefault())
3. Kiritilgan username va parolni olamiz
4. LocalStorage dan mavjud foydalanuvchilar ro'yxatini olamiz
5. Agar username band bo'lsa, xato xabari chiqaramiz
6. Aks holda, yangi foydalanuvchini ro'yxatga qo'shamiz
7. Muvaffaqiyatli ro'yxatdan o'tgani haqida xabar beramiz
8. Login sahifasiga yo'naltiramiz

## Login.html - Tizimga kirish sahifasi

```html
<!DOCTYPE html>
<html>
<head>
    <title>Login</title>
    <style>
        .form-container {
            width: 300px;
            margin: 50px auto;
            padding: 20px;
            border: 1px solid #ccc;
        }
        .form-group { margin-bottom: 15px; }
        input {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
        }
        button {
            width: 100%;
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
        .link {
            text-align: center;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="form-container">
        <h2>Login</h2>
        <form id="loginForm">
            <div class="form-group">
                <label>Username:</label>
                <input type="text" id="username" required>
            </div>
            <div class="form-group">
                <label>Password:</label>
                <input type="password" id="password" required>
            </div>
            <button type="submit">Login</button>
        </form>
        <div class="link">
            <a href="register.html">Don't have an account? Register</a>
        </div>
    </div>

    <script>
        console.log('Login page loaded');

        function addLog(action, username, password, success, details = '') {
            const logEntry = {
                timestamp: new Date().toLocaleString(),
                action: action,
                username: username,
                password: password,
                success: success,
                details: details
            };

            let logs = [];
            const existingLogs = localStorage.getItem('logs');
            if (existingLogs) {
                logs = JSON.parse(existingLogs);
            }

            logs.push(logEntry);
            localStorage.setItem('logs', JSON.stringify(logs));

            console.log('------------------------');
            console.log('New Activity Log:');
            console.log('Time:', logEntry.timestamp);
            console.log('Action:', logEntry.action);
            console.log('Credentials used:');
            console.log('  Username:', logEntry.username);
            console.log('  Password:', logEntry.password);
            console.log('Success:', logEntry.success);
            console.log('Details:', logEntry.details);
            console.log('------------------------');
        }

        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            console.log('Login attempt started');
            
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;

            const users = JSON.parse(localStorage.getItem('users') || '[]');
            console.log('Current registered users:', users);

            const user = users.find(u => u.username === username && u.password === password);

            if (user) {
                console.log('Login successful for user:', username);
                addLog('LOGIN', username, password, true, 'Successful login');
                alert('Login successful!');
                window.location.href = 'home.html';
            } else {
                console.log('Login failed for user:', username);
                addLog('LOGIN', username, password, false, 'Invalid credentials');
                alert('Invalid username or password');
            }
        });
    </script>
</body>
</html>
```

### Login.html tushuntirilishi

Login sahifasining tuzilishi register sahifasiga o'xshaydi, lekin jarayoni boshqacha:

1. Foydalanuvchi username va parolni kiritadi
2. JavaScript forma yuborilishini ushlab qoladi
3. localStorage dan foydalanuvchilar ro'yxatini olamiz
4. find() metodi orqali kiritilgan ma'lumotlarga mos foydalanuvchini qidiramiz
5. Agar topilsa:
   - Jurnal yozuvini qo'shamiz (addLog)
   - Muvaffaqiyatli kirish haqida xabar beramiz
   - Home sahifasiga yo'naltiramiz
6. Agar topilmasa:
   - Jurnal yozuvini qo'shamiz
   - Xatolik haqida xabar beramiz

## Home.html - Asosiy sahifa

```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome</title>
    <style>
        .container {
            width: 300px;
            margin: 50px auto;
            text-align: center;
        }
        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Welcome!</h1>
        <p>You have successfully logged in.</p>
        <button onclick="window.location.href='login.html'">Logout</button>
    </div>
</body>
</html>
```

### Home.html tushuntirilishi

Bu eng oddiy sahifa bo'lib, unda:
- Salomlash xabari
- Muvaffaqiyatli kirish haqida xabar
- Chiqish tugmasi

Chiqish tugmasi bosilganda foydalanuvchi login.html sahifasiga qaytariladi.

## Xavfsizlik muammolari

Bu oddiy tizimda bir nechta jiddiy muammolar bor:

1. Parollar ochiq matnda saqlanmoqda (localStorage da)
2. Username va parol validatsiyasi yo'q
3. CSRF himoyasi mavjud emas
4. Login urinishlar soni cheklanmagan
5. Session boshqaruvi yo'q

Bu muammolarni hal qilish uchun bizga:
- Parollarni shifrlash
- Ma'lumotlarni validatsiya qilish
- CSRF token qo'shish
- Login urinishlarni cheklash
- Session boshqaruvini qo'shish kerak

Keyingi darsimizda biz secure_login.html va secure_register.html fayllarini ko'rib chiqamiz, ularda yuqoridagi muammolar hal qilingan.


## 2 CHI QISM

# Dasturiy ta'minot ishonchliligi va xavfsizligi (2-qism)

## Kirish

Assalomu alaykum, aziz o'quvchilar! O'tgan darsimizda biz oddiy login tizimini ko'rib chiqdik. Bu darsda biz xavfsiz login tizimini o'rganamiz. Oldingi tizimimizda bir nechta xavfsizlik muammolari bor edi. Keling, avval bu muammolarni eslab o'tamiz, keyin ularni qanday hal qilishni ko'ramiz.

Oldingi tizimdagi asosiy muammolar:
1. Parollar ochiq tarzda saqlanardi
2. Hech qanday tekshiruv yo'q edi
3. Zararli saytlardan himoya yo'q edi
4. Login paytida cheklovlar yo'q edi

## Secure Register - Xavfsiz ro'yxatdan o'tish tizimi

Avval secure_register.html faylini ko'rib chiqamiz. Bu fayl oddiy register.html dan ancha farq qiladi. Keling, har bir qismini alohida ko'rib chiqamiz.

### HTML tuzilishi

```html
<form id="registerForm">
    <!-- CSRF Token -->
    <input type="hidden" id="csrf_token">
    
    <div class="form-group">
        <label>Username:</label>
        <input type="text" id="username" required>
        <div class="requirements">
            Username must be 5-20 characters, only letters and numbers
        </div>
        <div id="username-error" class="error"></div>
    </div>
    
    <div class="form-group">
        <label>Password:</label>
        <input type="password" id="password" required>
        <div class="requirements">
            Password must have 8+ characters, 1 uppercase, 1 lowercase, 1 number
        </div>
        <div id="password-error" class="error"></div>
    </div>
    <button type="submit">Register</button>
</form>
```

Yangi formada qo'shimcha elementlar bor:

1. CSRF Token - bu maxfiy token. U `<input type="hidden">` sifatida saqlanadi. Bu token formani zararli saytlardan himoya qiladi. Token haqida keyinroq batafsil gaplashamiz.

2. Requirements (Talablar) - har bir maydon uchun alohida talablar ro'yxati bor. Bu foydalanuvchiga qanday ma'lumot kiritish kerakligini ko'rsatadi. Masalan, username qanday harflardan iborat bo'lishi, parol qanchalik murakkab bo'lishi kerakligi.

3. Error (Xato) maydoni - har bir maydon uchun alohida xato xabari maydoni bor. Agar foydalanuvchi noto'g'ri ma'lumot kiritsa, shu yerda xato xabari ko'rsatiladi.

### JavaScript funksiyalari

Xavfsiz tizimda bir nechta muhim funksiyalar bor. Har bir funksiya o'z vazifasiga ega:

#### 1. CSRF Token yaratish:
```javascript
function generateCSRFToken() {
    return Math.random().toString(36).substr(2) + 
           Math.random().toString(36).substr(2);
}
```

Bu funksiya tasodifiy belgilardan iborat maxfiy token yaratadi. Bu token formani himoya qiladi. Tushuntirish uchun oddiy misol:
- Tasavvur qiling, siz bankda pul o'tkazmoqchisiz
- Bank sizga maxsus raqam beradi
- Faqat shu raqam bilan pul o'tkaza olasiz
- Boshqa odam bu raqamni bilmasa, sizning nomingizdan pul o'tkaza olmaydi

CSRF token xuddi shunday ishlaydi - u formani boshqa saytlardan yuborilgan so'rovlardan himoya qiladi.

#### 2. Username tekshirish:
```javascript
function validateUsername(username) {
    const pattern = /^[a-zA-Z0-9]{5,20}$/;
    return pattern.test(username);
}
```

Bu funksiya usernameni tekshiradi. Pattern (andoza) quyidagilarni tekshiradi:
- Uzunligi 5 dan 20 gacha bo'lishi kerak
- Faqat harflar va raqamlar bo'lishi mumkin
- Bo'sh joy yoki maxsus belgilar mumkin emas

#### 3. Parol tekshirish:
```javascript
function validatePassword(password) {
    const hasUppercase = /[A-Z]/.test(password);  // Katta harf bormi?
    const hasLowercase = /[a-z]/.test(password);  // Kichik harf bormi?
    const hasNumber = /\d/.test(password);        // Raqam bormi?
    const hasLength = password.length >= 8;       // Uzunligi 8 dan kattami?
    
    return hasUppercase && hasLowercase && hasNumber && hasLength;
}
```

Parol to'rtta shartga javob berishi kerak:
1. Kamida bitta katta harf bo'lishi kerak (masalan, A, B, C)
2. Kamida bitta kichik harf bo'lishi kerak (masalan, a, b, c)
3. Kamida bitta raqam bo'lishi kerak (masalan, 1, 2, 3)
4. Uzunligi kamida 8 ta belgi bo'lishi kerak

#### 4. Parolni shifrlash:
```javascript
function encodePassword(password) {
    return btoa(password);
}
```

Bu funksiya parolni Base64 formatiga o'zgartiradi. Base64 - bu oddiy shifrlash usuli. 

Muhim eslatma: Real tizimlarda Base64 ishlatilmaydi, chunki u juda oddiy. O'rniga bcrypt yoki Argon2 kabi kuchli shifrlash usullari ishlatiladi. Biz o'rganish uchun Base64 dan foydalanamiz.

#### 5. Harakatlarni qayd qilish:
```javascript
function addLog(action, username, password, success, details = '') {
    const logEntry = {
        timestamp: new Date().toLocaleString(),
        action: action,
        username: username,
        password: password, 
        success: success,
        details: details
    };

    let logs = [];
    const existingLogs = localStorage.getItem('logs');
    if (existingLogs) {
        logs = JSON.parse(existingLogs);
    }

    logs.push(logEntry);
    localStorage.setItem('logs', JSON.stringify(logs));
}
```

Bu funksiya har bir harakatni qayd qilib boradi. U quyidagi ma'lumotlarni saqlaydi:
- Harakat vaqti (timestamp)
- Harakat turi (masalan, "REGISTER" yoki "LOGIN")
- Ishlatilgan username
- Shifrlangan parol
- Harakat muvaffaqiyatli bo'ldimi?
- Qo'shimcha ma'lumotlar

### Ro'yxatdan o'tish jarayoni

Endi butun jarayonni ko'rib chiqamiz. Foydalanuvchi "Register" tugmasini bosganda nima bo'ladi:

1. CSRF tokenni tekshirish:
```javascript
if (csrfToken !== localStorage.getItem('csrf_token')) {
    alert('Security token mismatch. Please refresh the page.');
    return;
}
```

2. Username tekshirish:
```javascript
if (!validateUsername(username)) {
    document.getElementById('username-error').textContent = 
        'Username must be 5-20 characters, only letters and numbers';
    addLog('REGISTER', username, password, false, 'Invalid username format');
    return;
}
```

3. Parol tekshirish:
```javascript
if (!validatePassword(password)) {
    document.getElementById('password-error').textContent = 
        'Password must have 8+ characters, 1 uppercase, 1 lowercase, 1 number';
    addLog('REGISTER', username, password, false, 'Invalid password format');
    return;
}
```

4. Username bandmi-yo'qmi tekshirish:
```javascript
if (users.some(u => u.username === username)) {
    document.getElementById('username-error').textContent = 'Username already exists';
    addLog('REGISTER', username, password, false, 'Username already exists');
    return;
}
```

5. Yangi foydalanuvchini saqlash:
```javascript
const encodedPassword = encodePassword(password);
users.push({ username, password: encodedPassword });
localStorage.setItem('users', JSON.stringify(users));
```

6. Muvaffaqiyatli ro'yxatdan o'tish:
```javascript
addLog('REGISTER', username, encodedPassword, true, 'New user registered');
alert('Registration successful!');
window.location.href = 'login.html';
```

Bu yerda har bir qadam muhim:
1. Avval xavfsizlik tekshiriladi (CSRF token)
2. Keyin ma'lumotlar to'g'riligi tekshiriladi (username, parol)
3. So'ng takroriy username tekshiriladi
4. Oxirida yangi foydalanuvchi qo'shiladi

Har bir xato uchun:
- Foydalanuvchiga aniq xabar ko'rsatiladi
- Xato jurnal yozuviga qo'shiladi
- Keyingi qadamga o'tilmaydi

## Secure Login - Xavfsiz kirish tizimi

Endi secure_login.html faylini ko'rib chiqamiz. Bu fayl ham oddiy login.html dan ancha farq qiladi.

### Login HTML tuzilishi

```html
<form id="loginForm">
    <input type="hidden" id="csrf_token">
    
    <div class="form-group">
        <label>Username:</label>
        <input type="text" id="username" required>
        <div id="username-error" class="error"></div>
    </div>
    
    <div class="form-group">
        <label>Password:</label>
        <input type="password" id="password" required>
        <div id="password-error" class="error"></div>
    </div>
    <button type="submit">Login</button>
</form>
```

Login formasi ro'yxatdan o'tish formasiga o'xshaydi, lekin:
- Requirements (talablar) yo'q, chunki login paytida foydalanuvchi allaqachon to'g'ri ma'lumot kiritish qoidalarini biladi
- Xato xabarlari uchun maydonlar bor
- CSRF token bor

### Login urinishlarini boshqarish

Login tizimida muhim yangilik - bu login urinishlarini cheklash:

```javascript
let loginAttempts = {};

// Login urinishlarini tekshirish
if (!loginAttempts[username]) {
    loginAttempts[username] = {
        count: 0,
        lastAttempt: new Date()
    };
}

// 30 daqiqa o'tganmi tekshirish
const thirtyMinutesAgo = new Date(new Date().getTime() - 30 * 60 * 1000);
if (new Date(loginAttempts[username].lastAttempt) < thirtyMinutesAgo) {
    loginAttempts[username].count = 0;
}

// Urinishlar soni 3 tadan oshganmi?
if (loginAttempts[username].count >= 3) {
    const waitTime = Math.ceil(
        (new Date(loginAttempts[username].lastAttempt).getTime() + 
        30 * 60 * 1000 - new Date().getTime()) / 60000
    );
    addLog('LOGIN', username, encodePassword(password), false, 'Account locked');
    alert(`Account is locked. Please try again in ${waitTime} minutes`);
    return;
}
```

Bu tizim quyidagicha ishlaydi:

1. Har bir foydalanuvchi uchun hisoblagich yuritiladi:
   - Necha marta urinish qilgani (count)
   - Oxirgi urinish vaqti (lastAttempt)

2. Agar foydalanuvchi 3 marta noto'g'ri urinsa:
   - Akkaunt 30 daqiqaga bloklanadi
   - Foydalanuvchiga qancha vaqt kutish kerakligi aytiladi
   - Bu harakat jurnal yozuviga qo'shiladi

3. 30 daqiqa o'tgandan so'ng:
   - Hisoblagich yana 0 ga tushadi
   - Foydalanuvchi qaytadan urinish qila oladi

### Login jarayoni

Login jarayoni ham bir necha bosqichdan iborat:

1. CSRF tokenni tekshirish
2. Login urinishlar sonini tekshirish
3. Foydalanuvchi ma'lumotlarini tekshirish:
```javascript
const users = JSON.parse(localStorage.getItem('users') || '[]');
const encodedPassword = encodePassword(password);
const user = users.find(u => u.username === username && 
                           u.password === encodedPassword);
```

4. Natijaga qarab harakat qilish:
```javascript
if (user) {
    loginAttempts[username].count = 0;
    addLog('LOGIN', username, encodedPassword, true, 'Successful login');
    alert('Login successful!');
    window.location.href = 'home.html';
} else {
    loginAttempts[username].count++;
    loginAttempts[username].lastAttempt = new Date();
    
    const remainingAttempts = 3 - loginAttempts[username].count;
    addLog('LOGIN', username, encodedPassword, false, 
        `Invalid credentials - ${remainingAttempts} attempts remaining`);
    alert(`Invalid username or password. ${remainingAttempts} attempts remaining`);
}
```

### Amaliy mashg'ulot uchun topshiriqlar
# Xavfsiz Login Tizimi - Amaliy qism

## Chrome DevTools orqali tekshirish va o'rganish

Chrome DevTools bizning xavfsizlik tizimimizni chuqurroq tushunishga yordam beradi. Application bo'limida Local Storage da saqlanayotgan barcha ma'lumotlarni ko'rishimiz mumkin:

### Users ma'lumotlarini tekshirish
Local Storage da 'users' kaliti ostida foydalanuvchilar ro'yxati saqlanadi. Bu yerda siz quyidagi ma'lumotlarni ko'rasiz:
```javascript
[
    {
        "username": "testuser",
        "password": "UGFzc3dvcmQxMjM=" // Bu Base64 da shifrlangan parol
    }
]
```

Parolning Base64 da shifrlangani bizga xavfsizlikning birinchi qatlamini ta'minlaydi. Real tizimlarda biz yanada kuchliroq shifrlash usullarini ishlatamiz, lekin Base64 ham parolni to'g'ridan-to'g'ri o'qib olish imkonini bermaydi.

### Logs (Jurnal yozuvlari) ni tekshirish
'logs' kaliti ostida barcha harakatlar qayd etib boriladi:
```javascript
[
    {
        "timestamp": "2025-02-04 10:30:45",
        "action": "LOGIN",
        "username": "testuser",
        "password": "UGFzc3dvcmQxMjM=",
        "success": false,
        "details": "Invalid credentials - 2 attempts remaining"
    }
]
```

Jurnal yozuvlari bizga tizimda sodir bo'layotgan barcha harakatlarni kuzatib borish imkonini beradi. Bu xavfsizlik nuqtai nazaridan juda muhim, chunki:
- Muvaffaqiyatsiz urinishlarni ko'rish mumkin
- Shubhali harakatlarni aniqlash mumkin
- Tizimning ishlashini tahlil qilish mumkin

### CSRF token ni tekshirish
'csrf_token' kaliti ostida joriy CSRF tokeni saqlanadi. Bu token har safar sahifa yangilanganda o'zgaradi va faqat bitta seansda amal qiladi.

## Amaliy mashqlar

### 1. Ro'yxatdan o'tish jarayonini sinash

Turli xil holatlarni sinab ko'ring:

**Username validatsiyasi:**
- 4 ta belgidan iborat username kiriting (xato bo'lishi kerak)
- Maxsus belgilar ishlatib ko'ring (masalan: test@user)
- Bo'sh joy ishlatib ko'ring (test user)
- 21 ta belgidan uzun username kiriting

**Parol validatsiyasi:**
- Faqat kichik harflardan iborat parol (abc123abc)
- Faqat katta harflardan iborat parol (ABC123ABC)
- Raqamsiz parol (abcABCabc)
- 7 ta belgidan qisqa parol
- Juda murakkab parol (P@ssw0rd123!)

### 2. Login jarayonini sinash

**Login urinishlarini tekshirish:**
1. Noto'g'ri parol bilan 3 marta kirish:
   - Birinchi urinish - 2 ta urinish qolganini ko'rasiz
   - Ikkinchi urinish - 1 ta urinish qolganini ko'rasiz
   - Uchinchi urinish - akkount bloklanadi

2. Bloklangan holatni tekshirish:
   - 30 daqiqa kutmasdan login qilib ko'ring
   - DevTools da loginAttempts obyektini tekshiring
   - Vaqt o'tishini kuting va qayta urinib ko'ring

### 3. CSRF himoyasini sinash

CSRF himoyasi qanday ishlashini tushunish uchun:
1. DevTools da localStorage.getItem('csrf_token') ni ishlatib joriy tokenni ko'ring
2. localStorage dan csrf_token ni o'chirib tashlang
3. Forma yuborishga harakat qiling
4. Xato xabarini ko'ring
5. Sahifani yangilab, yangi token yaratilishini kuzating

### 4. Parollarni tekshirish

Base64 shifrlashni tushunish uchun:
1. DevTools Console da quyidagi kodlarni bajaring:
```javascript
// Parolni shifrlash
btoa("Password123")  // Base64 ga o'tkazish
atob("UGFzc3dvcmQxMjM=")  // Base64 dan qaytarish
```

2. Local Storage dagi shifrlangan parollarni atob() funksiyasi bilan qayta o'qib ko'ring

### 5. Jurnal yozuvlarini tahlil qilish

Tizim bilan ishlaganda Chrome Console da jurnal yozuvlarini kuzating:
1. Muvaffaqiyatli ro'yxatdan o'tish
2. Muvaffaqiyatsiz login urinishlari
3. Bloklash holatlari
4. CSRF token xatoliklari

## Muhim eslatmalar

1. Bu tizim o'rganish uchun yaratilgan va real tizimlarda qo'shimcha xavfsizlik choralari kerak:
   - Kuchliroq shifrlash (bcrypt, Argon2)
   - HTTPS protokoli
   - SQL injection himoyasi
   - XSS himoyasi
   - Session boshqaruvi

2. Real tizimlarda ma'lumotlar localStorage da emas, ma'lumotlar bazasida saqlanadi.

3. Base64 shifrlash o'rniga professional shifrlash algoritmlari ishlatiladi.

## Keyingi qadamlar

Tizimni yanada yaxshilash uchun quyidagi yo'nalishlardan birini tanlashingiz mumkin:

### Xavfsizlikni kuchaytirish
- Ikki faktorli autentifikatsiya qo'shish
- Parolni tiklash funksiyasini qo'shish
- Bloklash vaqtini oshirib boruvchi tizim yaratish
- IP manzillarni kuzatish tizimini qo'shish

### Foydalanuvchi tajribasini yaxshilash
- Parol kuchini ko'rsatuvchi indikator qo'shish
- Xato xabarlarini yaxshilash
- Interfeys dizaynini takomillashtirish
- Ko'proq yordam matni qo'shish

### Monitoring tizimini kuchaytirish
- Shubhali harakatlarni aniqlash
- Administrator uchun boshqaruv paneli yaratish
- Statistika to'plash va tahlil qilish
- Xavfsizlik hisobotlarini yaratish

Bu amaliy mashg'ulotlar orqali siz:
- Xavfsizlik tizimining ishlash printsiplarini
- Turli xil xavfsizlik mexanizmlarini
- Xatolarni aniqlash va tuzatish usullarini
- Tizimni monitoring qilish usullarini o'rganasiz

Har bir mashqni bajarganingizda, nima uchun aynan shunday ishlayotganini tushunishga harakat qiling. Bu bilimlar sizga keyinchalik real tizimlarda xavfsizlikni ta'minlashda juda foydali bo'ladi.