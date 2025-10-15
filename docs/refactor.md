# /code/refactor - Smart Refactoring Router

**Auto-detects project type and applies intelligent code quality improvements**

## Overview

The `/code/refactor` command automatically detects your project type (Go, JavaScript, PHP, Python, Rust) and applies appropriate refactoring strategies. Focus on making code more maintainable, readable, and reusable without changing behavior.

## When to Use

Use `/code/refactor` when:
- **Code smells detected** - Duplicated code, long functions, deep nesting
- **After feature completion** - Clean up before committing
- **Before production** - Ensure code quality standards
- **Technical debt paydown** - Systematic improvement time
- **Post-audit improvements** - Implementing `/code/repomix` recommendations
- **Preparing for team review** - Make code reviewer-friendly

## Auto-Detection

The command automatically detects your project type:

- **Go** - Finds `go.mod` → Applies Go idioms and patterns
- **JavaScript/TypeScript** - Finds `package.json` → Modern ES6+ patterns
- **React** - Detects React in `package.json` → Component best practices
- **Next.js** - Detects Next.js → App Router patterns
- **PHP** - Finds `composer.json` → PSR compliance, SOLID principles
- **Python** - Finds `requirements.txt`/`pyproject.toml` → PEP 8, type hints
- **Rust** - Finds `Cargo.toml` → Rust idioms and ownership patterns

## Refactoring Priorities

### 1. Reusability-First Design
- **Pure functions** - Transform stateful code into pure functions
- **Dependency injection** - Replace hard-coded dependencies
- **Single responsibility** - Each function does ONE thing
- **Composable building blocks** - Small, focused pieces that combine

### 2. Simplify
- **Reduce complexity** - Flatten deep nesting, reduce conditions
- **Clear naming** - Descriptive, intention-revealing names
- **Extract complex logic** - Break down into well-named functions

### 3. Organize
- **Proper structure** - Files in correct directories
- **Logical grouping** - Related functionality together
- **Separation of concerns** - Clear boundaries between layers

### 4. Clean
- **Remove dead code** - Unused imports, variables, functions
- **Delete commented code** - Use git history instead
- **Eliminate duplication** - Extract repeated logic (3+ occurrences)

### 5. Clarify
- **Self-documenting code** - Code explains itself
- **Explicit over clever** - Readable trumps concise
- **Consistent conventions** - Follow project patterns

## Usage Examples

```bash
# Auto-detect and refactor (recommended)
/code/refactor

# Focus on specific area
/code/refactor logging
/code/refactor performance
/code/refactor testing

# Package reorganization (Go projects)
/code/refactor package
```

## What to Expect

The command will:

1. **Analyze your codebase**
   - Scan for code smells and duplication
   - Identify complexity hotspots
   - Find hard-coded dependencies

2. **Apply improvements**
   - Extract pure functions from duplicated logic
   - Simplify complex conditionals
   - Improve naming and structure
   - Remove dead code and clutter

3. **Generate report**
   - List of refactorings performed
   - Before/after code samples
   - Duplication statistics
   - Recommendations for domain-specific utils

4. **Preserve functionality**
   - No behavior changes
   - All tests still pass
   - Maintains existing patterns

## Duplication Detection & Utils Extraction

When logic is duplicated 3+ times:

```
DUPLICATION DETECTED: Date formatting logic duplicated 7 times
RECOMMENDATION: Extract to date-utils.ts

// Suggested utils file:
export function formatLifespan(birth: Date | null, death: Date | null): string {
  if (!birth) return 'Unknown';
  if (!death) return `b. ${formatYear(birth)}`;
  return `${formatYear(birth)} - ${formatYear(death)}`;
}

IMPACT: Test once, use everywhere. 7 duplicate test suites → 1 comprehensive suite.
```

## Language-Specific Improvements

### Go Projects
- Pure functions with dependency injection via interfaces
- Idiomatic patterns (accept interfaces, return structs)
- Proper error handling (never ignore errors)
- Context usage for cancellation
- Table-driven tests

### JavaScript/TypeScript
- Pure functions with DI via parameters
- Modern ES6+ patterns (async/await, destructuring)
- Eliminate `:any` types (use proper types)
- React: pure components, custom hooks for reusable logic
- Domain-specific utils for transformations

### PHP Projects
- DI via constructors, interface-driven design
- PSR compliance (PSR-12 style, PSR-4 autoloading)
- Type declarations (strict_types, typed properties)
- SOLID principles
- Value objects for immutable data

## Output

After refactoring, you'll receive:

**Refactoring Report:**
- Summary of improvements made
- Before/after code samples for major changes
- Duplication analysis with metrics
- Domain-specific utils recommendations
- Remaining opportunities for future work

## When NOT to Use

Skip `/code/refactor` for:
- **Breaking changes needed** - Use feature branches
- **Major architectural changes** - Plan separately
- **Time-critical fixes** - Fix first, refactor later
- **Experimental code** - Let it stabilize first

## Philosophy

**"Make it better, not perfect - with reusability from day one."**

Refactoring is about incremental improvement toward reusable, maintainable code. Focus on practical changes that:
- Have clear value
- Don't break existing functionality
- Make future work easier
- Can be reviewed and understood quickly

Perfect code doesn't exist. Reusable, well-designed code does.

## Integration with Other Commands

```bash
# After refactoring, clean workspace
/code/refactor → /code/dry

# Refactor after debugging
/code/debug → /code/refactor → /code/dry

# Production workflow
/code/repomix → /code/refactor (fix issues) → /code/dry → /code/repomix

# Complete quality cycle
/code/refactor → /code/dry → /code/repomix (validate)
```

## Success Criteria

Refactoring succeeds when:
- ✅ Code is more maintainable than before
- ✅ All tests still pass (no behavior changes)
- ✅ Complexity reduced measurably
- ✅ Duplication eliminated or documented
- ✅ Code follows project conventions
- ✅ Future modifications will be easier

## Common Focus Areas

### Focus: Logging
```bash
/code/refactor logging

# Improves:
# - Consistent log levels (DEBUG, INFO, WARN, ERROR)
# - Structured logging (JSON format)
# - Meaningful log messages
# - Proper context inclusion
```

### Focus: Performance
```bash
/code/refactor performance

# Optimizes:
# - Hot paths in critical functions
# - Database query efficiency
# - Memory allocation patterns
# - Async/await usage
```

### Focus: Testing
```bash
/code/refactor testing

# Enhances:
# - Test structure and organization
# - Test naming and clarity
# - Setup/teardown patterns
# - Coverage of edge cases
```

## Requirements

**Required:**
- Language-specific specialists installed in `~/.claude/agents/`
  - `go-specialist.md`
  - `js-specialist.md`
  - `php-specialist.md`
  - `react-specialist.md` (optional, for React projects)
  - `nextjs-specialist.md` (optional, for Next.js projects)

**Fallback:**
- `project-analysis-specialist` used if language specialist unavailable

## Reusability Philosophy

**"Design for reusability from day one"**

### The 8 Reusability Principles

Applied during refactoring:

1. **Pure Functions First** - Same inputs → same outputs, no side effects
2. **Dependency Injection** - Pass dependencies as parameters, don't hard-code
3. **Interface-Driven Design** - Define clear contracts (TypeScript interfaces, Go interfaces)
4. **Composition Over Inheritance** - Build complex behavior from simple components
5. **Loose Coupling, High Cohesion** - Minimize dependencies between components
6. **Composable Building Blocks** - Small, focused pieces that combine into larger systems
7. **Clear, Stable APIs** - Descriptive names, consistent patterns
8. **Avoid Global State** - Explicit state management, pass context

### When to Extract to Utils

**The 3-Occurrence Rule:**

**1st occurrence** → Inline code (pure function, DI, SRP)
**2nd occurrence** → Copy-paste (keep clean principles)
**3rd occurrence** → Extract to domain-specific utils

**Example: Domain-specific utils (genealogy project)**

```typescript
// ✅ PERFECT - genealogy-utils.ts
// Domain-specific, pure functions, used 10+ times

export function formatLifespan(birth: Date | null, death: Date | null): string {
  if (!birth) return 'Unknown';
  if (!death) return `b. ${formatYear(birth)}`;
  return `${formatYear(birth)} - ${formatYear(death)}`;
}

export function calculateRelationship(
  person1: Person,
  person2: Person,
  tree: FamilyTree
): Relationship {
  // Complex but deterministic relationship calculation
  // Used in: UI cards, reports, search, relationship explorer
}
```

**Why This Works:**
- ✅ Domain-specific (genealogy, not generic "string utils")
- ✅ High reuse (10+ call sites across handlers, middleware, services)
- ✅ Pure functions (same input → same output, no side effects)
- ✅ Easy to test (table-driven tests, no mocking needed)
- ✅ Single responsibility (each function does ONE thing)
- ✅ Composable (combine for complex business logic)

### Decision Tree: Should I Create Utils?

```
Is this logic duplicated 3+ times?
├─ NO → Keep inline (copy-paste is fine for now)
└─ YES ↓

Is this a pure function (same input → same output)?
├─ NO → Consider service/repository pattern instead
└─ YES ↓

Is this domain-specific (genealogy, pricing, etc.)?
├─ NO → Reconsider - might be too generic
└─ YES ↓

Does it have a single, clear responsibility?
├─ NO → Split into multiple focused functions
└─ YES ↓

✅ CREATE DOMAIN-SPECIFIC UTILS FILE
   - Name: {domain}-utils.ts (e.g., genealogy-utils.ts)
   - Write comprehensive tests
   - Export pure functions only
   - Document complex logic
```

## Detailed Refactoring Process

### Phase 1: Code Smell Detection

**Scans for:**
- Duplicated code (3+ occurrences)
- Long functions (>20 lines need justification)
- Hard-coded dependencies (should be injected)
- Global state usage (should be passed explicitly)
- Complex conditionals (candidates for extraction)

**Commands used:**
```bash
# Find duplicated patterns
grep -rn "pattern" src/

# Identify long functions
# Check complexity metrics
```

### Phase 2: Reusability Analysis

**Reusability checklist applied to ALL code:**
- [ ] Can this be a pure function?
- [ ] Are dependencies injected or hard-coded?
- [ ] Does it have a single, clear responsibility?
- [ ] Is this composable with other components?
- [ ] Is this logic duplicated 3+ times? (extract to utils)
- [ ] Are there hard-coded configs? (inject instead)
- [ ] Does it use global state? (pass explicitly)

### Phase 3: Systematic Improvements

**Applied in order:**

1. **Extract pure functions** from duplicated logic
2. **Inject dependencies** replacing hard-coded values
3. **Simplify complex conditionals** into well-named functions
4. **Organize code** into proper file/package structure
5. **Remove dead code** and clutter
6. **Improve naming** for clarity and intent
7. **Suggest domain-specific utils** for 3+ duplicate patterns

### Phase 4: Validation

**Before completion:**
- [ ] All tests still pass (no behavior changes)
- [ ] Code follows project conventions
- [ ] Complexity reduced measurably
- [ ] Duplication documented with extraction recommendations
- [ ] Reusability principles applied consistently

## Language-Specific Patterns

### Go Projects

**Reusability patterns:**
```go
// ✅ Pure function - deterministic calculation
func CalculateDelay(current, limit int, window time.Duration) time.Duration {
	if current < limit {
		return 0
	}
	return window / time.Duration(limit)
}

// ✅ Dependency injection via interfaces
type OrderService struct {
	repo     OrderRepository  // interface, not concrete type
	notifier Notifier        // interface, injected
	config   Config          // injected
}

func (s *OrderService) ProcessOrder(ctx context.Context, orderID int) error {
	// Dependencies injected, testable, composable
	order, err := s.repo.GetOrder(ctx, orderID)
	if err != nil {
		return fmt.Errorf("fetch order: %w", err)
	}
	// ... rest of processing
}
```

**Anti-patterns fixed:**
- ❌ Global `var` → Pass explicitly or inject
- ❌ Hard-coded dependencies → Dependency injection via interfaces
- ❌ Ignoring errors → Proper error handling with context

### JavaScript/TypeScript Projects

**Reusability patterns:**
```typescript
// ✅ Pure transformation function
function transformUserData(data: UserData, settings: Settings): TransformedData {
  return { /* transformation logic */ };
}

// ✅ Dependency injection via parameters
async function processUserData(deps: {
  api: ApiClient;
  db: Database;
  mailer: Mailer;
  settings: Settings;
}): Promise<TransformedData> {
  const data = await fetchUserData(deps.api);
  const transformed = transformUserData(data, deps.settings);
  await saveToDatabase(deps.db, transformed);
  return transformed;
}
```

**Anti-patterns fixed:**
- ❌ Global state and hard-coded dependencies
- ❌ `:any` types → Proper type guards and interfaces
- ❌ Nested callbacks → async/await patterns

### React Projects

**Reusability patterns:**
```typescript
// ✅ Pure component with hooks
function UserCard({ user, onUpdate }: UserCardProps) {
  // Custom hook for reusable logic
  const { isLoading, error } = useUserStatus(user.id);

  // Pure render logic
  return (
    <div>
      {/* UI based on props and hook state */}
    </div>
  );
}

// ✅ Custom hook for reusable stateful logic
function useUserStatus(userId: string) {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);

  // Reusable hook logic used across multiple components

  return { isLoading, error };
}
```

**Anti-patterns fixed:**
- ❌ Business logic in components → Extract to custom hooks
- ❌ Uncontrolled components → Controlled components with state lifted
- ❌ Missing memoization → React.memo, useMemo for expensive calculations

### PHP Projects

**Reusability patterns:**
```php
// ✅ Dependency injection via constructor
class OrderService {
    public function __construct(
        private OrderRepository $repo,      // interface
        private Notifier $notifier,         // interface
        private Config $config              // injected
    ) {}

    public function processOrder(int $orderId): Order {
        // Dependencies injected, testable, follows SOLID
        $order = $this->repo->getOrder($orderId);
        // ... processing
        return $order;
    }
}

// ✅ Value object for immutable data
final class Money {
    public function __construct(
        private readonly int $amount,
        private readonly string $currency
    ) {}

    public function add(Money $other): Money {
        // Pure function - returns new instance
        return new Money($this->amount + $other->amount, $this->currency);
    }
}
```

**Anti-patterns fixed:**
- ❌ Static methods in business logic → Instance methods with DI
- ❌ Missing type declarations → strict_types, typed properties
- ❌ Global state → Dependency injection via constructor

## Common Refactoring Scenarios

### Scenario: Eliminating Duplication

**Before:**
```typescript
// Date formatting duplicated 7 times across components
function PersonCard() {
  const formatted = birth && death
    ? `${birth.getFullYear()} - ${death.getFullYear()}`
    : birth ? `b. ${birth.getFullYear()}` : 'Unknown';
}

function Timeline() {
  const formatted = birth && death
    ? `${birth.getFullYear()} - ${death.getFullYear()}`
    : birth ? `b. ${birth.getFullYear()}` : 'Unknown';
}

// ... 5 more components with same logic
```

**After:**
```typescript
// genealogy-utils.ts
export function formatLifespan(birth: Date | null, death: Date | null): string {
  if (!birth) return 'Unknown';
  if (!death) return `b. ${formatYear(birth)}`;
  return `${formatYear(birth)} - ${formatYear(death)}`;
}

// Components use the util
function PersonCard() {
  const formatted = formatLifespan(birth, death);
}
```

**Impact:**
- 7 duplicate implementations → 1 tested utility
- 7 duplicate test suites → 1 comprehensive suite
- Bug fix in ONE place, confidence everywhere

### Scenario: Dependency Injection

**Before:**
```go
// Hard-coded dependencies
var globalDB *sql.DB

func ProcessOrder(orderID int) error {
	order := fetchOrder(orderID) // Uses globalDB
	// ... processing
}
```

**After:**
```go
// Dependency injection via interfaces
type OrderService struct {
	repo OrderRepository // interface
}

func (s *OrderService) ProcessOrder(ctx context.Context, orderID int) error {
	order, err := s.repo.GetOrder(ctx, orderID)
	// ... processing
}
```

**Impact:**
- Testable (can mock OrderRepository)
- Flexible (can swap implementations)
- No global state (explicit dependencies)

## Reference

**Complete Reusability Philosophy**: See `~/.claude/CLAUDE.md` § "Reusability-First Design"

Key concepts:
- [The 8 Reusability Principles](~/.claude/CLAUDE.md#the-reusability-mandate)
- [When Utilities Are Perfect](~/.claude/CLAUDE.md#when-utilities-files-are-perfect-domain-specific-helpers)
- [Decision Tree: Should I Create Utils?](~/.claude/CLAUDE.md#decision-tree-should-i-create-a-utils-file)
- [Language-Specific Patterns](~/.claude/CLAUDE.md#language-specific-reusability-patterns)
