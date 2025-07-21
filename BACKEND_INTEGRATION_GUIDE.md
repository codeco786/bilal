# Backend Integration Guide

## Option 1: Replace Frontend API Calls (Recommended)

### Step 1: Update API Base URL
In `client/src/lib/queryClient.ts`, modify the fetch calls to use your backend URL:

```typescript
// Add your backend base URL
const API_BASE_URL = 'https://your-backend.com/api';

export async function apiRequest(
  method: string,
  url: string,
  data?: unknown | undefined,
): Promise<Response> {
  const fullUrl = url.startsWith('/api') ? `${API_BASE_URL}${url.replace('/api', '')}` : url;
  
  const res = await fetch(fullUrl, {
    method,
    headers: data ? { "Content-Type": "application/json" } : {},
    body: data ? JSON.stringify(data) : undefined,
    credentials: "include",
  });

  await throwIfResNotOk(res);
  return res;
}
```

### Step 2: Your Backend API Endpoints Required

Your backend needs these endpoints:

**Products:**
- `GET /products` - List all products (supports ?category= and ?featured=true)
- `GET /products/:id` - Get single product
- `POST /products` - Create product (admin only)

**Blog:**
- `GET /blog` - List blog posts (supports ?published=true)
- `GET /blog/:id` - Get single blog post
- `POST /blog` - Create blog post (admin only)

**Custom Orders:**
- `GET /custom-orders` - List custom orders (admin only)
- `POST /custom-orders` - Create custom order
- `PATCH /custom-orders/:id` - Update order status (admin only)

**Cart:**
- `GET /cart/:sessionId` - Get cart items
- `POST /cart` - Add item to cart
- `PATCH /cart/:id` - Update cart item quantity
- `DELETE /cart/:id` - Remove cart item
- `DELETE /cart/session/:sessionId` - Clear cart

### Step 3: Expected Data Formats

Your backend should return data matching these TypeScript interfaces:

```typescript
// Product
interface Product {
  id: number;
  title: string;
  titleUrdu: string | null;
  description: string;
  descriptionUrdu: string | null;
  price: string;
  category: string;
  image: string;
  inStock: boolean | null;
  featured: boolean | null;
  size: string | null;
  frameStyle: string | null;
  colorScheme: string | null;
  scriptStyle: string | null;
  createdAt: Date | null;
}

// Blog Post
interface BlogPost {
  id: number;
  title: string;
  titleUrdu: string | null;
  category: string;
  image: string;
  createdAt: Date | null;
  content: string;
  contentUrdu: string | null;
  excerpt: string;
  excerptUrdu: string | null;
  readTime: number;
  published: boolean | null;
}

// Custom Order
interface CustomOrder {
  id: number;
  frameStyle: string;
  size: string;
  colorScheme: string;
  createdAt: Date | null;
  status: string | null;
  customerName: string;
  customerEmail: string;
  urduText: string;
  englishTranslation: string | null;
  specialInstructions: string | null;
  quotedPrice: string | null;
}

// Cart Item
interface CartItem {
  id: number;
  createdAt: Date | null;
  productId: number | null;
  quantity: number | null;
  sessionId: string;
}
```

## Option 2: Keep Current Backend, Add Your Services

### Step 1: Add External API Integration
Create new services that connect to your backend while keeping the current structure:

```typescript
// In server/external-api.ts
export class ExternalAPIService {
  private baseURL = 'https://your-backend.com/api';
  
  async syncProducts() {
    // Fetch products from your backend and sync to local database
  }
  
  async sendOrderNotification(order: CustomOrder) {
    // Send order to your backend for processing
  }
  
  async updateInventory(productId: number, quantity: number) {
    // Update inventory in your backend
  }
}
```

### Step 2: Modify Current Routes
Update `server/routes.ts` to call your backend services alongside local storage.

## Option 3: Use Current Backend as Proxy

Transform the current Express server into a proxy that forwards requests to your backend:

```typescript
// In server/routes.ts
import httpProxy from 'http-proxy-middleware';

app.use('/api', httpProxy({
  target: 'https://your-backend.com',
  changeOrigin: true,
  pathRewrite: {
    '^/api': '/api/v1' // Adjust path as needed
  }
}));
```

## Option 4: Database-to-Database Sync

Keep the current PostgreSQL database but sync data with your backend:

1. Set up scheduled jobs to sync data
2. Use webhooks for real-time updates
3. Implement conflict resolution strategies

## Authentication Integration

If your backend has authentication, update the frontend:

```typescript
// Add to queryClient.ts
export async function apiRequest(
  method: string,
  url: string,
  data?: unknown | undefined,
): Promise<Response> {
  const token = localStorage.getItem('auth_token'); // Your auth token
  
  const res = await fetch(url, {
    method,
    headers: {
      ...(data ? { "Content-Type": "application/json" } : {}),
      ...(token ? { "Authorization": `Bearer ${token}` } : {}),
    },
    body: data ? JSON.stringify(data) : undefined,
  });

  await throwIfResNotOk(res);
  return res;
}
```

## Environment Configuration

Add environment variables for your backend:

```bash
# .env
VITE_API_BASE_URL=https://your-backend.com/api
VITE_AUTH_ENDPOINT=https://your-backend.com/auth
```

## Recommendation

**Option 1 (Replace API endpoints)** is recommended because:
- Minimal code changes
- Keeps the beautiful frontend intact
- Leverages your existing backend infrastructure
- Easy to maintain and update

Your backend just needs to implement the API endpoints with the expected data formats, and the frontend will work seamlessly with your system.