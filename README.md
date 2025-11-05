# Tornado Movement

![App Preview](demo_image/tornado_movement.png)

A comprehensive AI-driven fitness ecosystem connecting users, coaches, and admins through smart training, nutrition, and community engagement.

## Overview
The Tornado Movement ecosystem is built to merge AI intelligence with human coaching expertise.
It automates the process of generating fitness and nutrition plans while maintaining real-time engagement between users and their coaches through chat and community features.

## Applications

### User App
The User App empowers trainees to take full control of their fitness journey through a seamless, AI-assisted experience.
It combines personalized plans, real-time communication, and social engagement — all built with a scalable Flutter architecture.

- Detailed Health Profile — users fill in weight, measurements, body fat, injuries, and fitness goals.
  This data feeds directly into **Deepseek Resoner & GPT-o4** model for generating tailored plans.
- AI-Generated Plans — monthly workout and nutrition plans generated automatically based on    
  user progress and feedback.
- Subscription System using **flutter_stripe** in client & **Laravel Cashier** in backend users can subscribe to a coach’s  
  monthly plan.
- Coach subscription is fully managed via Stripe Subscriptions, fully synced with the Laravel backend using Cashier for  
  webhooks and event handling.
- In-App Purchases (App Store & Google Play) — integrated using the **in_app_purchase** package for handling platform-native 
  payments and digital product catalogs.
- Real-Time Chat (1-to-1 & Community) — powered by **pusher_client_socket**, enabling live communication with optimistic UI 
  updates and instant message delivery.
- Voice record users can send a voice notes, images, and text inside the chat interface.
- Social Feed & Interactions — includes posts, likes, and comments updated instantly via WebSocket events.
  Built to enhance community engagement within the app ecosystem.

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
