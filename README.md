# AWS Serverless API (Go + Lambda + API Gateway + DynamoDB)

Bu repository, **Go (Golang)** kullanılarak geliştirilmiş, **AWS Lambda** üzerinde çalışan, **API Gateway** üzerinden erişilen ve **DynamoDB** ile veri saklayan bir **serverless backend API** örneğidir.

Proje, modern AWS serverless mimarisini öğrenmek ve production’a yakın bir yapı kurmak amacıyla hazırlanmıştır.

---

## 🏗️ Mimari

Aşağıdaki AWS servisleri kullanılmaktadır:

- **AWS Lambda** – Go ile yazılmış backend fonksiyonları
- **API Gateway** – HTTP endpoint’ler
- **DynamoDB** – NoSQL veri tabanı
- **IAM** – Güvenli yetkilendirme
- **CloudWatch Logs** – Loglama ve hata ayıklama



Client → API Gateway → Lambda (Go) → DynamoDB

---

## 📁 Proje Yapısı

````
.
├── cmd/
│   └── main.go          # Lambda handler (Go)
├── .github/
│   └── workflows/       # GitHub Actions (CI/CD)
|
├── go.mod
├── go.sum
└── README.md

````

---

## 🚀 Çalışma Prensibi

- API Gateway HTTP isteğini alır
- İstek Lambda fonksiyonuna yönlendirilir
- Lambda fonksiyonu Go ile yazılmış handler’ı çalıştırır
- DynamoDB üzerinden veri okuma/yazma işlemleri yapılır
- Sonuç JSON response olarak API Gateway üzerinden döndürülür

---

## 🧩 Kullanılan Teknolojiler

- **Go 1.20+**
- **AWS Lambda (Custom Runtime - provided.al2)**
- **AWS API Gateway (HTTP / REST API)**
- **AWS DynamoDB**
- **GitHub Actions (CI/CD)**

---

## 🔨 Build & Deploy (Manuel)

Lambda için binary build:

```bash
GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o bootstrap
chmod +x bootstrap
zip function.zip bootstrap
````

Lambda ayarları:

* **Runtime:** `provided.al2`
* **Handler:** main
* **Architecture:** `x86_64`

---

## 🔐 IAM Gereksinimleri

Lambda execution role aşağıdaki DynamoDB izinlerine sahip olmalıdır:

```json
{
  "Effect": "Allow",
  "Action": [
    "dynamodb:GetItem",
    "dynamodb:PutItem",
    "dynamodb:UpdateItem",
    "dynamodb:DeleteItem",
    "dynamodb:Scan",
    "dynamodb:Query"
  ],
  "Resource": "arn:aws:dynamodb:*:*:table/*"
}
```

---

## 🧪 Loglama & Debug

* Lambda logları **CloudWatch Logs** altında tutulur
* API Gateway logları Stage ayarlarından aktif edilmelidir
* Hata durumlarında API Gateway `500 Internal Server Error` döner, detaylar CloudWatch’ta görülür

---

## 🎯 Amaç

Bu proje aşağıdaki konularda referans olması için hazırlanmıştır:

* Go ile AWS Lambda geliştirme
* API Gateway entegrasyonu
* DynamoDB CRUD işlemleri
* Serverless mimari
* AWS production ortamına uygun build süreci

---

## 📌 Notlar

* Lambda binary **mutlaka static build edilmelidir** (`CGO_ENABLED=0`)
* `bootstrap` dosyası zip’in **root** dizininde olmalıdır
* Amazon Linux 2 glibc uyumluluğuna dikkat edilmelidir

---

## 📄 Lisans

MIT License

---

## 👤 Author

**Ramazan Tüfekçi**

---

