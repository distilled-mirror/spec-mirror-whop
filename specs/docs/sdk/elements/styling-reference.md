> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Styling Reference

> Stable CSS class names applied to Whop embeddable components

## Overview

The class names below are the **stable styling contract** for Whop embeddable
components. Each name is guaranteed to be applied to a specific element rendered
inside the iframe, so you can target it from `appearance.classes`.

`appearance.classes` accepts any CSS selector string as a key — the value is
emitted verbatim into a stylesheet inside the iframe. That means you can combine
the class names below with any standard CSS selector syntax (`:hover`,
`:focus-visible`, descendant selectors, attribute selectors, etc.). Selectors
that don't match any of the names below will still be emitted, but they're not
part of the stable contract and may stop matching as components evolve.

See [`Appearance`](/sdk/elements/types#appearance) for full configuration.

## Dialog

Modal dialog primitives shared across elements.

```typescript theme={null}
new WhopElements({
	appearance: {
		classes: {
			DialogContent: {
				borderRadius: "12px",
			},
			DialogTitle: {
				fontSize: "18px",
				fontWeight: "600",
			},
		},
	},
});
```

| Class               | Description                                                                   |
| ------------------- | ----------------------------------------------------------------------------- |
| `DialogBody`        | The main content area of a dialog.                                            |
| `DialogClose`       | A wrapper that closes the dialog when clicked.                                |
| `DialogCloseButton` | The icon button used to dismiss a dialog.                                     |
| `DialogContent`     | The content panel of a dialog.                                                |
| `DialogFooter`      | The footer area of a dialog, typically containing action buttons.             |
| `DialogHeader`      | The header area of a dialog, typically containing the title and close button. |
| `DialogRoot`        | The root container of a dialog overlay.                                       |
| `DialogTitle`       | The title text of a dialog.                                                   |
