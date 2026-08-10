# CONTEXT.md Format

## Structure

```md
# {Context Name}

{One or two sentence description of what this context is and why it exists.}

## Language

**注文 (Order)**:
顧客が商品またはサービスの提供を依頼した記録。
_避ける_: オーダー, 購入

**請求書 (Invoice)**:
提供済みの商品またはサービスに対して顧客へ支払いを求める文書。
_避ける_: 明細書

**顧客 (Customer)**:
注文を行う個人または組織。
_避ける_: 取引先, 買い手
```

## Rules

- **Use both languages for terms.** Write each term as the user's language followed by English in parentheses (for example, `注文 (Order)`). Write definitions and avoided terms only in the user's language.
- **Be opinionated.** When multiple words exist for the same concept, pick the best one and list the others under `_避ける_`.
- **Keep definitions tight.** One or two sentences max. Define what it IS, not what it does.
- **Keep code identifiers in English.** Classes, functions, and other code identifiers should correspond to the English term. Do not add implementation details to `CONTEXT.md`.
- **Only include terms specific to this project's context.** General programming concepts (timeouts, error types, utility patterns) don't belong even if the project uses them extensively. Before adding a term, ask: is this a concept unique to this context, or a general programming concept? Only the former belongs.
- **Group terms under subheadings** when natural clusters emerge. If all terms belong to a single cohesive area, a flat list is fine.

## Single vs multi-context repos

**Single context (most repos):** One `CONTEXT.md` at the repo root.

**Multiple contexts:** A `CONTEXT-MAP.md` at the repo root lists the contexts, where they live, and how they relate to each other:

```md
# Context Map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md) — receives and tracks customer orders
- [Billing](./src/billing/CONTEXT.md) — generates invoices and processes payments
- [Fulfillment](./src/fulfillment/CONTEXT.md) — manages warehouse picking and shipping

## Relationships

- **Ordering → Fulfillment**: Ordering emits `OrderPlaced` events; Fulfillment consumes them to start picking
- **Fulfillment → Billing**: Fulfillment emits `ShipmentDispatched` events; Billing consumes them to generate invoices
- **Ordering ↔ Billing**: Shared types for `CustomerId` and `Money`
```

The skill infers which structure applies:

- If `CONTEXT-MAP.md` exists, read it to find contexts
- If only a root `CONTEXT.md` exists, single context
- If neither exists, create a root `CONTEXT.md` lazily when the first term is resolved

When multiple contexts exist, infer which one the current topic relates to. If unclear, ask.
