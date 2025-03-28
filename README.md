# AdwApp - Adwaita Application Library for Deno

A Deno library for creating native GTK4/Adwaita applications through Deno's FFI.

## Requirements

- Linux with GTK4 and libadwaita installed
- Deno with `--allow-ffi` permission

## Usage

### Basic Example

```typescript
import { AdwApp } from "jsr:@sigmasd/adw-app";

if (import.meta.main) {
  using app = new AdwApp({ id: "com.example.myapp" });
  app.run();
}
```

### Webview Example

You can integrate with webview to create hybrid desktop applications:

```typescript
import { AdwApp } from "jsr:@sigmasd/adw-app";
import { Webview } from "jsr:@webview/webview";

// Create AdwApp and embed webview with inline HTML
using app = new AdwApp({ id: "com.example.webviewapp" });
app.run((window) => {
  const webview = new Webview(false, undefined, window);

  // Load HTML content directly
  const html = `
    <!DOCTYPE html>
    <html>
    <head>
      <title>AdwApp Webview Example</title>
      <style>
        body {
          font-family: system-ui, sans-serif;
          max-width: 800px;
          margin: 0 auto;
          padding: 2rem;
          text-align: center;
        }
        button {
          padding: 0.5rem 1rem;
          background: #3584e4;
          color: white;
          border: none;
          border-radius: 4px;
          cursor: pointer;
        }
        button:hover {
          background: #1c71d8;
        }
      </style>
    </head>
    <body>
      <h1>Hello from AdwApp + Webview!</h1>
      <p>This is a simple example of embedding a webview in an Adwaita application.</p>
      <button id="btn">Click Me</button>

      <script>
        document.getElementById('btn').addEventListener('click', () => {
          alert('Button clicked!');
        });
      </script>
    </body>
    </html>
  `;

  webview.navigate(`data:text/html,${encodeURIComponent(html)}`);
});
```

## API

### AdwApp

The main class for creating GTK4/Adwaita applications.

#### Constructor

```typescript
new AdwApp({ id: string })
```

- `id`: Application ID in reverse domain notation (e.g., "com.example.myapp")

#### Methods

- `run(callback?: (window: Deno.PointerValue) => void)`: Runs the application and creates a window
  - The optional callback lets you customize the window or add widgets before display

#### Cleanup

The class implements `Symbol.dispose` for proper resource cleanup with `using` syntax.

## License

MIT
