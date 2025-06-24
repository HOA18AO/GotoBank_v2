# GotoBank - Gohub Auto Banking System

**GotoBank** is an automated banking transaction monitoring and notification system developed by **Gohub**. It provides real-time transaction tracking, intelligent captcha solving, and instant notifications for MB Bank Business accounts.

## 🚀 **What is GotoBank?**

GotoBank (**Go**hub Au**to** **Bank**ing) is a comprehensive banking automation solution that:

- **📊 Monitors** MB Bank Business transactions in (almost) real-time
- **🤖 Automates** login and captcha solving using AI
- **📱 Sends** instant notifications to Lark/Feishu channels
- **🛒 Integrates** with WooCommerce for order processing
- **🔄 Handles** session management and error recovery
- **📈 Scales** with Docker containers and Selenium Grid

## 🏗️ **System Architecture**

### **Core Services:**

- **🐳 Docker Compose** - Container orchestration and service management
- **🌐 Selenium Grid Hub** - Browser automation coordination (port 4444)
- **🔗 Microsoft Edge Container** - Headless browser for web scraping
- **⚡ FastAPI Application** - REST API server (port 8000)
- **📅 Scheduler Service** - Automated transaction monitoring
- **🔐 MB Bank Business Portal** - Target banking system
- **📱 Lark/Feishu Bot** - Real-time notification service
- **👁️ TrOCR Captcha Reader** - AI-powered captcha solving
- **📊 JSON Data Storage** - Local transaction data persistence
- **🛒 WooCommerce Integration** - E-commerce order automation

### **Technology Stack:**

- **Backend**: Python 3.11, FastAPI, Selenium WebDriver
- **Containerization**: Docker, Docker Compose
- **Browser Automation**: Selenium Grid, Microsoft Edge
- **AI/ML**: TrOCR (Transformer OCR), PyTorch, EasyOCR
- **Notifications**: Lark API, OAuth 2.0
- **E-commerce**: WooCommerce REST API
- **Data**: JSON files, timezone-aware timestamps (Vietnam GMT+7)
- **Monitoring**: Custom health checks, graceful shutdown

### **Service Dependencies:**

```
Docker Host (GotoBank System)
├── Selenium Grid Hub (selenium-hub-webhook:4444)
│   └── Edge Browser Container (auto-scaling)
├── Main Application Container (mb-fastapi-webhook)
│   ├── FastAPI Server (port 8000)
│   ├── Scheduler Process (background)
│   ├── Captcha Reading Service (TrOCR + EasyOCR)
│   ├── WooCommerce Integration
│   └── Data Storage (/data volume)
└── External Services
    ├── MB Bank BIZ Portal (ebank.mbbank.com.vn)
    ├── Lark API (open.feishu.cn)
    └── WooCommerce Sites (REST API)
```

### **Data Flow:**

```
Scheduler → WebDriver → MB Bank → AI Captcha Reader → Transaction Parser → 
JSON Storage → Lark Notifications → WooCommerce Order Processing
```

## 🛠️ **Key Features**

### **🔐 Intelligent Banking Automation:**
- **Multi-attempt login** with retry logic
- **AI-powered captcha solving** using TrOCR and EasyOCR
- **Session management** with automatic recovery
- **Timezone-aware** transaction timestamps (Vietnam GMT+7)

### **📊 Transaction Processing:**
- **Real-time monitoring** every 20 seconds
- **Duplicate detection** and filtering
- **Date range filtering** for efficient data retrieval
- **Pagination support** for large transaction sets

### **📱 Smart Notifications:**
- **Instant Lark messages** for new transactions
- **Formatted notifications** with transaction details
- **Rate limiting** to prevent spam
- **Error handling** with fallback mechanisms

### **🛒 E-commerce Integration:**
- **WooCommerce order detection** (GH######) pattern
- **Automatic order creation** from bank transactions
- **Order confirmation** and status updates
- **Webhook notifications** for external systems

### **🐳 Production Ready:**
- **Docker containerization** for easy deployment
- **Health checks** and monitoring endpoints
- **Graceful shutdown** handling
- **Environment-based configuration**
- **Comprehensive logging** with Vietnam timezone

## 🚀 **Quick Start**

### **Prerequisites:**
- Docker and Docker Compose
- MB Bank Business account credentials
- Lark/Feishu app credentials
- WooCommerce API credentials (optional)

### **Setup:**
1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/GotoBank.git
   cd GotoBank
   ```

2. **Configure environment variables:**
   ```bash
   cp .env.example .env
   # Edit .env with your credentials
   ```

3. **Start the system:**
   ```bash
   docker-compose up -d
   ```

4. **Access the API:**
   - FastAPI Docs: http://localhost:8000/docs
   - Health Check: http://localhost:8000/

## 🔧 **Docker Networking Configuration**

When running in a Docker environment, services need to communicate with each other using their service names instead of localhost or 127.0.0.1.

### **Connection Issues Solution**

The connection issues in the logs show that the scheduler can't connect to the FastAPI server because it's trying to use "localhost" or "127.0.0.1", but in Docker each container has its own network namespace.

**To fix this:**

1. Always use the service name (e.g., "mb-fastapi-webhook") when connecting from one container to another
2. Make sure port mappings are correctly configured in docker-compose.yml
3. Ensure the FastAPI service is running before the scheduler tries to connect

### **Configuration Checklist**

- [ ] FastAPI service is defined in docker-compose.yml
- [ ] Scheduler service depends_on FastAPI service
- [ ] API_service.py uses correct service hostname, not "localhost"
- [ ] Port 8000 is exposed in the FastAPI service
- [ ] Environment variables are properly configured
- [ ] Data volume is mounted for persistence

## 📞 **Support & Contact**

**GotoBank** is developed and maintained by **Gohub**.

For technical support, credentials, or business inquiries:

- **📧 Email** (dev): tranhoaibao9@gmail.com
- **📱 Mobile** (dev): 0369285329
- **🏢 Company**: GOHUB TRAVEL SOLUTIONS JSC

### **Note**
- This system is designed for MB Bank Business accounts only
- Contact author to get .env file configuration
- Requires proper API credentials for full functionality
- Built with enterprise-grade reliability and performance