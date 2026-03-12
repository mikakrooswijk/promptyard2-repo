# Refactor Angular Component to Signals

## Purpose
Refactor Angular components to use Signals to fix `NG0100 ExpressionChangedAfterItHasBeenCheckedError` errors and improve reactivity. Use this skill when asked to modernize a component or resolve change detection issues.

## When to Use
- Component uses `BehaviorSubject` / `Observable` for local state
- Component has `NG0100` errors in the console
- Component uses zone-based change detection with manual `markForCheck()` or `detectChanges()` calls
- User asks to "modernize", "use signals", or "refactor change detection"

## Step-by-Step Process

### 1. Identify Signal Candidates
Look for:
- `@Input()` — replace with `input()` / `input.required()`
- `@Output()` — replace with `output()`
- `@ViewChild` / `@ContentChild` — replace with `viewChild()` / `contentChild()`
- Local `BehaviorSubject` state — replace with `signal()`
- Derived values from observables — replace with `computed()`
- Side effects on state changes — replace with `effect()`

### 2. Update Imports
```typescript
// Remove
import { Input, Output, EventEmitter, OnInit } from '@angular/core';

// Add (only what you need)
import { signal, computed, effect, input, output, viewChild } from '@angular/core';
```

### 3. Apply Transformations

**Input:**
```typescript
// Before
@Input() title: string = '';

// After
title = input('');               // optional with default
title = input.required<string>(); // required
```

**Output:**
```typescript
// Before
@Output() clicked = new EventEmitter<void>();
this.clicked.emit();

// After
clicked = output<void>();
this.clicked.emit();
```

**Local state:**
```typescript
// Before
count = 0;
increment() { this.count++; }

// After
count = signal(0);
increment() { this.count.update(n => n + 1); }
```

**Derived/computed:**
```typescript
// Before
get fullName() { return `${this.firstName} ${this.lastName}`; }

// After
fullName = computed(() => `${this.firstName()} ${this.lastName()}`);
```

### 4. Update Templates
- `{{ title }}` → `{{ title() }}` for signal-based inputs
- `[disabled]="isLoading"` → `[disabled]="isLoading()"` for signals
- Do NOT call signals with `()` inside `@if`, `@for` — Angular resolves them automatically in control flow blocks

### 5. Remove Unnecessary Lifecycle Hooks
- `ngOnInit` with only subscription logic can often be replaced with `effect()`
- `ngOnDestroy` with only `unsubscribe()` calls can be removed (signals don't need unsubscribing)

## Common Pitfalls
- Don't mix `signal()` with `async` pipe — pick one pattern
- `computed()` is readonly; don't try to `.set()` on it
- `effect()` runs once immediately; guard against unintended side effects on init
- Setting a signal inside `effect()` requires `allowSignalWrites: true` — prefer `computed()` instead

## Example: Before & After

```typescript
// BEFORE
@Component({ ... })
export class CounterComponent {
  @Input() label: string = 'Count';
  @Output() countChanged = new EventEmitter<number>();
  count = 0;

  get doubled() { return this.count * 2; }

  increment() {
    this.count++;
    this.countChanged.emit(this.count);
  }
}
```

```typescript
// AFTER
@Component({ ... })
export class CounterComponent {
  label = input('Count');
  countChanged = output<number>();

  count = signal(0);
  doubled = computed(() => this.count() * 2);

  increment() {
    this.count.update(n => n + 1);
    this.countChanged.emit(this.count());
  }
}
```
