# NVX Go Helper

**nvx-go-helper** adalah koleksi utility functions _production-grade_ untuk mempercepat pengembangan backend service di lingkungan Go (Golang). Library ini dirancang untuk:
- **Zero dependencies** (sebisa mungkin).
- **High performance** (optimized for speed & allocs).
- **Opinionated** tapi fleksibel (standar enterprise Indonesia 2025).

## 📦 Instalasi

```bash
go get github.com/Jkenyut/nvx-go-helper
```

## ✨ Fitur Utama

### 1. Crypto (`/crypto`)
Enkripsi AES-256-GCM yang sangat cepat dan aman. Wrapper sederhana di atas `crypto/cipher` standar.

```go
import "github.com/Jkenyut/nvx-go-helper/crypto"

// Init (panggil sekali saat startup)
enc, err := crypto.NewAESGCM("32-byte-secret-key-must-be-exact!")

// Encrypt Any Struct -> URL specific Base64
token, _ := enc.Encrypt(map[string]string{"user_id": "123"})

// Decrypt
var data map[string]string
err := enc.Decrypt(token, &data)
```

### 2. Format (`/format`)
Helper untuk string, angka, dan format bank lokal (Indonesia).

```go
import "github.com/Jkenyut/nvx-go-helper/format"

// Format Rupiah
fmt.Println(format.Rupiah(150000)) // "150.000,00"

// Format Norek BRI
fmt.Println(format.BRINorek("123456789012345")) // "1234-56-789012-34-5"

// Title Case (Smart)
fmt.Println(format.Title("admin-role")) // "Admin-Role"

// Safe String (untuk filename/key)
fmt.Println(format.ToSafeString("User Name / 123")) // "User_Name___123"
```

### 3. Response (`/model`)
Standarisasi respons JSON API (`{ meta, data }`). Otomatis menangani `request_id` dari context.

```go
import "github.com/Jkenyut/nvx-go-helper/model"

func CreateUser(c *gin.Context) {
    // ... logic ...
    
    // Returns 201 Created
    c.JSON(201, response.Created(c.Request.Context(), "user created", user))
}

func GetUser(c *gin.Context) {
    // Returns 404 Not Found
    c.JSON(404, response.NotFound(c.Request.Context(), "user not found"))
}
```

### 4. Pagination (`/pagination`)
Helper untuk menangani pagination query param dan link header (RFC 5988).

```go
import "github.com/Jkenyut/nvx-go-helper/pagination"

// Parse dari query param
p := pagination.New("1", "10", 100) // page, limit, total

// Pakai di query DB
db.Limit(p.Limit).Offset(p.Offset()).Find(&users)

// Generate response info
c.JSON(200, gin.H{
    "data": users,
    "pagination": p,
})
```

## 🤝 Kontribusi

Pull requests dipersilakan. Untuk perubahan besar, harap buka issue terlebih dahulu untuk mendiskusikan apa yang ingin Anda ubah.

1. Fork project ini
2. Buat feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit perubahan Anda (`git commit -m 'Add some AmazingFeature'`)
4. Push ke branch (`git push origin feature/AmazingFeature`)
5. Buka Pull Request

## 📄 Lisensi

[MIT](https://choosealicense.com/licenses/mit/)
