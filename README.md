# 🌪️ Tornado Movement

**An enterprise-grade AI-powered fitness ecosystem that intelligently orchestrates personalized training, real-time coaching, and scalable payment infrastructure across a multi-tenant mobile platform.**

![Flutter](https://img.shields.io/badge/Flutter-3.10+-02569B?logo=flutter&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-10.x-FF2D20?logo=laravel&logoColor=white)
![AI](https://img.shields.io/badge/AI-GPT--4%20%7C%20DeepSeek-10a37f)
![WebSocket](https://img.shields.io/badge/Real--time-WebSocket-black)
![Stripe](https://img.shields.io/badge/Payments-Stripe-635bff?logo=stripe&logoColor=white)

---

## 📖 Overview

Tornado Movement is a sophisticated three-sided marketplace ecosystem designed to revolutionize fitness coaching through intelligent automation and real-time connectivity. The platform seamlessly integrates AI-driven plan generation, WebSocket-based communication, and enterprise payment processing to create a comprehensive fitness management solution.

### Platform Architecture

The system is built on a **microservices-inspired architecture** with three specialized mobile applications communicating with a centralized Laravel backend, supported by real-time WebSocket infrastructure and third-party AI/payment services.

```
┌──────────────────────────────────────────────────────────┐
│                   Mobile Ecosystem                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │
│  │  User App   │  │  Coach App  │  │  Admin App  │      │
│  │  (Flutter)  │  │  (Flutter)  │  │  (Flutter)  │      │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘      │
└─────────┼─────────────────┼─────────────────┼────────────┘
          │                 │                 │
          └─────────────────┴─────────────────┘
                            │
                  ┌─────────▼──────────┐
                  │  Nginx (SSL/WSS)   │
                  └─────────┬──────────┘
                            │
          ┌─────────────────┴─────────────────┐
          │                                   │
    ┌─────▼──────┐                    ┌──────▼──────┐
    │  Laravel   │◄──────────────────►│  WebSocket  │
    │  Backend   │      Broadcast      │  (Reverb)   │
    │  + Cashier │                     │   :6001     │
    └─────┬──────┘                    └─────────────┘
          │
          ▼
    ┌───────────┐      ┌──────────┐      ┌──────────┐
    │   MySQL   │      │ OpenAI   │      │  Stripe  │
    │ Database  │      │  GPT-4   │      │ Connect  │
    └───────────┘      │ DeepSeek │      └──────────┘
                       └──────────┘
```

---

## 🎯 Core Applications

### 1️⃣ User App
**End-user fitness tracking and AI-powered personalization**

- Comprehensive health profiling with body metrics, goals, and progress tracking
- Monthly AI-generated workout and nutrition plans powered by **GPT-4** and **DeepSeek Reasoner**
- Flexible subscription management via **Stripe Subscriptions** (monthly/annual tiers)
- **In-App Purchases (IAP)** integration for iOS App Store and Google Play Store
- Real-time chat (1-on-1 and community) with **optimistic UI** updates
- **Voice messaging** capabilities within conversations (record, send, playback)
- Social feed with posts, likes, comments (WebSocket-powered live updates)
- Direct coach subscription for personalized training services

### 2️⃣ Coach App
**Professional coaching dashboard and revenue management**

- Comprehensive onboarding with CV, certifications, and credentials upload
- **Admin approval workflow** before platform activation
- **Stripe Connect** integration for direct payment collection and automatic payouts
- Client management dashboard with progress tracking and plan customization
- Direct communication channel with subscribers
- Custom pricing control for coaching services (monthly/annual packages)

### 3️⃣ Admin App
**Centralized platform governance and analytics**

- Coach application review and approval/rejection system
- Platform-wide subscription and payment monitoring
- Webhook event tracking and health status
- Content moderation for community posts and interactions
- User management and dispute resolution
- Financial analytics and performance reporting

---

## 🚀 Technical Highlights

### AI Integration
The platform leverages dual AI models for intelligent plan generation:

- **GPT-4 (gpt-o4)**: Primary engine for generating personalized workout routines and nutrition plans based on user health data, goals, and progress history
- **DeepSeek Reasoner**: Advanced reasoning model for complex logical optimization, long-term planning strategies, and adaptive program adjustments

Plans are generated server-side and stored in the database, with background job processing to handle computational overhead.

### Real-Time Infrastructure
Built on **Laravel Echo Server (Reverb)** with **pusher_client_socket** on the Flutter side:

- Private and presence channels for chat rooms
- Broadcast channels for social feed updates (likes, comments)
- **Optimistic UI updates**: Messages appear instantly with local state, then sync with server confirmation
- Automatic reconnection handling and message queue management
- Nginx reverse proxy configuration for HTTPS → WSS upgrade on port 6001

### Payment Architecture
Sophisticated dual-payment system:

**Stripe Subscriptions (User Plans)**
- Managed via **Laravel Cashier** for seamless subscription lifecycle
- Support for trial periods, promo codes, and proration
- Webhook-driven state synchronization (checkout completed, invoice paid, subscription updated/canceled)
- Customer creation flow with stored `stripe_customer_id` in database

**Stripe Connect (Coach Payouts)**
- Express accounts for coaches with onboarding flow
- Direct payment routing to coach accounts
- Platform commission handling through application fees
- Each coach has dedicated product/price IDs in Stripe catalog

**In-App Purchases**
- Native IAP integration for App Store and Google Play
- Product catalog management in both stores
- Receipt validation and entitlement verification

### Data Flow: Subscription Creation
```
1. User selects plan → Client calls backend API
2. Backend creates/retrieves Stripe Customer (stores customer_id)
3. Backend creates Checkout Session with price_id
4. Client presents Stripe Payment Sheet (flutter_stripe)
5. User completes payment in native Stripe UI
6. Stripe fires webhook → checkout.session.completed
7. Laravel Cashier processes webhook → updates DB subscription status
8. Backend broadcasts WebSocket event → Client receives confirmation
9. UI updates to show active subscription
```

---

## 💻 Technology Stack

### Frontend (Mobile - Flutter)

| Package | Purpose | Version |
|---------|---------|---------|
| `provider` | State management architecture | Latest |
| `dio` | HTTP client with interceptors | Latest |
| `pusher_client_socket` | WebSocket client (Reverb/Pusher compatible) | ^0.0.5 |
| `flutter_stripe` | Native Stripe SDK integration | Latest |
| `in_app_purchase` | iOS/Android IAP handling | Latest |
| `shared_preferences` | Local key-value storage | Latest |

### Backend (Laravel)

| Component | Purpose |
|-----------|---------|
| **Laravel 10.x** | RESTful API framework with built-in queue, cache, and event broadcasting |
| **Laravel Cashier** | Stripe subscription management with webhook handling |
| **Laravel Echo/Reverb** | WebSocket server for real-time event broadcasting |
| **MySQL/PostgreSQL** | Relational database for structured data |
| **Redis** | Cache layer + queue backend + session storage |
| **Supervisor** | Process manager for queue workers and WebSocket server |

### Infrastructure & Deployment

| Layer | Technology |
|-------|------------|
| **Hosting** | Hostinger VPS (Ubuntu 22.04 LTS) |
| **Web Server** | **Nginx** (reverse proxy for Laravel + WebSocket) |
| **SSL** | Let's Encrypt (Certbot for auto-renewal) |
| **Process Management** | Supervisor (daemonizes queue workers and Reverb) |
| **Queue System** | Redis-backed Laravel queues for async jobs |

### External APIs

| Service | Integration Point |
|---------|-------------------|
| **OpenAI GPT-4** | POST to `https://api.openai.com/v1/chat/completions` for plan generation |
| **DeepSeek Reasoner** | Advanced reasoning API for optimization logic |
| **Stripe API** | Customer management, subscriptions, Connect accounts, webhooks |
| **Stripe Connect** | Coach payout infrastructure with Express accounts |

---

## 🏗️ Architecture Patterns

### Backend Architecture
- **Repository Pattern**: Clean separation between business logic and data access
- **Service Layer**: Dedicated services for AI generation, payment processing, and real-time events
- **Job Queues**: Background processing for heavy tasks (AI generation, webhook processing, email notifications)
- **Event Broadcasting**: Laravel events automatically broadcast to WebSocket channels

### Frontend Architecture
- **Provider State Management**: Reactive UI updates with scoped providers
- **Repository Pattern**: API abstraction layer for clean data fetching
- **Optimistic Updates**: Local state updates before server confirmation for perceived performance
- **Error Boundary Handling**: Graceful degradation with retry mechanisms

### Real-Time Communication Pattern
```
User Action (e.g., send message)
    ↓
1. Optimistic UI update (message appears instantly with "sending" indicator)
    ↓
2. API call to backend via Dio
    ↓
3. Backend validates → stores in DB → broadcasts event via Reverb
    ↓
4. WebSocket pushes to all connected clients in channel
    ↓
5. Client receives confirmation → updates message status to "sent"
    ↓
6. Other participants receive message via WebSocket listener
```

---

## 🔧 Setup & Configuration

### Prerequisites
- Flutter SDK ≥ 3.10
- PHP ≥ 8.1 with Composer
- MySQL/PostgreSQL ≥ 8.0
- Redis ≥ 6.0
- Node.js ≥ 18.x (for Laravel Mix/Vite)
- Nginx web server
- Supervisor process manager

### Quick Start

#### 1. Backend Setup
```bash
# Clone and install dependencies
git clone <repo-url>
cd backend
composer install

# Environment configuration
cp .env.example .env
php artisan key:generate

# Configure .env with your credentials:
# - Database connection
# - Stripe keys (STRIPE_KEY, STRIPE_SECRET, STRIPE_WEBHOOK_SECRET)
# - OpenAI API key
# - DeepSeek API key
# - Reverb configuration

# Database migration
php artisan migrate --seed

# Install Cashier
composer require laravel/cashier
php artisan vendor:publish --tag="cashier-migrations"
php artisan migrate
```

#### 2. Nginx Configuration
The critical part is configuring Nginx to handle both HTTPS traffic and WebSocket connections:

```nginx
server {
    listen 443 ssl http2;
    server_name yourdomain.com;
    
    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;
    
    root /var/www/tornado/public;
    
    # Standard Laravel routing
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    
    # PHP-FPM for Laravel
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }
    
    # WebSocket upgrade (critical for real-time)
    location /app {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;
        proxy_pass http://127.0.0.1:6001;
    }
}
```

#### 3. Supervisor Setup
```ini
[program:tornado-queue]
command=php /var/www/tornado/artisan queue:work --tries=3
user=www-data
autostart=true
autorestart=true
numprocs=4

[program:tornado-reverb]
command=php /var/www/tornado/artisan reverb:start --host=0.0.0.0 --port=6001
user=www-data
autostart=true
autorestart=true
```

#### 4. Flutter Configuration
```bash
cd mobile
flutter pub get

# Configure lib/config/env.dart
# - API_URL (your Laravel backend)
# - WS_URL (wss://yourdomain.com)
# - STRIPE_PUBLISHABLE_KEY
# - PUSHER_APP_KEY

flutter run
```

---

## 💳 Stripe Integration Guide

### Product Structure
```
User Subscriptions:
├── Product: "Premium Plan"
│   ├── Price: $29.99/month (price_xxxxx)
│   └── Price: $299/year (price_yyyyy)

Coach Services (per coach):
├── Product: "Coach John - Personal Training"
│   ├── Price: $99/month (price_zzzzz)
│   └── Price: $999/year (price_aaaaa)
```

### Webhook Configuration
Dashboard → Developers → Webhooks → Add Endpoint

**URL**: `https://yourdomain.com/stripe/webhook`

**Events to listen**:
- `checkout.session.completed`
- `invoice.paid`
- `customer.subscription.updated`
- `customer.subscription.deleted`

---

## 🔒 Security & Best Practices

- All Stripe secret keys stored server-side only (never in mobile client)
- Webhook signature verification for all Stripe events
- JWT token-based API authentication with refresh mechanism
- Rate limiting on all API endpoints
- HTTPS enforcement for all traffic
- Secure WebSocket connections (WSS protocol)
- Health data encryption at rest (GDPR/HIPAA considerations)

---

## 📊 Performance Optimizations

- **Optimistic UI**: Instant feedback before server confirmation
- **Redis Caching**: Frequent queries cached with TTL
- **Lazy Loading**: Paginated feeds and conversation history
- **Image Optimization**: Compressed uploads with CDN integration
- **Background Jobs**: Heavy AI processing moved to queue workers
- **Connection Pooling**: Persistent database connections

---

## 📝 API Documentation

Comprehensive API documentation available via Postman collection or Swagger UI at `/api/documentation` (when configured).

Key endpoint categories:
- Authentication (`/auth/*`)
- User management (`/users/*`)
- Subscriptions (`/subscriptions/*`)
- AI plans (`/plans/*`)
- Chat & messaging (`/chat/*`)
- Social feed (`/feed/*`)
- Coach services (`/coaches/*`)
- Admin operations (`/admin/*`)

---

## 🤝 Contributing

This project was developed as a **professional client engagement** showcasing enterprise-level Flutter development, AI integration, and scalable payment infrastructure.

---

## 📄 License

Proprietary - All rights reserved

---

## 📞 Support

For technical inquiries or integration support, please contact the development team.

---

**Built with ❤️ using Flutter, Laravel, and cutting-edge AI technology**