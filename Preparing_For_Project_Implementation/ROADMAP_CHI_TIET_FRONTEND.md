# ROADMAP CHI TIẾT CHO FRONTEND ENGINEER
## Call Center SaaS Platform - Frontend Detailed Roadmap

> [!IMPORTANT]
> Roadmap chi tiết cho vị trí Frontend Engineer (Senior & Mid) trong dự án Call Center SaaS Platform. Bao gồm công việc từng tuần, code examples, và UI/UX best practices.

**Phiên bản:** 1.0  
**Ngày tạo:** 15/01/2026  
**Thời gian:** 12 tuần (6 sprints)  
**Tech Stack:** Next.js 15, React 18, TypeScript, Redux Toolkit, Tailwind CSS, Ant Design

---

## 📋 MỤC LỤC

1. [Tổng quan vai trò Frontend](#1-tổng-quan-vai-trò-frontend)
2. [Tech Stack chi tiết](#2-tech-stack-chi-tiết)
3. [Roadmap chi tiết từng tuần](#3-roadmap-chi-tiết-từng-tuần)
4. [Component Examples](#4-component-examples)
5. [Best Practices](#5-best-practices)
6. [Learning Resources](#6-learning-resources)

---

## 1. TỔNG QUAN VAI TRÒ FRONTEND

### 1.1. Phân chia công việc

#### **Frontend Senior (40h/tuần)**
- Architecture & folder structure
- Complex components (IVR Builder, Softphone)
- State management (Redux)
- WebRTC integration
- SignalR real-time
- Code review
- Mentoring Frontend Mid

#### **Frontend Mid (40h/tuần)**
- UI components development
- Form handling
- API integration
- Charts & visualization
- Responsive design
- Bug fixing

### 1.2. Deliverables chính

| Deliverable | Owner | Deadline |
|-------------|-------|----------|
| **Next.js Project Setup** | Senior | Week 1 |
| **Login/Register Pages** | Senior | Week 2 |
| **Dashboard Layout** | Senior | Week 2 |
| **Extension Management** | Senior | Week 2 |
| **Rate Table UI** | Senior | Week 4 |
| **CDR Report** | Senior | Week 5 |
| **IVR Builder** | Senior | Week 6 |
| **Live Call Monitor** | Senior | Week 7 |
| **WebRTC Softphone** | Senior | Week 9 |
| **Mobile App** | Senior | Week 10 |

---

## 2. TECH STACK CHI TIẾT

### 2.1. Core Framework

```
Next.js 15 (App Router)
├── React 18
├── TypeScript 5.x
├── Redux Toolkit
├── Tailwind CSS 3.x
├── Ant Design 5.x
├── Axios
├── SignalR Client
└── JsSIP (WebRTC)
```

### 2.2. Package.json

```json
{
  "name": "callcenter-saas-frontend",
  "version": "1.0.0",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "^15.0.0",
    "react": "^18.3.0",
    "react-dom": "^18.3.0",
    "@reduxjs/toolkit": "^2.0.0",
    "react-redux": "^9.0.0",
    "axios": "^1.6.0",
    "antd": "^5.12.0",
    "@ant-design/icons": "^5.2.0",
    "@microsoft/signalr": "^8.0.0",
    "jssip": "^3.10.0",
    "recharts": "^2.10.0",
    "react-flow-renderer": "^10.3.0",
    "dayjs": "^1.11.0"
  },
  "devDependencies": {
    "typescript": "^5.3.0",
    "@types/react": "^18.2.0",
    "@types/node": "^20.10.0",
    "tailwindcss": "^3.4.0",
    "postcss": "^8.4.0",
    "autoprefixer": "^10.4.0",
    "eslint": "^8.55.0",
    "eslint-config-next": "^15.0.0"
  }
}
```

---

## 3. ROADMAP CHI TIẾT TỪNG TUẦN

### **TUẦN 0: Preparation**

#### **Học Next.js 15 (Frontend Senior - 20h)**

**Ngày 1-2: Next.js Basics (16h)**
- [ ] Next.js 15 App Router tutorial
- [ ] Server Components vs Client Components
- [ ] File-based routing
- [ ] Data fetching patterns
- [ ] Metadata API

**Ngày 3: Practice (4h)**
- [ ] Create demo Next.js project
- [ ] Practice routing
- [ ] Practice data fetching

#### **Setup Environment (Frontend Mid - 20h)**

**Ngày 1-2: Node.js Setup (16h)**
- [ ] Cài Node.js 20 LTS
- [ ] Cài VS Code + extensions
- [ ] Learn TypeScript basics
- [ ] Learn Tailwind CSS

**Ngày 3: Practice (4h)**
- [ ] Create demo React app
- [ ] Practice TypeScript
- [ ] Practice Tailwind

---

### **SPRINT 1: Infrastructure & Authentication (Tuần 1-2)**

#### **Tuần 1: Project Setup**

**Frontend Senior (40h)**

**Ngày 1: Next.js Setup (8h)**

```bash
# Create Next.js project
npx create-next-app@latest callcenter-saas-frontend --typescript --tailwind --app

# Install dependencies
cd callcenter-saas-frontend
npm install @reduxjs/toolkit react-redux axios antd @ant-design/icons

# Project structure
src/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   │   └── page.tsx
│   │   └── register/
│   │       └── page.tsx
│   ├── (dashboard)/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   ├── extensions/
│   │   ├── cdrs/
│   │   └── settings/
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   ├── ui/
│   ├── forms/
│   └── layouts/
├── lib/
│   ├── api/
│   ├── store/
│   └── utils/
└── types/
```

**Ngày 2: Redux Setup (8h)**

```typescript
// lib/store/store.ts
import { configureStore } from '@reduxjs/toolkit';
import authReducer from './slices/authSlice';
import extensionReducer from './slices/extensionSlice';

export const store = configureStore({
  reducer: {
    auth: authReducer,
    extensions: extensionReducer,
  },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;

// lib/store/slices/authSlice.ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface AuthState {
  user: User | null;
  token: string | null;
  isAuthenticated: boolean;
}

const initialState: AuthState = {
  user: null,
  token: null,
  isAuthenticated: false,
};

export const authSlice = createSlice({
  name: 'auth',
  initialState,
  reducers: {
    setCredentials: (state, action: PayloadAction<{ user: User; token: string }>) => {
      state.user = action.payload.user;
      state.token = action.payload.token;
      state.isAuthenticated = true;
    },
    logout: (state) => {
      state.user = null;
      state.token = null;
      state.isAuthenticated = false;
    },
  },
});

export const { setCredentials, logout } = authSlice.actions;
export default authSlice.reducer;
```

**Ngày 3: API Client (8h)**

```typescript
// lib/api/client.ts
import axios from 'axios';
import { store } from '../store/store';

const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL || 'http://localhost:5000/api',
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor
apiClient.interceptors.request.use(
  (config) => {
    const token = store.getState().auth.token;
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Response interceptor
apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response?.status === 401) {
      // Logout and redirect to login
      store.dispatch(logout());
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

export default apiClient;

// lib/api/auth.ts
export const authApi = {
  login: async (email: string, password: string) => {
    const response = await apiClient.post('/auth/login', { email, password });
    return response.data;
  },
  
  register: async (data: RegisterData) => {
    const response = await apiClient.post('/auth/register', data);
    return response.data;
  },
  
  logout: async () => {
    await apiClient.post('/auth/logout');
  },
};
```

**Ngày 4-5: Login/Register Pages (16h)**

```typescript
// app/(auth)/login/page.tsx
'use client';

import { useState } from 'react';
import { useRouter } from 'next/navigation';
import { Form, Input, Button, message } from 'antd';
import { UserOutlined, LockOutlined } from '@ant-design/icons';
import { useAppDispatch } from '@/lib/store/hooks';
import { setCredentials } from '@/lib/store/slices/authSlice';
import { authApi } from '@/lib/api/auth';

export default function LoginPage() {
  const [loading, setLoading] = useState(false);
  const router = useRouter();
  const dispatch = useAppDispatch();

  const onFinish = async (values: { email: string; password: string }) => {
    try {
      setLoading(true);
      const data = await authApi.login(values.email, values.password);
      
      dispatch(setCredentials({
        user: data.user,
        token: data.accessToken,
      }));
      
      // Save to localStorage
      localStorage.setItem('token', data.accessToken);
      localStorage.setItem('user', JSON.stringify(data.user));
      
      message.success('Login successful!');
      router.push('/dashboard');
    } catch (error: any) {
      message.error(error.response?.data?.message || 'Login failed');
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-gradient-to-br from-blue-500 to-purple-600">
      <div className="bg-white p-8 rounded-lg shadow-2xl w-full max-w-md">
        <h1 className="text-3xl font-bold text-center mb-8 text-gray-800">
          Call Center SaaS
        </h1>
        
        <Form
          name="login"
          onFinish={onFinish}
          autoComplete="off"
          size="large"
        >
          <Form.Item
            name="email"
            rules={[
              { required: true, message: 'Please input your email!' },
              { type: 'email', message: 'Please enter a valid email!' },
            ]}
          >
            <Input
              prefix={<UserOutlined />}
              placeholder="Email"
            />
          </Form.Item>

          <Form.Item
            name="password"
            rules={[{ required: true, message: 'Please input your password!' }]}
          >
            <Input.Password
              prefix={<LockOutlined />}
              placeholder="Password"
            />
          </Form.Item>

          <Form.Item>
            <Button
              type="primary"
              htmlType="submit"
              loading={loading}
              className="w-full"
            >
              Log in
            </Button>
          </Form.Item>
        </Form>
        
        <div className="text-center">
          <a href="/register" className="text-blue-500 hover:text-blue-700">
            Don't have an account? Register
          </a>
        </div>
      </div>
    </div>
  );
}
```

**Frontend Mid (40h)**

**Ngày 1-3: UI Components (24h)**

```typescript
// components/ui/Button.tsx
import { ButtonHTMLAttributes } from 'react';
import { cn } from '@/lib/utils';

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'danger';
  size?: 'sm' | 'md' | 'lg';
}

export function Button({ 
  variant = 'primary', 
  size = 'md', 
  className, 
  children, 
  ...props 
}: ButtonProps) {
  return (
    <button
      className={cn(
        'rounded-lg font-medium transition-colors',
        {
          'bg-blue-500 hover:bg-blue-600 text-white': variant === 'primary',
          'bg-gray-200 hover:bg-gray-300 text-gray-800': variant === 'secondary',
          'bg-red-500 hover:bg-red-600 text-white': variant === 'danger',
          'px-3 py-1.5 text-sm': size === 'sm',
          'px-4 py-2 text-base': size === 'md',
          'px-6 py-3 text-lg': size === 'lg',
        },
        className
      )}
      {...props}
    >
      {children}
    </button>
  );
}

// components/ui/DataTable.tsx
import { Table } from 'antd';
import type { ColumnsType } from 'antd/es/table';

interface DataTableProps<T> {
  columns: ColumnsType<T>;
  data: T[];
  loading?: boolean;
  pagination?: any;
  onRow?: (record: T) => any;
}

export function DataTable<T extends { id: string }>({ 
  columns, 
  data, 
  loading, 
  pagination,
  onRow 
}: DataTableProps<T>) {
  return (
    <Table
      columns={columns}
      dataSource={data}
      loading={loading}
      pagination={pagination}
      onRow={onRow}
      rowKey="id"
      className="shadow-sm"
    />
  );
}
```

**Ngày 4-5: Auth Context (16h)**

```typescript
// lib/contexts/AuthContext.tsx
'use client';

import { createContext, useContext, useEffect, useState } from 'react';
import { useRouter } from 'next/navigation';
import { User } from '@/types';

interface AuthContextType {
  user: User | null;
  loading: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const router = useRouter();

  useEffect(() => {
    // Check if user is logged in
    const token = localStorage.getItem('token');
    const userData = localStorage.getItem('user');
    
    if (token && userData) {
      setUser(JSON.parse(userData));
    }
    
    setLoading(false);
  }, []);

  const login = async (email: string, password: string) => {
    // Implementation
  };

  const logout = () => {
    localStorage.removeItem('token');
    localStorage.removeItem('user');
    setUser(null);
    router.push('/login');
  };

  return (
    <AuthContext.Provider value={{ user, loading, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
};
```

#### **Tuần 2: Dashboard & Extension Management**

**Frontend Senior (40h)**

**Ngày 1-2: Dashboard Layout (16h)**

```typescript
// app/(dashboard)/layout.tsx
'use client';

import { Layout, Menu } from 'antd';
import { 
  DashboardOutlined, 
  PhoneOutlined, 
  FileTextOutlined,
  SettingOutlined 
} from '@ant-design/icons';
import { usePathname } from 'next/navigation';
import Link from 'next/link';

const { Header, Sider, Content } = Layout;

export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  const pathname = usePathname();

  const menuItems = [
    { key: '/dashboard', icon: <DashboardOutlined />, label: 'Dashboard' },
    { key: '/extensions', icon: <PhoneOutlined />, label: 'Extensions' },
    { key: '/cdrs', icon: <FileTextOutlined />, label: 'Call Records' },
    { key: '/settings', icon: <SettingOutlined />, label: 'Settings' },
  ];

  return (
    <Layout className="min-h-screen">
      <Sider width={250} theme="dark">
        <div className="p-4 text-white text-xl font-bold">
          Call Center
        </div>
        <Menu
          theme="dark"
          mode="inline"
          selectedKeys={[pathname]}
          items={menuItems.map(item => ({
            ...item,
            label: <Link href={item.key}>{item.label}</Link>
          }))}
        />
      </Sider>
      
      <Layout>
        <Header className="bg-white shadow-sm px-6 flex items-center justify-between">
          <h1 className="text-xl font-semibold">Dashboard</h1>
          <div>User Menu</div>
        </Header>
        
        <Content className="p-6 bg-gray-50">
          {children}
        </Content>
      </Layout>
    </Layout>
  );
}
```

**Ngày 3-5: Extension Management (24h)**

```typescript
// app/(dashboard)/extensions/page.tsx
'use client';

import { useState, useEffect } from 'react';
import { Button, Table, Modal, Form, Input, message } from 'antd';
import { PlusOutlined } from '@ant-design/icons';
import { extensionApi } from '@/lib/api/extensions';

export default function ExtensionsPage() {
  const [extensions, setExtensions] = useState([]);
  const [loading, setLoading] = useState(false);
  const [modalOpen, setModalOpen] = useState(false);
  const [form] = Form.useForm();

  useEffect(() => {
    fetchExtensions();
  }, []);

  const fetchExtensions = async () => {
    try {
      setLoading(true);
      const data = await extensionApi.getAll();
      setExtensions(data);
    } catch (error) {
      message.error('Failed to fetch extensions');
    } finally {
      setLoading(false);
    }
  };

  const handleCreate = async (values: any) => {
    try {
      await extensionApi.create(values);
      message.success('Extension created successfully');
      setModalOpen(false);
      form.resetFields();
      fetchExtensions();
    } catch (error: any) {
      message.error(error.response?.data?.message || 'Failed to create extension');
    }
  };

  const columns = [
    { title: 'Extension', dataIndex: 'extensionNumber', key: 'extensionNumber' },
    { title: 'Agent Name', dataIndex: 'agentName', key: 'agentName' },
    { title: 'Email', dataIndex: 'email', key: 'email' },
    { title: 'Status', dataIndex: 'isActive', key: 'isActive', 
      render: (isActive: boolean) => (
        <span className={isActive ? 'text-green-500' : 'text-red-500'}>
          {isActive ? 'Active' : 'Inactive'}
        </span>
      )
    },
  ];

  return (
    <div>
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-bold">Extensions</h1>
        <Button 
          type="primary" 
          icon={<PlusOutlined />}
          onClick={() => setModalOpen(true)}
        >
          Create Extension
        </Button>
      </div>

      <Table
        columns={columns}
        dataSource={extensions}
        loading={loading}
        rowKey="id"
      />

      <Modal
        title="Create Extension"
        open={modalOpen}
        onCancel={() => setModalOpen(false)}
        footer={null}
      >
        <Form form={form} onFinish={handleCreate} layout="vertical">
          <Form.Item
            name="extensionNumber"
            label="Extension Number"
            rules={[
              { required: true, message: 'Please input extension number!' },
              { pattern: /^\d{3,5}$/, message: 'Extension must be 3-5 digits!' }
            ]}
          >
            <Input placeholder="101" />
          </Form.Item>

          <Form.Item
            name="agentName"
            label="Agent Name"
            rules={[{ required: true, message: 'Please input agent name!' }]}
          >
            <Input placeholder="John Doe" />
          </Form.Item>

          <Form.Item
            name="email"
            label="Email"
            rules={[{ type: 'email', message: 'Please enter a valid email!' }]}
          >
            <Input placeholder="john@example.com" />
          </Form.Item>

          <Form.Item>
            <Button type="primary" htmlType="submit" className="w-full">
              Create
            </Button>
          </Form.Item>
        </Form>
      </Modal>
    </div>
  );
}
```

**Deliverables Tuần 1-2:**
- ✅ Next.js project setup
- ✅ Redux store configured
- ✅ Login/Register pages
- ✅ Dashboard layout
- ✅ Extension management UI
- ✅ API integration

---

### **SPRINT 2-6: Continued Development**

**Sprint 2 (Tuần 3-4): Rate Table & Billing UI**
- Rate table management
- Balance widget
- Transaction history
- Charts (Recharts)

**Sprint 3 (Tuần 5-6): CDR & IVR**
- CDR report with filters
- Audio player component
- IVR Builder (React Flow)
- Queue management UI

**Sprint 4 (Tuần 7-8): Real-time Dashboard**
- SignalR integration
- Live call monitor
- Agent status board
- Real-time notifications

**Sprint 5 (Tuần 9-10): WebRTC & Mobile**
- WebRTC Softphone (JsSIP)
- Click-to-call
- React Native app setup
- Mobile screens

**Sprint 6 (Tuần 11-12): Polish**
- UI/UX improvements
- Performance optimization
- Responsive design
- Bug fixes

---

## 4. COMPONENT EXAMPLES

### 4.1. WebRTC Softphone

```typescript
// components/Softphone/Softphone.tsx
'use client';

import { useState, useEffect, useRef } from 'react';
import JsSIP from 'jssip';
import { Button, Input } from 'antd';
import { PhoneOutlined, PhoneFilled } from '@ant-design/icons';

export function Softphone() {
  const [registered, setRegistered] = useState(false);
  const [calling, setCalling] = useState(false);
  const [number, setNumber] = useState('');
  const uaRef = useRef<any>(null);
  const sessionRef = useRef<any>(null);

  useEffect(() => {
    // Initialize JsSIP
    const socket = new JsSIP.WebSocketInterface('wss://freeswitch-server:8082');
    const configuration = {
      sockets: [socket],
      uri: 'sip:101@domain.pbx.local',
      password: 'password',
    };

    const ua = new JsSIP.UA(configuration);
    
    ua.on('registered', () => setRegistered(true));
    ua.on('unregistered', () => setRegistered(false));
    ua.on('newRTCSession', (e: any) => {
      const session = e.session;
      sessionRef.current = session;
      
      session.on('accepted', () => setCalling(true));
      session.on('ended', () => setCalling(false));
    });

    ua.start();
    uaRef.current = ua;

    return () => {
      ua.stop();
    };
  }, []);

  const makeCall = () => {
    if (!uaRef.current || !number) return;
    
    const options = {
      mediaConstraints: { audio: true, video: false },
    };
    
    uaRef.current.call(number, options);
  };

  const hangup = () => {
    if (sessionRef.current) {
      sessionRef.current.terminate();
    }
  };

  return (
    <div className="p-4 bg-white rounded-lg shadow">
      <div className="mb-4">
        <span className={registered ? 'text-green-500' : 'text-red-500'}>
          {registered ? 'Registered' : 'Not Registered'}
        </span>
      </div>

      <Input
        value={number}
        onChange={(e) => setNumber(e.target.value)}
        placeholder="Enter number"
        className="mb-4"
      />

      {!calling ? (
        <Button
          type="primary"
          icon={<PhoneOutlined />}
          onClick={makeCall}
          disabled={!registered}
          className="w-full"
        >
          Call
        </Button>
      ) : (
        <Button
          danger
          icon={<PhoneFilled />}
          onClick={hangup}
          className="w-full"
        >
          Hangup
        </Button>
      )}
    </div>
  );
}
```

---

## 5. BEST PRACTICES

### 5.1. Component Design

- ✅ Use TypeScript for type safety
- ✅ Keep components small and focused
- ✅ Use composition over inheritance
- ✅ Implement proper error boundaries
- ✅ Use React.memo for performance

### 5.2. State Management

- ✅ Use Redux for global state
- ✅ Use local state for UI-only state
- ✅ Normalize state shape
- ✅ Use selectors for derived data

### 5.3. Performance

- ✅ Code splitting with dynamic imports
- ✅ Image optimization with Next.js Image
- ✅ Lazy load components
- ✅ Debounce user inputs
- ✅ Use React.memo and useMemo

---

## 6. LEARNING RESOURCES

**Official Docs:**
- [Next.js Documentation](https://nextjs.org/docs)
- [React Documentation](https://react.dev/)
- [Redux Toolkit](https://redux-toolkit.js.org/)

**Courses:**
- Udemy: "Next.js 15 & React - The Complete Guide"
- Frontend Masters: "Complete Intro to React"

---

**Ngày tạo:** 15/01/2026  
**Phiên bản:** 1.0
