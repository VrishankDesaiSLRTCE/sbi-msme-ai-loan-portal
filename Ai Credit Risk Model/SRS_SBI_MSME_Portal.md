# Software Requirements Specification (SRS)
## SBI MSME AI Credit Risk & Loan Approval System

**Document Version:** 2.0  
**Date:** November 2024  
**Prepared for:** State Bank of India — MSME Division  
**Project Type:** Mini Project / Academic Presentation

---

## Table of Contents
1. Introduction
2. System Overview
3. Functional Requirements
4. Non-Functional Requirements
5. System Architecture
6. Module Descriptions
7. AI/ML Model Specifications
8. Database Schema
9. API Endpoints
10. Technology Stack
11. Security Requirements
12. Use Case Diagrams (Text)

---

## 1. Introduction

### 1.1 Purpose
This SRS defines the complete requirements for the SBI MSME AI Credit Risk & Loan Approval Portal — an intelligent, end-to-end loan origination and decision system for Micro, Small and Medium Enterprises (MSMEs) seeking credit from State Bank of India.

### 1.2 Scope
The system automates the loan evaluation pipeline from application intake to final credit decision using Machine Learning, Explainable AI (XAI), anomaly detection, and rule-based validation. It includes a human-in-the-loop mechanism for borderline cases and real-time Gmail notifications.

### 1.3 Definitions
| Term | Definition |
|------|-----------|
| MSME | Micro, Small and Medium Enterprise |
| XAI | Explainable Artificial Intelligence |
| SHAP | SHapley Additive exPlanations |
| DTI | Debt-to-Income Ratio |
| CIBIL | Credit Information Bureau (India) Limited |
| EMI | Equated Monthly Installment |
| KYC | Know Your Customer |
| HITL | Human-in-the-Loop |
| OCR | Optical Character Recognition |

### 1.4 References
- RBI Master Circular on MSME Lending, 2023
- MSME Development Act, 2006
- Information Technology Act, 2000
- UIDAI Aadhaar Verification API Guidelines
- Income Tax Department PAN Verification API

---

## 2. System Overview

### 2.1 System Description
The SBI MSME AI Loan Portal is a web-based application providing:
- Secure MSME applicant registration and authentication
- Multi-step loan application with document upload
- AI-powered credit risk scoring using a trained ML model
- XAI (SHAP-based) decision explanation
- Rule-based validation engine (RBI compliance)
- Anomaly detection for fraud prevention
- Human-in-the-loop for borderline applications
- Real-time email notifications via Gmail API
- EMI calculator with amortization schedules
- Applicant and admin dashboards

### 2.2 System Actors
| Actor | Role |
|-------|------|
| MSME Applicant | Submits loan applications, tracks status |
| Credit Officer (Admin) | Reviews borderline/flagged applications |
| System Admin | Manages users, configures rules |
| AI Engine | Scores creditworthiness, detects anomalies |

---

## 3. Functional Requirements

### 3.1 User Registration & Authentication (FR-AUTH)

| ID | Requirement |
|----|-------------|
| FR-AUTH-01 | System shall allow MSME applicants to register with business name, email, mobile, MSME registration number |
| FR-AUTH-02 | System shall send OTP/email verification on registration |
| FR-AUTH-03 | System shall implement secure JWT-based session management |
| FR-AUTH-04 | System shall support password reset via registered email |
| FR-AUTH-05 | Admin login shall require 2FA authentication |
| FR-AUTH-06 | System shall log all authentication events for audit |

### 3.2 Loan Application (FR-LOAN)

| ID | Requirement |
|----|-------------|
| FR-LOAN-01 | System shall collect personal info: name, DOB, Aadhaar, PAN, address, state, yearly income |
| FR-LOAN-02 | System shall collect business info: business name, type, sector, MSME class, GSTIN, years of operation |
| FR-LOAN-03 | System shall collect financial info: loan amount, tenure, purpose, reason, pending loans, CIBIL |
| FR-LOAN-04 | System shall collect business financials: annual revenue, expenses, employee count |
| FR-LOAN-05 | System shall accept document uploads: Aadhaar, PAN, Income Certificate, ITR, MSME cert, Bank Statement |
| FR-LOAN-06 | System shall validate documents via OCR and API cross-checks |
| FR-LOAN-07 | System shall display real-time EMI preview during application |
| FR-LOAN-08 | System shall generate unique application reference ID |
| FR-LOAN-09 | System shall save incomplete applications for later resumption |

### 3.3 AI Credit Scoring (FR-AI)

| ID | Requirement |
|----|-------------|
| FR-AI-01 | AI model shall evaluate minimum 12 financial and non-financial features |
| FR-AI-02 | System shall compute a credit score in the range 300–900 |
| FR-AI-03 | Decision thresholds: Approved (≥700), Review (580–699), Rejected (<580) |
| FR-AI-04 | AI model shall be a trained Random Forest / XGBoost classifier |
| FR-AI-05 | Model training dataset shall include at minimum 10,000 MSME loan records |
| FR-AI-06 | Model accuracy shall exceed 90% on validation set |
| FR-AI-07 | System shall compute prediction confidence interval |

### 3.4 Explainable AI / XAI (FR-XAI)

| ID | Requirement |
|----|-------------|
| FR-XAI-01 | System shall generate SHAP values for every loan decision |
| FR-XAI-02 | XAI report shall show top 6 contributing factors with impact direction (positive/negative) |
| FR-XAI-03 | XAI explanation shall be displayed in plain language to the applicant |
| FR-XAI-04 | Factors shall include: DTI ratio, CIBIL score, business age, profit margin, pending loans, collateral |
| FR-XAI-05 | Admin shall see raw SHAP waterfall chart |

### 3.5 Rule-Based Validation (FR-RULE)

| ID | Requirement |
|----|-------------|
| FR-RULE-01 | PAN format must match regex: ^[A-Z]{5}[0-9]{4}[A-Z]{1}$ |
| FR-RULE-02 | Aadhaar must be exactly 12 digits |
| FR-RULE-03 | Minimum annual income: ₹1,80,000 |
| FR-RULE-04 | Maximum loan amount: ₹5 Crore (MSME limit) |
| FR-RULE-05 | Loan-to-income ratio must not exceed 7x annual income |
| FR-RULE-06 | Minimum CIBIL score: 600 |
| FR-RULE-07 | Loan purpose must be one of approved RBI categories |
| FR-RULE-08 | Tenure must be between 12 and 120 months |
| FR-RULE-09 | Applicant age must be between 21 and 65 years |

### 3.6 Anomaly Detection (FR-ANOMALY)

| ID | Requirement |
|----|-------------|
| FR-ANOMALY-01 | System shall flag loan amount > 6x annual income |
| FR-ANOMALY-02 | System shall flag loan amount > 3x annual business revenue |
| FR-ANOMALY-03 | System shall flag negative profit margin (expenses > revenue) |
| FR-ANOMALY-04 | System shall flag CIBIL score inconsistency with income profile |
| FR-ANOMALY-05 | System shall flag duplicate applications within 30 days |
| FR-ANOMALY-06 | System shall use Isolation Forest algorithm for statistical outlier detection |
| FR-ANOMALY-07 | High-severity anomalies shall automatically escalate to admin |

### 3.7 Human-in-the-Loop (FR-HITL)

| ID | Requirement |
|----|-------------|
| FR-HITL-01 | Applications with score 580–699 shall be escalated to credit officer |
| FR-HITL-02 | Admin shall view full application details, AI score, XAI factors, and anomaly flags |
| FR-HITL-03 | Credit officer shall be able to Approve, Reject, or Request-More-Information |
| FR-HITL-04 | Admin decision shall trigger automatic email to applicant |
| FR-HITL-05 | SLA for human review: 2 business days |
| FR-HITL-06 | System shall track admin override rate for model monitoring |

### 3.8 EMI Calculator (FR-EMI)

| ID | Requirement |
|----|-------------|
| FR-EMI-01 | Calculator shall accept principal, interest rate, and tenure as inputs |
| FR-EMI-02 | EMI formula: P × r × (1+r)^n / ((1+r)^n - 1) |
| FR-EMI-03 | Calculator shall display principal, total interest, and total payable |
| FR-EMI-04 | Calculator shall generate month-by-month amortization schedule |
| FR-EMI-05 | Calculator shall show principal vs interest pie chart |
| FR-EMI-06 | Results shall update in real-time as sliders change |

### 3.9 Notifications (FR-NOTIF)

| ID | Requirement |
|----|-------------|
| FR-NOTIF-01 | System shall send Gmail notification on application submission |
| FR-NOTIF-02 | System shall send Gmail notification on AI decision (approved/rejected/review) |
| FR-NOTIF-03 | System shall send Gmail notification on document verification completion |
| FR-NOTIF-04 | System shall send SMS via Twilio/MSG91 on EMI due dates |
| FR-NOTIF-05 | Admin shall receive email alerts for anomaly-flagged applications |
| FR-NOTIF-06 | All emails shall be sent via Gmail OAuth2 / SMTP with TLS |

### 3.10 Dashboard (FR-DASH)

| ID | Requirement |
|----|-------------|
| FR-DASH-01 | Applicant dashboard shall display: active loans, EMI schedule, application history |
| FR-DASH-02 | Admin dashboard shall display: total apps, approval rate, disbursed amount, pending reviews |
| FR-DASH-03 | Admin shall view real-time anomaly alerts |
| FR-DASH-04 | System shall display AI model performance metrics (accuracy, false positive rate) |

---

## 4. Non-Functional Requirements

### 4.1 Performance
- Page load time: < 3 seconds
- AI decision time: < 10 seconds
- System uptime: 99.5% SLA
- Concurrent users: 500+

### 4.2 Security
- All data encrypted at rest (AES-256) and in transit (TLS 1.3)
- OWASP Top 10 vulnerabilities addressed
- PII data masked in logs
- Session timeout: 30 minutes of inactivity
- Rate limiting: 100 requests/minute per IP

### 4.3 Usability
- Mobile-responsive design
- WCAG 2.1 AA accessibility compliance
- Support for Hindi and English languages
- Maximum form completion time: 15 minutes

### 4.4 Scalability
- Horizontal scaling via containerization (Docker + Kubernetes)
- Database: PostgreSQL with read replicas
- CDN for static assets

---

## 5. System Architecture

```
┌──────────────────────────────────────────────────────────┐
│                    CLIENT LAYER                          │
│  Browser (HTML5/CSS3/JS)  ←→  Mobile (Responsive)       │
└──────────────────────┬───────────────────────────────────┘
                       │ HTTPS
┌──────────────────────▼───────────────────────────────────┐
│                  APPLICATION LAYER                       │
│  ┌────────────┐  ┌─────────────┐  ┌──────────────────┐  │
│  │ Auth API   │  │  Loan API   │  │  Notification API│  │
│  │ (JWT/OAuth)│  │ (CRUD+File) │  │  (Gmail/SMS)     │  │
│  └────────────┘  └──────┬──────┘  └──────────────────┘  │
│                         │                                │
│  ┌──────────────────────▼─────────────────────────────┐  │
│  │              AI/ML ENGINE                          │  │
│  │  ┌──────────────┐  ┌──────────────┐  ┌─────────┐  │  │
│  │  │ Credit Score │  │  XAI (SHAP)  │  │Anomaly  │  │  │
│  │  │ (XGBoost/RF) │  │  Explainer   │  │Detector │  │  │
│  │  └──────────────┘  └──────────────┘  └─────────┘  │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────┬───────────────────────────────────┘
                       │
┌──────────────────────▼───────────────────────────────────┐
│                    DATA LAYER                            │
│  PostgreSQL DB    Redis Cache    AWS S3 (Documents)      │
└──────────────────────────────────────────────────────────┘
```

---

## 6. Technology Stack

### Frontend
| Component | Technology |
|-----------|-----------|
| UI Framework | HTML5, CSS3, Vanilla JS / React.js |
| Styling | Custom CSS with CSS Variables |
| Charts | Chart.js / Recharts |
| Fonts | Google Fonts (Noto Serif + Noto Sans) |
| Icons | SVG Icons / Emoji |

### Backend
| Component | Technology |
|-----------|-----------|
| Runtime | Node.js 18+ / Python 3.11+ (Flask/FastAPI) |
| API Framework | Express.js / FastAPI |
| Authentication | JWT + bcrypt + OAuth2 |
| File Handling | Multer (Node) / Boto3 (AWS S3) |
| Email | Nodemailer + Gmail OAuth2 |

### AI/ML
| Component | Technology |
|-----------|-----------|
| Model | XGBoost / Random Forest (scikit-learn) |
| XAI | SHAP library (Python) |
| Anomaly Detection | Isolation Forest (scikit-learn) |
| OCR | Tesseract.js / Google Vision API |
| Feature Engineering | Pandas + NumPy |

### Database
| Component | Technology |
|-----------|-----------|
| Primary DB | PostgreSQL 15 |
| Cache | Redis |
| Document Storage | AWS S3 / Cloudinary |
| ORM | Sequelize (Node) / SQLAlchemy (Python) |

### DevOps
| Component | Technology |
|-----------|-----------|
| Containerization | Docker + Docker Compose |
| CI/CD | GitHub Actions |
| Hosting | AWS EC2 / Railway / Render |
| SSL | Let's Encrypt |

---

## 7. AI Model Specifications

### 7.1 Input Features (12 features)
| Feature | Type | Description |
|---------|------|-------------|
| debt_to_income | Float | Monthly loan obligation / monthly income |
| cibil_score | Integer | CIBIL credit score (300–900) |
| business_age_years | Integer | Years business has been operating |
| profit_margin | Float | (Revenue - Expenses) / Revenue |
| loan_to_income_ratio | Float | Loan amount / Annual income |
| has_pending_loan | Binary | 0 = No, 1 = Yes |
| collateral_type | Categorical | none/property/fdr/equipment |
| business_sector_risk | Float | Sector-level default rate (0–1) |
| annual_income | Float | Annual personal income |
| loan_amount | Float | Requested loan amount |
| loan_tenure_months | Integer | Tenure in months |
| msme_classification | Categorical | Micro/Small/Medium |

### 7.2 Model Performance Targets
| Metric | Target |
|--------|--------|
| Accuracy | ≥ 90% |
| Precision | ≥ 88% |
| Recall | ≥ 87% |
| F1 Score | ≥ 88% |
| AUC-ROC | ≥ 0.92 |

### 7.3 Decision Thresholds
| Score Range | Decision | Action |
|-------------|----------|--------|
| 700 – 900 | APPROVED | Auto-approve, send offer letter |
| 580 – 699 | REVIEW | Escalate to credit officer |
| 300 – 579 | REJECTED | Auto-reject with XAI explanation |

---

## 8. Database Schema

### users
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  email VARCHAR(255) UNIQUE NOT NULL,
  mobile VARCHAR(15),
  password_hash VARCHAR(255),
  business_name VARCHAR(255),
  msme_reg_no VARCHAR(100),
  role ENUM('applicant', 'admin', 'superadmin'),
  is_verified BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### loan_applications
```sql
CREATE TABLE loan_applications (
  id UUID PRIMARY KEY,
  reference_id VARCHAR(50) UNIQUE,
  user_id UUID REFERENCES users(id),
  -- Personal
  full_name VARCHAR(200), aadhaar VARCHAR(12),
  pan VARCHAR(10), annual_income DECIMAL(15,2),
  -- Business
  business_name VARCHAR(255), business_type VARCHAR(50),
  sector VARCHAR(100), years_in_operation INTEGER,
  annual_revenue DECIMAL(15,2), annual_expenses DECIMAL(15,2),
  -- Loan
  loan_amount DECIMAL(15,2), tenure_months INTEGER,
  purpose VARCHAR(100), reason TEXT,
  has_pending_loan BOOLEAN, pending_loan_amount DECIMAL(15,2),
  cibil_score INTEGER, collateral_type VARCHAR(50),
  -- AI Results
  ai_credit_score INTEGER,
  ai_decision ENUM('approved','rejected','review'),
  shap_values JSONB,
  anomaly_flags JSONB,
  rule_violations JSONB,
  -- Status
  status ENUM('draft','submitted','processing','approved','rejected','review'),
  admin_decision VARCHAR(20),
  admin_notes TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### documents
```sql
CREATE TABLE documents (
  id UUID PRIMARY KEY,
  application_id UUID REFERENCES loan_applications(id),
  doc_type ENUM('aadhaar','pan','income_cert','itr','msme_cert','bank_stmt'),
  file_url VARCHAR(500),
  file_size INTEGER,
  ocr_extracted_data JSONB,
  verification_status ENUM('pending','verified','rejected'),
  uploaded_at TIMESTAMP DEFAULT NOW()
);
```

### notifications
```sql
CREATE TABLE notifications (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  type VARCHAR(50),
  subject VARCHAR(255),
  body TEXT,
  channel ENUM('email','sms','push'),
  sent_at TIMESTAMP,
  status ENUM('pending','sent','failed')
);
```

---

## 9. API Endpoints

### Authentication
```
POST   /api/auth/register         Register new MSME user
POST   /api/auth/login            Login and get JWT
POST   /api/auth/logout           Invalidate token
POST   /api/auth/verify-email     Verify email OTP
POST   /api/auth/reset-password   Send password reset email
```

### Loan Applications
```
POST   /api/loans/apply           Submit new loan application
GET    /api/loans/:id             Get application details
GET    /api/loans/user/:userId    Get all applications for user
PUT    /api/loans/:id/documents   Upload documents
GET    /api/loans/:id/status      Get application status
```

### AI Engine
```
POST   /api/ai/score              Run credit scoring on application
GET    /api/ai/explain/:loanId    Get SHAP XAI explanation
POST   /api/ai/anomaly-check      Run anomaly detection
GET    /api/ai/model-stats        Get model performance metrics
```

### Admin
```
GET    /api/admin/pending         Get all pending review applications
PUT    /api/admin/decide/:id      Approve/Reject/Request-info
GET    /api/admin/stats           Get system statistics
GET    /api/admin/anomalies       Get flagged anomaly cases
```

### EMI & Utilities
```
POST   /api/emi/calculate         Calculate EMI and schedule
GET    /api/emi/schedule/:loanId  Get amortization schedule
POST   /api/notify/email          Send email notification
```

---

## 10. Security Requirements

| ID | Requirement |
|----|-------------|
| SEC-01 | All passwords hashed with bcrypt (salt rounds: 12) |
| SEC-02 | JWT tokens expire in 24 hours |
| SEC-03 | Aadhaar/PAN stored encrypted (AES-256-GCM) |
| SEC-04 | File uploads scanned for malware before storage |
| SEC-05 | HTTPS enforced (HSTS header) |
| SEC-06 | SQL injection prevention via parameterized queries |
| SEC-07 | XSS prevention via Content-Security-Policy headers |
| SEC-08 | CSRF tokens for all state-changing requests |
| SEC-09 | Input validation on both frontend and backend |
| SEC-10 | Admin actions logged with IP and timestamp |

---

## 11. Deployment Requirements

```yaml
# docker-compose.yml (simplified)
services:
  frontend:
    image: nginx:alpine
    ports: ["80:80", "443:443"]
  
  backend:
    image: node:18-alpine
    environment:
      - DATABASE_URL=postgresql://...
      - JWT_SECRET=...
      - GMAIL_CLIENT_ID=...
      - GMAIL_CLIENT_SECRET=...
  
  ai-service:
    image: python:3.11-slim
    command: uvicorn main:app --host 0.0.0.0 --port 8001
  
  db:
    image: postgres:15
    volumes: [pgdata:/var/lib/postgresql/data]
  
  redis:
    image: redis:7-alpine
```

---

## 12. Constraints & Assumptions

1. This is a **simulation** for academic/presentation purposes
2. Gmail API requires OAuth2 credentials from Google Cloud Console
3. Aadhaar/PAN verification APIs require UIDAI/IT Dept partnerships (simulated here)
4. ML model trained on synthetic data for demo; real deployment requires actual MSME loan datasets
5. CIBIL integration requires a formal data-sharing agreement with TransUnion CIBIL

---

*End of SRS Document — Version 2.0*  
*For academic/demo use only. Not an official SBI document.*
