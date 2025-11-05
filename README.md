# Tornado Movement

![App Preview](demo_image/tornado_movement.png)

A comprehensive AI-driven fitness ecosystem connecting users, coaches, and admins through smart training, nutrition, and community engagement.

## Overview
The Tornado Movement ecosystem is built to merge AI intelligence with human coaching expertise.
It automates the process of generating fitness and nutrition plans while maintaining real-time engagement between users and their coaches through chat and community features.

## Applications

### User App
The User App empowers trainees to take full control of their fitness journey through a seamless, AI-assisted experience
it combines personalized plans, real-time communication, and social engagement — all built with a scalable Flutter architecture.

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
The Coach App provides personal trainers with the tools to manage their clients.

- Coach Registration & Verification — each coach submits a profile with certifications, experience, and specialization.
  The Admin manually reviews and approves each application before the coach can go live.
- Subscription Management (Stripe Dashboard) — users subscribe to coaches’ plans via the User App.
  then the profits of the coach appear in his profile After deducting the platform's profits.
- Earnings System (30-Day Holding Period) To ensure service quality and prevent fraud, when a user subscribes:
  - The payment first appears in the Pending Balance.
  - After 30 days, if no dispute or issue is reported, it moves to Withdrawable Earnings.
  - The coach can submit a withdrawal request, and we will transfer the money via bank transfer.
  

### Admin App
The Admin App serves as the central hub for managing the entire Tornado Movement ecosystem.
It provides full visibility and control over users, coaches, subscriptions, payments, and revenue distribution, ensuring transparency and system stability.

- Coach Application Review & Approval — admins review registration requests submitted by coaches, including their CVs,  
  certificates, and experience.
- Admins have complete visibility over each coach’s financial activity, including pending and available balances.
- All data is synchronized automatically through Stripe and Laravel Cashier.
- The Admin dashboard have the next for each coach:
  - Total Balance — represents the coach’s overall earnings After deducting the platform's profits.
  - Pending Balance — the coach profit's goe there for 30 day's since the user subscription.
  - Withdrawable Earnings — funds cleared after the hold period and available for instant payout.
  - Platform Fee Tracking — the system automatically calculates and deducts a predefined platform commission from each transaction.
  - This value is displayed transparently in the Admin dashboard for auditing and revenue monitoring.

## Tech Stack

| Category | Technology |
|-----------|-------------|
| **Framework** | Flutter (Dart) |
| **Backend** | Laravel 11 (PHP 8.x) |
| **Payments** | Stripe API + Laravel Cashier + flutter_stripe |
| **AI Integration** | GPT-o4 + DeepSeek Reasoner |
| **State Management** | Provider |
| **Real-Time Communication** | Pusher + pusher_client_socket |
| **In-App Purchases** | in_app_purchase (Play Store & App Store) |
| **Architecture** | Clean Architecture + Repository Pattern |
| **Hosting** | Ubuntu VPS (Nginx + Hostinger) |
| **Database** | MySQL |

## Payment Flow (Stripe + Laravel Cashier)

1. The user subscribes through the **User App** using `flutter_stripe`.  
2. The backend (Laravel) creates a **Stripe Customer** and links it to the user.  
3. Each coach has a unique **Stripe Connect account**.  
4. Subscriptions are managed via **Laravel Cashier**, which handles:
   - Creating products and price IDs.
   - Generating payment intents.
   - Listening to Stripe **webhooks** for events like:
     - `invoice.payment_succeeded`
     - `customer.subscription.updated`
     - `invoice.payment_failed`
5. Once a payment is confirmed:
   - The system moves funds to **Pending Balance**.
   - After 30 days, the funds become **Withdrawable Earnings**.
6. Admin dashboard displays both coach revenue and platform fees in real time.

## AI Integration

Tornado Movement leverages **GPT-o4** and **DeepSeek Reasoner** to generate personalized workout and nutrition plans.  
The AI considers:
- User health data (weight, goals, injuries)
- Previous progress
- Dietary preferences The generated plans are reviewed and displayed automatically each month through the backend API.

## Real-Time Communication

All real-time interactions (chat, posts, likes, and comments) are powered by **Pusher Channels**.  
Using the `pusher_client_socket` package in Flutter, the app listens to server-side events emitted from Laravel through:
- `post.like`
- `post.unLike` 
- `post.comment`
- `message.received`
- `audio.received`
This ensures instant updates and optimistic UI feedback.

## Project Architecture

Each app (User, Coach, Admin) follows **Clean Architecture** principles:
- **Presentation Layer** — Flutter UI & Provider state management.  
- **Domain Layer** — business logic and models.  
- **Data Layer** — API calls via Dio, connected to Laravel backend.
This modular structure ensures scalability and easy maintenance across all three apps.

## License
This project was developed as a **client commission on Khamsat**, demonstrating advanced Flutter engineering,  
AI integration, and scalable multi-application architecture.

© 2025 Tornado Movement Team — All Rights Reserved.