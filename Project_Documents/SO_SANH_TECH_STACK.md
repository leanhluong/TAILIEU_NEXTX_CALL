# SO SÁNH TECH STACK
## Phân tích chi tiết lựa chọn công nghệ

> [!IMPORTANT]
> Tài liệu này giải thích chi tiết tại sao chọn từng công nghệ và so sánh với các lựa chọn thay thế

**Ngày tạo:** 06/01/2026

---

## 1. BACKEND FRAMEWORK

### .NET 10 vs Các lựa chọn khác

#### So sánh Performance (TechEmpower Benchmark)

| Framework | Requests/sec | Latency (ms) | Memory (MB) | Verdict |
|-----------|--------------|--------------|-------------|---------|
| **.NET 10** | **7,000,000** | **0.5** | **50** | ✅ **Winner** |
| Go (Gin) | 6,500,000 | 0.6 | 30 | ⭐ Very Good |
| Rust (Actix) | 7,200,000 | 0.4 | 25 | ⭐ Best Performance |
| Node.js (Fastify) | 1,500,000 | 2.0 | 80 | ⚠️ Slower |
| Java (Spring Boot) | 3,000,000 | 1.5 | 200 | ⚠️ Heavy |
| Python (FastAPI) | 500,000 | 5.0 | 100 | ❌ Too Slow |

#### Tại sao KHÔNG chọn các framework khác?

**Rust (Actix-web)**
- ✅ Performance cao nhất
- ✅ Memory safety
- ❌ **Learning curve dốc** - Team mất 3-6 tháng để productive
- ❌ **Development speed chậm** - Borrow checker, lifetime
- ❌ **Ecosystem nhỏ** - Ít libraries cho telephony
- ❌ **Ít developers** - Khó tuyển

**Go (Gin, Echo)**
- ✅ Performance cao
- ✅ Concurrency tốt (goroutines)
- ✅ Simple syntax
- ❌ **Ecosystem nhỏ hơn .NET** - Ít libraries enterprise
- ❌ **No generics** (trước Go 1.18)
- ❌ **Error handling verbose**
- ⚠️ **Ít developers Việt Nam**

**Node.js (Express, NestJS, Fastify)**
- ✅ JavaScript/TypeScript - Dễ tuyển developers
- ✅ NPM ecosystem lớn
- ✅ Tốt cho I/O-bound tasks
- ❌ **Single-threaded** - Không tốt cho CPU-intensive
- ❌ **Performance thấp** - Chậm hơn .NET 7x
- ❌ **Type safety yếu** - TypeScript vẫn runtime errors
- ❌ **Callback hell** (đã giải quyết với async/await)

**Java (Spring Boot)**
- ✅ Enterprise-proven
- ✅ Ecosystem lớn
- ✅ Mature
- ❌ **Verbose code** - Nhiều boilerplate
- ❌ **Startup time chậm** - JVM warmup
- ❌ **Memory footprint lớn** - 200MB+ cho simple app
- ❌ **Development speed chậm** - Compile time

**Python (Django, FastAPI)**
- ✅ Easy to learn
- ✅ Tốt cho ML, Data Science
- ❌ **Performance rất thấp** - GIL (Global Interpreter Lock)
- ❌ **Không phù hợp real-time telephony**
- ❌ **Type hints không enforce** - Runtime errors
- ❌ **Async support yếu**

#### Tại sao chọn .NET 10?

✅ **Performance**
- Top 3 fastest frameworks (TechEmpower)
- Native AOT compilation
- Minimal API overhead

✅ **Productivity**
- C# 12 - Modern language features
- LINQ - Expressive queries
- Built-in DI, logging, configuration
- Hot reload

✅ **Ecosystem**
- NuGet packages phong phú
- Entity Framework Core - Best ORM
- SignalR - Real-time built-in
- MediatR, FluentValidation, AutoMapper

✅ **Enterprise-Ready**
- Microsoft backing
- Long-term support (LTS)
- Security updates
- Azure integration (future)

✅ **Team**
- Nhiều .NET developers Việt Nam
- Dễ tuyển
- Mature community

✅ **Cross-platform**
- Linux (Debian 12)
- Docker support tốt
- Cloud-native

---

## 2. FRONTEND FRAMEWORK

### Next.js 15 vs React vs Vue vs Angular

#### So sánh tính năng

| Feature | Next.js 15 | React (Vite) | Vue (Nuxt) | Angular |
|---------|------------|--------------|------------|---------|
| **SSR** | ✅ Built-in | ❌ Manual | ✅ Built-in | ✅ Built-in |
| **SEO** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **File Routing** | ✅ Auto | ❌ Manual | ✅ Auto | ❌ Manual |
| **API Routes** | ✅ Built-in | ❌ No | ✅ Built-in | ❌ No |
| **Image Optimization** | ✅ Auto | ❌ Manual | ⚠️ Plugin | ❌ Manual |
| **Code Splitting** | ✅ Auto | ⚠️ Manual | ✅ Auto | ✅ Auto |
| **Learning Curve** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| **Ecosystem** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |

#### Tại sao KHÔNG chọn frameworks khác?

**React (CRA, Vite) - SPA**
- ✅ Ecosystem lớn nhất
- ✅ Dễ học
- ✅ Nhiều developers
- ❌ **Client-side only** - SEO kém
- ❌ **Initial load chậm** - Download full bundle
- ❌ **Cần setup SSR manually** - react-router, data fetching
- ❌ **No built-in optimization**

**Vue.js + Nuxt.js**
- ✅ Tương tự Next.js (SSR, file routing)
- ✅ Dễ học hơn React
- ✅ Performance tốt
- ❌ **Ecosystem nhỏ hơn React** - Ít libraries
- ❌ **Ít developers Việt Nam**
- ❌ **Job market nhỏ hơn**

**Angular**
- ✅ Full framework (opinionated)
- ✅ TypeScript native
- ✅ Enterprise adoption
- ❌ **Steep learning curve** - RxJS, Dependency Injection
- ❌ **Verbose code** - Decorators, modules
- ❌ **Bundle size lớn**
- ❌ **Ít flexibility**

**Svelte + SvelteKit**
- ✅ Performance tốt nhất (compile-time)
- ✅ Less code
- ✅ No virtual DOM
- ❌ **Ecosystem nhỏ** - Ít libraries
- ❌ **Ít developers**
- ❌ **Chưa mature** - v1.0 mới ra 2021

#### Tại sao chọn Next.js 15?

✅ **SEO-Friendly**
- Server-Side Rendering (SSR)
- Static Site Generation (SSG)
- Incremental Static Regeneration (ISR)

✅ **Performance**
- Automatic code splitting
- Image optimization (WebP, lazy load)
- Font optimization
- Edge runtime

✅ **Developer Experience**
- File-based routing
- API routes (Backend for Frontend)
- Hot reload
- TypeScript support

✅ **Production-Ready**
- Vercel deployment (1-click)
- Edge caching
- Analytics built-in

✅ **React Ecosystem**
- Tất cả React libraries hoạt động
- Nhiều developers
- Large community

---

## 3. DATABASE

### PostgreSQL vs MySQL vs MongoDB vs SQL Server

#### So sánh tính năng

| Feature | PostgreSQL | MySQL | MongoDB | SQL Server |
|---------|------------|-------|---------|------------|
| **JSONB** | ✅ Native | ⚠️ JSON | ✅ Native | ⚠️ JSON |
| **Full-text Search** | ✅ Built-in | ⚠️ Limited | ✅ Text Index | ✅ Built-in |
| **Partitioning** | ✅ Declarative | ✅ Manual | ✅ Sharding | ✅ Built-in |
| **Replication** | ✅ Streaming | ✅ Binary Log | ✅ Replica Set | ✅ Always On |
| **ACID** | ✅ Full | ✅ Full | ✅ (v4.0+) | ✅ Full |
| **Performance** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Cost** | Free | Free | Free | $$$ |

#### Tại sao KHÔNG chọn databases khác?

**MySQL/MariaDB**
- ✅ Popular, easy to learn
- ✅ Good performance
- ❌ **JSONB support kém** - Không index được
- ❌ **Full-text search limited**
- ❌ **Ít features** - No window functions (trước 8.0)
- ⚠️ **Oracle ownership** (MySQL)

**MongoDB**
- ✅ Flexible schema
- ✅ Horizontal scaling tốt
- ✅ JSONB native
- ❌ **No ACID transactions** (trước v4.0)
- ❌ **No JOIN** - Phải denormalize
- ❌ **Không phù hợp billing** - Cần ACID cho transactions
- ❌ **Memory hungry**

**SQL Server**
- ✅ Enterprise features tốt
- ✅ Integration với .NET
- ✅ Management tools tốt
- ❌ **Licensing cost** - $$$
- ❌ **Windows-only** (Express trên Linux limited)
- ❌ **Overkill cho startup**

**Oracle**
- ❌ **Very expensive**
- ❌ **Overkill**

#### Tại sao chọn PostgreSQL?

✅ **Features**
- JSONB support (IVR flows)
- Full-text search (CDR search)
- Partitioning (CDR tables by month)
- Materialized views (reports)
- Window functions
- CTEs (Common Table Expressions)

✅ **Performance**
- Faster than MySQL for complex queries
- Better indexing (GIN, GiST, BRIN)
- Parallel queries
- MVCC (Multi-Version Concurrency Control)

✅ **Reliability**
- ACID compliant
- Point-in-time recovery
- Streaming replication

✅ **Extensibility**
- Extensions (pg_stat_statements, pgcrypto)
- Custom functions
- Triggers

✅ **Cost**
- Free, open source
- No licensing

---

## 4. MESSAGE QUEUE

### RabbitMQ vs Redis Pub/Sub vs Kafka vs AWS SQS

#### So sánh

| Feature | RabbitMQ | Redis Pub/Sub | Kafka | AWS SQS |
|---------|----------|---------------|-------|---------|
| **Persistence** | ✅ Disk | ❌ Memory | ✅ Disk | ✅ Cloud |
| **Acknowledgments** | ✅ Yes | ❌ No | ✅ Yes | ✅ Yes |
| **Routing** | ✅ Flexible | ❌ Simple | ⚠️ Topics | ⚠️ Limited |
| **Dead Letter Queue** | ✅ Yes | ❌ No | ❌ No | ✅ Yes |
| **Management UI** | ✅ Built-in | ❌ No | ⚠️ 3rd party | ✅ AWS Console |
| **Complexity** | ⭐⭐⭐ | ⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| **Cost** | Free | Free | Free | $$$ |

#### Tại sao chọn RabbitMQ?

✅ **Reliability**
- Message persistence
- Acknowledgments
- Dead letter queues

✅ **Routing**
- Exchanges (direct, topic, fanout, headers)
- Flexible routing rules
- Message filtering

✅ **Features**
- Priority queues
- Delayed messages
- Message TTL
- Clustering

✅ **Management**
- Web UI
- Monitoring
- Easy to debug

**Use cases trong dự án:**
- CDR processing (async)
- Recording conversion (async)
- Email notifications
- SMS notifications
- Billing calculations

---

## 5. REAL-TIME

### SignalR vs Socket.IO vs WebSocket (raw)

#### So sánh

| Feature | SignalR | Socket.IO | WebSocket |
|---------|---------|-----------|-----------|
| **Fallback** | ✅ SSE, Long Polling | ✅ Long Polling | ❌ No |
| **Auto Reconnect** | ✅ Yes | ✅ Yes | ❌ Manual |
| **.NET Integration** | ✅ Native | ❌ 3rd party | ⚠️ Manual |
| **TypeScript Client** | ✅ Yes | ✅ Yes | ✅ Yes |
| **Backplane** | ✅ Redis | ✅ Redis | ❌ Manual |
| **Strongly-typed** | ✅ Yes | ❌ No | ❌ No |

#### Tại sao chọn SignalR?

✅ **Native .NET**
- Built-in ASP.NET Core
- No additional dependencies

✅ **Reliability**
- Automatic reconnection
- Fallback transports

✅ **Scalability**
- Redis backplane
- Azure SignalR Service (future)

✅ **Developer Experience**
- Strongly-typed hubs
- TypeScript client
- Easy to use

---

## KẾT LUẬN

Tech stack được chọn để **cân bằng**:
- ⚡ **Performance** - Top-tier frameworks
- 💰 **Cost** - Open source, no licensing
- 👥 **Team** - Dễ tuyển developers
- 📈 **Scalability** - Dễ scale horizontal
- 🚀 **Productivity** - Fast development

**Phù hợp cho:** Startup/SMB với tham vọng scale lên Enterprise.
