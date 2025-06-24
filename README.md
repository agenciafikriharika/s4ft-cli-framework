# s4ft Framework

**s4ft** (Simple And Fast Templates) is a declarative web development language and framework inspired by Next.js. It provides an intuitive syntax for building modern web applications with built-in routing, state management, and component architecture.

## 🚀 Features

- **Declarative Syntax**: Clean, readable component definitions with built-in props, state, and events
- **File-based Routing**: Automatic routing based on your folder structure in the \`app/\` directory
- **Hot Reload**: Lightning-fast development with instant updates
- **Production Ready**: Optimized builds with minification and tree-shaking
- **TypeScript Support**: Optional TypeScript integration for type safety
- **CSS Modules**: Scoped styling with CSS Modules support
- **API Routes**: Built-in API routing system
- **Static & Dynamic Routes**: Support for both static and dynamic routing patterns

## 📦 Installation

\`\`\`bash
npm install -g s4ft-framework
\`\`\`

## 🏗️ Quick Start

Create a new s4ft project:

\`\`\`bash
s4ft create my-app
cd my-app
s4ft dev
\`\`\`

Your app will be running at \`http://localhost:3000\`

## 📁 Project Structure

\`\`\`
my-app/
├── app/                 # App router (pages, layouts, API routes)
│   ├── api/            # API routes
│   ├── about/          # About page route
│   │   └── page.sft    # About page component
│   ├── layout.sft      # Root layout
│   └── page.sft        # Home page
├── components/         # Reusable components
│   ├── Button.sft
│   └── Card.sft
├── styles/            # CSS files
│   └── globals.css
├── public/            # Static assets
├── scripts/           # Build and utility scripts
└── docs/              # Documentation
\`\`\`

## 🎯 s4ft Syntax

### Components

\`\`\`s4ft
component Button {
  props {
    text: string = "Click me",
    variant: string = "primary",
    onClick: function
  }
  
  state {
    isPressed: boolean = false
  }
  
  event handleClick() {
    setIsPressed(true);
    if (onClick) {
      onClick();
    }
    setTimeout(() => setIsPressed(false), 100);
  }
  
  <button 
    className={\`btn btn-\${variant} \${isPressed ? 'pressed' : ''}\`}
    onClick={handleClick}
  >
    {text}
  </button>
}

export Button;
\`\`\`

### Pages

\`\`\`s4ft
page HomePage {
  state {
    count: number = 0,
    message: string = "Welcome to s4ft!"
  }
  
  event incrementCount() {
    setCount(count + 1);
  }
  
  <div className="container">
    <h1>{message}</h1>
    <p>Count: {count}</p>
    <Button text="Increment" onClick={incrementCount} />
  </div>
}

export HomePage;
\`\`\`

### Layouts

\`\`\`s4ft
layout RootLayout {
  props {
    children: ReactNode
  }
  
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <title>My s4ft App</title>
    </head>
    <body>
      <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
      </nav>
      <main>
        {children}
      </main>
    </body>
  </html>
}

export RootLayout;
\`\`\`

### API Routes

\`\`\`s4ft
// app/api/users.sft
export function GET(request) {
  return {
    status: 200,
    body: {
      users: [
        { id: 1, name: "Alice" },
        { id: 2, name: "Bob" }
      ]
    }
  };
}

export function POST(request) {
  const { name, email } = request.body;
  
  return {
    status: 201,
    body: {
      message: "User created",
      user: { id: Date.now(), name, email }
    }
  };
}
\`\`\`

## 🛠️ CLI Commands

### Development

\`\`\`bash
s4ft dev                 # Start development server
s4ft dev -p 8080        # Start on custom port
\`\`\`

### Building

\`\`\`bash
s4ft build              # Build for production
s4ft build --no-minify  # Build without minification
s4ft build --source-maps # Build with source maps
\`\`\`

### Production

\`\`\`bash
s4ft serve              # Serve production build
s4ft serve -p 8080      # Serve on custom port
\`\`\`

## 🎨 Styling

s4ft supports multiple styling approaches:

### Global CSS

\`\`\`css
/* styles/globals.css */
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}
\`\`\`

### CSS Modules

\`\`\`css
/* components/Button.module.css */
.button {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 0.375rem;
  cursor: pointer;
}

.primary {
  background-color: #3b82f6;
  color: white;
}
\`\`\`

## 🔄 State Management

s4ft provides built-in state management:

\`\`\`s4ft
component Counter {
  state {
    count: number = 0,
    step: number = 1,
    history: array = []
  }
  
  event increment() {
    const newCount = count + step;
    setCount(newCount);
    setHistory([...history, newCount]);
  }
  
  event reset() {
    setCount(0);
    setHistory([]);
  }
  
  <div>
    <p>Count: {count}</p>
    <button onClick={increment}>+{step}</button>
    <button onClick={reset}>Reset</button>
  </div>
}
\`\`\`

## 🌐 Routing

### Static Routes

\`\`\`
app/
├── page.sft           # /
├── about/
│   └── page.sft       # /about
└── contact/
    └── page.sft       # /contact
\`\`\`

### Dynamic Routes

\`\`\`
app/
├── blog/
│   ├── page.sft       # /blog
│   └── [slug]/
│       └── page.sft   # /blog/[slug]
└── users/
    └── [id]/
        └── page.sft   # /users/[id]
\`\`\`

### Dynamic Route Component

\`\`\`s4ft
// app/blog/[slug]/page.sft
page BlogPost {
  props {
    params: object  // { slug: string }
  }
  
  state {
    post: object = null,
    loading: boolean = true
  }
  
  event loadPost() {
    // Fetch post data using params.slug
  }
  
  <div>
    {loading ? (
      <p>Loading...</p>
    ) : (
      <article>
        <h1>{post.title}</h1>
        <p>{post.content}</p>
      </article>
    )}
  </div>
}

export BlogPost;
\`\`\`

## 🔧 Configuration

### TypeScript Support

Create a \`tsconfig.json\`:

\`\`\`json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "jsx": "react-jsx",
    "strict": true,
    "esModuleInterop": true
  },
  "include": ["**/*.sft", "**/*.ts", "**/*.tsx"]
}
\`\`\`

### Environment Variables

Create a \`.env.local\` file:

\`\`\`
DATABASE_URL=postgresql://localhost:5432/mydb
API_KEY=your-api-key
NEXT_PUBLIC_APP_URL=http://localhost:3000
\`\`\`

## 📚 Advanced Features

### Conditional Rendering

\`\`\`s4ft
component UserProfile {
  props {
    user: object
  }
  
  <div>
    {if (user) {
      <div>
        <h2>{user.name}</h2>
        <p>{user.email}</p>
      </div>
    } else {
      <p>Please log in</p>
    }}
  </div>
}
\`\`\`

### Loops

\`\`\`s4ft
component TodoList {
  state {
    todos: array = [
      { id: 1, text: "Learn s4ft", done: false },
      { id: 2, text: "Build an app", done: false }
    ]
  }
  
  <ul>
    {for (todo in todos) {
      <li key={todo.id} className={todo.done ? 'completed' : ''}>
        {todo.text}
      </li>
    }}
  </ul>
}
\`\`\`

## 🚀 Deployment

### Build for Production

\`\`\`bash
s4ft build
\`\`\`

### Deploy to Vercel

\`\`\`bash
npm install -g vercel
vercel --prod
\`\`\`

### Deploy to Netlify

\`\`\`bash
npm run build
# Upload dist/ folder to Netlify
\`\`\`

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🆘 Support

- 📖 [Documentation](https://s4ft-framework.dev/docs)
- 💬 [Discord Community](https://discord.gg/s4ft)
- 🐛 [Issue Tracker](https://github.com/s4ft-framework/s4ft/issues)
- 📧 [Email Support](mailto:support@s4ft-framework.dev)

---

**Happy coding with s4ft! 🎉**
\`\`\`
