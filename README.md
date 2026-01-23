# TCP Chat Application

> **⚠️ WARNING - CRITICAL INFRASTRUCTURE**  
> This repository is designed to facilitate communication for Iranian citizens during protests and internet shutdowns. It operates on domestic servers to maintain connectivity when external internet access is restricted or censored. Use responsibly and ensure secure deployment in sensitive environments.

A feature-rich TCP chat application built in Go with user registration and file sharing capabilities, designed for resilient local network communication.

---

##  Features

* **User Registration** - Register users with first and last name
* **Real-time Messaging** - Instant messaging between connected users
* **File Sharing** - Upload and share files (images, videos, documents)
* **Multi-Client Support** - Support for multiple simultaneous users
* **TCP Server** - Chat server on port 8080
* **HTTP File Server** - File upload server on port 8081
* **Dockerized** - Ready for deployment with Docker and Docker Compose
* **Offline-First** - Works on local networks without internet connectivity

---

##  Project Structure

```
├── server.go          # Chat and file upload server code
├── client.go          # Client code with registration and upload
├── Dockerfile         # Docker image for server and client
├── docker-compose.yml # Service configuration
├── go.mod             # Go module file
├── go.sum             # Dependency checksums
└── README.md          # Project documentation
```

---

##  Quick Start

### Prerequisites

* Go 1.21 or later
* Docker (optional - for containerized deployment)
* Docker Compose (optional)

---

##  Running Locally

### 1. Run Server

```bash
# Build
go build -o chat-server ./server.go

# Run
./chat-server
```

The server runs on two ports:
- **Port 8080**: TCP Chat Server
- **Port 8081**: HTTP File Upload Server

### 2. Run Client

```bash
# Build
go build -o chat-client ./client.go

# Run
./chat-client
```

Upon running, you will be prompted for:
1. **First name**
2. **Last name**

---

##  How to Use

### Send Text Message
Simply type your message and press Enter:
```
Hello everyone!
```

### Upload File
Use the `/upload` command:
```
/upload /path/to/your/file.jpg
```

**Notes:**
- Image files: `.jpg`, `.png`, `.gif`, `.webp` (displayed as image)
- Video files: `.mp4`, `.mov`, `.webm` (displayed as video)
- Other files: displayed as file
- Maximum file size: 50MB

---

##  Running with Docker

### 1. Build Images

```bash
docker-compose build
```

### 2. Start Services

```bash
docker-compose up
```

### 3. Stop Services

```bash
docker-compose down
```

---

##  Architecture

### Server Architecture

**The server consists of two main components:**

1. **TCP Server (Port 8080)**
   - Manages client connections
   - User registration
   - Broadcasts messages to all connected users

2. **HTTP Server (Port 8081)**
   - Receives and stores files in `uploads/` directory
   - Serves uploaded files
   - Generates URLs for file access

### Message Types

```json
// Registration
{
  "type": "register",
  "first": "John",
  "last": "Doe"
}

// Text Message
{
  "type": "text",
  "text": "Your message here"
}

// Media Message
{
  "type": "media",
  "url": "http://localhost:8081/uploads/file.jpg",
  "mediaType": "image|video|file"
}

// Info Message (from server)
{
  "type": "info",
  "text": "Information message"
}
```

---

## 🔧 Configuration

### Change Server Address

In `client.go`:
```go
conn, err := net.Dial("tcp", "localhost:8080")
```

### Change Maximum File Size

In `server.go`:
```go
err := r.ParseMultipartForm(50 << 20) // 50MB - you can change this
```

---

##  CI/CD with GitHub Actions

The project includes GitHub Actions workflow for:
- Building Go binaries
- Running tests
- Building Docker images
- Deployment (optional)

---

##  Development

### Add New Features

Some development ideas:
- **Chat Rooms** - Create multiple channels
- **Message Encryption** - Enhanced security for data transmission
- **Message History** - Store messages in database
- **WebSocket Support** - Web browser compatibility
- **Private Messages** - Direct messaging between two users
- **Notifications** - Alerts for new messages
- **Authentication** - Login system with passwords
- **End-to-End Encryption** - For maximum privacy in sensitive communications

### Code Structure

```go
// Server Components
- handleConnection()  // Manage connections
- broadcast()         // Broadcast messages to all
- uploadHandler()     // Handle file uploads

// Client Components
- uploadFile()        // Upload file
- detectMediaType()   // Detect media type
```

---

##  Troubleshooting

### "Connection refused" Error
```bash
# Make sure the server is running
ps aux | grep server.go
```

### File Upload Issues
- Check file size (maximum 50MB)
- Enter the correct file path
- Make sure HTTP server is running on port 8081

### Port Already in Use
```bash
# Find process on port 8080 or 8081
lsof -i :8080
lsof -i :8081

# Kill the process
kill -9 <PID>
```

---

##  API Reference

### TCP Protocol

**Register:**
```json
{"type": "register", "first": "Ali", "last": "Ahmadi"}
```

**Send Text:**
```json
{"type": "text", "text": "Hello World"}
```

**Send Media:**
```json
{"type": "media", "url": "http://...", "mediaType": "image"}
```

### HTTP API

**Upload File:**
```bash
curl -X POST http://localhost:8081/upload \
  -F "file=@/path/to/file.jpg"
```

**Response:**
```json
{"url": "http://localhost:8081/uploads/1234567890.jpg"}
```

---

##  Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest new features
- Submit pull requests

---

## 👨‍💻 Author

Built with ❤️ using Go

---

## 📄 License

This project is licensed under the MIT License.


