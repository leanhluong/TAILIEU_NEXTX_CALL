# ONBOARDING - BACKEND ENGINEER
## Call Center SaaS Platform - Backend Onboarding Guide

> [!IMPORTANT]
> Chào mừng bạn đến với Backend Team! Tài liệu này sẽ hướng dẫn bạn setup môi trường, hiểu codebase, và bắt đầu contribute.

**Vai trò:** Backend Engineer (Senior/Mid)  
**Ngày bắt đầu:** [Điền ngày]  
**Mentor:** [Tên Backend Senior]  
**Tech Stack:** .NET 10, PostgreSQL 16, Redis 7, FreeSWITCH

---

## 📋 TUẦN ĐẦU TIÊN

### Ngày 1: Welcome & Environment Setup

#### **Morning: Introduction (9:00 AM - 12:00 PM)**

**9:00 - 9:30: Welcome Meeting**
- [ ] Gặp Tech Lead/Backend Senior
- [ ] Nhận laptop & equipment
- [ ] Tour văn phòng

**9:30 - 10:30: Accounts Setup**
- [ ] Email account
- [ ] GitHub account + add to organization
- [ ] Jira account
- [ ] Slack/Discord
- [ ] VPN access (if remote)
- [ ] SSH keys generation

**10:30 - 12:00: Install Development Tools**

```powershell
# .NET 10 SDK
winget install Microsoft.DotNet.SDK.10

# Visual Studio 2022 hoặc JetBrains Rider
winget install Microsoft.VisualStudio.2022.Community
# hoặc
winget install JetBrains.Rider

# Git
winget install Git.Git

# PostgreSQL 16
winget install PostgreSQL.PostgreSQL.16

# Redis (via Docker)
docker pull redis:7-alpine

# Postman
winget install Postman.Postman
```

#### **Afternoon: Project Setup (1:00 PM - 5:00 PM)**

**1:00 - 2:00: Clone Repository**

```bash
# Clone main repository
git clone https://github.com/[org]/callcenter-saas-backend.git
cd callcenter-saas-backend

# Checkout develop branch
git checkout develop

# Create your feature branch
git checkout -b feature/onboarding-[yourname]
```

**2:00 - 3:00: Database Setup**

```sql
-- Connect to PostgreSQL
psql -U postgres

-- Create database
CREATE DATABASE callcenter_dev;

-- Create user
CREATE USER callcenter WITH PASSWORD 'dev_password';

-- Grant privileges
GRANT ALL PRIVILEGES ON DATABASE callcenter_dev TO callcenter;

-- Run migrations
dotnet ef database update
```

**3:00 - 4:00: Run Project Locally**

```bash
# Restore packages
dotnet restore

# Build solution
dotnet build

# Run API
cd src/Presentation/CallCenterSaaS.WebAPI
dotnet run

# Should see: Now listening on: https://localhost:5001
```

**4:00 - 5:00: Test API**
- [ ] Open https://localhost:5001/swagger
- [ ] Test health check endpoint
- [ ] Test authentication endpoint
- [ ] Create first user via Swagger

**Deliverables Day 1:**
- ✅ Development environment ready
- ✅ Project running locally
- ✅ Can access Swagger UI
- ✅ Database connected

---

### Ngày 2: Codebase Walkthrough

#### **Morning: Architecture Overview (9:00 AM - 12:00 PM)**

**9:00 - 10:00: Clean Architecture**

```
CallCenterSaaS/
├── src/
│   ├── Core/
│   │   ├── Domain/              # Entities, Enums, Exceptions
│   │   └── Application/         # Use Cases, DTOs, Interfaces
│   ├── Infrastructure/
│   │   └── Persistence/         # DbContext, Repositories
│   │       └── Configurations/  # EF Core configs
│   └── Presentation/
│       ├── WebAPI/              # Controllers, Middleware
│       └── WorkerService/       # Background jobs
└── tests/
    ├── UnitTests/
    └── IntegrationTests/
```

- [ ] Đọc [03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md](../Project_Documents/03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md)
- [ ] Hiểu Clean Architecture layers
- [ ] Hiểu dependency flow

**10:00 - 11:00: CQRS Pattern với MediatR**

```csharp
// Command Example
public record CreateExtensionCommand(
    string ExtensionNumber,
    string AgentName
) : IRequest<Guid>;

// Handler
public class CreateExtensionCommandHandler 
    : IRequestHandler<CreateExtensionCommand, Guid>
{
    public async Task<Guid> Handle(
        CreateExtensionCommand request, 
        CancellationToken ct)
    {
        // Implementation
    }
}

// Controller
[HttpPost]
public async Task<IActionResult> Create(
    [FromBody] CreateExtensionCommand command)
{
    var id = await _mediator.Send(command);
    return CreatedAtAction(nameof(GetById), new { id }, null);
}
```

- [ ] Hiểu CQRS pattern
- [ ] Hiểu MediatR pipeline
- [ ] Review existing commands/queries

**11:00 - 12:00: Database Schema**
- [ ] Open pgAdmin
- [ ] Review tables structure
- [ ] Understand relationships
- [ ] Review indexes

#### **Afternoon: FreeSWITCH Integration (1:00 PM - 5:00 PM)**

**1:00 - 2:00: FreeSWITCH Basics**
- [ ] Đọc [00_TONG_QUAN_FREESWITCH.md](../FreeSwitchs/00_TONG_QUAN_FREESWITCH.md)
- [ ] Hiểu SIP protocol basics
- [ ] Hiểu Extension vs Trunk
- [ ] Hiểu Dialplan

**2:00 - 3:00: mod_xml_curl**
- [ ] Đọc [NGAY_06_07_MOD_XML_CURL.md](../FreeSwitchs/NGAY_06_07_MOD_XML_CURL.md)
- [ ] Hiểu cách FreeSWITCH gọi API
- [ ] Review `FreeSwitchController.cs`
- [ ] Test directory endpoint

**3:00 - 4:00: ESL (Event Socket Layer)**
- [ ] Đọc [NGAY_08_09_EVENT_SOCKET_LAYER.md](../FreeSwitchs/NGAY_08_09_EVENT_SOCKET_LAYER.md)
- [ ] Hiểu ESL events
- [ ] Review `EslWorkerService.cs`
- [ ] Understand CDR generation

**4:00 - 5:00: Code Walkthrough với Senior**
- [ ] Walk through 1 complete feature
- [ ] From Controller → Handler → Repository
- [ ] Understand error handling
- [ ] Understand logging

**Deliverables Day 2:**
- ✅ Understand Clean Architecture
- ✅ Understand CQRS pattern
- ✅ Know FreeSWITCH basics
- ✅ Can navigate codebase

---

### Ngày 3: First Task

#### **Morning: Pick First Task (9:00 AM - 12:00 PM)**

**9:00 - 9:30: Daily Standup**
- [ ] Attend first Daily Standup
- [ ] Introduce yourself
- [ ] Listen to team updates

**9:30 - 10:30: Task Assignment**
- [ ] Review Jira board
- [ ] Pick a "Good First Issue" task
- [ ] Understand requirements
- [ ] Ask clarifying questions

**Suggested First Tasks:**
- [ ] Add new field to Extension entity
- [ ] Create simple CRUD endpoint
- [ ] Add validation rule
- [ ] Fix minor bug
- [ ] Write unit test

**10:30 - 12:00: Implementation Planning**
- [ ] Design approach
- [ ] Identify files to change
- [ ] Discuss with mentor
- [ ] Get approval to proceed

#### **Afternoon: Implementation (1:00 PM - 5:00 PM)**

**1:00 - 4:00: Coding**
- [ ] Create feature branch
- [ ] Implement changes
- [ ] Write unit tests
- [ ] Test locally

**4:00 - 5:00: Code Review Prep**
- [ ] Self-review code
- [ ] Run all tests
- [ ] Check code style
- [ ] Write commit message

```bash
# Commit convention
git commit -m "feat(extension): add email field to extension entity

- Add Email property to Extension entity
- Update database migration
- Add email validation
- Add unit tests

Closes #123"
```

**Deliverables Day 3:**
- ✅ First task implemented
- ✅ Unit tests written
- ✅ Code self-reviewed
- ✅ Ready for PR

---

### Ngày 4: Code Review & Testing

#### **Morning: Create Pull Request (9:00 AM - 12:00 PM)**

**9:00 - 10:00: Create PR**

```markdown
## Description
Add email field to Extension entity to support email notifications.

## Type of Change
- [x] New feature
- [ ] Bug fix
- [ ] Breaking change
- [ ] Documentation update

## Changes Made
- Added `Email` property to `Extension` entity
- Created migration `AddEmailToExtension`
- Added email validation in `CreateExtensionCommandValidator`
- Updated `ExtensionDto` to include email
- Added unit tests for email validation

## Testing
- [x] Unit tests pass
- [x] Integration tests pass
- [x] Tested locally with Postman
- [x] Database migration applied successfully

## Checklist
- [x] Code follows style guidelines
- [x] Self-review completed
- [x] Comments added for complex code
- [x] Unit tests added
- [x] All tests passing
- [x] Documentation updated

## Screenshots
[Add Postman screenshot if applicable]

## Related Issues
Closes #123
```

- [ ] Create PR on GitHub
- [ ] Request review from mentor
- [ ] Wait for feedback

**10:00 - 12:00: Address Feedback**
- [ ] Read review comments
- [ ] Ask questions if unclear
- [ ] Make requested changes
- [ ] Push updates

#### **Afternoon: Learn Testing (1:00 PM - 5:00 PM)**

**1:00 - 3:00: Unit Testing Workshop**

```csharp
// Example Unit Test
public class CreateExtensionCommandHandlerTests
{
    [Fact]
    public async Task Handle_ValidRequest_ReturnsExtensionId()
    {
        // Arrange
        var context = GetInMemoryDbContext();
        var handler = new CreateExtensionCommandHandler(context);
        var command = new CreateExtensionCommand("101", "John Doe");
        
        // Act
        var result = await handler.Handle(command, CancellationToken.None);
        
        // Assert
        result.Should().NotBeEmpty();
        var extension = await context.Extensions.FindAsync(result);
        extension.Should().NotBeNull();
        extension!.ExtensionNumber.Should().Be("101");
    }
    
    [Fact]
    public async Task Handle_DuplicateExtension_ThrowsException()
    {
        // Arrange
        var context = GetInMemoryDbContext();
        await SeedExtension(context, "101");
        var handler = new CreateExtensionCommandHandler(context);
        var command = new CreateExtensionCommand("101", "Jane Doe");
        
        // Act & Assert
        await Assert.ThrowsAsync<ConflictException>(
            () => handler.Handle(command, CancellationToken.None));
    }
}
```

- [ ] Understand xUnit framework
- [ ] Understand FluentAssertions
- [ ] Understand Moq for mocking
- [ ] Write 2-3 more tests

**3:00 - 5:00: Integration Testing**
- [ ] Review integration test examples
- [ ] Understand WebApplicationFactory
- [ ] Run integration tests
- [ ] Debug failing tests

**Deliverables Day 4:**
- ✅ PR created
- ✅ Feedback addressed
- ✅ Understand testing
- ✅ Tests written

---

### Ngày 5: Merge & Retrospective

#### **Morning: PR Approval & Merge (9:00 AM - 12:00 PM)**

**9:00 - 10:00: Final Review**
- [ ] All comments resolved
- [ ] All tests passing
- [ ] CI/CD pipeline green
- [ ] Get approval from reviewers

**10:00 - 11:00: Merge PR**
- [ ] Squash and merge
- [ ] Delete feature branch
- [ ] Verify in develop branch
- [ ] Close Jira ticket

**11:00 - 12:00: Documentation**
- [ ] Update API documentation
- [ ] Update README if needed
- [ ] Update migration guide

#### **Afternoon: Week 1 Retrospective (1:00 PM - 5:00 PM)**

**1:00 - 2:00: 1-on-1 với Mentor**
- [ ] Discuss what went well
- [ ] Discuss challenges
- [ ] Get feedback on code quality
- [ ] Set goals for Week 2

**2:00 - 4:00: Learn Next Topic**

**For Backend Senior:**
- [ ] FreeSWITCH advanced topics
- [ ] SignalR Hub implementation
- [ ] Billing engine design

**For Backend Mid:**
- [ ] More CQRS examples
- [ ] Advanced EF Core
- [ ] Performance optimization

**4:00 - 5:00: Plan Week 2**
- [ ] Review next sprint backlog
- [ ] Pick next tasks
- [ ] Identify learning needs

**Deliverables Week 1:**
- ✅ First PR merged
- ✅ Understand codebase structure
- ✅ Can write tests
- ✅ Know team workflow

---

## 📊 TUẦN 2-4: RAMP UP

### Week 2: More Features

**Mục tiêu:**
- [ ] Complete 2-3 tasks independently
- [ ] Participate in code reviews
- [ ] Improve test coverage
- [ ] Learn advanced patterns

**Suggested Tasks:**
- [ ] Implement CRUD for another entity
- [ ] Add caching with Redis
- [ ] Implement background job
- [ ] Optimize database query

### Week 3: Complex Features

**Mục tiêu:**
- [ ] Work on FreeSWITCH integration
- [ ] Implement SignalR feature
- [ ] Work on billing logic
- [ ] Mentor junior developer

**Suggested Tasks:**
- [ ] mod_xml_curl endpoint
- [ ] ESL event handler
- [ ] Real-time notification
- [ ] Rate calculation

### Week 4: Full Ownership

**Mục tiêu:**
- [ ] Own complete feature end-to-end
- [ ] Lead technical discussions
- [ ] Review others' code
- [ ] Improve architecture

---

## 🎯 THÁNG ĐẦU TIÊN: GOALS

### Technical Skills
- [ ] Master Clean Architecture
- [ ] Master CQRS pattern
- [ ] Understand FreeSWITCH integration
- [ ] Write quality unit tests (>70% coverage)
- [ ] Optimize database queries

### Deliverables
- [ ] 10+ PRs merged
- [ ] 5+ code reviews done
- [ ] 1 complex feature delivered
- [ ] Test coverage improved by 10%

---

## 📚 TÀI LIỆU HỌC TẬP

### Bắt buộc đọc
1. [KIEN_TRUC_VA_TECH_STACK.md](../Project_Documents/KIEN_TRUC_VA_TECH_STACK.md) ⭐
2. [03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md](../Project_Documents/03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md)
3. [ROADMAP_CHI_TIET_BACKEND.md](../Preparing_For_Project_Implementation/ROADMAP_CHI_TIET_BACKEND.md)

### External Resources
- [Clean Architecture by Jason Taylor](https://github.com/jasontaylordev/CleanArchitecture)
- [.NET 10 Documentation](https://docs.microsoft.com/en-us/dotnet/)
- [FreeSWITCH Wiki](https://freeswitch.org/confluence/)

---

## ✅ CHECKLIST

### Day 1
- [ ] Environment setup complete
- [ ] Project running locally
- [ ] Database connected

### Week 1
- [ ] First PR merged
- [ ] Understand architecture
- [ ] Can write tests

### Month 1
- [ ] 10+ PRs merged
- [ ] Deliver complex feature
- [ ] Mentor others

---

**Ngày tạo:** 15/01/2026  
**Phiên bản:** 1.0  
**Mentor:** [Tên]

> [!TIP]
> Code quality > Speed. Hãy viết code clean, test kỹ, và ask for review sớm!
