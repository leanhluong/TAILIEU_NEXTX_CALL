# ONBOARDING - FRONTEND ENGINEER
## Call Center SaaS Platform - Frontend Onboarding Guide

> [!IMPORTANT]
> Chào mừng bạn đến với Frontend Team! Tài liệu này sẽ hướng dẫn bạn setup môi trường Next.js 15 và bắt đầu contribute.

**Vai trò:** Frontend Engineer (Senior/Mid)  
**Ngày bắt đầu:** [Điền ngày]  
**Mentor:** [Tên Frontend Senior]  
**Tech Stack:** Next.js 15, React 18, TypeScript, Redux Toolkit, Tailwind CSS

---

##  TUẦN ĐẦU TIÊN

### Ngày 1: Environment Setup

#### **Morning: Installation**

```powershell
# Node.js 20 LTS
winget install OpenJS.NodeJS.LTS

# VS Code
winget install Microsoft.VisualStudioCode

# Git
winget install Git.Git

# Verify installations
node --version  # v20.x.x
npm --version   # 10.x.x
```

**VS Code Extensions:**
- ES7+ React/Redux/React-Native snippets
- Tailwind CSS IntelliSense
- TypeScript + JavaScript
- Prettier
- ESLint

#### **Afternoon: Project Setup**

```bash
# Clone repository
git clone https://github.com/[org]/callcenter-saas-frontend.git
cd callcenter-saas-frontend

# Install dependencies
npm install

# Run development server
npm run dev

# Open http://localhost:3000
```

**Deliverables Day 1:**
-  Node.js environment ready
-  Project running on localhost:3000
-  Can see login page

---

### Ngày 2: Project Structure

**Next.js 15 App Router:**
```
src/
 app/
    (auth)/
       login/page.tsx
       register/page.tsx
    (dashboard)/
       layout.tsx
       page.tsx
       extensions/
       cdrs/
    layout.tsx
 components/
    ui/
    forms/
    layouts/
 lib/
    api/
    store/
    utils/
 types/
```

**Learn:**
- [ ] Next.js App Router vs Pages Router
- [ ] Server Components vs Client Components
- [ ] File-based routing
- [ ] Redux Toolkit setup

---

### Ngày 3: First Component

**Task: Create a simple UI component**

```typescript
// components/ui/Button.tsx
'use client';

import { ButtonHTMLAttributes } from 'react';

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary';
}

export function Button({ variant = 'primary', children, ...props }: ButtonProps) {
  return (
    <button
      className={px-4 py-2 rounded-lg }
      {...props}
    >
      {children}
    </button>
  );
}
```

**Deliverables Day 3:**
-  First component created
-  Understand TypeScript
-  Understand Tailwind CSS

---

### Ngày 4-5: First Feature

**Task: Implement Extension List Page**

```typescript
// app/(dashboard)/extensions/page.tsx
'use client';

import { useEffect, useState } from 'react';
import { Table } from 'antd';
import { extensionApi } from '@/lib/api/extensions';

export default function ExtensionsPage() {
  const [extensions, setExtensions] = useState([]);
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    fetchExtensions();
  }, []);

  const fetchExtensions = async () => {
    setLoading(true);
    try {
      const data = await extensionApi.getAll();
      setExtensions(data);
    } finally {
      setLoading(false);
    }
  };

  const columns = [
    { title: 'Extension', dataIndex: 'extensionNumber', key: 'extensionNumber' },
    { title: 'Agent Name', dataIndex: 'agentName', key: 'agentName' },
    { title: 'Status', dataIndex: 'isActive', key: 'isActive' },
  ];

  return (
    <div>
      <h1 className='text-2xl font-bold mb-4'>Extensions</h1>
      <Table columns={columns} dataSource={extensions} loading={loading} />
    </div>
  );
}
```

**Deliverables Week 1:**
-  First feature implemented
-  Understand API integration
-  PR created

---

##  TÀI LIỆU HỌC TẬP

### Bắt buộc đọc
1. [Next.js 15 Docs](https://nextjs.org/docs)
2. [ROADMAP_CHI_TIET_FRONTEND.md](../Preparing_For_Project_Implementation/ROADMAP_CHI_TIET_FRONTEND.md)
3. [02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md](../Project_Documents/02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md)

---

**Ngày tạo:** 15/01/2026  
**Phiên bản:** 1.0

> [!TIP]
> Focus on component reusability and TypeScript type safety!
