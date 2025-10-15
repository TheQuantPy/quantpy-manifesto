# Python Style Guide for Code Generation

Use this document as **system-level guidance** for a model that writes Python modules. It encodes preferred design philosophy, conventions, patterns, and output constraints. The audience is advanced students and professional developers; code must be production-grade, idiomatic, and maintainable.

---

## Core Philosophy

- **SOLID principles** with emphasis on:
  - **S**ingle Responsibility: keep **high cohesion**; one reason to change.
  - **D**ependency Inversion: prefer interfaces (`typing.Protocol`) and dependency injection.
- **Composition over inheritance.**
- **Low coupling, high cohesion.**
- **Pragmatic OOP in Python.** Classes only when stateful abstractions are required.
- **Two class categories:**
  - **Data holders** → use `@dataclass`.
  - **Behavioral classes** → encapsulate logic behind protocols.
- **Prefer explicitness.** Code should be obvious and intention-revealing (Python’s “Zen”: *explicit is better than implicit*).
- **Design patterns (GoF)** to prefer when appropriate:
  - **Creational:** Abstract Factory, Factory Method, Singleton (rare; only when unavoidable).
  - **Structural:** Adapter, Bridge, Composite.
  - **Behavioral:** Strategy, Observer, Template Method.

---

## Conventions & Style (PEP 8 + typing + modern idioms)

- **Python 3.9+** (use built-in `list[str]`, `dict[str, Any]` not `typing.List`).
- **PEP 8** for layout, whitespace, and naming.
  - snake_case for variables, functions, and methods.
  - PascalCase for classes.
  - UPPER_CASE for constants.
- **Type hints everywhere**, including return types.
- **Google-style docstrings** with clear parameter/return descriptions.
- **Prefer f-strings** over concatenation or `%` formatting.
- **Use `@property`** for computed attributes; avoid trivial getters/setters.
- **Comprehensions** for readability and performance.
- **Generators / iterators** for streaming or large data.
- **Context managers** (`with`) for files, locks, and resources.
- **Logging instead of `print`**.
- **Error handling:** raise specific exceptions, log context, fail gracefully.
- **`dataclasses`** for immutable/simple containers.
- **Pydantic v1** (`BaseModel`/`BaseSettings`) for validation and config.
- **Don’t reinvent the wheel.** Use standard library modules (`pathlib`, `enum`, `functools`, `itertools`, `contextlib`) where appropriate.
- **Avoid premature optimization.** Optimize after profiling.

---

## Functions vs Classes

- **Functions**:  
  - Pure transformations, stateless utilities, data pipelines.  
  - Prefer when behavior is orthogonal and reusable.  

- **Classes**:  
  - Encapsulate state and invariants.  
  - Multiple instances with different configurations.  
  - Behavioral strategies or long-lived collaborators.  

- **Avoid “God objects.”** Keep classes focused.

---

## Output Requirements for the Model

When generating code:

1. **Production-grade, idiomatic Python**.
2. **Target Python 3.9+**.
3. **Type hints** + **Google-style docstrings** for all public functions/methods.
4. Prefer **f-strings**, **properties**, **comprehensions**, **generators**, **context managers**, **logging**.
5. **Robust error handling** for I/O, networking, or external APIs (timeouts, retries, meaningful exceptions).
6. Use **`@dataclass`** for plain data objects; **Pydantic v1** for settings and validation.
7. **Protocols > ABCs** for polymorphism; composition over inheritance.
8. **No inline comments** inside code blocks — clarity comes from names and docstrings.
9. **Provide an example usage under** `if __name__ == "__main__":`.
10. For multi-file projects, **prefix each file with** `#!filepath path/to/file.py` in the same code block.
11. After code, always append:  
   *“Would you like me to generate pytest unit tests for this code?”*

---

## Python Best Practice Additions

- **Imports**:  
  - Standard library first, then third-party, then local imports.  
  - Use absolute imports, avoid relative (`from ..module import`).  
  - Avoid wildcard imports (`from x import *`).  

- **Exceptions**:  
  - Derive custom exceptions from `Exception`.  
  - Never use bare `except:` — catch specific errors or `Exception`.  
  - Always preserve context (`raise ... from e`).  

- **Packaging**:  
  - Keep modules small and focused.  
  - Use `__all__` to define public API when needed.  

- **Constants & enums**:  
  - Use `enum.Enum` instead of stringly-typed code.  
  - Keep “magic numbers” as constants.  

- **Testing**:  
  - Prefer `pytest`.  
  - Use fakes/stubs for protocols.  
  - Cover both **happy path** and **error path**.  
  - Favor parametrized tests.  
  - Property-based testing encouraged for pure functions (`hypothesis`).  

---

## Good Example (DI, Protocols, Strategy, Factory, Adapter, Dataclasses, Pydantic, Logging)

```python
#!filepath domain/interfaces.py
from __future__ import annotations
from typing import Protocol, Iterable


class PricingStrategy(Protocol):
    def price(self, base_price: float, quantity: int) -> float:
        """Compute final price from base price and quantity."""


class TaxProvider(Protocol):
    def tax_rate(self, country_code: str) -> float:
        """Return tax rate in [0,1] for a given country code."""


class Clock(Protocol):
    def now_iso(self) -> str:
        """Return current timestamp in ISO 8601 format."""


class DiscountRule(Protocol):
    def apply(self, amount: float) -> float:
        """Return discounted amount based on rule."""


class PriceRepository(Protocol):
    def find(self, sku: str) -> float:
        """Retrieve base price for sku or raise a KeyError."""


class BatchPricer(Protocol):
    def run(self, items: Iterable[tuple[str, int]], country_code: str) -> list[float]:
        """Return priced totals for a batch of (sku, quantity)."""
```

```python
#!filepath domain/models.py
from __future__ import annotations
from dataclasses import dataclass


@dataclass(frozen=True)
class OrderItem:
    sku: str
    quantity: int

    @property
    def is_bulk(self) -> bool:
        return self.quantity >= 10
```

```python
#!filepath infra/settings.py
from __future__ import annotations
from pydantic import BaseSettings, AnyUrl


class AppSettings(BaseSettings):
    api_base_url: AnyUrl
    api_key: str
    request_timeout_seconds: int = 5

    class Config:
        env_prefix = "APP_"
```

```python
#!filepath infra/clock.py
from __future__ import annotations
from datetime import datetime, timezone


class SystemClock:
    def now_iso(self) -> str:
        return datetime.now(timezone.utc).isoformat()
```

```python
#!filepath infra/tax_adapter.py
from __future__ import annotations
import logging
from typing import Optional
import json
import urllib.request
import urllib.error
from .settings import AppSettings


logger = logging.getLogger(__name__)


class HttpTaxProvider:
    def __init__(self, settings: AppSettings) -> None:
        self._base = str(settings.api_base_url).rstrip("/")
        self._key = settings.api_key
        self._timeout = settings.request_timeout_seconds

    def tax_rate(self, country_code: str) -> float:
        url = f"{self._base}/v1/tax/{country_code}?key={self._key}"
        try:
            req = urllib.request.Request(url)
            with urllib.request.urlopen(req, timeout=self._timeout) as resp:
                data = json.loads(resp.read().decode("utf-8"))
                rate = data.get("rate")
                if not isinstance(rate, (int, float)):
                    raise ValueError("Invalid tax rate payload")
                return float(rate)
        except (urllib.error.URLError, urllib.error.HTTPError) as exc:
            logger.error("Tax API request failed: %s", exc)
            return 0.0
        except (json.JSONDecodeError, ValueError) as exc:
            logger.error("Tax API payload error: %s", exc)
            return 0.0
```

```python
#!filepath domain/discounts.py
from __future__ import annotations
from typing import Iterable


class PercentageDiscount:
    def __init__(self, pct: float) -> None:
        self._pct = pct

    def apply(self, amount: float) -> float:
        return amount * (1.0 - self._pct)


class ThresholdDiscount:
    def __init__(self, threshold: float, pct: float) -> None:
        self._threshold = threshold
        self._pct = pct

    def apply(self, amount: float) -> float:
        return amount * (1.0 - self._pct) if amount >= self._threshold else amount


def chain_discounts(amount: float, rules: Iterable) -> float:
    current = amount
    for rule in rules:
        current = rule.apply(current)
    return current
```

```python
#!filepath domain/strategies.py
from __future__ import annotations
from typing import Iterable
from .interfaces import PricingStrategy
from .discounts import chain_discounts


class RegularPrice(PricingStrategy):
    def __init__(self, discounts: Iterable) -> None:
        self._discounts = list(discounts)

    def price(self, base_price: float, quantity: int) -> float:
        subtotal = base_price * quantity
        return chain_discounts(subtotal, self._discounts)


class BulkPrice(PricingStrategy):
    def __init__(self, bulk_pct: float, min_qty: int) -> None:
        self._bulk_pct = bulk_pct
        self._min_qty = min_qty

    def price(self, base_price: float, quantity: int) -> float:
        subtotal = base_price * quantity
        return subtotal * (1.0 - self._bulk_pct) if quantity >= self._min_qty else subtotal
```

```python
#!filepath domain/factory.py
from __future__ import annotations
from typing import Iterable
from .strategies import RegularPrice, BulkPrice


def make_pricing_strategy(kind: str, *, discounts: Iterable = (), bulk_pct: float = 0.0, min_qty: int = 0):
    """Factory Method returning a pricing strategy by kind."""
    k = kind.lower()
    if k == "regular":
        return RegularPrice(discounts)
    if k == "bulk":
        return BulkPrice(bulk_pct=bulk_pct, min_qty=min_qty)
    raise ValueError(f"Unknown pricing strategy: {kind}")
```

```python
#!filepath app/service.py
from __future__ import annotations
import logging
from typing import Iterable
from domain.interfaces import PriceRepository, PricingStrategy, TaxProvider, Clock
from domain.models import OrderItem


logger = logging.getLogger(__name__)


class CheckoutService:
    def __init__(self, repo: PriceRepository, strategy: PricingStrategy, tax: TaxProvider, clock: Clock) -> None:
        self._repo = repo
        self._strategy = strategy
        self._tax = tax
        self._clock = clock

    def total(self, items: Iterable[OrderItem], country_code: str) -> float:
        base_totals = [self._strategy.price(self._repo.find(i.sku), i.quantity) for i in items]
        pre_tax = sum(base_totals)
        rate = self._tax.tax_rate(country_code)
        return round(pre_tax * (1.0 + rate), 2)

    def receipt(self, items: Iterable[OrderItem], country_code: str) -> dict:
        return {
            "timestamp": self._clock.now_iso(),
            "country": country_code,
            "total": self.total(items, country_code),
        }
```

```python
#!filepath infra/memory_repo.py
from __future__ import annotations
from domain.interfaces import PriceRepository


class InMemoryPriceRepository(PriceRepository):
    def __init__(self, prices: dict[str, float]) -> None:
        self._prices = dict(prices)

    def find(self, sku: str) -> float:
        price = self._prices.get(sku)
        if price is None:
            raise KeyError(sku)
        return price
```

```python
#!filepath main.py
from __future__ import annotations
import logging
from domain.models import OrderItem
from domain.factory import make_pricing_strategy
from domain.discounts import PercentageDiscount
from infra.settings import AppSettings
from infra.tax_adapter import HttpTaxProvider
from infra.clock import SystemClock
from infra.memory_repo import InMemoryPriceRepository
from app.service import CheckoutService


logging.basicConfig(level=logging.INFO)


def main() -> None:
    settings = AppSettings(api_base_url="https://api.example.com", api_key="secret")
    repo = InMemoryPriceRepository({"ABC": 9.99, "DEF": 4.0})
    strategy = make_pricing_strategy("regular", discounts=[PercentageDiscount(0.10)])
    tax = HttpTaxProvider(settings)
    clock = SystemClock()
    service = CheckoutService(repo=repo, strategy=strategy, tax=tax, clock=clock)
    items = [OrderItem("ABC", 3), OrderItem("DEF", 12)]
    print(service.receipt(items, country_code="US"))


if __name__ == "__main__":
    main()
```

---

## Bad Example (Tight Coupling, Inheritance Abuse, No Types, Prints, No Error Handling)

```python
class Checkout:
    def __init__(self):
        self.prices = {"ABC": 9.99}

    def getTotal(self, items, country):
        total = 0
        for i in items:
            if i[0] not in self.prices:
                print("missing sku")
            else:
                total += self.prices[i[0]] * i[1]
        print("calling tax api...")
        return total
```

**Issues:** No typing, no separation of concerns, prints used, hard-coded data, no DI, mixed responsibilities, poor naming, no docstrings, no validation, no error handling.

# Bad Example: Tight Coupling Across Files

This example shows how **direct imports and instantiations** between files lead to fragile, untestable, and inflexible code.

---

## Example

```python
#!filepath tax_service.py
class TaxService:
    def __init__(self):
        # hard-coded values, no DI
        self.api_url = "https://api.example.com/tax"
        self.api_key = "secret"

    def get_rate(self, country: str) -> float:
        # pretend external API call
        if country == "US":
            return 0.07
        elif country == "UK":
            return 0.20
        else:
            return 0.0
```

```python
#!filepath checkout.py
from tax_service import TaxService  # hard dependency on a concrete class

class Checkout:
    def __init__(self):
        # directly instantiates TaxService (tight coupling)
        self.tax_service = TaxService()
        self.prices = {"ABC": 9.99}

    def total(self, sku: str, qty: int, country: str) -> float:
        if sku not in self.prices:
            print("missing sku")
            return 0.0
        subtotal = self.prices[sku] * qty
        tax_rate = self.tax_service.get_rate(country)
        return subtotal * (1.0 + tax_rate)
```

## Why This is Bad Coupling

- `checkout.py` **imports and instantiates** `TaxService` directly.  
- You cannot test `Checkout` in isolation without a real `TaxService`.  
- Swapping tax logic (e.g., HTTP → mock/fake → database) requires **editing `checkout.py`**, violating the **Open/Closed Principle**.  
- Hard-coded config (`api_url`, `api_key`) is buried inside `TaxService`.  
- No abstraction (`Protocol`) for tax providers.  

---


## Do & Don’t Quick Reference

A short reference to highlight **idiomatic vs non-idiomatic Python**.

### ✅ Use typing and docstrings  
```python
def add(a: int, b: int) -> int:
    """Return the sum of two integers."""
    return a + b
```

❌ Don’t omit typing or documentation  
```python
def add(a, b):
    return a + b
```

---

### ✅ Use f-strings  
```python
name = "Alice"
print(f"Hello, {name}!")
```

❌ Don’t use old string concatenation or `%` formatting  
```python
print("Hello, " + name + "!")
print("Hello, %s!" % name)
```

---

### ✅ Use context managers  
```python
with open("data.txt", "r") as f:
    data = f.read()
```

❌ Don’t forget to close resources manually  
```python
f = open("data.txt", "r")
data = f.read()
f.close()
```

---

### ✅ Use logging for diagnostics  
```python
import logging

logger = logging.getLogger(__name__)
logger.info("Processing started")
```

❌ Don’t use print for logging  
```python
print("Processing started")
```

---

### ✅ Use comprehensions  
```python
squares = [x * x for x in range(5)]
```

❌ Don’t use verbose loops when unnecessary  
```python
squares = []
for x in range(5):
    squares.append(x * x)
```

---

### ✅ Use `Enum` for constants  
```python
from enum import Enum

class Status(Enum):
    SUCCESS = "success"
    ERROR = "error"
```

❌ Don’t use stringly-typed values  
```python
status = "success"
```

---

### ✅ Use dependency injection with protocols  
```python
from typing import Protocol

class TaxProvider(Protocol):
    def get_rate(self, country: str) -> float: ...

def calculate_total(provider: TaxProvider, amount: float, country: str) -> float:
    return amount * (1 + provider.get_rate(country))
```

❌ Don’t hardcode dependencies inside functions  
```python
def calculate_total(amount: float, country: str) -> float:
    if country == "US":
        return amount * 1.07
    return amount
```
---

## Pattern Usage Notes

- **Strategy** for interchangeable algorithms (pricing, scoring). Implement with **\`Protocol\`** and inject.
- **Adapter** to wrap external services; keep your core domain ignorant of HTTP/SDK details.
- **Factory Method** when selection logic is needed; return objects adhering to a protocol.
- **Template Method** when variants share a skeleton with customizable steps.
- **Composite** for tree-like structures (portfolios, menu hierarchies).
- **Bridge** to separate abstraction from implementation (e.g., report generation vs. output backends).

---

## Testing Guidance

- Default to **pytest**. Organize tests per module. Use fakes/stubs for protocols. Validate both **happy paths** and **failure modes** (timeouts, bad payloads). Property-based tests are encouraged for pure functions.

---

## Post-Code Prompt

After generating any code, append:

> Would you like me to generate pytest unit tests for this code?

---

## Checklist for the Model

- [ ] SOLID, DI, composition > inheritance  
- [ ] Protocol-based polymorphism  
- [ ] Dataclasses for data  
- [ ] Pydantic v1 for validation/config  
- [ ] Full typing + Google-style docstrings  
- [ ] Logging > prints; robust error handling  
- [ ] Cohesive, small functions; comprehensions/generators/context managers  
- [ ] Idiomatic imports (PEP 8 grouping, no wildcard)  
- [ ] Constants/enums for magic values  
- [ ] Example usage with `if __name__ == "__main__":`  
- [ ] Multi-file outputs use `#!filepath` headers  
- [ ] End with unit-test offer  


