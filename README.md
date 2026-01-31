# Pars Chat

> **WARNING - CRITICAL INFRASTRUCTURE**  
> This repository is designed to facilitate communication for Iranian citizens during protests and internet shutdowns. It operates on domestic servers to maintain connectivity when external internet access is restricted or censored. Use responsibly and ensure secure deployment in sensitive environments.

A feature-rich real-time chat application with a Telegram-inspired dark UI, built with Go and Server-Sent Events for secure, reliable communication.

---

## ✨ Features

### 🔐 Authentication and Security
* **JWT Authentication** - Secure token-based authentication with 24-hour expiry
* **User Registration and Login** - Complete authentication system
* **Password Hashing** - Using SHA256 with salt
* **Secure Sessions** - Token management with localStorage
* **End-to-End Encryption** - RSA-OAEP encryption for private messages

### 💬 Messaging
* **Private Chat** - One-on-one secure messaging
* **Group Chats** - Create and manage group conversations
* **File Sharing** - Send images, videos, and audio files
* **Location Sharing** - Share real-time GPS coordinates with Google Maps integration
* **Voice Messages** - Record and send audio messages
* **Typing Indicator** - Real-time typing status
* **Read Receipts** - Double checkmark (✓✓) when messages are read
* **Message Reactions** - React to messages with emojis (24+ emojis)

### 🎨 User Interface
* **Telegram-inspired Dark Theme** - Professional dark mode design
* **Modern UI/UX** - Clean and intuitive interface
* **Responsive Design** - Works seamlessly on desktop and mobile
* **Media Preview** - View images and videos in full-screen modal
* **Smooth Animations** - Professional transitions and effects
* **Online Status Indicators** - Real-time user presence
* **Message Filtering** - Filter by media type (images, videos, audio)

### 🔧 Technical
* **SQLite Database** - Lightweight and reliable data storage
* **Server-Sent Events (SSE)** - Real-time bidirectional communication
* **RESTful API** - Clean and modern API architecture
* **Docker Ready** - Containerized deployment
* **Offline Capability** - Works on local networks without internet
* **Real-time Notifications** - Instant updates for reactions, read receipts, and messages

---

## 📋 Recent Updates

### v2.0.0 - Major Release
* ✅ JWT authentication system implemented
* ✅ Location sharing with GPS coordinates
* ✅ Enhanced read receipts with real-time notifications
* ✅ Complete UI redesign with dark theme
* ✅ Real-time emoji reactions
* ✅ Improved message delivery system
* ✅ Better error handling and logging

---

## 📁 Project Structure

```
chatApp/
├── cmd/
│   └── server/
│       └── main.go        # Main server with JWT, SSE and API
├── public/
│   ├── index.html         # Web UI with dark theme
│   └── app.js             # Frontend logic with JWT handling
├── uploads/               # Uploaded media files
├── assets/                # Static assets
├── chat.db                # SQLite database
├── Dockerfile             # Docker image configuration
├── docker-compose.yml     # Service orchestration
├── go.mod                 # Go dependencies
├── go.sum                 # Dependency checksums
├── LOCATION_SHARING.md    # Location feature documentation
├── E2E_ENCRYPTION.md      # Encryption documentation
├── EMOJI_REACTIONS_FEATURE.md  # Reactions documentation
└── README.md              # This file
```

---

## 🚀 Quick Start

### Prerequisites

* Go 1.21 or later
* Docker and Docker Compose (optional)
* Modern web browser with JavaScript enabled

---

## 💻 Running Locally

### 1. Install Dependencies

```bash
go mod download
```

### 2. Run Server

```bash
go run cmd/server/main.go
```

Server runs on `http://localhost:8080`

### 3. Open in Browser

Go to `http://localhost:8080` and:
1. Register a new account or login
2. Search for other users
3. Start chatting with end-to-end encryption!

---

## 🐳 Running with Docker

### Build and Run

```bash
docker-compose up --build
```

### Access the Application

Go to `http://localhost:8080`

### Stop Services

```bash
docker-compose down
```

---

## 📖 Usage Guide

### Registration and Login

1. Open the application at `http://localhost:8080`
2. Click "Register" to create a new account
3. Enter username, full name, and password
4. After registration, login to receive your JWT token
5. Token is automatically stored and used for authenticated requests

### Starting a Private Chat

1. Click the 💬 button (New Chat)
2. Select a user from the list
3. Start sending messages with end-to-end encryption!
4. Messages show encryption indicator 🔒 in private chats

### Creating a Group

1. Click the 👥 button (New Group)
2. Enter group name
3. Select members from the list
4. Click "Create Group"

### Sharing Location

1. Click the 📍 button in the input area
2. Allow browser location access
3. Your location is shared as a Google Maps link
4. Recipients can click to view location

### Sending Media

1. Click the 📎 attachment button
2. Select image, video, or audio file
3. File is automatically uploaded and sent
4. Click on media to view in full-screen

### Adding Reactions

1. Hover over any message
2. Click the 😊 reaction button
3. Select an emoji from 24+ available options
4. Reactions appear in real-time for all users

### Additional Features

* **Search**: Use the search box to find contacts quickly
* **Typing Indicator**: See when someone is typing
* **Online Status**: Green dot for online, gray for offline
* **Read Receipts**: ✓ sent, ✓✓ delivered and read (blue)
* **Voice Messages**: Hold 🎤 button to record
* **Block Users**: Prevent unwanted messages

---

## 🔌 API Endpoints

### Authentication

* `POST /api/register` - Register new user
  ```json
  {
    "username": "string",
    "fullName": "string",
    "password": "string"
  }
  ```

* `POST /api/login` - User login (returns JWT token)
  ```json
  {
    "username": "string",
    "password": "string"
  }
  ```
  Response includes `token` field for JWT authentication

### Users and Groups

* `GET /api/users` - Get user list
* `GET /api/groups?userId={id}` - Get user's groups
* `POST /api/groups` - Create new group
* `GET /api/group/members?groupId={id}` - Get group members
* `POST /api/group/leave` - Leave group
* `POST /api/group/remove` - Remove member from group

### Messages

* `GET /api/messages?userId={id}&contactId={id}` - Get private messages
* `GET /api/messages?userId={id}&groupId={id}` - Get group messages
* `POST /api/send` - Send message (supports location with latitude/longitude)
* `POST /api/messages/read` - Mark message as read
* `GET /events?userId={id}` - SSE connection for real-time updates

### Reactions

* `POST /api/reactions/add` - Add emoji reaction
* `POST /api/reactions/remove` - Remove reaction
* `GET /api/reactions?messageId={id}` - Get message reactions

### Media

* `POST /api/upload` - Upload file
* `GET /uploads/{filename}` - Get uploaded file

### User Management

* `POST /api/block` - Block user
* `POST /api/unblock` - Unblock user
* `GET /api/blocked?userId={id}` - Get blocked users

### Encryption

* `POST /api/keys/save` - Save public key
* `GET /api/keys/get?userId={id}` - Get user's public key

---

## 🗄️ Database Schema

### users table
```sql
- id (INTEGER PRIMARY KEY)
- username (TEXT UNIQUE)
- full_name (TEXT)
- password (TEXT - SHA256 hash with salt)
- created_at (DATETIME)
```

### groups table
```sql
- id (INTEGER PRIMARY KEY)
- name (TEXT)
- creator_id (INTEGER)
- created_at (DATETIME)
```

### group_members table
```sql
- group_id (INTEGER)
- user_id (INTEGER)
- joined_at (DATETIME)
- PRIMARY KEY(group_id, user_id)
```

### messages table
```sql
- id (INTEGER PRIMARY KEY)
- from_user (INTEGER)
- to_user (INTEGER, nullable)
- group_id (INTEGER, nullable)
- content (TEXT, nullable)
- media_url (TEXT, nullable)
- media_type (TEXT, nullable)
- latitude (REAL, nullable)    -- New: Location support
- longitude (REAL, nullable)   -- New: Location support
- timestamp (DATETIME)
```

### message_reads table
```sql
- message_id (INTEGER)
- user_id (INTEGER)
- read_at (DATETIME)
- PRIMARY KEY(message_id, user_id)
```

### message_reactions table
```sql
- id (INTEGER PRIMARY KEY)
- message_id (INTEGER)
- user_id (INTEGER)
- emoji (TEXT)
- created_at (DATETIME)
- UNIQUE(message_id, user_id, emoji)
```

### blocked_users table
```sql
- blocker_id (INTEGER)
- blocked_id (INTEGER)
- blocked_at (DATETIME)
- PRIMARY KEY(blocker_id, blocked_id)
```

### user_keys table
```sql
- user_id (INTEGER PRIMARY KEY)
- public_key (TEXT)
- created_at (DATETIME)
- updated_at (DATETIME)
```

---

## 🚢 Production Deployment

### Environment Variables

```bash
PORT=8080                          # Server port
JWT_SECRET=your-secret-key-here    # JWT signing key (change in production!)
```

### Security Best Practices

1. **Use HTTPS** - Deploy behind reverse proxy (nginx/caddy) with SSL/TLS
2. **Strong JWT Secret** - Use cryptographically secure random string
3. **Rate Limiting** - Implement API rate limiting
4. **Regular Backups** - Backup SQLite database regularly
5. **Input Validation** - Server-side validation for all inputs
6. **Update Dependencies** - Keep Go modules up to date
7. **Monitor Logs** - Set up logging and monitoring
8. **File Upload Limits** - Restrict file sizes and types

### Recommended Nginx Configuration

```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    location / {
        proxy_pass http://localhost:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
    
    # SSE specific settings
    location /events {
        proxy_pass http://localhost:8080;
        proxy_buffering off;
        proxy_cache off;
        proxy_set_header Connection '';
        proxy_http_version 1.1;
        chunked_transfer_encoding off;
    }
}
```

---

## 🛠️ Development

### Build from Source

```bash
CGO_ENABLED=1 go build -o pars-chat ./cmd/server
./pars-chat
```

### Dependencies

* `github.com/mattn/go-sqlite3` - SQLite driver with CGO
* `github.com/golang-jwt/jwt/v5` - JWT authentication

### Run Tests

```bash
go test ./...
```

---

## ✅ TODO / Future Improvements

- [x] JWT authentication
- [x] Location sharing  
- [x] Enhanced read receipts
- [x] Emoji reactions
- [x] Dark theme UI
- [ ] Voice/video calls (WebRTC)
- [ ] Push notifications (PWA)
- [ ] Message search functionality
- [ ] User profile pictures
- [ ] Custom themes and personalization
- [ ] Mobile app (React Native/Flutter)
- [ ] Message forwarding
- [ ] Pinned messages
- [ ] Admin panel
- [ ] Analytics dashboard

---

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Commit Message Convention

Follow the conventional commits specification:
- `feat:` - New features
- `fix:` - Bug fixes
- `docs:` - Documentation updates
- `style:` - Code style changes
- `refactor:` - Code refactoring
- `test:` - Adding tests
- `chore:` - Maintenance tasks

---

## 📄 License

This project is released under the MIT License.

---

## ⚖️ Ethical Use

This tool is designed to help free communication during critical times. Please:

* Use responsibly and legally
* Respect user privacy and data
* Avoid misuse or malicious activities
* Be aware of local laws and regulations
* Ensure secure deployment in sensitive environments
* Contribute to the open-source community

---

## 💡 Support

For issues, questions, or feature requests:
* Create an Issue on GitHub
* Join community discussions
* Submit Pull Requests

---

## 🌟 Acknowledgments

Built with ❤️ for free communication and privacy-focused messaging.

Special thanks to:
- Go community for excellent libraries
- Open-source contributors worldwide
- Iranian developers and activists

---

**Pars Chat - Secure Communication for Everyone**


