---
name: API Integration
description: Guide for integrating external APIs securely and correctly
triggers:
  - integrate api
  - add api integration
  - connect to api
  - setup api
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash(npm install*)
  - Bash(pnpm add*)
---

# API Integration Skill

Implement secure, robust integrations with external APIs.

## Integration Checklist

### 1. Authentication Setup
- [ ] Store API keys in environment variables
- [ ] Never commit secrets to git
- [ ] Use appropriate auth method (API key, OAuth, JWT)
- [ ] Implement token refresh if needed

### 2. Client Configuration
- [ ] Set appropriate timeouts
- [ ] Configure retry logic
- [ ] Add request/response logging (without sensitive data)
- [ ] Use HTTPS only

### 3. Error Handling
- [ ] Handle rate limiting (429 status)
- [ ] Handle authentication errors (401, 403)
- [ ] Handle server errors (500+)
- [ ] Handle network failures
- [ ] Provide meaningful error messages

### 4. Data Validation
- [ ] Validate API responses match expected schema
- [ ] Handle missing/null fields gracefully
- [ ] Sanitize data before storing

## Standard API Client Pattern

```typescript
// lib/api/external-service.ts

import { z } from 'zod';

const API_BASE = 'https://api.example.com/v1';

// Response schema validation
const UserSchema = z.object({
  id: z.string(),
  email: z.string().email(),
  name: z.string(),
});

type User = z.infer<typeof UserSchema>;

class ExternalServiceClient {
  private apiKey: string;

  constructor() {
    const apiKey = process.env.EXTERNAL_SERVICE_API_KEY;
    if (!apiKey) {
      throw new Error('EXTERNAL_SERVICE_API_KEY is required');
    }
    this.apiKey = apiKey;
  }

  private async request<T>(
    endpoint: string,
    options: RequestInit = {}
  ): Promise<T> {
    const url = `${API_BASE}${endpoint}`;

    const response = await fetch(url, {
      ...options,
      headers: {
        'Authorization': `Bearer ${this.apiKey}`,
        'Content-Type': 'application/json',
        ...options.headers,
      },
    });

    if (!response.ok) {
      if (response.status === 429) {
        throw new RateLimitError('Rate limit exceeded');
      }
      if (response.status === 401) {
        throw new AuthenticationError('Invalid API key');
      }
      throw new APIError(`API error: ${response.status}`);
    }

    return response.json();
  }

  async getUser(userId: string): Promise<User> {
    const data = await this.request(`/users/${userId}`);
    return UserSchema.parse(data);
  }
}

export const externalService = new ExternalServiceClient();
```

## Environment Variables

```env
# .env.example (commit this)
EXTERNAL_SERVICE_API_KEY=your_api_key_here

# .env.local (never commit)
EXTERNAL_SERVICE_API_KEY=sk_live_xxxxxxxxxxxxx
```

## Webhook Handling

```typescript
// app/api/webhooks/external-service/route.ts

import { headers } from 'next/headers';
import crypto from 'crypto';

export async function POST(request: Request) {
  const body = await request.text();
  const signature = headers().get('x-webhook-signature');

  // ALWAYS verify webhook signatures
  const expectedSignature = crypto
    .createHmac('sha256', process.env.WEBHOOK_SECRET!)
    .update(body)
    .digest('hex');

  if (signature !== expectedSignature) {
    return new Response('Invalid signature', { status: 401 });
  }

  const event = JSON.parse(body);

  // Handle webhook event
  switch (event.type) {
    case 'user.created':
      await handleUserCreated(event.data);
      break;
    // ... other events
  }

  return new Response('OK', { status: 200 });
}
```

## Rate Limiting Strategies

1. **Exponential Backoff**
   ```typescript
   async function withRetry<T>(fn: () => Promise<T>, maxRetries = 3): Promise<T> {
     for (let i = 0; i < maxRetries; i++) {
       try {
         return await fn();
       } catch (error) {
         if (error instanceof RateLimitError && i < maxRetries - 1) {
           await sleep(Math.pow(2, i) * 1000);
           continue;
         }
         throw error;
       }
     }
     throw new Error('Max retries exceeded');
   }
   ```

2. **Request Queue**
   - Queue requests and process at allowed rate
   - Use libraries like `p-queue` or `bottleneck`

## Testing API Integrations

- Use mock servers for unit tests
- Use sandbox/test environments when available
- Test error scenarios explicitly
- Never use production keys in tests
