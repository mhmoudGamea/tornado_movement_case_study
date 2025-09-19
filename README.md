# Tornado Movement App

![App Preview](demo_image/tornado_movement.png)

A **fitness and health application suite** consisting of three apps (User, Coach, Admin) built with Flutter.  
The system delivers personalized AI-powered workout & nutrition plans, real-time community features, and integrated payment flows.

## Overview
Tornado Movement App connects users with coaches and admins through an ecosystem that promotes health, fitness, and accountability.  
The platform leverages **ChatGPT (gpt-o4)** for generating monthly workout and nutrition plans tailored to each user’s progress and health data.

## Applications

### User App
- Fill detailed health forms & progress updates
- Receive **AI-generated (gpt-o4)** workout and nutrition plans monthly
- **Stripe Subscriptions** for recurring payments
- Real-time **chat** (one-to-one & community) with optimistic UI updates
- Social feed: post likes, comments (socket-based)
- Voice messages support in chat
- User have the ability to subscribe with a coach
- Built using **pusher_client_socket: ^0.0.5**

### Coach App
- Submit registration form with CV (certifications, skills, experience)
- Wait for **admin approval** before onboarding
- **Stripe Connect** integration for direct consultation payments
- Manage users who subscribe directly to their services

### Admin App
- Oversee and manage all users & coaches
- Approve/reject coach applications
- Monitor subscriptions, payments, and community activity

## Key Features
- **AI Integration**: Personalized GPT-powered plans  
- **Real-Time Communication**: WebSocket-based chat & community  
- **Payments**: Stripe Subscriptions + Stripe Connect  
- **Social Features**: Posts, comments, likes  
- **Voice Notes in Chat**  
- **Multi-App Ecosystem**: Separate flows for users, coaches, and admins  

## Tech Stack
| Category | Technology |
|----------|------------|
| **Framework** | Flutter (Dart) |
| **State Management** | Provider |
| **AI Integration** | GPT-o4 (OpenAI API) |
| **Networking** | Dio + WebSocket (pusher_client_socket) |
| **Payments** | Stripe Subscriptions & Stripe Connect |
| **Storage** | SharedPreferences |
| **Architecture** | Clean Architecture + Repository Pattern |

---

**Note**: This project was developed as a **client project** on Khamsat, showcasing advanced Flutter development, AI integration, and scalable multi-application architecture.
