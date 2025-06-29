# asasvirtuais-next

Next.js utilities for CRUD operations, designed to work with @asasvirtuais/crud.

## Installation

```bash
npm install asasvirtuais-next @asasvirtuais/crud
```

## Usage

### Option 1: Individual Route Handlers

```typescript
// app/api/v1/users/route.ts
import { routes } from 'asasvirtuais-next'
import { crud, someBackend } from '@asasvirtuais/crud'

const db = crud(someBackend())
const { list, create } = routes({ implementation: db })

export { list as GET, create as POST }
```

```typescript
// app/api/v1/users/[id]/route.ts
import { routes } from 'asasvirtuais-next'
import { crud, someBackend } from '@asasvirtuais/crud'

const db = crud(someBackend())
const { find, update, remove } = routes({ implementation: db })

export { find as GET, update as PATCH, remove as DELETE }
```

### Option 2: Dynamic Catch-All Route

```typescript
// app/api/v1/[...slug]/route.ts
import { createDynamicRoute } from 'asasvirtuais-next'
import { crud, someBackend } from '@asasvirtuais/crud'

const db = crud(someBackend())
const handler = createDynamicRoute(db)

export { 
  handler as GET, 
  handler as POST, 
  handler as PATCH, 
  handler as DELETE 
}
```

This creates routes that automatically handle:
- `GET /api/v1/users` → list users
- `GET /api/v1/users/123` → find user by ID  
- `POST /api/v1/users` → create user
- `PATCH /api/v1/users/123` → update user
- `DELETE /api/v1/users/123` → delete user

## API

### routes(config)

Creates individual route handlers.

**Parameters:**
- `config.implementation` - CRUD backend instance

**Returns:**
- `find` - GET handler for single records
- `list` - GET handler for listing records  
- `create` - POST handler for creating records
- `update` - PATCH handler for updating records
- `remove` - DELETE handler for deleting records

### createDynamicRoute(implementation)

Creates a single handler that routes to appropriate CRUD operations based on HTTP method and URL parameters.

**Parameters:**
- `implementation` - CRUD backend instance

**Returns:**
- Dynamic route handler function

## License

MIT