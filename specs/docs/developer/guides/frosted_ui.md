> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

> ## Agent Instructions
> Treat these docs and the Whop MCP server as the source of truth for Whop API behavior. Don't infer endpoint behavior from marketing pages.
> MCP server: connect https://mcp.whop.com/mcp (streamable HTTP, browser sign-in, no API key in the client config) to search these docs and read or change live Whop data. Setup: https://docs.whop.com/developer/guides/ai_and_mcp.md
> Whop CLI: install with `curl -fsSL https://whop.com/install.sh | sh` to scaffold, run, and deploy Whop apps and websites. It acts on production, not the sandbox. Overview: https://docs.whop.com/cli/overview.md Agent mode: https://docs.whop.com/cli/agent-mode.md
> Hosting: Whop serves a site or app from the build `whop apps deploy` uploads, with rollbacks, secrets, and server logs. Guide: https://docs.whop.com/developer/websites/hosting.md
> Plugins: in Claude Code, run `/plugin marketplace add whopio/plugins`, then `/plugin install whop@whop`. Guide: https://docs.whop.com/developer/guides/plugins/claude-code.md Every agent tool: https://docs.whop.com/developer/ai/overview.md
> Environments: build and test in the sandbox (API `https://sandbox-api.whop.com/api/v1`, keys at https://sandbox.whop.com/dashboard/developer, SDK environment `Sandbox`; no real money). Production (API `https://api.whop.com/api/v1`, keys at https://whop.com/dashboard/developer, SDK environment `Production`, the SDK default) moves real money, so use it only at launch. The two share no accounts, keys, products, or webhooks, and their keys look identical: never mix them. Sandbox guide: https://docs.whop.com/developer/guides/sandbox.md Comparison: https://docs.whop.com/developer/quickstart.md
> API: send `Authorization: Bearer <API_KEY>` from your server only, and pin a dated version with the `Api-Version-Date` header. OpenAPI spec: https://api.whop.com/api/v1/openapi.json
> Which API: build on the Current API, whose pages live under https://docs.whop.com/api-reference/ (start at https://docs.whop.com/api-reference/overview.md). Pages under /api-reference/legacy/ are the Legacy API. Use one only when https://docs.whop.com/api-reference/stability.md lists the resource as Legacy-only.

# Frosted UI

> Install and use Frosted UI, Whop's React component library and design system, to build your app's interface.

Frosted UI is Whop's comprehensive React component library and design system. It provides more than 60 components that meet accessibility standards. It also includes a sophisticated color system, typography scale, and seamless dark mode support—all built on Radix UI primitives and Tailwind CSS.

## Installation

**1. Install the package**

```bash pnpm theme={null}
pnpm add @whop/react
```

```bash npm theme={null}
npm install @whop/react
```

```bash yarn theme={null}
yarn add @whop/react
```

```bash bun theme={null}
bun install @whop/react
```

> [!NOTE]
>
> The `@whop/react` package includes Frosted UI and all necessary dependencies.

**2. Set up the WhopApp provider**

Wrap your application with the `WhopApp` component in your root layout. This sets up the theme and iframe SDK.

```tsx theme={null}
// app/layout.tsx
import { WhopApp } from "@whop/react";

export default function RootLayout({
    children,
}: {
    children: React.ReactNode;
}) {
    return (
        <html lang="en">
            <body>
                <WhopApp accentColor="blue" appearance="inherit">
                    {children}
                </WhopApp>
            </body>
        </html>
    );
}
```

**3. Configure Next.js (optional)**

Use the `withWhopAppConfig` wrapper in your Next.js configuration:

```typescript theme={null}
// next.config.ts
import type { NextConfig } from "next";
import { withWhopAppConfig } from "@whop/react/next.config";

const nextConfig: NextConfig = {
    // your config options
};

export default withWhopAppConfig(nextConfig);
```

> [!NOTE]
>
> This configures server actions and optimizes Frosted UI imports.

**4. Set up Tailwind (optional)**

If you're using Tailwind CSS, add the Frosted UI plugin to access design tokens:

```javascript theme={null}
// tailwind.config.js
import { frostedThemePlugin } from "@whop/react/tailwind";

export default {
    content: ["./src/**/*.{js,ts,jsx,tsx}"],
    plugins: [frostedThemePlugin()],
};
```

> [!TIP]
>
> Frosted UI is now set up and ready to use.

***

## Typography

Frosted UI provides a comprehensive typography system with two main components: `Heading` and `Text`.

### Type scale

Both components use a 10-step size scale (0-9):

| Size | Font Size | Usage |
| - | - | - |
| 0 | 10px | Smallest text |
| 1 | 12px | Small labels |
| 2 | 14px | Body text (small) |
| 3 | 16px | Body text (default) |
| 4 | 18px | Emphasized text |
| 5 | 20px | Subheadings |
| 6 | 24px | Headings |
| 7 | 28px | Large headings |
| 8 | 32px | Display text |
| 9 | 40px | Hero text |

### Text component

```tsx theme={null}
import { Text } from "@whop/react";

<Text size="3" weight="medium">
    This is body text
</Text>

<Text size="1" color="gray">
    Small muted text
</Text>

<Text as="label" size="2" weight="bold">
    Form Label
</Text>
```

**Props:**

* `size`: `"0"` through `"9"` (default: `"3"`)
* `weight`: `"light"` | `"regular"` | `"medium"` | `"bold"`
* `align`: `"left"` | `"center"` | `"right"`
* `color`: Any accent color or `"gray"`
* `highContrast`: Increases contrast for accessibility
* `as`: `"span"` | `"div"` | `"p"` | `"label"`

### Heading component

```tsx theme={null}
import { Heading } from "@whop/react";

<Heading size="6" as="h1">
    Page Title
</Heading>

<Heading size="4" as="h2" weight="medium">
    Section Heading
</Heading>
```

**Props:**

* `size`: `"0"` through `"9"` (default: `"6"`)
* `weight`: `"light"` | `"regular"` | `"medium"` | `"bold"`
* `align`: `"left"` | `"center"` | `"right"`
* `as`: `"h1"` | `"h2"` | `"h3"` | `"h4"` | `"h5"` | `"h6"`
* `color`: Any accent color or `"gray"`
* `highContrast`: Increases contrast for accessibility

### Other typography components

```tsx theme={null}
import { Code, Strong, Em, Kbd } from "@whop/react";

<Code>const example = "code";</Code>
<Strong>Bold text</Strong>
<Em>Italic text</Em>
<Kbd>⌘ K</Kbd>
```

***

## Colors

Frosted UI uses Radix color scales, providing 12-step color palettes with both solid and alpha variants.

### Accent colors

27 accent colors are available:

**Regular Colors:** `tomato`, `red`, `ruby`, `crimson`, `pink`, `plum`, `purple`, `violet`, `iris`, `indigo`, `blue`, `cyan`, `teal`, `jade`, `green`, `grass`, `brown`, `orange`

**Bright Colors:** `sky`, `mint`, `lime`, `yellow`, `amber`, `lemon`, `magenta`

**Metallic Colors:** `gold`, `bronze`

### Gray colors

6 gray scales with different color temperatures:

* `gray` - Pure neutral gray
* `mauve` - Slightly purple-tinted
* `slate` - Cool blue-tinted
* `sage` - Subtle green-tinted
* `olive` - Warm olive-tinted
* `sand` - Warm sandy-tinted
* `auto` - Automatically matches accent color

### Semantic colors

Pre-configured colors for common use cases:

* **Danger:** `red` (default), `tomato`, `ruby`
* **Warning:** `amber` (default), `yellow`
* **Success:** `green` (default), `teal`, `jade`, `grass`
* **Info:** `sky` (default), `blue`

### Using colors

```tsx theme={null}
import { Button, Badge, Text } from "@whop/react";

<Button color="blue">Primary Action</Button>
<Button color="red">Delete</Button>
<Badge color="green">Active</Badge>
<Text color="gray">Muted text</Text>
```

### Tailwind color classes

With the Tailwind plugin, you can use Frosted UI colors directly:

```tsx theme={null}
<div className="bg-blue-2 text-blue-11 border border-blue-6">
    Blue themed box
</div>

<div className="bg-red-a3 text-red-11">
    Red with alpha background
</div>
```

**Color Scale:**

* Steps 1-3: Backgrounds and subtle fills
* Steps 4-6: Borders and separators
* Steps 7-9: Interactive elements
* Steps 10-12: Text and high contrast elements

**Alpha Variants:**

* Use `{color}-a1` through `{color}-a12` for transparency

> [!WARNING]
>
> With the `frostedUiPlugin()` added to your tailwind config, the default
> tailwind colors like `bg-blue-200` don't work. Use `bg-blue-2` instead
> according to the above documentation.

***

## Dark mode

Frosted UI includes automatic dark mode support that respects system preferences and user choices.

### How it works

Manage dark mode through three appearance modes:

* `"inherit"` (default) - Respects system preference
* `"light"` - Forces light mode
* `"dark"` - Forces dark mode

The browser stores the theme preference in a cookie (`whop-frosted-theme`) and falls back to the system preference when none exists.

### Configuration

Configure the appearance in your `WhopApp` component:

```tsx theme={null}
<WhopApp appearance="dark">{children}</WhopApp>
```

Or create nested themes with different appearances:

```tsx theme={null}
import { Theme } from "@whop/react";

<Theme appearance="light">
    <div>This section is always light</div>
</Theme>

<Theme appearance="dark">
    <div>This section is always dark</div>
</Theme>
```

### Tailwind dark mode classes

Use the `dark:` prefix for dark mode styles:

```tsx theme={null}
<div className="bg-white dark:bg-black text-black dark:text-white">
	Adapts to theme
</div>
```

***

## Breakpoints

Frosted UI uses a mobile-first responsive system with 5 breakpoints.

### Breakpoint scale

| Name | Min Width | Usage |
| - | - | - |
| `initial` | 0px | Mobile (default) |
| `xs` | 520px | Large mobile |
| `sm` | 768px | Tablet |
| `md` | 1024px | Small desktop |
| `lg` | 1280px | Desktop |
| `xl` | 1640px | Large desktop |

### Responsive props

Many components support responsive values using an object syntax:

```tsx theme={null}
import { Text, Box } from "@whop/react";

<Text size={{ initial: "2", md: "4", lg: "6" }}>Responsive text size</Text>;
```

### Tailwind responsive classes

```tsx theme={null}
<div className="p-2 md:p-4 lg:p-8">
    Responsive padding
</div>

<div className="text-2 sm:text-3 md:text-4">
    Responsive text
</div>
```

***

## Icons

Frosted UI includes an icon system through the `@frosted-ui/icons` package.

### Using icons

```tsx theme={null}
import { IconButton } from "@whop/react";
import { MagnifyingGlassIcon, Cross2Icon } from "@frosted-ui/icons";

<IconButton>
    <MagnifyingGlassIcon />
</IconButton>

<IconButton color="red">
    <Cross2Icon />
</IconButton>
```

### Common icons

Frosted UI uses Radix Icons, which includes icons like:

* Navigation: `ChevronRightIcon`, `ChevronDownIcon`, `ArrowRightIcon`
* Actions: `Cross2Icon`, `CheckIcon`, `PlusIcon`, `MinusIcon`
* UI: `MagnifyingGlassIcon`, `GearIcon`, `PersonIcon`
* More icons are available in the library.

***

## Tailwind plugin

The Frosted UI Tailwind plugin provides design tokens as Tailwind utilities.

### What's included

**Typography:**

```tsx theme={null}
<p className="text-3 leading-3 font-medium tracking-2">
	Styled with Frosted UI tokens
</p>
```

**Colors:**

```tsx theme={null}
<div className="bg-blue-2 text-blue-11 border-blue-6">Blue themed</div>
```

**Spacing & Layout:**

```tsx theme={null}
<div className="p-4 md:p-6 lg:p-8">Responsive padding</div>
```

### Key differences from standard Tailwind

1. **Font Size Scale:** Uses 0-9 instead of xs/sm/base/lg
2. **Line Height Scale:** Matches font size scale (0-9)
3. **Color System:** 12-step Radix color scales with alpha variants
4. **Semantic Colors:** Built-in danger, warning, success, info colors
5. **Design Tokens:** All values use CSS variables for theme consistency

***

## Common components

### Button

```tsx theme={null}
import { Button } from "@whop/react";

<Button size="2" variant="solid" color="blue">
    Primary Action
</Button>

<Button size="2" variant="soft" color="gray">
    Secondary
</Button>

<Button size="2" variant="ghost" color="red">
    Danger
</Button>
```

**Props:**

* `size`: `"1"` | `"2"` | `"3"` | `"4"`
* `variant`: `"solid"` | `"soft"` | `"surface"` | `"ghost"` | `"classic"`
* `color`: Any accent color
* `highContrast`: Enhanced contrast
* `loading`: Shows loading spinner
* `disabled`: Disables interaction

### Input

```tsx theme={null}
import { TextInput, TextArea } from "@whop/react";

<TextInput
    size="2"
    placeholder="Enter text..."
    variant="surface"
/>

<TextArea
    size="2"
    placeholder="Enter multiple lines..."
    rows={4}
/>
```

### Card

```tsx theme={null}
import { Card } from "@whop/react";

<Card size="2" variant="surface">
	<Heading size="4">Card Title</Heading>
	<Text>Card content goes here</Text>
</Card>;
```

### Badge

```tsx theme={null}
import { Badge } from "@whop/react";

<Badge color="green" variant="soft">Active</Badge>
<Badge color="red" variant="solid">Error</Badge>
<Badge color="gray" variant="surface">Draft</Badge>
```

### Dialog

```tsx theme={null}
import { Dialog, Button } from "@whop/react";

<Dialog.Root>
	<Dialog.Trigger>
		<Button>Open Dialog</Button>
	</Dialog.Trigger>
	<Dialog.Content>
		<Dialog.Title>Dialog Title</Dialog.Title>
		<Dialog.Description>This is the dialog content</Dialog.Description>
		<Dialog.Close>
			<Button>Close</Button>
		</Dialog.Close>
	</Dialog.Content>
</Dialog.Root>;
```

### Select

```tsx theme={null}
import { Select } from "@whop/react";

<Select.Root defaultValue="1">
	<Select.Trigger />
	<Select.Content>
		<Select.Item value="1">Option 1</Select.Item>
		<Select.Item value="2">Option 2</Select.Item>
		<Select.Item value="3">Option 3</Select.Item>
	</Select.Content>
</Select.Root>;
```

### Checkbox and radio

```tsx theme={null}
import { Checkbox, RadioGroup } from "@whop/react";

<Checkbox size="2" color="blue">
    Accept terms
</Checkbox>

<RadioGroup.Root defaultValue="1">
    <RadioGroup.Item value="1">Option 1</RadioGroup.Item>
    <RadioGroup.Item value="2">Option 2</RadioGroup.Item>
</RadioGroup.Root>
```

***

## Advanced usage

### Theme customization

Customize the theme with multiple options:

```tsx theme={null}
<WhopApp
	appearance="dark"
	accentColor="violet"
	grayColor="mauve"
	dangerColor="ruby"
	warningColor="amber"
	successColor="jade"
	infoColor="sky"
>
	{children}
</WhopApp>
```

### Nested themes

Create sections with different themes:

```tsx theme={null}
import { Theme } from "@whop/react";

<div>
	<Theme accentColor="blue">
		<Button>Blue button</Button>
	</Theme>

	<Theme accentColor="red" appearance="dark">
		<Button>Red button in dark mode</Button>
	</Theme>
</div>;
```

### Accessing theme context

```tsx theme={null}
import { useThemeContext } from "@whop/react";

function ThemedComponent() {
	const theme = useThemeContext();

	console.log(theme.appearance); // "light" | "dark"
	console.log(theme.accentColor); // "blue" | "red" | ...

	return <div>Current theme: {theme.appearance}</div>;
}
```

***

## Component library

Frosted UI includes 60+ components organized by category:

**Layout:** `Box`, `Flex`, `Grid`, `Container`, `Section`, `AspectRatio`, `Inset`

**Typography:** `Heading`, `Text`, `Code`, `Strong`, `Em`, `Kbd`, `Quote`, `Blockquote`

**Forms:** `TextInput`, `TextArea`, `Checkbox`, `RadioGroup`, `Select`, `Switch`, `Slider`, `DatePicker`, `OTPField`

**Buttons:** `Button`, `IconButton`

**Display:** `Card`, `Avatar`, `AvatarGroup`, `Badge`, `Callout`, `Table`, `DataList`, `Skeleton`

**Navigation:** `Tabs`, `Breadcrumbs`, `Link`, `SegmentedControl`

**Overlay:** `Dialog`, `AlertDialog`, `Drawer`, `Sheet`, `Popover`, `HoverCard`, `Tooltip`, `DropdownMenu`, `ContextMenu`

**Feedback:** `Progress`, `CircularProgress`, `Spinner`

**Utilities:** `ScrollArea`, `Separator`, `Portal`, `VisuallyHidden`, `AccessibleIcon`

> [!NOTE]
>
> All components are built on Radix UI primitives and follow accessibility best
> practices.

***

## Resources

* [Radix UI Documentation](https://www.radix-ui.com/) - Component primitives
* [Radix Colors](https://www.radix-ui.com/colors) - Color system documentation
* [Tailwind CSS](https://tailwindcss.com/) - Utility-first CSS framework


## Related topics

- [Platform Map](https://docs.whop.com/developer/platform-map.md)
- [Build Whop Apps with React Native](https://docs.whop.com/developer/guides/react-native.md)
- [Getting Started with Elements](https://docs.whop.com/elements/latest/getting-started.md)
