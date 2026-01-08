# Codebase Exploration

Before writing requirements, explore the codebase to inform:
- Data requirements (align with existing patterns)
- Technical considerations (note relevant code)
- Acceptance criteria (match existing formats)
- Business rules (understand current logic)

## What to Look For

| Writing | Find in Codebase |
|---------|------------------|
| Data requirements | Models, schemas, migrations |
| Technical considerations | Architecture, patterns, dependencies |
| Integration points | Existing APIs, services, auth |
| Business rules | Validation logic, constants, enums |
| Edge cases | Error handling patterns |

## Exploration Patterns

### 1. Understand Project Structure

```bash
# View top-level structure
view .

# Common directories
view app/models/
view app/controllers/
view app/services/
view db/
view src/
```

### 2. Find Related Code

```bash
# Search for related terms
grep -r "consent" --include="*.rb" app/
grep -r "consent" --include="*.ts" src/
grep -r "customer" --include="*.rb" app/models/

# Find files by name
find . -name "*consent*" -type f
find . -name "*customer*" -type f
```

### 3. Check Database Schema

```bash
# Rails
view db/schema.rb
grep -A 20 "create_table.*customers" db/schema.rb

# Migrations
ls db/migrate/ | grep consent
view db/migrate/XXXXX_create_consents.rb
```

### 4. Review Existing Models

```bash
# Find model relationships
grep -r "belongs_to\|has_many\|has_one" app/models/
view app/models/customer.rb
```

### 5. Check API Patterns

```bash
# Find routes
view config/routes.rb
grep -r "consents" config/routes.rb

# Find controllers
view app/controllers/api/
grep -r "def create\|def update" app/controllers/
```

### 6. Find Validation/Business Rules

```bash
# Rails validations
grep -r "validates" app/models/
grep -r "before_save\|after_create" app/models/

# Constants/enums
grep -r "enum\|CONSTANT\|STATUS" app/models/
```

## How to Use Findings

### In Data Requirements

Found in codebase:
```ruby
# app/models/customer.rb
belongs_to :brand
has_many :orders
```

Write in spec:
```markdown
| Data Element | Required | Format | Notes |
|--------------|----------|--------|-------|
| Customer identifier | Yes | Reference | Link to existing customer (belongs_to :brand) |
```

### In Technical Considerations

Found in codebase:
```ruby
# app/models/concerns/auditable.rb
module Auditable
  # existing audit trail pattern
end
```

Write in spec:
```markdown
## Technical Considerations

- **Pattern:** Consider using existing `Auditable` concern for audit trail
```

### In Acceptance Criteria

Found in codebase:
```ruby
# app/controllers/api/v1/base_controller.rb
def render_error(message, status: :unprocessable_entity)
  render json: { error: message }, status: status
end
```

Write in story:
```markdown
## Acceptance Criteria

- If consent already withdrawn, returns error: "Already withdrawn" (422)
```

## When Codebase is Not Available

If no codebase is provided in the context:
1. Ask user if they can share relevant code
2. Write requirements at a higher level (less specific)
3. Add more items to "Open Questions for Engineering"
4. Note assumptions explicitly
