---

# Messaging Module

This module implements a **company-volunteer messaging system** using **Vue 3, Pinia, and Axios**.
It handles **conversations, recipients, sending and receiving messages**

---

## Features

- Fetch and display valid message recipients.
- Fetch and display all conversations.
- Fetch and display messages for a given conversation.
- Send messages in real-time to other users.
- Sidebar for message previews with **search and filtering**.
- Chat interface with **message bubbles (sender/receiver styles)**.
- Profile image support for companies.
- Delete and mark conversations as unread.

## Store: `companyMessageStore.ts`

The **Pinia store** manages state and API calls.

### State

```ts
validRecipients: []; // list of people you can message
conversations: []; // list of conversations
messages: []; // messages inside conversations
error: ""; // API errors
```

### Actions

- `fetchValidRecipients()` → Get list of messageable users.
- `fetchConversations()` → Get all conversations for the logged-in user.
- `fetchMessages()` → Get all messages.
- `sendMessages(formData)` → Send a message to a user.

Each request attaches the `Authorization: Bearer <token>` header from the `authStore`.

---

## Component: `CompanyMessages.vue`

The **sidebar + main chat container wrapper**.

- Fetches conversations, messages, recipients, and companies on **mount**.
- Implements **search** to filter recipients.
- Automatically redirects to the **first conversation** if one exists.
- Renders:

  - **Sidebar** with search and message previews.
  - **Main chat area** (`RouterView`) that loads conversation.
  - **Loading overlay** when fetching data.

---

## Component: `CompanyConversation.vue`

The **chat window** for a selected conversation.

- Filters messages between the logged-in user and the current chat partner.
- Uses **`MessageBubble1` (left)** for received messages and **`MessageBubble2` (right)** for sent messages.
- Watches for route changes to reload messages when switching conversations.
- Handles **sending messages**:

  - Resolves the correct receiver ID.
  - Posts message via `sendMessages()`.
  - Refetches messages and scrolls to bottom.

---

## Component: `CompanyMessagePreview.vue`

A **message preview card** in the sidebar.

- Displays **owner name**, **profile image**, and **kebab menu** with actions.
- Provides quick actions:

  - **View Profile**
  - **Mark as Unread**
  - **Delete Conversation (with confirmation modal)**

- Uses Vue Router to navigate into a specific conversation.

---

## UI Kit Components

### `MessageBubble1.vue`

For **incoming messages** (left-aligned, grey background).

### `MessageBubble2.vue`

For **outgoing messages** (right-aligned, colored background).

---

## API Endpoints

| Action               | Endpoint                       | Method |
| -------------------- | ------------------------------ | ------ |
| Get valid recipients | `/messaging/messageable-users` | GET    |
| Get conversations    | `/messaging/conversations`     | GET    |
| Get messages         | `/messaging/messages`          | GET    |
| Send a message       | `/messaging/send`              | POST   |

---

## Usage Flow

1. **On page load**:

   - Fetch conversations, messages, valid recipients, and companies.
   - If conversations exist → redirect to the first one.

2. **Sidebar**:

   - Shows a searchable list of conversations.
   - Click recipient → opens conversation in main view.

3. **Conversation window**:

   - Displays messages in bubble format.
   - Allows sending new messages with **textarea + send button**.

4. **Kebab menu**:

   - Manage conversations (view profile, mark unread, delete).



## Next Steps
- Implement messaging functionality for **SuperAdmin**
- Handle permissions
- Add caching
- Add **real-time updates** with WebSockets (e.g., Laravel Echo + Pusher).
- Add **unread count badges** in sidebar.

---
