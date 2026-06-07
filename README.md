# System Design Notes (Part 1)

## What is System Design?

> "Anyone can write code that works. System Design is what makes it work for millions of users at once."

When we build an application, it may work perfectly for:

- 10 users
- 100 users
- 1000 users

But what happens when **10 million users** use it simultaneously?

System Design focuses on:

- Scalability
- Reliability
- Performance
- Availability
- Fault Tolerance

Examples:

- WhatsApp
- Instagram
- YouTube
- Amazon
- Netflix

---

# What is a System?

A System consists of:

```text
System = Components + Common Goal
```

### Components

Different parts working together:

- Database
- Server
- Cache
- Load Balancer
- APIs

### Common Goal

All components work together to solve the same problem.

Example:

Instagram's goal:

- Upload Posts
- View Posts
- Like Posts
- Comment
- Share Content

---

# Topics Covered in System Design

## Foundation

- What is System Design
- Components of System Design
- Data Intensive Applications
- Compute Intensive Applications
- Functional Requirements
- Non-Functional Requirements

## Communication

- DNS
- APIs
- REST

## Data Layer

- SQL Databases
- NoSQL Databases
- Cache

## Scaling & Distribution

- Load Balancer
- Replication
- Partitioning
- CAP Theorem

## Reliability

- Message Queues
- Monitoring
- Fault Tolerance

---

# Alien Bank Example

The instructor explains System Design using a simple bank.

## Initial Architecture

```text
Customers
    ↓
 Cash Counter
    ↓
  Cashier
```

### Customer Flow

1. Customer visits counter
2. Deposit/Withdraw money
3. Get receipt
4. Leave

---

# Issue 1: Slow Processing

### Problem

Cashier takes:

```text
10 minutes per customer
```

Therefore:

```text
6 customers/hour
```

Only 6 customers are served.

---

## Why is it Slow?

Cashier must:

- Understand customer requirement
- Count cash manually
- Create receipt manually

---

## Solution

Improve cashier performance.

Examples:

- Faster counting
- Faster typing

Result:

```text
10 min → 5 min
```

---

## System Design Concept

### Code Optimization

Equivalent to:

- Better algorithms
- Better DSA
- Better code quality
- Better implementation

Without adding hardware.

---

# Issue 2: Increasing Customers

Customer count grows rapidly.

Cashier is already optimized.

---

## Solution

Upgrade infrastructure.

Examples:

- Bigger desk
- Cash counting machine
- Customer forms before reaching counter

Result:

```text
5 min → 3 min
```

---

## System Design Concept

# Vertical Scaling

Upgrade the existing server.

Examples:

```text
8 GB RAM → 32 GB RAM

4 CPU → 16 CPU
```

Same machine becomes stronger.

---

# Issue 3: Long Waiting Time

Even with:

```text
3 minutes/customer
```

10th customer still waits:

```text
27 minutes
```

because only one counter exists.

---

## Solution

Add another counter.

```text
Counter 1
Counter 2
```

---

## System Design Concept

# Horizontal Scaling

Instead of upgrading one server:

```text
Server 1
Server 2
Server 3
```

Add more servers.

---

# Issue 4: Data Inconsistency

Now we have:

```text
Counter 1
Counter 2
```

Both maintain separate records.

---

## Problem

Customer withdraws money from Counter 1.

Counter 2 doesn't know.

Customer may withdraw again.

---

## Result

Data inconsistency.

---

## Solution

# Centralized Database

```text
Counter 1
    \
     Database
    /
Counter 2
```

Both counters access the same database.

---

## Benefits

- Single Source of Truth
- Consistent Data
- Easier Validation

---

# Issue 5: Uneven Counter Usage

Most customers go to Counter 1.

Counter 2 stays idle.

---

## Problem

```text
Counter 1 → Overloaded

Counter 2 → Underutilized
```

---

## Solution

# Load Balancer

```text
Customers
     ↓
Load Balancer
   /      \
Counter1 Counter2
```

---

## Responsibilities of Load Balancer

### 1. Health Check

Checks:

```text
Is server alive?
Is server down?
```

---

### 2. Traffic Distribution

Example:

```text
Counter1 = 10 customers

Counter2 = 5 customers
```

Next customer goes to Counter2.

---

### If One Server Fails

```text
Counter1 ❌

All traffic → Counter2
```

---

# Mapping Bank Example to System Design

| Bank Concept | System Design Concept |
|-------------|----------------------|
| Customers | Requests |
| Queue | Traffic |
| Cashier | Application Code |
| Counter | Server |
| Records | Database |
| Middleman | Load Balancer |

---

# Core Components of System Design

---

# 1. Database

Stores application data.

Examples:

- User data
- Messages
- Posts
- Orders

---

## Database Operations

CRUD:

```text
Create
Read
Update
Delete
```

---

## Types of Databases

### SQL Databases

Examples:

- MySQL
- PostgreSQL

---

### NoSQL Databases

Examples:

- MongoDB
- Cassandra

---

# 2. Application Layer

Users should never interact directly with the database.

Architecture:

```text
User
 ↓
API
 ↓
Application Code
 ↓
Database
```

---

## Purpose

- Hide database complexity
- Business logic handling
- Security
- Validation

---

## API Endpoints

Examples:

```http
GET /users

GET /products

POST /order

PUT /profile

DELETE /account
```

---

# 3. Client Application

User-facing interface.

Examples:

- Mobile App
- Website
- ATM Machine

Architecture:

```text
Client
  ↓
Application
  ↓
Database
```

---

# 4. Cache

Database calls are expensive.

Store frequently accessed data in memory.

Architecture:

```text
Client
  ↓
Application
  ↓
Cache
  ↓
Database
```

---

## Benefits

- Faster responses
- Reduced DB load
- Better user experience

---

# 5. Load Balancer

Architecture:

```text
Client
   ↓
Load Balancer
   ↓
Servers
```

---

## Responsibilities

### Health Check

Checks whether servers are alive.

### Request Distribution

Distributes traffic evenly.

---

# 6. Message Queue

Used for asynchronous communication.

Example:

Order Placed:

Tasks:

- Update inventory
- Send email
- Send SMS
- Notify vendor

Architecture:

```text
Order Service
      ↓
 Message Queue
      ↓
  SMS Service
```

---

## Benefits

- Decoupling
- Reliability
- Retry Mechanisms
- Better Performance

---

## Examples

- Kafka
- RabbitMQ
- AWS SQS

---

# Synchronous vs Asynchronous

## Synchronous

Wait for response.

```text
Request
   ↓
Response
```

---

## Asynchronous

Don't wait.

```text
Request
   ↓
Queue
   ↓
Continue Working
```

---

# 7. Monitoring & Logs

Large systems require monitoring.

---

## Why?

Monitor:

- Server Health
- API Errors
- Database Failures
- User Activity
- Traffic

---

## Benefits

- Bug Detection
- Debugging
- Reliability
- Performance Tracking

---

# Types of Applications

System Design broadly divides applications into:

```text
1. Data Intensive Applications

2. Compute Intensive Applications
```

---

# Data Intensive Applications

Focus:

```text
Data Storage
Data Retrieval
Data Movement
```

Not heavy calculations.

---

## Examples

- Instagram Feed
- WhatsApp Messages
- Banking Systems
- Analytics Dashboards
- Log Processing

---

## Common Problems

- Slow database queries
- Network delays
- Server bottlenecks

---

## Main Concerns

### Fast Reads

Data should be fetched quickly.

### Safe Storage

Data should never be lost.

### Concurrent Users

Millions of users can access simultaneously.

### Fault Tolerance

Should survive failures.

---

## Solutions

- Caching
- Replication
- Sharding
- Consistency Mechanisms

---

# Instagram Example

Operations:

- Fetch posts
- Sort posts
- Like
- Comment
- Share

Challenge:

```text
Millions of users

Millions of posts
```

---

## How Instagram Scales

### Multiple Databases

Store huge data.

### Aggressive Caching

Fetch data faster.

### CDN

Store images/videos separately.

Database stores only references.

---

## Rule

If time is lost in:

```text
Moving Data
```

Then it is:

# Data Intensive

---

# Compute Intensive Applications

Focus:

```text
Heavy Computation
```

Data volume is smaller.

Processing requirements are huge.

---

## Examples

- Machine Learning Training
- Image Processing
- Video Rendering
- Cryptography
- Simulations

---

## Main Concerns

### Fast Computation

Use better processors.

### Parallel Processing

Multiple computations together.

### Lower Cost

Reduce processing cost.

### GPU Utilization

Use GPU when needed.

---

## Solutions

- Better CPU
- Better GPU
- Better Algorithms
- Parallel Processing

---

# Flight Simulator Example

Goal:

Train pilots safely.

Requirements:

- Realistic physics
- Real-time calculations
- Fast processing

Needs:

```text
CPU
GPU
Algorithms
```

More than databases.

---

# Quick Trick

## Data Intensive

Time lost in:

```text
Data Movement
```

Examples:

- Instagram
- WhatsApp

---

## Compute Intensive

Time lost in:

```text
Calculations
```

Examples:

- AI Training
- Video Rendering

---

# Functional Requirements

Defines:

> What the system should do.

---

## Amazon Example

Features:

- Register
- Login
- View Products
- Search Products
- Filter Products
- Add to Cart
- Apply Coupons
- Make Payment
- Track Order

These are Functional Requirements.

---

# Non-Functional Requirements

Defines:

> How the system should perform.

---

## Amazon Example

### Scalability

```text
1 Million Daily Active Users
```

---

### Performance

```text
Response Time < 200 ms
```

---

### Availability

```text
99.9% Uptime
```

---

### Security

- Authentication
- Authorization
- Encryption

---

### Traffic Spikes

Handle:

```text
10x traffic during sales
```

---

### Fault Tolerance

System should continue working during failures.

---

# Common Non-Functional Requirements

| Requirement | Meaning |
|------------|---------|
| Scalability | Handle growth |
| Availability | System uptime |
| Reliability | No data loss |
| Performance | Fast response |
| Security | Protect data |
| Maintainability | Easy updates |
| Observability | Monitoring & Logging |
| Fault Tolerance | Survive failures |

---

# DNS (Domain Name System)

Humans use:

```text
google.com
youtube.com
amazon.com
```

Computers use:

```text
IP Addresses
```

---

## Purpose of DNS

Convert:

```text
google.com
```

into

```text
IP Address
```

---

## DNS Flow

```text
Browser
   ↓
 DNS
   ↓
 IP Address
   ↓
 Website
```

---

# Domain vs Subdomain

## Domain

```text
google.com
```

---

## Subdomain

```text
mail.google.com

docs.google.com
```

---

# Why Not Store All Domains in Browser?

Because:

- More than 350 million domains exist.
- Storage requirement is huge.
- IP addresses change frequently.

Therefore DNS is required.

---

# Final Revision Sheet

```text
System = Components + Common Goal

Vertical Scaling = Upgrade Existing Server

Horizontal Scaling = Add More Servers

Database = Store Data

API = Access Data

Client = User Interface

Cache = Faster Data Access

Load Balancer =
    Health Check +
    Traffic Distribution

Message Queue =
    Asynchronous Communication

Monitoring =
    Error Tracking +
    Performance Tracking

Data Intensive =
    Data Movement Problem

Compute Intensive =
    Computation Problem

Functional Requirement =
    What System Does

Non-Functional Requirement =
    How System Performs

DNS =
    Domain → IP Address
```"# System-Design-Notes-" 
