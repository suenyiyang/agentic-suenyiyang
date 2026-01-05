# API Design Review

## REST API Patterns

### Resource Naming
```
// GOOD
GET    /employees
GET    /employees/:id
POST   /employees
PUT    /employees/:id
DELETE /employees/:id

// BAD
GET    /getEmployees
POST   /createEmployee
POST   /employee/delete/:id
```

### Endpoint Consistency
- Are naming conventions consistent?
- Is the resource hierarchy logical?
- Are query parameters used appropriately?

## Request/Response Design

### Request Validation
- Are inputs validated before processing?
- Are error messages helpful?

### Response Structure
```typescript
// GOOD - Consistent structure
{
  data: Employee | Employee[],
  meta: { total: number, page: number },
  errors?: ErrorDetail[]
}

// BAD - Inconsistent responses
{ employee: {...} }  // Single
{ employees: [...] } // List
{ error: "..." }     // Error
```

### Pagination
- Is pagination implemented for list endpoints?
- Are pagination params consistent?

## Error Handling

### HTTP Status Codes
```
200 - Success
201 - Created
400 - Bad Request (validation error)
401 - Unauthorized
403 - Forbidden
404 - Not Found
500 - Server Error
```

### Error Response Format
```typescript
{
  error: {
    code: 'VALIDATION_ERROR',
    message: 'Human readable message',
    details: [
      { field: 'email', message: 'Invalid format' }
    ]
  }
}
```

## Frontend SDK Design

### Method Naming
```typescript
// GOOD - Domain-focused
sdk.employees.list()
sdk.employees.getById(id)
sdk.employees.create(data)
sdk.onboarding.startProcess(employeeId)

// BAD - HTTP-focused
sdk.get('/employees')
sdk.post('/employees', data)
```

### Type Safety
- Are request/response types defined?
- Do types match backend contracts?

### Error Handling
- Are API errors transformed to app errors?
- Is retry logic implemented for transient failures?

## Performance

### Request Optimization
- Is data overfetching avoided?
- Can multiple requests be batched?
- Is caching strategy appropriate?

### Response Size
- Are responses paginated?
- Is unnecessary data excluded?
