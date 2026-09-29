🚀 Next.js Stripe Supabase Starter Kit ⚡

Welcome to the `Next.js SaaS Starter with Stripe` – your ultimate boilerplate for building modern, scalable SaaS applications! This powerful starter kit combines the best of Next.js, Stripe, Supabase, Typescript and Tailwind CSS to help you launch your SaaS project faster than ever! 🌟

## 🎯 Overview

A powerful SaaS starter-kit built with Next.js, Stripe, Supabase, Typescript, and Tailwind CSS. Perfect for launching your next SaaS project!

## ✨ Key Features

### 💳 Complete Stripe Integration

- Subscription management
- Usage-based billing
- Multiple pricing tiers
- Secure payment processing

### 🔐 Authentication & Authorization

- Supabase Auth integration
- Social login providers
- Role-based access control

### 🎨 Modern UI/UX

- Custom components
- Loading states & animations & skeletons
- Responsive design with Tailwind CSS
- Dark/Light mode support

### ⚡ Performance Optimized

- Server-side rendering
- Incremental static regeneration
- Optimized images
- Fast page loads

## 🛠️ Tech Stack

### 🔍 Code Quality

[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=mustafacagri_next-stripe-supabase-tailwind-typescript&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=mustafacagri_next-stripe-supabase-tailwind)
[![Maintainability Rating](https://sonarcloud.io/api/project_badges/measure?project=mustafacagri_next-stripe-supabase-tailwind-typescript&metric=sqale_rating)](https://sonarcloud.io/summary/new_code?id=mustafacagri_next-stripe-supabase-tailwind)
[![Reliability Rating](https://sonarcloud.io/api/project_badges/measure?project=mustafacagri_next-stripe-supabase-tailwind-typescript&metric=reliability_rating)](https://sonarcloud.io/summary/new_code?id=mustafacagri_next-stripe-supabase-tailwind)
[![Security Rating](https://sonarcloud.io/api/project_badges/measure?project=mustafacagri_next-stripe-supabase-tailwind-typescript&metric=security_rating)](https://sonarcloud.io/summary/new_code?id=mustafacagri_next-stripe-supabase-tailwind)

This project is analyzed by:

- 🔍 **SonarCloud** - Continuous code quality analysis
- 🤖 **CodeRabbitAI** - AI-powered code reviews

### 🎨 Frontend

- ⚛️ Next.js 14
- 🌐 React.js 18
- 🎨 Tailwind CSS
- 📝 TypeScript
- 🔄 React Query / Tanstack

### 🔐 Backend

- 🗄️ Supabase
- 💳 Stripe API
- 🔒 Auth Providers

## 🚀 Getting Started

### 📋 Clone the repository

```
git clone https://github.com/mustafacagri/next-stripe-supabase-tailwind.git
cd next-stripe-supabase-tailwind
```

### 📦 Install dependencies

```
npm install
# or
yarn install
# or
pnpm install
```

### 🔑 Set up environment variables

```
cp .env.example .env.local
```

Fill in your environment variables:

```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
NEXT_SITE_URL=http://localhost:3216 or whatever you are using
STRIPE_WEBHOOK_SECRET=whsec_xxxxxxx
STRIPE_SECRET_KEY=sk_test_xxxxxx
NEXT_PUBLIC_STRIPE_PUBLISAHEBLE_KEY=pk_test_xxxx
```

### 🏃‍♂️ Run the development server

```
npm run dev
# or
yarn dev
# or
pnpm dev
```

Visit http://localhost:3216 to see your app! 🎉

## 🔄 Database Schema

Our Supabase schema includes:

```
-- Create profiles table
CREATE TABLE profiles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),  -- Automatically generate UUID
    first_name TEXT,
    last_name TEXT,
    email TEXT UNIQUE,
    picture TEXT,
    name TEXT,
    is_subscribed BOOLEAN DEFAULT FALSE,
    plan_id UUID REFERENCES pricing_plans(id) ON DELETE SET NULL,  -- Foreign key to pricing_plans
    stripe_customer_id TEXT,
    last_plan_update TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Create pricing_plans table
CREATE TABLE pricing_plans (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),  -- Automatically generate UUID
    name TEXT,
    slug TEXT UNIQUE,
    price_monthly INTEGER,
    price_yearly INTEGER,
    description TEXT,
    cta TEXT,
    most_popular BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE,
    is_featured BOOLEAN DEFAULT FALSE
);

-- Create pricing_features table
CREATE TABLE pricing_features (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),  -- Automatically generate UUID
    name TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    plan_id UUID REFERENCES pricing_plans(id) ON DELETE CASCADE  -- Foreign key to pricing_plans
);
```

## Database Triggers and Functions

### Function: `handle_new_user`

This function is triggered when a new user is created via authentication. It inserts a record into the `profiles` table with the relevant user details.

#### Function Definition

```sql
CREATE OR REPLACE FUNCTION handle_new_user()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO public.profiles (id, first_name, last_name, email, picture, name)
    VALUES (
        NEW.id,
        NEW.raw_user_meta_data ->> 'first_name',
        NEW.raw_user_meta_data ->> 'last_name',
        NEW.raw_user_meta_data ->> 'email',
        COALESCE(NEW.raw_user_meta_data ->> 'picture', NEW.raw_user_meta_data ->> 'avatar_url'),
        NEW.raw_user_meta_data ->> 'name'
    );
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

```

### 👾 How can I contribute?

- ⭐ Star the repository
- 🛠️ Submit pull requests, report bugs, or suggest features

Made using React.js 18 ⚛️ Next.js 14 🌐 Stripe 💳 TailwindCSS 🎨 TypeScript 🔧 React Query / Tanstack 🔄 and Lodash 🛠️!
