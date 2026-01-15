# ROADMAP CHI TIẾT CHO BACKEND ENGINEER
## Call Center SaaS Platform - Backend Detailed Roadmap

> [!IMPORTANT]
> Roadmap chi tiết cho vị trí Backend Engineer (Senior & Mid) trong dự án Call Center SaaS Platform. Bao gồm công việc từng tuần, code examples, và best practices.

**Phiên bản:** 1.0  
**Ngày tạo:** 15/01/2026  
**Thời gian:** 12 tuần (6 sprints)  
**Tech Stack:** .NET 10, PostgreSQL 16, Redis 7, RabbitMQ, SignalR, FreeSWITCH

---

## 📋 MỤC LỤC

1. [Tổng quan vai trò Backend](#1-tổng-quan-vai-trò-backend)
2. [Tech Stack chi tiết](#2-tech-stack-chi-tiết)
3. [Roadmap chi tiết từng tuần](#3-roadmap-chi-tiết-từng-tuần)
4. [Code Examples & Patterns](#4-code-examples--patterns)
5. [Best Practices](#5-best-practices)
6. [Learning Resources](#6-learning-resources)

---

## 1. TỔNG QUAN VAI TRÒ BACKEND

### 1.1. Phân chia công việc

#### **Backend Senior (40h/tuần)**
- Architecture design & decision making
- Core API development (mod_xml_curl, ESL)
- FreeSWITCH integration
- SignalR Hub implementation
- Code review
- Mentoring Backend Mid
- Performance optimization

#### **Backend Mid (40h/tuần)**
- CRUD APIs implementation
- Database migrations
- Worker Services
- Unit testing
- Bug fixing
- Documentation

### 1.2. Deliverables chính

| Deliverable | Owner | Deadline |
|-------------|-------|----------|
| **Clean Architecture Setup** | Senior | Week 1 |
| **mod_xml_curl Integration** | Senior | Week 2 |
| **Extension CRUD API** | Mid | Week 2 |
| **ESL Worker Service** | Senior | Week 4 |
| **Billing Engine** | Senior | Week 4 |
| **CDR Processing** | Mid | Week 5 |
| **IVR Engine** | Senior | Week 6 |
| **SignalR Hub** | Senior | Week 7 |
| **WebRTC Support** | Senior | Week 9 |

---

## 2. TECH STACK CHI TIẾT

### 2.1. Core Framework

```
.NET 10 (LTS)
├── ASP.NET Core Web API
├── Entity Framework Core 10
├── MediatR 12.x (CQRS)
├── FluentValidation 11.x
├── AutoMapper 12.x
├── SignalR
├── Serilog (Logging)
└── xUnit (Testing)
```

### 2.2. Libraries & Packages

**NuGet Packages cần cài:**
```xml
<!-- Core -->
<PackageReference Include="Microsoft.AspNetCore.OpenApi" Version="10.0.0" />
<PackageReference Include="Swashbuckle.AspNetCore" Version="6.5.0" />

<!-- Database -->
<PackageReference Include="Npgsql.EntityFrameworkCore.PostgreSQL" Version="10.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="10.0.0" />

<!-- CQRS & Validation -->
<PackageReference Include="MediatR" Version="12.2.0" />
<PackageReference Include="FluentValidation.AspNetCore" Version="11.3.0" />
<PackageReference Include="AutoMapper.Extensions.Microsoft.DependencyInjection" Version="12.0.1" />

<!-- Authentication -->
<PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="10.0.0" />

<!-- Real-time -->
<PackageReference Include="Microsoft.AspNetCore.SignalR" Version="10.0.0" />

<!-- Caching -->
<PackageReference Include="StackExchange.Redis" Version="2.7.10" />

<!-- Message Queue -->
<PackageReference Include="RabbitMQ.Client" Version="6.8.1" />

<!-- Storage -->
<PackageReference Include="Minio" Version="6.0.2" />

<!-- FreeSWITCH -->
<PackageReference Include="NEventSocket" Version="3.0.0" />

<!-- Logging -->
<PackageReference Include="Serilog.AspNetCore" Version="8.0.0" />
<PackageReference Include="Serilog.Sinks.PostgreSQL" Version="2.3.0" />

<!-- Testing -->
<PackageReference Include="xUnit" Version="2.6.5" />
<PackageReference Include="Moq" Version="4.20.70" />
<PackageReference Include="FluentAssertions" Version="6.12.0" />
```

---

## 3. ROADMAP CHI TIẾT TỪNG TUẦN

### **TUẦN 0: Preparation**

#### **Học FreeSWITCH (Backend Senior - 40h)**

**Ngày 1-2: Cơ bản (16h)**
- [ ] Đọc [00_TONG_QUAN_FREESWITCH.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/FreeSwitchs/00_TONG_QUAN_FREESWITCH.md)
- [ ] Đọc [NGAY_01_02_CAI_DAT_FREESWITCH.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/FreeSwitchs/NGAY_01_02_CAI_DAT_FREESWITCH.md)
- [ ] Cài FreeSWITCH trên WSL2/Docker
- [ ] Test SIP registration với Softphone

**Ngày 3-4: mod_xml_curl (16h)**
- [ ] Đọc [NGAY_06_07_MOD_XML_CURL.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/FreeSwitchs/NGAY_06_07_MOD_XML_CURL.md)
- [ ] Hiểu cách FreeSWITCH gọi API
- [ ] Hiểu XML response format
- [ ] Test với mock API

**Ngày 5: ESL (8h)**
- [ ] Đọc [NGAY_08_09_EVENT_SOCKET_LAYER.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/FreeSwitchs/NGAY_08_09_EVENT_SOCKET_LAYER.md)
- [ ] Hiểu ESL events
- [ ] Test với NEventSocket library

#### **Setup Environment (Backend Mid - 40h)**

**Ngày 1-2: .NET Setup (16h)**
- [ ] Cài .NET 10 SDK
- [ ] Cài Visual Studio 2022 / Rider
- [ ] Cài PostgreSQL local
- [ ] Cài Redis local
- [ ] Cài Postman

**Ngày 3-4: Đọc tài liệu (16h)**
- [ ] Đọc [03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/Project_Documents/03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md)
- [ ] Hiểu Clean Architecture
- [ ] Hiểu Database schema
- [ ] Hiểu API design

**Ngày 5: Practice (8h)**
- [ ] Tạo demo .NET project
- [ ] Practice EF Core migrations
- [ ] Practice MediatR pattern

---

### **SPRINT 1: Infrastructure & Authentication (Tuần 1-2)**

#### **Tuần 1: Project Setup & Database**

**Backend Senior (40h)**

**Ngày 1-2: Solution Structure (16h)**

```bash
# Create solution
dotnet new sln -n CallCenterSaaS

# Create projects
dotnet new classlib -n CallCenterSaaS.Domain -o src/Core/CallCenterSaaS.Domain
dotnet new classlib -n CallCenterSaaS.Application -o src/Core/CallCenterSaaS.Application
dotnet new classlib -n CallCenterSaaS.Infrastructure -o src/Infrastructure/CallCenterSaaS.Infrastructure
dotnet new webapi -n CallCenterSaaS.WebAPI -o src/Presentation/CallCenterSaaS.WebAPI
dotnet new worker -n CallCenterSaaS.WorkerService -o src/Presentation/CallCenterSaaS.WorkerService

# Add projects to solution
dotnet sln add src/Core/CallCenterSaaS.Domain
dotnet sln add src/Core/CallCenterSaaS.Application
dotnet sln add src/Infrastructure/CallCenterSaaS.Infrastructure
dotnet sln add src/Presentation/CallCenterSaaS.WebAPI
dotnet sln add src/Presentation/CallCenterSaaS.WorkerService

# Add project references
dotnet add src/Core/CallCenterSaaS.Application reference src/Core/CallCenterSaaS.Domain
dotnet add src/Infrastructure/CallCenterSaaS.Infrastructure reference src/Core/CallCenterSaaS.Application
dotnet add src/Presentation/CallCenterSaaS.WebAPI reference src/Infrastructure/CallCenterSaaS.Infrastructure
```

**Ngày 3-4: Database Setup (16h)**

```csharp
// Domain/Entities/Tenant.cs
public class Tenant
{
    public Guid Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public string Domain { get; set; } = string.Empty;
    public decimal Balance { get; set; }
    public int MaxAgents { get; set; }
    public int MaxConcurrentCalls { get; set; }
    public bool IsActive { get; set; } = true;
    public bool AutoRecord { get; set; } = true;
    public DateTime CreatedAt { get; set; }
    public DateTime UpdatedAt { get; set; }
    
    // Navigation properties
    public ICollection<User> Users { get; set; } = new List<User>();
    public ICollection<Extension> Extensions { get; set; } = new List<Extension>();
}

// Infrastructure/Persistence/ApplicationDbContext.cs
public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
    
    public DbSet<Tenant> Tenants { get; set; }
    public DbSet<User> Users { get; set; }
    public DbSet<Extension> Extensions { get; set; }
    public DbSet<CallDetailRecord> CallDetailRecords { get; set; }
    
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());
        
        // Global query filter for multi-tenancy
        modelBuilder.Entity<Extension>().HasQueryFilter(e => !e.IsDeleted);
    }
}

// Create migration
dotnet ef migrations add InitialCreate -p src/Infrastructure/CallCenterSaaS.Infrastructure -s src/Presentation/CallCenterSaaS.WebAPI
dotnet ef database update -p src/Infrastructure/CallCenterSaaS.Infrastructure -s src/Presentation/CallCenterSaaS.WebAPI
```

**Ngày 5: JWT Authentication (8h)**

```csharp
// Application/Features/Auth/Commands/Login/LoginCommand.cs
public record LoginCommand(string Email, string Password) : IRequest<LoginResponse>;

public class LoginCommandHandler : IRequestHandler<LoginCommand, LoginResponse>
{
    private readonly ApplicationDbContext _context;
    private readonly IConfiguration _configuration;
    
    public async Task<LoginResponse> Handle(LoginCommand request, CancellationToken ct)
    {
        var user = await _context.Users
            .Include(u => u.Tenant)
            .FirstOrDefaultAsync(u => u.Email == request.Email, ct);
            
        if (user == null || !BCrypt.Net.BCrypt.Verify(request.Password, user.PasswordHash))
            throw new UnauthorizedException("Invalid credentials");
            
        var token = GenerateJwtToken(user);
        var refreshToken = GenerateRefreshToken();
        
        // Save refresh token
        user.RefreshToken = refreshToken;
        user.RefreshTokenExpiry = DateTime.UtcNow.AddDays(7);
        await _context.SaveChangesAsync(ct);
        
        return new LoginResponse
        {
            AccessToken = token,
            RefreshToken = refreshToken,
            ExpiresIn = 86400,
            User = new UserDto
            {
                Id = user.Id,
                Email = user.Email,
                Role = user.Role,
                TenantId = user.TenantId
            }
        };
    }
    
    private string GenerateJwtToken(User user)
    {
        var claims = new[]
        {
            new Claim(JwtRegisteredClaimNames.Sub, user.Id.ToString()),
            new Claim(JwtRegisteredClaimNames.Email, user.Email),
            new Claim(ClaimTypes.Role, user.Role),
            new Claim("tenant_id", user.TenantId.ToString())
        };
        
        var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_configuration["Jwt:Key"]!));
        var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);
        
        var token = new JwtSecurityToken(
            issuer: _configuration["Jwt:Issuer"],
            audience: _configuration["Jwt:Audience"],
            claims: claims,
            expires: DateTime.UtcNow.AddHours(24),
            signingCredentials: creds
        );
        
        return new JwtSecurityTokenHandler().WriteToken(token);
    }
}
```

**Backend Mid (40h)**

**Ngày 1-3: Entities & Migrations (24h)**

```csharp
// Domain/Entities/Extension.cs
public class Extension
{
    public Guid Id { get; set; }
    public Guid TenantId { get; set; }
    public Guid? UserId { get; set; }
    public string ExtensionNumber { get; set; } = string.Empty;
    public string PasswordHash { get; set; } = string.Empty;
    public string AgentName { get; set; } = string.Empty;
    public string? Email { get; set; }
    public bool IsActive { get; set; } = true;
    public bool IsDeleted { get; set; } = false;
    public DateTime CreatedAt { get; set; }
    public DateTime UpdatedAt { get; set; }
    
    // Navigation
    public Tenant Tenant { get; set; } = null!;
    public User? User { get; set; }
}

// Infrastructure/Persistence/Configurations/ExtensionConfiguration.cs
public class ExtensionConfiguration : IEntityTypeConfiguration<Extension>
{
    public void Configure(EntityTypeBuilder<Extension> builder)
    {
        builder.HasKey(e => e.Id);
        
        builder.Property(e => e.ExtensionNumber)
            .HasMaxLength(10)
            .IsRequired();
            
        builder.HasIndex(e => new { e.TenantId, e.ExtensionNumber })
            .IsUnique()
            .HasFilter("is_deleted = false");
            
        builder.HasOne(e => e.Tenant)
            .WithMany(t => t.Extensions)
            .HasForeignKey(e => e.TenantId)
            .OnDelete(DeleteBehavior.Cascade);
    }
}
```

**Ngày 4-5: Extension CRUD API (16h)**

```csharp
// Application/Features/Extensions/Commands/CreateExtension/CreateExtensionCommand.cs
public record CreateExtensionCommand(
    string ExtensionNumber,
    string AgentName,
    string? Email,
    string? Password
) : IRequest<Guid>;

public class CreateExtensionCommandValidator : AbstractValidator<CreateExtensionCommand>
{
    public CreateExtensionCommandValidator()
    {
        RuleFor(x => x.ExtensionNumber)
            .NotEmpty()
            .Matches(@"^\d{3,5}$")
            .WithMessage("Extension must be 3-5 digits");
            
        RuleFor(x => x.AgentName)
            .NotEmpty()
            .MaximumLength(255);
            
        RuleFor(x => x.Email)
            .EmailAddress()
            .When(x => !string.IsNullOrEmpty(x.Email));
    }
}

public class CreateExtensionCommandHandler : IRequestHandler<CreateExtensionCommand, Guid>
{
    private readonly ApplicationDbContext _context;
    private readonly ICurrentUserService _currentUser;
    
    public async Task<Guid> Handle(CreateExtensionCommand request, CancellationToken ct)
    {
        var tenantId = _currentUser.TenantId;
        
        // Check if extension already exists
        var exists = await _context.Extensions
            .AnyAsync(e => e.TenantId == tenantId && e.ExtensionNumber == request.ExtensionNumber, ct);
            
        if (exists)
            throw new ConflictException($"Extension {request.ExtensionNumber} already exists");
            
        // Generate password if not provided
        var password = request.Password ?? GenerateRandomPassword();
        
        var extension = new Extension
        {
            Id = Guid.NewGuid(),
            TenantId = tenantId,
            ExtensionNumber = request.ExtensionNumber,
            PasswordHash = BCrypt.Net.BCrypt.HashPassword(password),
            AgentName = request.AgentName,
            Email = request.Email,
            CreatedAt = DateTime.UtcNow,
            UpdatedAt = DateTime.UtcNow
        };
        
        _context.Extensions.Add(extension);
        await _context.SaveChangesAsync(ct);
        
        return extension.Id;
    }
}
```

#### **Tuần 2: mod_xml_curl Integration**

**Backend Senior (40h)**

**Ngày 1-3: Directory Handler (24h)**

```csharp
// Infrastructure/FreeSWITCH/XmlCurlService.cs
public class XmlCurlService : IXmlCurlService
{
    private readonly ApplicationDbContext _context;
    private readonly IDistributedCache _cache;
    private readonly ILogger<XmlCurlService> _logger;
    
    public async Task<string> HandleDirectoryRequest(DirectoryRequest request)
    {
        _logger.LogInformation("Directory request for user: {User}, domain: {Domain}", 
            request.User, request.Domain);
            
        // Try cache first
        var cacheKey = $"directory:{request.Domain}:{request.User}";
        var cachedXml = await _cache.GetStringAsync(cacheKey);
        if (cachedXml != null)
        {
            _logger.LogInformation("Cache hit for {CacheKey}", cacheKey);
            return cachedXml;
        }
        
        // Parse domain to get tenant
        var tenantDomain = request.Domain.Replace(".pbx.local", "");
        var tenant = await _context.Tenants
            .FirstOrDefaultAsync(t => t.Domain == tenantDomain);
            
        if (tenant == null)
        {
            _logger.LogWarning("Tenant not found for domain: {Domain}", tenantDomain);
            return GenerateNotFoundXml();
        }
        
        // Find extension
        var extension = await _context.Extensions
            .FirstOrDefaultAsync(e => 
                e.TenantId == tenant.Id && 
                e.ExtensionNumber == request.User &&
                e.IsActive);
                
        if (extension == null)
        {
            _logger.LogWarning("Extension not found: {Extension}", request.User);
            return GenerateNotFoundXml();
        }
        
        // Generate XML
        var xml = GenerateDirectoryXml(extension, tenant);
        
        // Cache for 10 minutes
        await _cache.SetStringAsync(cacheKey, xml, new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10)
        });
        
        return xml;
    }
    
    private string GenerateDirectoryXml(Extension extension, Tenant tenant)
    {
        return $@"<?xml version=""1.0"" encoding=""UTF-8"" standalone=""no""?>
<document type=""freeswitch/xml"">
  <section name=""directory"">
    <domain name=""{tenant.Domain}.pbx.local"">
      <params>
        <param name=""dial-string"" value=""{{presence_id=${{dialed_user}}@${{dialed_domain}}}}${{sofia_contact(${{dialed_user}}@${{dialed_domain}})}}""/>
      </params>
      <groups>
        <group name=""default"">
          <users>
            <user id=""{extension.ExtensionNumber}"">
              <params>
                <param name=""password"" value=""{extension.PasswordHash}""/>
                <param name=""vm-password"" value=""1234""/>
              </params>
              <variables>
                <variable name=""tenant_id"" value=""{tenant.Id}""/>
                <variable name=""extension_id"" value=""{extension.Id}""/>
                <variable name=""user_context"" value=""default""/>
                <variable name=""effective_caller_id_name"" value=""{extension.AgentName}""/>
                <variable name=""effective_caller_id_number"" value=""{extension.ExtensionNumber}""/>
              </variables>
            </user>
          </users>
        </group>
      </groups>
    </domain>
  </section>
</document>";
    }
}

// WebAPI/Controllers/FreeSwitchController.cs
[ApiController]
[Route("api/freeswitch")]
public class FreeSwitchController : ControllerBase
{
    private readonly IXmlCurlService _xmlCurlService;
    
    [HttpPost("configuration")]
    public async Task<IActionResult> Configuration([FromForm] FreeSwitchRequest request)
    {
        if (request.Section == "directory")
        {
            var xml = await _xmlCurlService.HandleDirectoryRequest(new DirectoryRequest
            {
                User = request.User,
                Domain = request.Domain,
                Action = request.Action
            });
            
            return Content(xml, "application/xml");
        }
        
        if (request.Section == "dialplan")
        {
            var xml = await _xmlCurlService.HandleDialplanRequest(new DialplanRequest
            {
                CallerIdNumber = request.CallerIdNumber,
                DestinationNumber = request.DestinationNumber,
                Context = request.Context
            });
            
            return Content(xml, "application/xml");
        }
        
        return NotFound();
    }
}
```

**Ngày 4-5: Dialplan Handler (16h)**

```csharp
public async Task<string> HandleDialplanRequest(DialplanRequest request)
{
    _logger.LogInformation("Dialplan request: Caller={Caller}, Destination={Dest}", 
        request.CallerIdNumber, request.DestinationNumber);
        
    // Internal call (extension to extension)
    if (request.DestinationNumber.Length <= 5 && int.TryParse(request.DestinationNumber, out _))
    {
        return GenerateInternalCallXml(request);
    }
    
    // Outbound call
    if (request.DestinationNumber.StartsWith("0") || request.DestinationNumber.StartsWith("+"))
    {
        return await GenerateOutboundCallXml(request);
    }
    
    return GenerateNotFoundXml();
}

private string GenerateInternalCallXml(DialplanRequest request)
{
    return $@"<?xml version=""1.0"" encoding=""UTF-8"" standalone=""no""?>
<document type=""freeswitch/xml"">
  <section name=""dialplan"" description=""Internal Call"">
    <context name=""default"">
      <extension name=""internal_call"">
        <condition field=""destination_number"" expression=""^({request.DestinationNumber})$"">
          <action application=""set"" data=""call_direction=internal""/>
          <action application=""set"" data=""hangup_after_bridge=true""/>
          <action application=""bridge"" data=""user/{request.DestinationNumber}@${{domain}}""/>
        </condition>
      </extension>
    </context>
  </section>
</document>";
}
```

**Deliverables Tuần 1-2:**
- ✅ Clean Architecture solution
- ✅ Database schema + migrations
- ✅ JWT Authentication
- ✅ Extension CRUD API
- ✅ mod_xml_curl integration
- ✅ Internal calls working

---

### **SPRINT 2-6: Continued Development**

*Due to length constraints, I'll provide key highlights for remaining sprints:*

**Sprint 2 (Tuần 3-4): SIP Trunk & Billing**
- ESL Worker Service
- Billing calculation engine
- Rate table API
- CDR generation

**Sprint 3 (Tuần 5-6): CDR & IVR**
- Recording worker (WAV → MP3)
- MinIO integration
- IVR engine (JSON → XML)
- Queue API

**Sprint 4 (Tuần 7-8): Real-time**
- SignalR Hub
- Real-time events broadcasting
- Agent status management

**Sprint 5 (Tuần 9-10): WebRTC**
- WebRTC SIP profile
- STUN/TURN configuration
- Click-to-call API

**Sprint 6 (Tuần 11-12): Polish**
- Performance optimization
- Security hardening
- Load testing
- Bug fixes

---

## 4. CODE EXAMPLES & PATTERNS

### 4.1. CQRS Pattern

```csharp
// Command
public record CreateTenantCommand(string Name, string Domain) : IRequest<Guid>;

// Handler
public class CreateTenantCommandHandler : IRequestHandler<CreateTenantCommand, Guid>
{
    public async Task<Guid> Handle(CreateTenantCommand request, CancellationToken ct)
    {
        // Implementation
    }
}

// Controller
[HttpPost]
public async Task<IActionResult> Create([FromBody] CreateTenantCommand command)
{
    var id = await _mediator.Send(command);
    return CreatedAtAction(nameof(GetById), new { id }, null);
}
```

### 4.2. Repository Pattern

```csharp
public interface IRepository<T> where T : class
{
    Task<T?> GetByIdAsync(Guid id);
    Task<List<T>> GetAllAsync();
    Task<T> AddAsync(T entity);
    Task UpdateAsync(T entity);
    Task DeleteAsync(T entity);
}
```

### 4.3. Unit Testing

```csharp
public class CreateExtensionCommandHandlerTests
{
    [Fact]
    public async Task Handle_ValidRequest_ReturnsExtensionId()
    {
        // Arrange
        var context = GetInMemoryDbContext();
        var handler = new CreateExtensionCommandHandler(context, mockCurrentUser);
        var command = new CreateExtensionCommand("101", "John Doe", null, null);
        
        // Act
        var result = await handler.Handle(command, CancellationToken.None);
        
        // Assert
        result.Should().NotBeEmpty();
        var extension = await context.Extensions.FindAsync(result);
        extension.Should().NotBeNull();
        extension!.ExtensionNumber.Should().Be("101");
    }
}
```

---

## 5. BEST PRACTICES

### 5.1. Clean Code

- ✅ SOLID principles
- ✅ DRY (Don't Repeat Yourself)
- ✅ KISS (Keep It Simple, Stupid)
- ✅ Meaningful names
- ✅ Small functions (< 20 lines)

### 5.2. Performance

- ✅ Use async/await
- ✅ Cache frequently accessed data
- ✅ Use pagination
- ✅ Optimize database queries
- ✅ Use indexes

### 5.3. Security

- ✅ Never store passwords in plain text
- ✅ Use parameterized queries
- ✅ Validate all inputs
- ✅ Use HTTPS
- ✅ Implement rate limiting

---

## 6. LEARNING RESOURCES

**Official Docs:**
- [.NET 10 Documentation](https://docs.microsoft.com/en-us/dotnet/)
- [EF Core Documentation](https://docs.microsoft.com/en-us/ef/core/)
- [FreeSWITCH Wiki](https://freeswitch.org/confluence/)

**Courses:**
- Pluralsight: "Clean Architecture with ASP.NET Core"
- Udemy: "Complete Guide to ASP.NET Core Web API"

**Books:**
- "Clean Architecture" by Robert C. Martin
- "Domain-Driven Design" by Eric Evans

---

**Ngày tạo:** 15/01/2026  
**Phiên bản:** 1.0  
**Next Review:** Sprint 1 Planning
