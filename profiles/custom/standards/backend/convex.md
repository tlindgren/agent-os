## Convex Backend Development Guidelines

### Overview

Convex is a full-stack platform that combines a serverless database, real-time queries, and backend functions. This guide provides development best practices and instructions for accessing the most current documentation.

### Getting Current Documentation

When working with Convex, always use the WebFetch or WebSearch tools to access the most recent API documentation and development guidelines:

```
WebFetch URL: https://www.convex.dev/llms.txt
```

This llms.txt file provides direct links to the most current Convex documentation, API references, and development guides.

### Core Concepts

#### 1. Schema Definition

- ✅ **DO:** Define your database schema in `convex/schema.ts`
- ✅ **DO:** Use TypeScript types for strong typing throughout your backend
- ✅ **DO:** Define indexes for commonly queried fields

```typescript
// convex/schema.ts
import { defineSchema, defineTable } from "convex/server";
import { v } from "convex/values";

export default defineSchema({
  posts: defineTable({
    title: v.string(),
    content: v.string(),
    authorId: v.id("users"),
    published: v.boolean(),
    createdAt: v.number(),
  })
    .index("by_author", ["authorId"])
    .index("by_published", ["published", "createdAt"]),
    
  users: defineTable({
    name: v.string(),
    email: v.string(),
  })
    .index("by_email", ["email"]),
});
```

#### 2. Queries (Data Fetching)

- ✅ **DO:** Use queries for reading data
- ✅ **DO:** Queries are real-time and automatically update when data changes
- ✅ **DO:** Use indexes to optimize query performance
- ❌ **DON'T:** Perform mutations or side effects in queries

```typescript
// convex/posts.ts
import { query } from "./_generated/server";
import { v } from "convex/values";

export const list = query({
  args: { authorId: v.optional(v.id("users")) },
  handler: async (ctx, args) => {
    if (args.authorId) {
      return await ctx.db
        .query("posts")
        .withIndex("by_author", (q) => q.eq("authorId", args.authorId))
        .filter((q) => q.eq(q.field("published"), true))
        .collect();
    }
    
    return await ctx.db
      .query("posts")
      .withIndex("by_published", (q) => q.eq("published", true))
      .order("desc")
      .collect();
  },
});

export const get = query({
  args: { id: v.id("posts") },
  handler: async (ctx, args) => {
    return await ctx.db.get(args.id);
  },
});
```

#### 3. Mutations (Data Modifications)

- ✅ **DO:** Use mutations for creating, updating, or deleting data
- ✅ **DO:** Validate input data in mutations
- ✅ **DO:** Check authorization before modifying data
- ✅ **DO:** Return meaningful data from mutations when needed

```typescript
// convex/posts.ts
import { mutation } from "./_generated/server";
import { v } from "convex/values";

export const create = mutation({
  args: {
    title: v.string(),
    content: v.string(),
  },
  handler: async (ctx, args) => {
    const identity = await ctx.auth.getUserIdentity();
    if (!identity) {
      throw new Error("Unauthenticated");
    }
    
    const user = await ctx.db
      .query("users")
      .withIndex("by_email", (q) => q.eq("email", identity.email))
      .unique();
      
    if (!user) {
      throw new Error("User not found");
    }
    
    const postId = await ctx.db.insert("posts", {
      title: args.title,
      content: args.content,
      authorId: user._id,
      published: false,
      createdAt: Date.now(),
    });
    
    return postId;
  },
});

export const update = mutation({
  args: {
    id: v.id("posts"),
    title: v.optional(v.string()),
    content: v.optional(v.string()),
    published: v.optional(v.boolean()),
  },
  handler: async (ctx, args) => {
    const { id, ...updates } = args;
    
    const post = await ctx.db.get(id);
    if (!post) {
      throw new Error("Post not found");
    }
    
    await ctx.db.patch(id, updates);
  },
});

export const remove = mutation({
  args: { id: v.id("posts") },
  handler: async (ctx, args) => {
    await ctx.db.delete(args.id);
  },
});
```

#### 4. Actions (External API Integration)

- ✅ **DO:** Use actions when you need to call external APIs or perform non-deterministic operations
- ✅ **DO:** Use actions for operations that can't be replayed (like sending emails)
- ❌ **DON'T:** Use actions for simple database operations (use mutations instead)

```typescript
// convex/notifications.ts
import { action } from "./_generated/server";
import { api } from "./_generated/api";
import { v } from "convex/values";

export const sendWelcomeEmail = action({
  args: { userId: v.id("users") },
  handler: async (ctx, args) => {
    const user = await ctx.runQuery(api.users.get, { id: args.userId });
    
    if (!user) {
      throw new Error("User not found");
    }
    
    // Call external email service
    await fetch("https://api.emailservice.com/send", {
      method: "POST",
      headers: { "Authorization": `Bearer ${process.env.EMAIL_API_KEY}` },
      body: JSON.stringify({
        to: user.email,
        subject: "Welcome!",
        body: `Welcome ${user.name}!`,
      }),
    });
    
    // Record that email was sent
    await ctx.runMutation(api.notifications.markSent, {
      userId: args.userId,
      type: "welcome_email",
    });
  },
});
```

#### 5. Authentication

- ✅ **DO:** Use Convex Auth or integrate with your auth provider
- ✅ **DO:** Check authentication in mutations and sensitive queries
- ✅ **DO:** Use `ctx.auth.getUserIdentity()` to get the current user

```typescript
// convex/auth.ts
import { mutation } from "./_generated/server";

export const storeUser = mutation({
  args: {},
  handler: async (ctx) => {
    const identity = await ctx.auth.getUserIdentity();
    
    if (!identity) {
      throw new Error("Unauthenticated");
    }
    
    // Check if user already exists
    const existingUser = await ctx.db
      .query("users")
      .withIndex("by_email", (q) => q.eq("email", identity.email!))
      .unique();
      
    if (existingUser) {
      return existingUser._id;
    }
    
    // Create new user
    const userId = await ctx.db.insert("users", {
      name: identity.name ?? "Anonymous",
      email: identity.email!,
    });
    
    return userId;
  },
});
```

#### 6. File Storage

- ✅ **DO:** Use Convex's built-in file storage for images and documents
- ✅ **DO:** Generate URLs for file access
- ✅ **DO:** Store file metadata in your database

```typescript
// convex/files.ts
import { mutation, query } from "./_generated/server";
import { v } from "convex/values";

export const generateUploadUrl = mutation({
  args: {},
  handler: async (ctx) => {
    return await ctx.storage.generateUploadUrl();
  },
});

export const saveFile = mutation({
  args: {
    storageId: v.id("_storage"),
    name: v.string(),
    type: v.string(),
  },
  handler: async (ctx, args) => {
    const fileId = await ctx.db.insert("files", {
      storageId: args.storageId,
      name: args.name,
      type: args.type,
      uploadedAt: Date.now(),
    });
    
    return fileId;
  },
});

export const getUrl = query({
  args: { storageId: v.id("_storage") },
  handler: async (ctx, args) => {
    return await ctx.storage.getUrl(args.storageId);
  },
});
```

### Best Practices

#### Error Handling

- ✅ **DO:** Throw descriptive errors
- ✅ **DO:** Validate inputs before processing
- ✅ **DO:** Handle edge cases explicitly

```typescript
export const myMutation = mutation({
  args: { value: v.number() },
  handler: async (ctx, args) => {
    if (args.value < 0) {
      throw new Error("Value must be positive");
    }
    
    // Process...
  },
});
```

#### Performance Optimization

- ✅ **DO:** Use indexes for frequently queried fields
- ✅ **DO:** Limit the number of documents returned in queries
- ✅ **DO:** Batch related database operations when possible
- ❌ **DON'T:** Perform expensive computations in queries

```typescript
// ✅ CORRECT: Using index and limiting results
export const recent = query({
  args: { limit: v.optional(v.number()) },
  handler: async (ctx, args) => {
    return await ctx.db
      .query("posts")
      .withIndex("by_published")
      .order("desc")
      .take(args.limit ?? 10);
  },
});
```

#### Real-Time Subscriptions

- ✅ **DO:** Leverage Convex's automatic real-time updates
- ✅ **DO:** Use queries for data that needs to be reactive
- ❌ **DON'T:** Implement polling when real-time queries work

```typescript
// In React component (Next.js Client Component)
'use client';
import { useQuery } from "convex/react";
import { api } from "@/convex/_generated/api";

export function PostsList() {
  // This automatically updates when posts change
  const posts = useQuery(api.posts.list);
  
  if (posts === undefined) return <div>Loading...</div>;
  
  return (
    <div>
      {posts.map((post) => (
        <div key={post._id}>{post.title}</div>
      ))}
    </div>
  );
}
```

### Naming Conventions

- ✅ **DO:** Use descriptive function names (e.g., `list`, `get`, `create`, `update`, `remove`)
- ✅ **DO:** Group related functions in the same file
- ✅ **DO:** Use kebab-case for file names
- ✅ **DO:** Export functions with clear, action-oriented names

### When to Fetch Documentation

Use WebFetch with the llms.txt URL when:
- Setting up new Convex projects
- Working with specific Convex APIs
- Implementing authentication flows
- Configuring database schemas
- Debugging Convex-specific issues
- Learning about new features or updates

### AI Model Verification Checklist

Before generating code, verify:

1. **Function Type:** Is the correct function type used (query/mutation/action)?
2. **Arguments:** Are arguments properly typed with `v` validators?
3. **Authentication:** Are auth checks included where needed?
4. **Error Handling:** Are inputs validated and errors descriptive?
5. **Indexes:** Are database queries using appropriate indexes?
6. **Real-time:** Is the pattern leveraging Convex's real-time capabilities?
