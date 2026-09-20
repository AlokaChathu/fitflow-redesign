# FitFlow High-Level Architecture

![FitFlow High-Level Architecture](fitflow-architecture.svg)

## Main Components

### React Native Mobile Application
Provides onboarding, AI Daily Flow recommendations, workout tracking, nutrition logging, social
challenges, notifications, dashboards, and account/privacy controls.

### React Web Application
Provides browser access to compatible FitFlow features while sharing TypeScript models and service
logic with the mobile ecosystem where practical.

### NestJS Backend
Handles business rules, authorization, workout management, user profiles, progress calculations,
nutrition records, subscriptions, integration orchestration, and administrative APIs.

### FastAPI AI Microservice
Handles recommendation algorithms, AI model inference, and computer-vision-related nutrition
functions. It scales independently from normal API traffic.

### PostgreSQL
Stores authoritative structured data such as users, workout programs, exercises, completed sessions,
nutrition records, goals, subscriptions, and consent-related information.

### Firebase Real-Time Layer
Supports live social interactions, community feeds, challenges, activity updates, selected
notifications, and synchronization.

### Redis
Caches frequently requested data such as recommendation summaries and common workout templates.

### Object Storage
Stores meal photographs and social media assets securely outside the relational database.

## Critical Data Flows

### Personalized Workout Plan
1. User requests a personalized plan from the mobile app.
2. NestJS verifies authenticated user context.
3. NestJS loads profile, goals, and history from PostgreSQL.
4. Required data is sent to the FastAPI AI service.
5. The AI service returns a recommendation.
6. NestJS validates/stores the plan and returns it to the application.
7. Selected personalization may use TensorFlow Lite on-device.

### Social Sharing
1. User completes an activity or joins a challenge.
2. NestJS validates the request and stores permanent metadata.
3. Firebase publishes the real-time community update.
4. Authorized friends or group members receive the update.

### Nutrition Tracking
1. User captures a food image.
2. The app uploads it securely to object storage.
3. Backend provides a protected reference to the AI service.
4. AI service estimates the food/category.
5. User reviews/corrects the result.
6. Confirmed nutrition data is stored in PostgreSQL.

## Scalability

- Keep NestJS services stateless where practical.
- Use PostgreSQL indexing, pooling, backups, and read scaling as needed.
- Use Redis to reduce repeated expensive queries.
- Scale FastAPI independently.
- Use Firebase managed services for selected live events.
- Serve media from object storage/CDN infrastructure.

## Security

- Firebase Authentication verifies user identity.
- NestJS performs authorization on protected backend requests.
- Use HTTPS/TLS for external communication.
- Never embed database credentials or secrets in client apps.
- Encrypt sensitive data at rest and use least-privilege access.
- Apply validation, sanitization, rate limiting, and audit logging.
- Give the AI service only the data required for the prediction.
