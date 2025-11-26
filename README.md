# Blogger Comment System

Firebase tabanlı modern yorum sistemi - Blogger temaları için.

## 🚀 Kurulum

### 1. Firebase SDK'yı Ekle

Tema XML dosyanda `</head>` etiketinden önce:

```xml
<script src='https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js'/>
<script src='https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js'/>
<script src='https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js'/>
```

### 2. Yorum Container'ı Ekle

Post içeriğinin gösterildiği yerde:

```xml
<b:if cond='data:view.isPost'>
  <div id='blogger-comments-container' style='margin-top: 30px;'/>
</b:if>
```

### 3. Bundle ve Init Kodunu Ekle

`</body>` etiketinden önce:

```xml
<b:if cond='data:view.isPost'>
<!-- Comment System Bundle -->
<script src='https://cdn.jsdelivr.net/gh/KULLANICI/REPO@main/blogger-comments.min.js'></script>

<!-- Initialize -->
<script>
//<![CDATA[
(function() {
  var firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    projectId: "YOUR_PROJECT",
    storageBucket: "YOUR_PROJECT.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
  };

  function init() {
    if (typeof window.BloggerComments === 'undefined') return;
    var container = document.getElementById('blogger-comments-container');
    if (!container) return;
    
    var widget = new window.BloggerComments(container, {
      firebaseConfig: firebaseConfig,
      postId: window.location.pathname,
      postTitle: document.title,
      adminEmails: ['admin@example.com']
    });
    widget.init();
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init);
  } else {
    setTimeout(init, 100);
  }
})();
//]]>
</script>
</b:if>
```

## 📝 Firebase Kurulumu

1. https://console.firebase.google.com adresine git
2. Yeni proje oluştur
3. Firestore Database aktifleştir
4. Authentication > Email/Password aktifleştir
5. Project Settings > Web app ekle > Config bilgilerini al

## 🔒 Firestore Kuralları

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /comments/{commentId} {
      allow read: if true;
      allow create: if request.auth != null;
      allow update, delete: if request.auth != null && 
        request.auth.uid == resource.data.userId;
    }
    match /reactions/{reactionId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

## 📦 Dosyalar

- `blogger-comments.min.js` - Ana bundle (360KB)

## 🎨 Özellikler

- ✅ Dark tema
- ✅ Emoji reaksiyonları
- ✅ Nested yanıtlar (3 seviye)
- ✅ Like/Dislike
- ✅ Admin badge
- ✅ Mobil uyumlu
- ✅ Karakter sayacı
