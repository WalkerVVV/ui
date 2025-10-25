# shadcn/ui Setup Guide for firstmile-deals-pipeline

Complete guide for setting up shadcn/ui in your project with multiple framework scenarios.

---

## 🚀 Quick Start (Once You're on Desktop)

```bash
# 1. Clone your repository
git clone https://github.com/WalkerVVV/firstmile-deals-pipeline.git
cd firstmile-deals-pipeline

# 2. Check what framework you have
cat package.json | grep -E "(next|vite|react-scripts)"

# 3. Follow the appropriate guide below based on your framework
```

---

## 📋 Scenario 1: Next.js Project (Most Common)

### Prerequisites Check
```bash
# Verify you have Next.js
cat package.json | grep "next"
```

### Installation Steps

#### 1. Initialize shadcn/ui
```bash
npx shadcn@latest init
```

You'll be prompted with questions. Here are recommended answers for a mobile dashboard:

```
✔ Which style would you like to use? › New York
✔ Which color would you like to use as base color? › Slate
✔ Would you like to use CSS variables for colors? › yes
✔ Where is your global CSS file? › app/globals.css (or src/app/globals.css)
✔ Would you like to use CSS variables for colors? › yes
✔ Are you using a custom tailwind prefix eg. tw-? › no
✔ Where is your tailwind.config.js located? › tailwind.config.js
✔ Configure the import alias for components: › @/components
✔ Configure the import alias for utils: › @/lib/utils
✔ Are you using React Server Components? › yes
```

#### 2. Add Essential Dashboard Components
```bash
# Core UI components
npx shadcn@latest add card
npx shadcn@latest add badge
npx shadcn@latest add button
npx shadcn@latest add sheet
npx shadcn@latest add table

# Navigation components
npx shadcn@latest add tabs
npx shadcn@latest add drawer
npx shadcn@latest add sidebar

# Forms & Inputs
npx shadcn@latest add input
npx shadcn@latest add select
npx shadcn@latest add form
npx shadcn@latest add calendar
npx shadcn@latest add date-picker

# Feedback components
npx shadcn@latest add toast
npx shadcn@latest add alert
npx shadcn@latest add progress
npx shadcn@latest add skeleton

# Data visualization
npx shadcn@latest add chart
```

#### 3. Project Structure (Next.js)
After setup, your structure should look like:
```
firstmile-deals-pipeline/
├── app/
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   └── ui/              # shadcn components installed here
│       ├── card.tsx
│       ├── badge.tsx
│       ├── button.tsx
│       └── ...
├── lib/
│   └── utils.ts         # Utility functions
├── components.json      # shadcn configuration
├── tailwind.config.ts
└── package.json
```

#### 4. Update tailwind.config for Mobile
Edit `tailwind.config.ts`:
```typescript
import type { Config } from "tailwindcss"

const config = {
  darkMode: ["class"],
  content: [
    './pages/**/*.{ts,tsx}',
    './components/**/*.{ts,tsx}',
    './app/**/*.{ts,tsx}',
    './src/**/*.{ts,tsx}',
  ],
  prefix: "",
  theme: {
    container: {
      center: true,
      padding: "1rem",
      screens: {
        "2xl": "1400px",
      },
    },
    extend: {
      // Mobile-first breakpoints
      screens: {
        'xs': '375px',
        'sm': '640px',
        'md': '768px',
        'lg': '1024px',
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
} satisfies Config

export default config
```

---

## 📋 Scenario 2: Vite + React Project

### Prerequisites Check
```bash
# Verify you have Vite
cat package.json | grep "vite"
```

### Installation Steps

#### 1. Initialize shadcn/ui
```bash
npx shadcn@latest init
```

Recommended answers for Vite:
```
✔ Which style would you like to use? › New York
✔ Which color would you like to use as base color? › Slate
✔ Would you like to use CSS variables for colors? › yes
✔ Where is your global CSS file? › src/index.css
✔ Would you like to use CSS variables for colors? › yes
✔ Are you using a custom tailwind prefix eg. tw-? › no
✔ Where is your tailwind.config.js located? › tailwind.config.js
✔ Configure the import alias for components: › @/components
✔ Configure the import alias for utils: › @/lib/utils
✔ Are you using React Server Components? › no
```

#### 2. Update vite.config.ts for Path Aliases
```typescript
import path from "path"
import react from "@vitejs/plugin-react"
import { defineConfig } from "vite"

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
})
```

#### 3. Update tsconfig.json
```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

#### 4. Add Components (Same as Next.js)
```bash
# Add the same components listed in Scenario 1, Step 2
npx shadcn@latest add card badge button sheet table tabs...
```

#### 5. Project Structure (Vite)
```
firstmile-deals-pipeline/
├── src/
│   ├── components/
│   │   └── ui/          # shadcn components
│   ├── lib/
│   │   └── utils.ts
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── components.json
├── tailwind.config.js
├── vite.config.ts
└── package.json
```

---

## 📋 Scenario 3: Create React App (Legacy)

### Prerequisites Check
```bash
# Verify you have CRA
cat package.json | grep "react-scripts"
```

### Installation Steps

#### 1. Install Dependencies First
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

#### 2. Install craco for Path Aliases
```bash
npm install @craco/craco
```

#### 3. Create craco.config.js
```javascript
const path = require('path')

module.exports = {
  webpack: {
    alias: {
      '@': path.resolve(__dirname, 'src'),
    },
  },
}
```

#### 4. Update package.json scripts
```json
{
  "scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test"
  }
}
```

#### 5. Initialize shadcn/ui
```bash
npx shadcn@latest init
```

#### 6. Add Components
```bash
npx shadcn@latest add card badge button sheet table...
```

---

## 📱 Mobile Dashboard Sample Components

### Sample 1: Deal Card Component
Create `components/deal-card.tsx`:
```typescript
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"

interface DealCardProps {
  title: string
  amount: number
  status: 'active' | 'pending' | 'closed'
  company: string
  date: string
}

export function DealCard({ title, amount, status, company, date }: DealCardProps) {
  const statusColor = {
    active: 'bg-green-500',
    pending: 'bg-yellow-500',
    closed: 'bg-gray-500'
  }

  return (
    <Card className="w-full">
      <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
        <CardTitle className="text-sm font-medium">
          {title}
        </CardTitle>
        <Badge className={statusColor[status]}>
          {status}
        </Badge>
      </CardHeader>
      <CardContent>
        <div className="text-2xl font-bold">
          ${amount.toLocaleString()}
        </div>
        <p className="text-xs text-muted-foreground mt-1">
          {company}
        </p>
        <p className="text-xs text-muted-foreground">
          {date}
        </p>
      </CardContent>
    </Card>
  )
}
```

### Sample 2: Mobile Dashboard Layout
Create `components/mobile-dashboard.tsx`:
```typescript
"use client" // Remove this if not using Next.js

import { Sheet, SheetContent, SheetTrigger } from "@/components/ui/sheet"
import { Button } from "@/components/ui/button"
import { Menu } from "lucide-react"

export function MobileDashboard({ children }: { children: React.ReactNode }) {
  return (
    <div className="min-h-screen bg-background">
      {/* Mobile Header */}
      <header className="sticky top-0 z-50 w-full border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60">
        <div className="container flex h-14 items-center">
          <Sheet>
            <SheetTrigger asChild>
              <Button variant="ghost" size="icon" className="md:hidden">
                <Menu className="h-5 w-5" />
              </Button>
            </SheetTrigger>
            <SheetContent side="left" className="w-64">
              <nav className="flex flex-col gap-4">
                <a href="#" className="text-sm font-medium">Dashboard</a>
                <a href="#" className="text-sm font-medium">Deals</a>
                <a href="#" className="text-sm font-medium">Pipeline</a>
                <a href="#" className="text-sm font-medium">Reports</a>
                <a href="#" className="text-sm font-medium">Settings</a>
              </nav>
            </SheetContent>
          </Sheet>
          <h1 className="ml-4 text-lg font-semibold">FirstMile Deals</h1>
        </div>
      </header>

      {/* Main Content */}
      <main className="container px-4 py-6">
        {children}
      </main>
    </div>
  )
}
```

### Sample 3: Dashboard Page Example
Create `app/page.tsx` (Next.js) or `src/App.tsx` (Vite):
```typescript
import { MobileDashboard } from "@/components/mobile-dashboard"
import { DealCard } from "@/components/deal-card"
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs"

export default function DashboardPage() {
  const deals = [
    {
      id: 1,
      title: "Enterprise Deal",
      amount: 150000,
      status: 'active' as const,
      company: "TechCorp",
      date: "2024-01-15"
    },
    {
      id: 2,
      title: "Mid-Market Lead",
      amount: 75000,
      status: 'pending' as const,
      company: "StartupXYZ",
      date: "2024-01-18"
    },
    {
      id: 3,
      title: "SMB Contract",
      amount: 25000,
      status: 'closed' as const,
      company: "LocalBiz",
      date: "2024-01-10"
    },
  ]

  return (
    <MobileDashboard>
      <div className="space-y-4">
        <h2 className="text-2xl font-bold tracking-tight">Deals Pipeline</h2>

        <Tabs defaultValue="all" className="w-full">
          <TabsList className="grid w-full grid-cols-3">
            <TabsTrigger value="all">All</TabsTrigger>
            <TabsTrigger value="active">Active</TabsTrigger>
            <TabsTrigger value="closed">Closed</TabsTrigger>
          </TabsList>

          <TabsContent value="all" className="space-y-4 mt-4">
            {deals.map(deal => (
              <DealCard key={deal.id} {...deal} />
            ))}
          </TabsContent>

          <TabsContent value="active" className="space-y-4 mt-4">
            {deals.filter(d => d.status === 'active').map(deal => (
              <DealCard key={deal.id} {...deal} />
            ))}
          </TabsContent>

          <TabsContent value="closed" className="space-y-4 mt-4">
            {deals.filter(d => d.status === 'closed').map(deal => (
              <DealCard key={deal.id} {...deal} />
            ))}
          </TabsContent>
        </Tabs>
      </div>
    </MobileDashboard>
  )
}
```

---

## 🎨 Mobile-First Styling Tips

### 1. Use Responsive Utilities
```tsx
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  {/* Cards automatically stack on mobile, grid on desktop */}
</div>
```

### 2. Touch-Friendly Targets
```tsx
<Button size="lg" className="min-h-[44px] min-w-[44px]">
  {/* iOS/Android minimum touch target */}
</Button>
```

### 3. Safe Area Insets (for mobile notches)
Add to your global CSS:
```css
@supports (padding: max(0px)) {
  .safe-area-top {
    padding-top: max(1rem, env(safe-area-inset-top));
  }

  .safe-area-bottom {
    padding-bottom: max(1rem, env(safe-area-inset-bottom));
  }
}
```

---

## 🧪 Testing Your Setup

### 1. Run Development Server
```bash
# Next.js
npm run dev

# Vite
npm run dev

# CRA
npm start
```

### 2. Test Mobile View
- Open in browser: `http://localhost:3000` (or respective port)
- Open DevTools (F12)
- Toggle device toolbar (Ctrl+Shift+M)
- Test on iPhone/Android simulators

### 3. Test Dark Mode
Add to your root layout/component:
```tsx
import { ThemeProvider } from "next-themes"

export default function RootLayout({ children }) {
  return (
    <html lang="en" suppressHydrationWarning>
      <body>
        <ThemeProvider attribute="class" defaultTheme="system" enableSystem>
          {children}
        </ThemeProvider>
      </body>
    </html>
  )
}
```

---

## 📚 Additional Components for Deals Pipeline

### Recommended Components to Add
```bash
# Data display
npx shadcn@latest add avatar
npx shadcn@latest add separator
npx shadcn@latest add scroll-area

# Dialogs & Overlays
npx shadcn@latest add dialog
npx shadcn@latest add popover
npx shadcn@latest add dropdown-menu

# Advanced inputs
npx shadcn@latest add combobox
npx shadcn@latest add command
npx shadcn@latest add slider

# Loading states
npx shadcn@latest add spinner
npx shadcn@latest add skeleton
```

---

## 🚨 Troubleshooting

### Issue: "Cannot find module '@/components/ui/...'"
**Solution:** Check your `tsconfig.json` or `jsconfig.json` has paths configured:
```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./*"]  // or ["./src/*"] for Vite
    }
  }
}
```

### Issue: Tailwind styles not applying
**Solution:** Check your `tailwind.config.js` content paths:
```javascript
module.exports = {
  content: [
    './app/**/*.{js,ts,jsx,tsx}',
    './components/**/*.{js,ts,jsx,tsx}',
    './src/**/*.{js,ts,jsx,tsx}',
  ],
}
```

### Issue: Components look unstyled
**Solution:** Make sure you imported the global CSS:
- Next.js: Import in `app/layout.tsx`
- Vite: Import in `src/main.tsx`
- CRA: Import in `src/index.tsx`

```tsx
import './globals.css'  // or './index.css'
```

---

## 📝 Next Steps

1. ✅ Clone your repository
2. ✅ Follow the appropriate scenario guide above
3. ✅ Test the sample components
4. 🔨 Customize for your specific deals pipeline needs
5. 🚀 Build your mobile dashboard!

---

## 💡 Questions?

When you're on desktop and working through this, feel free to ask me:
- "Help me customize component X"
- "How do I add feature Y"
- "Debug this error: ..."

Good luck! 🎉
