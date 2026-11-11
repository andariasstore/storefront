# DIVOOM STORE — GEMINI CANVAS STOREFRONT PROMPT

## ROLE

Act as a **Senior E-Commerce Application Architect and Storefront Development Engineer**. Combine the responsibilities of:

1. E-Commerce Product Architect
2. Senior Frontend Engineer
3. UI/UX Designer
4. Mobile-First Design Engineer
5. Design System Architect
6. E-Commerce UX Specialist
7. Product Catalog Engineer
8. Shopping Cart Engineer
9. Search & Filter Engineer
10. Responsive Web Engineer
11. Accessibility Engineer
12. QA & Frontend Auditor

Work as a practical implementation engineer. Make architecture and implementation decisions that are necessary to complete the defined scope, but do not invent business requirements or expand the product.

## CONTEXT

Build **Divoom Store**, a single-store online Divoom product storefront.

- Store model: one independent Divoom-focused store with its own brand identity.
- Store name: Divoom Store.
- Catalog: Divoom products supplied by the configured Product API.
- Application type: customer-facing storefront.
- Primary target: mobile devices.
- Design direction: Minimalist Modern.
- Development environment: Gemini Canvas.
- Frontend stack: React, TypeScript, CSS, and Lucide Icons.

This is a single-store customer storefront. It is not a marketplace, seller platform, or store administration system.

## MISSION

Create a complete, usable, polished mobile-first storefront in Gemini Canvas. The result must be a working React application, not a static mockup or a collection of disconnected screens.

Prioritize:
- Clear product discovery.
- Understandable product information.
- Simple navigation.
- Predictable cart behavior.
- Consistent mobile layout.
- Accessible and usable controls.
- Maintainable component and state organization.

## OBJECTIVE

Deliver the following six customer-facing pages:

1. HomePage
2. CatalogPage
3. ProductDetailPage
4. CartPage
5. ProfilePage
6. LivechatPage

All pages must share one consistent application shell and design system. Product catalog data must be loaded from the specified read-only Product API. Cart and other included frontend interactions remain client-side; do not add a separate backend or order service.

## TECHNOLOGY AND IMPLEMENTATION

Use:
- React for the application UI.
- TypeScript for components, data models, and state.
- CSS for layout, responsive behavior, and visual styling.
- Lucide Icons for interface icons.

Implementation rules:
- Build directly in the existing Gemini Canvas application context.
- Inspect the current Canvas project before making changes.
- Preserve existing working code and approved behavior where applicable.
- Follow the existing project setup and conventions.
- Do not replace the project with a different framework or build system.
- Do not add dependencies unless they are essential and not already available.
- Use reusable, clearly named React components.
- Use typed data models and avoid unnecessary `any`.
- Keep application state predictable and shared where multiple pages depend on it.
- Do not create a second or parallel application shell.

## PLATFORM AND RESPONSIVE CONSTRAINTS

This is a **mobile-only storefront**, not a desktop-responsive commerce website.

Required layout behavior:
- Maximum application width: `500px`.
- On mobile, the application occupies the available viewport width.
- On wider screens, the application remains centered horizontally and does not exceed `500px`.
- Do not create desktop-specific navigation, multi-column desktop layouts, or a separate desktop experience.
- Prevent horizontal overflow.
- Keep the Header and BottomNav visible while the page content scrolls.
- Only the main page-content region should own vertical scrolling.

Use this layout foundation:
- `html`, `body`, and `#root`: `height: 100%`.
- `#root`: full width, maximum width `500px`, centered with `margin-inline: auto`, and a vertical flex layout.
- AppShell: fill the available root height and use a vertical flex layout.
- Header: remain outside the scrollable page-content region.
- Main content region: `flex: 1`, `min-height: 0`, and `overflow-y: auto`.
- BottomNav: remain outside the scrollable page-content region at the bottom of AppShell.
- Do not use `100vh` or `100dvh` as a replacement for the required root height foundation.
- Avoid nested vertical scroll containers unless a specific component genuinely requires one.

## APPLICATION ARCHITECTURE

Use one React application with this logical component structure:

```text
App
├── AppShell
│   ├── Header
│   │   ├── BrandName
│   │   ├── CartAction
│   │   └── ProfileAction
│   ├── Main
│   │   └── Content
│   │       └── ActivePage
│   └── BottomNav
│       ├── Home
│       ├── Catalog
│       └── Livechat
│
├── Pages
│   ├── HomePage
│   ├── CatalogPage
│   ├── ProductDetailPage
│   ├── CartPage
│   ├── ProfilePage
│   └── LivechatPage
│
├── Components
│   ├── ProductCard
│   ├── ProductPrice
│   ├── ProductGallery
│   ├── QuantityControl
│   ├── CartItem
│   ├── EmptyState
│   └── SharedUI
│
├── Services
│   └── ProductApiService
│
├── Types
│   └── StorefrontTypes
│
└── Styles
    ├── GlobalStyles
    ├── DesignTokens
    └── ComponentStyles
```

This is a logical organization guide. Adapt file placement to the existing Canvas project structure without creating unnecessary files or layers.

### Application shell responsibilities

**Header**
- Display the Divoom Store brand identity.
- Provide an action to open CartPage.
- Provide an action to open ProfilePage.
- Keep the header visually consistent across the six pages.

**Main / Content**
- Render the currently selected page.
- Own the vertical scrolling behavior.
- Preserve the shared application shell when navigating between pages.

**BottomNav**
- Display exactly three primary items: Home, Catalog, and Livechat.
- Show a clear active state for the current primary page.
- Remain visible while page content scrolls.
- Do not add Cart or Profile to BottomNav.

## PAGE REQUIREMENTS

### 1. HomePage

Purpose: introduce the store and help customers discover products.

Include:
- Storefront introduction or promotional banner.
- Product category shortcuts.
- Featured products.
- Reusable product cards.
- Clear navigation to the catalog and product details.

Do not invent discount campaigns, shipping promises, ratings, or brand claims. Do not create substitute or demo products when the API is unavailable; show the defined loading, error, or empty state instead.

### 2. CatalogPage

Purpose: let customers browse the available product catalog.

Include:
- Product listing.
- Product image, name, and price.
- Category selection.
- Search input.
- Sorting or filtering controls only where they can be implemented meaningfully.
- Clear empty state when no products match the search or selected category.

Search, category filtering, and sorting must operate on the same normalized API product dataset. Do not create controls that look interactive but have no effect.

### 3. ProductDetailPage

Purpose: show the information needed before adding a product to the cart.

Include:
- Product image or gallery.
- Product name.
- Product price.
- Product description when available.
- Product variants when applicable.
- Quantity selection where appropriate.
- Add to Cart action.

Rules:
- Open the correct product when a product card is selected.
- Variant selection must be reflected in the selected cart item.
- Prevent adding an invalid or unselected required variant.
- Give clear feedback after a successful Add to Cart action.
- Do not invent product specifications that are not present in the API response.

### 4. CartPage

Purpose: let customers review and adjust selected items.

Include:
- Cart item list.
- Product image, name, selected variant, and unit price.
- Quantity controls.
- Item subtotal.
- Cart total.
- Remove-item action.
- Clear empty-cart state.
- Checkout action.

Cart behavior:
- Adding the same product with the same selected variant updates its quantity instead of creating an unnecessary duplicate line.
- The same product with a different variant is treated as a separate cart line.
- Quantity cannot be less than one.
- Removing an item updates the cart immediately.
- Subtotal and total are recalculated from current cart contents.
- An empty cart must not display a misleading payable total.

Checkout boundary:
- This version does not integrate a payment gateway, backend, or real order service.
- Do not simulate a successful payment or claim that an order has been placed.
- The Checkout action may show a clear frontend-only notice that checkout is not available in this version.
- Do not add payment, address, shipping, or order-processing flows.

### 5. ProfilePage

Purpose: provide a basic customer profile interface.

Include:
- Customer profile presentation.
- Basic account information area.
- Clearly labeled profile-related navigation options only when they have a defined frontend behavior.

Constraints:
- No authentication or account backend is included.
- Do not imply that a customer is authenticated.
- Do not invent personal customer data.
- If no profile data source exists, show a clear neutral guest or empty state.
- Do not add registration, login, or account-management flows.

### 6. LivechatPage

Purpose: provide a customer-service chat interface layout.

Include:
- Conversation area.
- Message history presentation.
- Message input.
- Send action.
- Empty conversation state.

Constraints:
- No livechat provider, backend, or real-time service is included.
- Do not claim that messages are delivered to a real customer-service agent.
- If implementing local message interaction, make it clear that it is a local frontend demonstration only.
- Do not invent automated agent replies or external chat integrations.

## DATA AND STATE RULES

## PRODUCT API INTEGRATION

The product catalog must use this exact read-only HTTP GET endpoint:

`https://mvwflakurihhakdrlteg.supabase.co/functions/v1/get-divoom-products`

Integration rules:
- Fetch products from this endpoint; do not use hardcoded product arrays, local demo catalogs, or mock product data as the source of truth.
- Implement one reusable typed ProductApiService. Do not fetch the same catalog independently from each page.
- Load the catalog once at the shared application/data layer and expose the resulting state to HomePage, CatalogPage, and ProductDetailPage.
- Use the browser Fetch API with GET. Do not assume an API key, authorization header, or additional query parameter unless the endpoint explicitly requires it.
- Check the HTTP response status before parsing the response body.
- Validate that the response is an object, `success === true`, and `data` is an array before treating it as successful.
- Treat `success !== true`, malformed JSON, invalid response shape, network failure, and non-2xx HTTP status as API errors.
- Read `total` when supplied, but do not assume it always equals the current array length.
- Do not silently replace API errors with static or fabricated product records.
- Do not add Supabase client SDK, database access, authentication, write operations, or other Supabase integration. This storefront only consumes the specified Edge Function.
- Do not modify, deploy, or reconfigure the Supabase function.

### API response and product fields

The endpoint returns a JSON object shaped like:

```json
{
  "success": true,
  "data": [
    {
      "id": "astrotooblack",
      "item_group_id": "astrotoo",
      "title": "Astrotoo Black",
      "description": "Product description",
      "rich_text_description": "<p>Product details</p>",
      "availability": "in stock",
      "condition": "new",
      "price": 3627819,
      "sale_price": null,
      "link": "/products/astrotoo",
      "image_link": "https://example.com/product.jpg",
      "additional_image_link": [],
      "brand": "Divoom",
      "product_type": "Display",
      "color": "Black",
      "size": null,
      "material": null,
      "pattern": null,
      "gender": null,
      "age_group": null,
      "additional_variant_attribute": null
    }
  ],
  "total": 42
}
```

This is an illustrative schema based on the supplied API example, not a complete fixed list of every possible field or product. Preserve optional and nullable fields in the TypeScript model.

Map fields as follows:
- `id`: unique identifier for an individual product variant/record; use as the stable product-record key.
- `item_group_id`: shared identifier that groups variants belonging to the same product family.
- `title`: display name for the individual product record.
- `description`: plain-text product description and safe fallback.
- `rich_text_description`: optional HTML-rich description; sanitize it with a trusted safe approach before rendering. Never inject untrusted HTML directly with `dangerouslySetInnerHTML`. If safe sanitization is unavailable, render the plain-text `description` instead.
- `availability`: product availability; do not present unavailable products as purchasable.
- `condition`: product condition, when useful to display.
- `price`: regular numeric price in Indonesian rupiah (IDR).
- `sale_price`: nullable numeric sale price. Use it as the active price only when it is a valid positive number; otherwise use `price`.
- `link`: product path/reference from the API. Do not assume it is an absolute URL or use it as the unique record key.
- `image_link`: primary product image URL.
- `additional_image_link`: optional array of additional image URLs.
- `brand`: brand name.
- `product_type`: product category/type source.
- `color`, `size`, `material`, `pattern`, `gender`, `age_group`, `additional_variant_attribute`: optional product or variant attributes. Preserve null values and do not display empty attributes.

Use Indonesian Rupiah formatting with `Intl.NumberFormat("id-ID", { style: "currency", currency: "IDR", maximumFractionDigits: 0 })`. Do not hardcode currency strings or convert the API price using an assumed exchange rate.

### Product grouping and variants

- API records sharing the same `item_group_id` belong to the same product family.
- Treat each distinct `id` as an individual purchasable variant/record.
- Group records by `item_group_id` for product-family discovery and product detail presentation.
- On a product-family detail page, show the available records in that group as selectable variants, using available attributes such as color or size to distinguish them.
- The selected variant must resolve to one exact API record and its `id`.
- Do not infer that every product has color or size variants; use only fields actually supplied by the API.
- If `item_group_id` is absent or empty, treat that record as its own product family using its `id`.
- Do not merge records from different groups merely because their titles or images look similar.
- Use `image_link` as the primary image and `additional_image_link` as the gallery when available. Handle missing or failed images with a neutral fallback.
- Use `product_type` for category navigation/filtering when present. Do not invent categories unsupported by the API.

### API loading, error, and refresh states

Maintain explicit shared catalog states:
- Loading: show a non-blocking loading presentation while the initial request is pending.
- Success with products: render the API catalog.
- Success with an empty `data` array: show a genuine empty-catalog state.
- Error: show a clear error state and a Retry action that re-runs the request.
- Retry must not create duplicate requests or duplicate product records.
- Do not show stale mock products during loading or after an API failure.
- Do not claim the catalog is current or refreshed unless a successful request confirms it.

### Product data and frontend state

- Keep a normalized typed representation of the API response as the single source of truth for all product displays.
- HomePage, CatalogPage, and ProductDetailPage must consume the same API-backed product state.
- Search, category filtering, and sorting must derive their results from that shared state; do not maintain separate divergent product arrays.
- Cart entries must retain the selected API record `id`, its `item_group_id`, selected variant attributes, quantity, and the price snapshot used when the item was added.
- Resolve cart items against the current API data when possible, while handling an item that is no longer returned by the API without crashing or silently changing its identity.
- Do not fabricate missing product fields or treat null values as real values.

Cart state:
- Use one shared cart state accessible from ProductDetailPage, Header, and CartPage.
- Keep cart state intact when navigating between pages.
- Update the cart count and totals whenever cart contents change.
- Persistence across a browser reload is optional; do not introduce storage complexity unless the existing project already uses it.

Navigation state:
- Use one consistent navigation mechanism for all pages.
- Header actions must open CartPage and ProfilePage.
- BottomNav must open HomePage, CatalogPage, and LivechatPage.
- Product cards must open the selected ProductDetailPage.
- Provide a predictable way to return from ProductDetailPage to the previous browsing context.
- Keep the active BottomNav state accurate. ProductDetailPage, CartPage, and ProfilePage must not incorrectly appear as a different primary tab.

## DESIGN SYSTEM

Design direction: **Minimalist Modern**.

Visual characteristics:
- Clean, calm, and product-focused.
- Clear hierarchy and readable typography.
- Balanced whitespace.
- Consistent alignment and spacing.
- Subtle borders and restrained shadows.
- Consistent corner radii.
- Clear distinction between primary and secondary actions.
- Product imagery should receive visual priority.

Create a small set of reusable design tokens for:
- Brand and neutral colors.
- Page background and surface colors.
- Primary text and secondary text.
- Border colors.
- Typography sizes and weights.
- Spacing scale.
- Border radii.
- Shadows.
- Icon sizes.
- Button heights and control dimensions.

Rules:
- Define tokens once and reuse them.
- Maintain consistent styling across every page.
- Use Lucide Icons; do not use emoji as interface icons.
- Use semantic HTML elements where appropriate.
- Provide visible focus states and accessible labels for icon-only actions.
- Maintain readable text contrast and usable touch targets.
- Do not introduce an unrelated visual theme or excessive decoration.

Do not invent a detailed brand palette, logo asset, or typography identity if none is supplied. Choose restrained, coherent defaults and keep them centralized in design tokens.

## INTERACTION AND ACCESSIBILITY RULES

- Every visible interactive control must have a defined and working behavior.
- Do not use placeholder links such as `href="#"` for application navigation.
- Buttons must have the correct button semantics.
- Inputs must have labels or accessible names.
- Icon-only buttons must have accessible labels.
- Provide visible focus and active states.
- Provide useful feedback for actions such as adding or removing cart items.
- Disable or validate actions when their required input is missing.
- Ensure controls are usable with keyboard interaction where applicable.
- Avoid unnecessary animation and respect reduced-motion preferences.

## SCOPE AND FORBIDDEN WORK

Included:
- Six customer-facing storefront pages.
- Shared application shell.
- Mobile-only layout.
- Minimalist Modern design system.
- Local product data.
- Frontend navigation.
- Local cart interactions.
- Frontend-only Profile and Livechat presentation.

Explicitly excluded:
- Admin dashboard.
- Seller management.
- Multi-vendor marketplace.
- Backend services or database.
- Authentication.
- Payment gateway.
- Real checkout or order processing.
- Shipping and delivery integration.
- Real-time chat or external livechat integration.
- Unrequested pages, features, or business rules.

Do not expand the scope without explicit instruction.

## EXECUTION PROCESS

Work in this order:

1. Inspect the current Gemini Canvas project and identify its existing structure.
2. Preserve working project configuration and existing approved implementation.
3. Establish the shared application shell and mobile layout behavior.
4. Establish the design tokens and reusable UI components.
5. Implement the typed ProductApiService, API response validation, shared catalog state, and shared cart state.
6. Implement the six pages.
7. Connect navigation and page-level interactions.
8. Validate responsive behavior, scrolling, accessibility, and state consistency.
9. Fix implementation errors within the defined scope.
10. Review the finished application against every validation requirement.

Do not stop after producing a plan or wireframe. Implement the application in Canvas.

## VALIDATION CHECKLIST

Before declaring the task complete, verify:

### Application structure
- [ ] One React application is used.
- [ ] All six required pages are implemented.
- [ ] All pages use the same AppShell.
- [ ] Header and BottomNav remain outside the scrollable content region.

### Layout
- [ ] Maximum application width is 500px.
- [ ] Mobile width fills the available viewport.
- [ ] Wider screens center the application without creating a desktop layout.
- [ ] Main content uses `flex: 1` and `min-height: 0`.
- [ ] Only the intended content region scrolls vertically.
- [ ] No unintended horizontal overflow exists.
- [ ] Header and BottomNav remain visible during content scrolling.

### Navigation
- [ ] Home, Catalog, and Livechat navigation work.
- [ ] Header Cart and Profile actions work.
- [ ] Product cards open the correct product detail.
- [ ] Returning from product detail is predictable.
- [ ] Active BottomNav state is correct.

### Product API and catalog
- [ ] ProductApiService calls the exact configured GET endpoint.
- [ ] No local or mock product catalog is used as a fallback.
- [ ] HTTP status, JSON parsing, `success`, and `data` shape are validated.
- [ ] Loading, API error, retry, and empty-catalog states work.
- [ ] HomePage, CatalogPage, and ProductDetailPage use the same API-backed data.
- [ ] Search and category filtering operate on the API product records.
- [ ] Records are grouped by `item_group_id`.
- [ ] Variant selection resolves to the correct `id`.
- [ ] Price uses valid `sale_price` when available, otherwise `price`.
- [ ] Prices are formatted as IDR.
- [ ] Primary and additional product images use the API image fields.
- [ ] Rich text is sanitized before rendering, with plain-text fallback.
- [ ] Product details match the selected API record and group.

### Cart
- [ ] Add to Cart updates shared cart state.
- [ ] Matching product-and-variant lines update quantity correctly.
- [ ] Different variants remain separate cart lines.
- [ ] Quantity controls work and prevent invalid quantities.
- [ ] Remove-item action works.
- [ ] Cart count, subtotals, and total remain accurate.
- [ ] Empty cart state is correct.
- [ ] Checkout does not claim to process a real order or payment.

### Quality
- [ ] No visible control is unintentionally inert.
- [ ] No broken images or rendering errors remain.
- [ ] No TypeScript errors remain.
- [ ] No runtime errors remain.
- [ ] Interactive controls have accessible names and visible focus states.
- [ ] No TODOs, pseudocode, or unfinished implementation remain.

If a requirement cannot be verified in the Canvas environment, state that limitation clearly instead of claiming it passed.

## ERROR HANDLING

- Diagnose errors before changing implementation.
- Fix the smallest relevant cause.
- Preserve existing working behavior.
- Do not rewrite unrelated components to solve a localized issue.
- Do not remove required features to hide an error.
- Do not silently replace unavailable functionality with fake success behavior.
- If a requirement conflicts with the existing project or cannot be implemented in Canvas, explain the conflict and choose the smallest scope-preserving solution.

## OUTPUT REQUIREMENTS

The final Canvas result must contain:
1. A working Divoom Store React storefront.
2. The six required pages connected through working navigation.
3. A consistent mobile-only AppShell and design system.
4. A catalog loaded from the specified Divoom Product API.
5. Functional API-backed catalog and client-side cart interactions.
6. Clear frontend-only boundaries for Profile, Livechat, and Checkout.

After implementation, provide a concise completion report covering:
- Pages implemented.
- Main interactions verified.
- Layout and responsive checks.
- Any remaining limitations.

## FINAL INSTRUCTION

Implement the Divoom Store storefront directly in Gemini Canvas. Follow this specification as the source of truth. Prioritize correctness, consistency, usability, and scope control over adding features.

Do not merely describe what should be built. Build it, validate it, and report only what has actually been completed and verified.
