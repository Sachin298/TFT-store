# TFT Store - Backend API

## Setup Instructions

### 1. Install dependencies
```bash
npm install
```

### 2. Setup .env file
```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/tftstore
JWT_SECRET=tft_store_super_secret_2026
RAZORPAY_KEY_ID=rzp_test_YOUR_KEY_HERE
RAZORPAY_KEY_SECRET=YOUR_SECRET_HERE
FRONTEND_URL=http://localhost:3000
```

### 3. Start MongoDB
```bash
mongod
```

### 4. Run server
```bash
npm run dev   # development
npm start     # production
```

---

## API Endpoints

### Auth
| Method | URL | Description |
|--------|-----|-------------|
| POST | /api/auth/register | Register buyer/seller |
| POST | /api/auth/login | Login |
| GET  | /api/auth/me | Get current user |
| POST | /api/auth/add-address | Add delivery address |
| POST | /api/auth/wishlist/:id | Toggle wishlist |

### Products
| Method | URL | Description |
|--------|-----|-------------|
| GET  | /api/products | List all products (with filters) |
| GET  | /api/products/:id | Single product |
| POST | /api/products | Add product (seller only) |
| PUT  | /api/products/:id | Edit product (seller only) |
| DELETE | /api/products/:id | Remove product (seller only) |

### Orders
| Method | URL | Description |
|--------|-----|-------------|
| POST | /api/orders | Place order |
| GET  | /api/orders/my | My orders (buyer) |
| GET  | /api/orders/:id | Order detail |
| PUT  | /api/orders/:id/status | Update status (seller) |

### Payment (Razorpay)
| Method | URL | Description |
|--------|-----|-------------|
| POST | /api/payment/create-confirmation-fee | Create ₹12 order |
| POST | /api/payment/verify-confirmation-fee | Verify ₹12 payment |
| POST | /api/payment/create-product-payment | Create product payment |
| POST | /api/payment/verify-product-payment | Verify & split payment |
| POST | /api/payment/webhook | Razorpay webhook |

### Seller Dashboard
| Method | URL | Description |
|--------|-----|-------------|
| GET | /api/seller/dashboard | Stats overview |
| GET | /api/seller/orders | All orders |
| GET | /api/seller/products | My products |
| GET | /api/seller/earnings | Earnings breakdown |

---

## Payment Flow

1. Buyer places order → POST /api/orders
2. ₹12 confirmation fee → create-confirmation-fee → verify-confirmation-fee
3. Order status: pending → confirmed
4. Product payment → create-product-payment → verify-product-payment
5. Auto split: 4% TFT, 96% Seller
6. Order status: confirmed → packed → shipped → delivered
