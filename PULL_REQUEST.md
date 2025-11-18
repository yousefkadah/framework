# Add Schedule::runInMaintenanceMode() Method

## Problem

Currently, there's no intuitive way to specify that a scheduled task should run during maintenance mode. Developers must use the less intuitive `evenInMaintenanceMode()` method, which doesn't clearly communicate its purpose.

### Current Approach:
```php
$schedule->command('backup:run')
    ->daily()
    ->evenInMaintenanceMode(); // Not immediately clear what this does
```

---

## Solution

Add a more intuitive `runInMaintenanceMode()` method as an alias to `evenInMaintenanceMode()`.

### New API:
```php
$schedule->command('backup:run')
    ->daily()
    ->runInMaintenanceMode(); // Crystal clear intent
```

---

## Implementation

Added `runInMaintenanceMode()` method to the `ManagesAttributes` trait that simply delegates to the existing `evenInMaintenanceMode()` method:

```php
public function runInMaintenanceMode()
{
    return $this->evenInMaintenanceMode();
}
```

---

## Benefits

✅ **More intuitive API** - Method name clearly describes what it does  
✅ **Solves real-world use case** - Running backups during maintenance  
✅ **Fully backwards compatible** - `evenInMaintenanceMode()` still works  
✅ **Zero breaking changes** - Simple alias pattern  
✅ **Better developer experience** - Self-documenting code  
✅ **Comprehensive tests** - Added tests for both methods  

---

## Use Cases

### Database Backups During Maintenance
```php
$schedule->command('backup:database')
    ->hourly()
    ->runInMaintenanceMode();
```

### System Health Checks
```php
$schedule->command('health:check')
    ->everyFiveMinutes()
    ->runInMaintenanceMode();
```

### Cache Warming After Deployment
```php
$schedule->command('cache:warm')
    ->everyTenMinutes()
    ->runInMaintenanceMode();
```

---

## Testing

All existing tests pass, plus new tests added:
- ✅ Test `runInMaintenanceMode()` works correctly
- ✅ Test `evenInMaintenanceMode()` still works (backwards compatibility)
- ✅ Both methods properly set the maintenance mode flag

---

## Breaking Changes

**None** - This is a simple alias addition with full backwards compatibility.
