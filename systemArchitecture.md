# <ins>**Kansella's System Architecture**</ins>


## **Overview**
This document presents the technical blueprint of Kansella, a self-counselling mobile application that enables users to track, reflect, and heal from unwanted habits over time. This document is meant for developers and tech-savvy audience.

---

## **Architectural Layout & Components**

### **Diagrams/Schematics**
Below is a high-level system diagram illustrating data flow and component interactions:



**System Diagram:**
[Diagram link to figma](https://www.figma.com/board/rayQpPb7mJm4xO9jaEMvRZ/Kansella-System-Architecture-Diagram?node-id=0-1&t=nVz3Oyt3WoBUVKht-1)

The architecture follows a pretty simple **client-serverless model** where the mobile app directly communicates with Firebase services without a dedicated backend server.

---

### **Component Descriptions**

| Component | Description |
|------------|--------------|
| **Kansella App (Frontend)** | Built with Flutter (and dart), it manages user interactions, daily reflections, and progress tracking. Handles UI rendering and Firebase SDK integrations. |
| **Firebase Authentication** | Provides secure user sign-in using email/password or Google OAuth. Ensures data access isolation per user. |
| **Cloud Firestore (Database)** | NoSQL database storing user profiles, habits, reflections, and progress logs. Offers real-time synchronization and offline persistence. |
| **Cloud Functions** | Handles background operations such as progress computation, weekly summary generation, and notification triggers. |
| **Firebase Cloud Messaging (FCM)** | Manages push notifications and daily check-in reminders. |
| **Firestore Security Rules** | Defines data access permissions and ensures privacy by restricting access to authenticated users only. |

---

### **Interfaces and Interactions**

- **Frontend ↔ Firebase Auth:** User login and account management via Firebase SDK.  
- **Frontend ↔ Firestore:** Real-time CRUD operations for habits and reflections.  
- **Cloud Functions ↔ Firestore:** Event-driven triggers for analytics, summaries, and reminders.  
- **Cloud Functions ↔ FCM:** Sends daily push notifications to users.  
- **Firestore ↔ Frontend:** Bidirectional updates using Firebase’s real-time listener.  

**Protocols and Formats:**
- Communication via HTTPS and WebSockets (for real-time updates).  
- Data exchange in JSON format.  
- API calls secured with Firebase Authentication tokens.

---

### **Dependencies**

- The **frontend** depends on Firebase SDKs for Authentication, Firestore, and Messaging.  
- **Cloud Functions** depend on Firestore events and FCM APIs.  
- **Analytics and Security Rules** depend on structured Firestore data consistency.  
- No external servers are required, reducing operational complexity.

---

## **Technical Specifications & Quality Attributes**

### **Technology Stack**

| Layer | Technology |
|-------|-------------|
| **Frontend** | Flutter (Dart), Provider/Riverpod for state management |
| **Backend (Serverless)** | Firebase Cloud Functions (Node.js/TypeScript) |
| **Database** | Cloud Firestore (NoSQL, document-based) |
| **Authentication** | Firebase Authentication |
| **Notifications** | Firebase Cloud Messaging |
| **Analytics & Monitoring** | Firebase Analytics, Crashlytics |
| **Version Control** | GitHub |
| **Design Tools** | Figma for prototyping |

---

### **Non-Functional Requirements**

| Attribute | Description |
|------------|--------------|
| **Performance** | Firestore’s real-time listeners ensure sub-100ms data sync between client and cloud. |
| **Scalability** | Firebase auto-scales seamlessly with user base growth. |
| **Security** | Firebase Auth with token-based access and Firestore security rules prevent unauthorized access. |
| **Maintainability** | Modular Flutter architecture and Cloud Functions enable quick updates. |
| **Reliability** | Cloud Firestore offers multi-region redundancy and offline data caching. |
| **Availability** | 99.9% uptime SLA from Firebase infrastructure. |

---

### **Data Management**

**Data Model Overview**
- /users/{userId}
    - profile: { name, email, theme }
- /habits/{habitId}
    - habitName: string
    - createdAt: timestamp
- /reflections/{reflectionId}
    - habitId: string
    - date: timestamp
    - didDo: boolean (for each habit, basically)
    - cause: string (just notes)
    - actionPoints: string
    - progress: { streakCount, weeklyReport }

<ins>**NB:**</ins> The data model isn't limited to this, it can be changed. This is just an overview of its structure.


**Data Flow Summary**
1. User inputs reflection -> App writes data to Firestore.  
2. Firestore triggers Cloud Function -> updates streaks/progress.  
3. App listens for changes -> updates local dashboard in real time.  
4. Daily reminders sent via Cloud Messaging if no check-in is detected. Or if it was reminders where activated by the user.

---

## **Why This Approach Is Technically Feasible**

- Kansella’s architecture is lightweight, scalable, and built with proven, serverless technologies.

- Flutter + Firebase enables rapid cross-platform development with minimal setup.

- Serverless backend removes infrastructure maintenance while scaling automatically.

- Firestore provides real-time sync, strong security rules, and offline support.

- Cloud Functions automate background tasks like reminders and analytics.

- Google Cloud infrastructure ensures reliability, uptime, and global performance.

In short, this approach minimizes cost and complexity while providing a solid, secure foundation that can easily grow with user demand.

--- 
## **Summary**

The Kansella architecture is lightweight, scalable, and secure.
By leveraging **Flutter + Firebase**, the app achieves:
- Minimal development overhead  
- Real-time responsiveness  
- Effortless scaling and low maintenance  

This system design supports rapid MVP delivery while maintaining the flexibility to grow into a full-fledged digital wellness platform.

---
**Note for Developers:** The content of ths document focuses mainly on the MVP version. We want it fast, so we're building it lean.

---
