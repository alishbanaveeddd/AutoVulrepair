# Final Year Project - AutoVulRepair 🔐

**Automated Vulnerability Detection, Fuzzing & AI-Powered Repair for C/C++ Code**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Docker Support](https://img.shields.io/badge/Docker-Ready-brightgreen.svg)](https://www.docker.com/)

AutoVulRepair is a comprehensive security workflow platform that automatically detects vulnerabilities in C/C++ code through static analysis, generates intelligent fuzzing strategies, creates test harnesses, and provides AI-powered vulnerability repair suggestions.

## 🌟 Key Features

- **🔍 Static Analysis**: Scan C/C++ code using Cppcheck or CodeQL
- **⚡ Intelligent Fuzzing**: Auto-generate fuzz plans and test harnesses
- **🤖 AI-Powered Repair**: Get intelligent patch suggestions using Groq AI
- **🎯 Web Dashboard**: Beautiful UI for managing scans and analysis
- **📊 RESTful API**: Integrate into CI/CD pipelines seamlessly
- **🐳 Docker Support**: One-command setup with pre-configured environment
- **📈 Comprehensive Reporting**: JSON, CSV, and Markdown export formats
- **🔒 Signature-Aware Generation**: Context-aware harness creation

## 📋 Table of Contents

- [Quick Start](#-quick-start-docker--recommended)
- [Manual Setup](#-manual-setup-for-development)
- [Architecture](#-architecture)
- [Usage Guide](#-usage-guide)
- [API Reference](#-api-reference)
- [Configuration](#-configuration)
- [Contributing](#-contributing)
- [Troubleshooting](#-troubleshooting)
- [License](#-license)

---

## 🚀 Quick Start (Docker - Recommended)

### Prerequisites

- **Docker Desktop** only! Everything else is bundled.
  - **Windows/macOS**: [Download Docker Desktop](https://www.docker.com/products/docker-desktop/)
  - **Linux**: `sudo apt-get install docker.io docker-compose`

### One-Command Setup

```bash
docker-compose up
```

**That's it!** Open http://localhost:5000 in your browser.

The Docker container includes:
- ✅ Clang/LLVM (fuzzing compiler)
- ✅ Python 3.8+ with all dependencies
- ✅ Redis server
- ✅ Celery worker (background tasks)
- ✅ Pre-configured environment variables

**No manual installation of clang or other tools needed!**

### First Steps After Starting

1. **Open Dashboard**: Navigate to http://localhost:5000
2. **Create a Scan**: Upload code or provide a GitHub repository URL
3. **Review Results**: View vulnerabilities in the dashboard
4. **Generate Fuzz Plan**: Create a fuzzing strategy for detected issues
5. **Generate Harnesses**: Create LibFuzzer-compatible test harnesses
6. (Optional) **AI Repair**: Get patch suggestions for vulnerabilities

---

## 🛠️ Manual Setup (For Development)

### Prerequisites

- **Python 3.8+**: [Download Python](https://www.python.org/downloads/)
- **Git**: Version control system
- **Docker Desktop**: Required for Redis
- **C/C++ Compiler**:
  - **Linux**: `sudo apt-get install clang`
  - **macOS**: `brew install llvm`
  - **Windows**: Use WSL2 or Docker (recommended)

### Installation Steps

**1. Clone or Download the Repository**

```bash
git clone https://github.com/alishbanaveeddd/AutoVulrepair.git
cd AutoVulrepair
```

**2. Create & Activate Virtual Environment**

```powershell
# Windows
python -m venv .venv
.venv\Scripts\Activate.ps1

# macOS/Linux
python3 -m venv .venv
source .venv/bin/activate
```

**3. Install Dependencies**

```bash
pip install -r requirements.txt
```

**4. Configure Environment Variables**

Create a `.env` file in the project root:

```env
# Flask Configuration
FLASK_SECRET_KEY=your-super-secret-key-change-this
FLASK_ENV=development

# GitHub Integration (optional)
GITHUB_CLIENT_ID=your-github-client-id
GITHUB_CLIENT_SECRET=your-github-client-secret

# Redis/Celery Configuration
REDIS_URL=redis://localhost:6379/0

# Directory Configuration
SCANS_DIR=./scans

# AI Model Configuration (optional)
GROQ_API_KEY=your-groq-api-key-from-console.groq.com
```

**5. Start Redis**

```powershell
docker-compose up -d
```

**6. Start Celery Worker (New Terminal)**

```powershell
python celery_worker.py
```

You should see:
```
* Ready to accept tasks
```

**7. Start Flask Application (Another Terminal)**

```powershell
python app.py
```

You should see:
```
* Running on http://127.0.0.1:5000
```

**8. Access the Application**

Open http://localhost:5000 in your browser

---

## 📐 Architecture

AutoVulRepair is built on a modular architecture with 6 main components:

```
┌─────────────────────────────────────────────────────────────┐
│             Flask Web Dashboard & API                       │
│  (UI for scanning, reporting, visualization, downloads)     │
└────────────┬────────────────────────────────────────────────┘
             │
     ┌───────┴────────────────────────────────────────┐
     │                                                 │
┌────▼─────────────┐                     ┌────────────▼────────┐
│ Module 1: Static │                     │ Module 2: Fuzz Plan │
│ Analysis         │                     │ Generation          │
│ (Cppcheck/Code QL)│◄────────────────────┤ (AI-powered)         │
└────┬─────────────┘                     └────────────┬────────┘
     │                                                 │
     │                 ┌─────────────────────────────────┤
     │                 │                                 │
┌────▼────────────────▼─────────┐            ┌──────────▼──────────┐
│  Module 3: Harness Generation │            │ Module 4: Build     │
│  (LibFuzzer-compatible)       │───────────►│ Orchestration       │
│  (Signature-aware templates)  │            │ (Docker-based)      │
└────────────┬──────────────────┘            └──────────┬──────────┘
             │                                          │
             └──────────────┬───────────────────────────┘
                            │
                    ┌───────▼────────┐
                    │ Module 5: Fuzz │
                    │ Execution      │
                    │ (LibFuzzer run)│
                    └────────────────┘
                            │
                    ┌───────▼────────┐
                    │ Module 6: AI   │
                    │ Repair         │
                    │ (Groq API)     │
                    └────────────────┘
```

### Component Overview

| Component | Purpose | Technology |
|-----------|---------|------------|
| **Static Analysis** | Scan code for vulnerabilities | Cppcheck, CodeQL |
| **Fuzz Plan Generation** | Create fuzzing strategy | Python, AI analysis |
| **Harness Generation** | Create test code | Jinja2 templates, LibFuzzer |
| **Build Orchestration** | Compile harnesses | Docker, Clang |
| **Fuzz Execution** | Run fuzzing campaigns | LibFuzzer |
| **AI Repair** | Generate patches | Groq AI API |

---

## 📖 Usage Guide

### Workflow 1: Basic Vulnerability Scanning

**Step 1: Submit Code**
- Go to Dashboard → New Scan
- Choose: GitHub URL OR Upload ZIP file
- Select analysis tool (Cppcheck or CodeQL)

**Step 2: Review Results**
- Wait for scan to complete
- View vulnerabilities table with:
  - Severity level
  - Description
  - File location
  - Line number

**Step 3: Export Report**
- Download as JSON, CSV, or Markdown
- Share with team members

### Workflow 2: Generate Fuzzing Strategy

**Step 1: Create Fuzz Plan**
- From dashboard, click "Generate Fuzz Plan"
- System analyzes vulnerabilities and creates strategy

**Step 2: Review Fuzz Plan**
- View target functions for fuzzing
- See priority rankings
- Review bug class categorization

**Step 3: Generate Harnesses**
- Click "Generate Harnesses"
- System creates LibFuzzer-compatible test files
- Download individual harnesses or ZIP archive

### Workflow 3: Build & Execute Fuzzing

**Step 1: Build Harnesses**
- Click "Build Harnesses"
- System compiles using Clang with sanitizers

**Step 2: Review Build Status**
- Check for compilation errors
- Verify all harnesses compiled successfully

**Step 3: Execute Fuzzing**
- Click "Execute Fuzzing"
- System runs LibFuzzer
- Collects crash reports and findings

### Workflow 4: AI-Powered Repair (Optional)

**Requirements:**
- Groq API key (free from https://console.groq.com/keys)

**Steps:**
1. Set `GROQ_API_KEY` in `.env`
2. After scan completes, click "Get AI Repair Suggestions"
3. Review AI-generated patches
4. Download patch files

---

## 🔌 API Reference

### Authentication

All API endpoints require no authentication by default (can be configured).

### Base URL

```
http://localhost:5000/api
```

### Endpoints

#### 1. Create Scan

**POST** `/scan`

Create a new vulnerability scan.

```bash
curl -X POST http://localhost:5000/api/scan \
  -H "Content-Type: application/json" \
  -d '{
    "source_type": "github",
    "github_url": "https://github.com/user/repo",
    "analysis_tool": "cppcheck"
  }'
```

**Parameters:**
- `source_type`: `"github"` or `"upload"`
- `github_url`: GitHub repository URL (if source_type is github)
- `analysis_tool`: `"cppcheck"` or `"codeql"`

**Response:**
```json
{
  "scan_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "queued",
  "created_at": "2024-03-18T10:30:00Z"
}
```

#### 2. Get Scan Status

**GET** `/scan/<scan_id>`

Retrieve scan details and results.

```bash
curl http://localhost:5000/api/scan/550e8400-e29b-41d4-a716-446655440000
```

**Response:**
```json
{
  "scan_id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "completed",
  "progress": 100,
  "vulnerabilities": [
    {
      "severity": "high",
      "type": "buffer-overflow",
      "description": "Buffer overflow in function X",
      "file": "src/main.c",
      "line": 42
    }
  ],
  "created_at": "2024-03-18T10:30:00Z",
  "completed_at": "2024-03-18T10:35:00Z"
}
```

#### 3. Generate Fuzz Plan

**POST** `/scan/<scan_id>/fuzz-plan`

Generate fuzzing strategy for scan results.

```bash
curl -X POST http://localhost:5000/api/scan/550e8400-e29b-41d4-a716-446655440000/fuzz-plan
```

**Response:**
```json
{
  "fuzz_plan_id": "abc-123-def",
  "targets": [
    {
      "function": "process_input",
      "file": "src/main.c",
      "priority": "high",
      "bug_class": "buffer_overflow"
    }
  ],
  "created_at": "2024-03-18T10:36:00Z"
}
```

#### 4. Generate Harnesses

**POST** `/fuzz-plan/<fuzz_plan_id>/harnesses`

Create LibFuzzer-compatible test harnesses.

```bash
curl -X POST http://localhost:5000/api/fuzz-plan/abc-123-def/harnesses
```

**Response:**
```json
{
  "harness_id": "xyz-789-uvw",
  "harnesses": [
    {
      "function": "process_input",
      "language": "cpp",
      "status": "generated"
    }
  ],
  "download_url": "/downloads/harnesses.zip"
}
```

#### 5. Get AI Repair Suggestions

**POST** `/scan/<scan_id>/repair`

Get AI-powered patch suggestions.

```bash
curl -X POST http://localhost:5000/api/scan/550e8400-e29b-41d4-a716-446655440000/repair
```

**Response:**
```json
{
  "repair_id": "repair-123-abc",
  "vulnerabilities_patched": 3,
  "patches": [
    {
      "vulnerability_id": "vuln-1",
      "original_code": "...",
      "patched_code": "...",
      "explanation": "Added bounds checking..."
    }
  ]
}
```

---

## ⚙️ Configuration

### Environment Variables

Create or edit `.env` file with:

```env
# Flask Configuration
FLASK_SECRET_KEY=your-strong-secret-key
FLASK_ENV=production  # or development
FLASK_DEBUG=False

# Database
DATABASE_URL=sqlite:///autovulrepair.db
# or use PostgreSQL: postgresql://user:pass@localhost/autovulrepair

# Redis & Celery
REDIS_URL=redis://localhost:6379/0
CELERY_BROKER_URL=redis://localhost:6379/0
CELERY_RESULT_BACKEND=redis://localhost:6379/1

# File Storage
SCANS_DIR=./scans
UPLOADS_DIR=./uploads
MAX_UPLOAD_SIZE=104857600  # 100MB

# GitHub Integration (optional)
GITHUB_CLIENT_ID=your-id
GITHUB_CLIENT_SECRET=your-secret
GITHUB_ACCESS_TOKEN=your-token

# AI Models (for repair module)
GROQ_API_KEY=your-groq-api-key

# Logging
LOG_LEVEL=INFO
LOG_FILE=./logs/autovulrepair.log

# Security
ALLOWED_HOSTS=localhost,127.0.0.1
CORS_ORIGINS=http://localhost:5000
```

### Docker Compose Configuration

The `docker-compose.yml` includes:
- Redis service (port 6379)
- Celery worker
- Flask application (port 5000)

To customize, edit `docker-compose.yml` and run:

```bash
docker-compose up --build
```

---

## 📁 Project Structure

```
autovulrepair/
├── src/
│   ├── analysis/           # Static analysis module
│   │   ├── cppcheck.py    # Cppcheck integration
│   │   ├── codeql.py      # CodeQL integration
│   │   └── parser.py      # Results parser
│   │
│   ├── fuzz_plan/         # Fuzz plan generation
│   │   ├── generator.py   # Plan generation logic
│   │   ├── analyzer.py    # Vulnerability analysis
│   │   └── formatter.py   # Result formatting
│   │
│   ├── harness/           # Harness generation
│   │   ├── generator.py   # Harness generation engine
│   │   ├── templates/     # Jinja2 templates
│   │   └── compiler.py    # Harness compilation
│   │
│   ├── fuzz_exec/         # Fuzzing execution
│   │   ├── executor.py    # LibFuzzer integration
│   │   └── reporter.py    # Result reporting
│   │
│   ├── repair/            # AI repair module
│   │   ├── ai_patcher.py  # Groq AI integration
│   │   └── patch_gen.py   # Patch generation
│   │
│   ├── models/            # Database models
│   │   ├── scan.py       # Scan model
│   │   ├── finding.py    # Finding model
│   │   └── session.py    # Session management
│   │
│   ├── queue/             # Celery tasks
│   │   └── tasks.py      # Background job definitions
│   │
│   └── utils/             # Utilities
│       ├── validation.py  # Input validation
│       ├── docker_helper.py  # Docker integration
│       └── file_ops.py    # File operations
│
├── templates/             # Flask HTML templates
│   ├── dashboard.html    # Main dashboard
│   ├── scan_results.html # Results display
│   └── base.html         # Base template
│
├── static/                # CSS, JavaScript, images
│   ├── css/
│   ├── js/
│   └── img/
│
├── tests/                 # Test suite
│   ├── test_analysis.py
│   ├── test_harness.py
│   └── test_api.py
│
├── docs/                  # Documentation
│   ├── USER_GUIDE.md
│   ├── API_GUIDE.md
│   └── ARCHITECTURE.md
│
├── app.py                 # Flask application entry point
├── celery_worker.py       # Celery worker entry point
├── requirements.txt       # Python dependencies
├── docker-compose.yml     # Docker services
├── Dockerfile             # Container definition
└── .env.example          # Environment template
```

---

## ✅ Testing

### Run Tests

```bash
# Run all tests
pytest tests/

# Run with coverage
pytest tests/ --cov=src --cov-report=html

# Run specific test file
pytest tests/test_analysis.py -v

# Run property-based tests
pytest tests/test_harness_generation_properties.py -v
```

### Manual Testing

Use test scripts provided:

```bash
# Test API endpoints
python test_api_manual.sh

# Test vulnerability scanning
python test_extension_scan.py

# Test harness generation
python test_smart_fix.c
```

---

## 🔧 Troubleshooting

### Common Issues & Solutions

#### 1. **Redis Connection Error**

**Error:**
```
ERROR: Redis connection refused at localhost:6379
```

**Solution:**
```bash
# Ensure Docker Desktop is running, then:
docker-compose up -d

# Verify Redis is running:
docker ps | grep redis

# Check Redis status:
docker-compose logs redis
```

#### 2. **Celery Worker Not Starting**

**Error:**
```
Error: Celery worker failed to connect to broker
```

**Solution:**
```bash
# Kill any existing worker processes
taskkill /F /IM python.exe  # Windows
pkill -f celery_worker      # macOS/Linux

# Make sure Redis is running
docker-compose up -d

# Restart the worker
python celery_worker.py
```

#### 3. **Port Already in Use**

**Error:**
```
Address already in use. Port 5000 is in use
```

**Solution:**
```bash
# Windows
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# macOS/Linux
lsof -i :5000
kill -9 <PID>

# Or use a different port
FLASK_PORT=5001 python app.py
```

#### 4. **Clang Not Found**

**Error:**
```
clang: command not found
```

**Solution:**
- Use Docker (recommended): `docker-compose up`
- Or install compilers:
  - **Windows**: Use WSL2 or Docker
  - **macOS**: `brew install llvm`
  - **Linux**: `sudo apt-get install clang`

#### 5. **Scan Stuck in "Processing"**

**Issue:** Scan never completes

**Solution:**
```bash
# Check Celery worker logs
# Ensure worker is running in another terminal

# Clear Redis queue:
docker exec -it $(docker-compose ps -q redis) redis-cli FLUSHALL

# Restart everything:
docker-compose down
docker-compose up
```

#### 6. **GitHub Repository Clone Fails**

**Error:**
```
fatal: could not read Username for 'https://github.com': No such file or directory
```

**Solution:**
- Set GitHub token in `.env`:
  ```env
  GITHUB_ACCESS_TOKEN=your-personal-access-token
  ```
- Generate token: https://github.com/settings/tokens

#### 7. **Out of Disk Space**

**Error:**
```
No space left on device
```

**Solution:**
```bash
# Clean up old scans
rm -rf scans/old_scan_id

# Clean Docker:
docker system prune -a

# Check disk space
df -h  # macOS/Linux
Get-Volume  # Windows
```

#### 8. **Permission Denied Errors**

**Solution:**
```bash
# macOS/Linux
chmod +x app.py celery_worker.py
chmod -R 755 scans/

# Windows PowerShell
# Run as Administrator
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

## 🤝 Contributing

We welcome contributions! Here's how to get involved:

### Development Setup

```bash
# Fork and clone the repository
git clone https://github.com/YOUR_USERNAME/AutoVulrepair.git
cd AutoVulrepair

# Create a feature branch
git checkout -b feature/your-feature-name

# Set up development environment
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### Making Changes

1. **Code Style**: Follow PEP 8
2. **Type Hints**: Add type annotations where possible
3. **Tests**: Write tests for new features
4. **Documentation**: Update docstrings and README

### Submitting Changes

```bash
# Commit with clear messages
git add .
git commit -m "feat: add new fuzzing strategy"

# Push to your fork
git push origin feature/your-feature-name

# Create Pull Request on GitHub
```

### Areas for Contribution

- 🐛 **Bug Fixes**: Report and fix issues
- ✨ **New Features**: Implement new analysis tools or repair strategies
- 📚 **Documentation**: Improve guides and API docs
- 🧪 **Tests**: Increase test coverage
- 🎨 **UI/UX**: Improve dashboard and user experience

### Code Review

All pull requests go through:
1. Automated tests
2. Code review
3. Integration testing
4. Deployment to staging

---

## 📞 Support & Community

### Getting Help

- **Issues**: [GitHub Issues](https://github.com/alishbanaveeddd/AutoVulrepair/issues)
- **Discussions**: [GitHub Discussions](https://github.com/alishbanaveeddd/AutoVulrepair/discussions)
- **Documentation**: [Full Docs](./docs/)

### Roadmap

Planned features:
- [ ] Support for Java/Python code analysis
- [ ] Integration with more AI models (Claude, GPT)
- [ ] Real-time collaborative scanning
- [ ] Advanced reporting with graphs
- [ ] Custom vulnerability rules
- [ ] Machine learning-based patch ranking

### Release Notes

See [RELEASES.md](./RELEASES.md) for version history and changelog.

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

### Summary
- ✅ Free to use for personal and commercial projects
- ✅ Can modify and distribute
- ✅ Must include license and copyright notice
- ❌ No warranty or liability

---

## 🙏 Acknowledgments

- **Cppcheck**: Static analysis engine
- **CodeQL**: GitHub's query language for code analysis
- **LibFuzzer**: Fuzzing engine from LLVM
- **Groq**: AI model provider for vulnerability repair
- **Flask**: Web framework
- **Celery**: Task queue system

---

## 📊 Statistics

- **Lines of Code**: 5,000+
- **Test Coverage**: 80%+
- **Supported Languages**: C, C++
- **Analysis Tools**: Cppcheck, CodeQL
- **Fuzzing Engine**: LibFuzzer
- **API Endpoints**: 10+
- **Database Models**: 5+

---

## 🔗 Quick Links

- [GitHub Repository](https://github.com/alishbanaveeddd/AutoVulrepair)
- [Bug Tracker](https://github.com/alishbanaveeddd/AutoVulrepair/issues)
- [Documentation](./docs/)
- [API Reference](#-api-reference)
- [Cppcheck Docs](http://cppcheck.sourceforge.net/)
- [CodeQL Docs](https://codeql.github.com/docs/)
- [LibFuzzer Docs](https://llvm.org/docs/LibFuzzer/)

---

**Made with ❤️ for secure C/C++ code**

*Last Updated: March 18, 2024*
1. Check Celery worker logs for conversion errors
2. Verify `cppcheck-report.xml` exists in `scans/<scan_id>/artifacts/`
3. Run manual conversion: `python convert_scan_to_findings.py <scan_id>`

## Architecture

The system follows a modular pipeline architecture:

1. **Module 1 (Detection):** Static analysis with Cppcheck/CodeQL
2. **Module 2 (Planning):** Fuzz plan generation from findings
3. **Module 3 (Generation):** Harness code generation
4. **Module 4 (Execution):** Build orchestration and compilation

Each module is independent and can be tested separately.

## Contributing

See `.kiro/specs/harness-parameter-passing/` for detailed design documentation and implementation tasks.

## License

[Your License Here]


## Windows Setup

### Option 1: WSL2 (Recommended)

WSL2 provides a full Linux environment on Windows and is the easiest way to run this tool.

1. **Install WSL2:**
```powershell
wsl --install
```

2. **Install Ubuntu from Microsoft Store** (if not auto-installed)

3. **Inside WSL2, install dependencies:**
```bash
sudo apt-get update
sudo apt-get install python3 python3-pip clang git docker.io
```

4. **Clone and run the tool in WSL2**

### Option 2: Native Windows (Advanced)

Native Windows requires additional setup and may have compatibility issues.

**Requirements:**
1. Install LLVM from https://releases.llvm.org/
2. Install Visual Studio Build Tools
3. Run from "x64 Native Tools Command Prompt"

**Verification:**
```powershell
python verify_fuzzing_setup.py
```

If this fails, use WSL2 instead.

## Troubleshooting

### "No fuzzing compiler found"
Run the verification script:
```bash
python verify_fuzzing_setup.py
```

This will test your environment and provide specific installation instructions.

### Build failures on Windows
If you see errors like `'cstring' file not found`, your clang installation can't find the C++ standard library. Solutions:
1. Switch to WSL2 (recommended)
2. Install Visual Studio Build Tools
3. Run from Developer Command Prompt

### For more help
See [FUZZING_SETUP_GUIDE.md](FUZZING_SETUP_GUIDE.md) for detailed setup instructions.
