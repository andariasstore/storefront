# ASRI COLLECTION — GEMINI CANVAS STOREFRONT PROMPT

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

Build **Asri Collection**, a single-store online fashion storefront.

- Store model: one independent store with its own brand identity.
- Application type: customer-facing storefront.
- Primary target: mobile devices.
- Design direction: Minimalist Modern.
- Development environment: Gemini Canvas.
- Frontend stack: React, TypeScript, CSS, and Lucide Icons.

This is a frontend storefront application. It is not a marketplace, seller platform, or store administration system.

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

All pages must share one consistent application shell and design system. Core frontend interactions must work without requiring a backend.

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
├── Data
│   └── LocalStoreData
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
- Display the Asri Collection brand identity.
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

Do not invent discount campaigns, shipping promises, ratings, or brand claims. If content is needed to demonstrate the UI, use clearly identifiable local demo content.

### 2. CatalogPage

Purpose: let customers browse the available product catalog.

Include:
- Product listing.
- Product image, name, and price.
- Category selection.
- Search input.
- Sorting or filtering controls only where they can be implemented meaningfully.
- Clear empty state when no products match the search or selected category.

Search and category filtering must operate on the same local product dataset. Do not create controls that look interactive but have no effect.

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
- Do not invent product specifications that are not present in the local product data.

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

This version uses frontend-local data and state only.

Product data:
- Use one consistent typed product dataset as the source for HomePage, CatalogPage, and ProductDetailPage.
- Product cards and product details must refer to the same product records.
- Each product should have a stable identifier.
- Support product variants only when the product data defines them.
- Use local demo product records only when needed to demonstrate the storefront; do not present demo information as verified business information.

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
5. Define the typed local product data and shared cart state.
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

### Catalog and product
- [ ] Search filters the shared product dataset.
- [ ] Category selection filters the shared product dataset.
- [ ] Empty search results display an appropriate empty state.
- [ ] Product details match the selected product record.
- [ ] Required variants are validated before adding to cart.

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
1. A working Asri Collection React storefront.
2. The six required pages connected through working navigation.
3. A consistent mobile-only AppShell and design system.
4. Functional local catalog and cart interactions.
5. Clear frontend-only boundaries for Profile, Livechat, and Checkout.

After implementation, provide a concise completion report covering:
- Pages implemented.
- Main interactions verified.
- Layout and responsive checks.
- Any remaining limitations.

## FINAL INSTRUCTION

Implement the Asri Collection storefront directly in Gemini Canvas. Follow this specification as the source of truth. Prioritize correctness, consistency, usability, and scope control over adding features.

Do not merely describe what should be built. Build it, validate it, and report only what has actually been completed and verified.
