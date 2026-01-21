# 🔌 TMPDIR Claude Code Plugins

A collection of Claude Code plugins I use to enhance development workflows.

## 📦 Available Plugins

### 📝 Doc-Driven Development

A documentation-driven development plugin that helps you plan and implement code
changes based on markdown documentation.

[View Plugin Documentation](./doc-driven-development/README.md)

## 🚀 Installation

### From GitHub (Recommended)

1. Add this marketplace to Claude Code:

```bash
/plugin marketplace add tmpdir-org/tmpdir-claude-code-marketplace
```

2. Install the plugin:

```bash
/plugin install doc-driven-development@tmpdir
```

### 🛠️ Local Development

For local plugin development and testing, create a local marketplace:

```bash
# Add local marketplace
/plugin marketplace add /path/to/claude-plugins

# Install from local marketplace
/plugin install doc-driven-development@tmpdir
```

## 🔄 Updating

To get the latest:

```bash
/plugin marketplace update tmpdir
```

This appears to update all the plugins from this marketplace.

## 🏗️ Plugin Development

This repository follows the standard Claude Code plugin structure:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata
└── commands/
    └── command-name.md      # Command definitions
```

### 💡 Key Concepts

1. **Command definitions are markdown files** - Each `.md` file in `commands/`
   becomes a slash command
2. **Commands are prompts** - The markdown content is given to Claude Code when
   invoked
3. **Plugin metadata** - The `plugin.json` defines the plugin's identity

See [CLAUDE.md](./CLAUDE.md) for detailed development guidance.

## 🤝 Contributing

To add a new plugin to this repository:

1. Create a new directory with your plugin structure
2. Add plugin metadata in `.claude-plugin/plugin.json`
3. Define commands in the `commands/` directory
4. Update `.claude-plugin/marketplace.json` to include your plugin
5. Submit a pull request

## 📋 Requirements

- Claude Code CLI
- Git (for plugins that use git-based workflows)

## 📄 License

MIT

## 👥 Contributors

- [@cbrake](https://github.com/cbrake)
- [@jsnapoli1](https://github.com/jsnapoli1)

If you use this, please star the repo and
[let us know](https://github.com/tmpdir-org/tmpdir-claude-code-marketplace/discussions).
