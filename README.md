# UPF UI Shell

React Native Module Federation host application for the Unified Plugin Framework.

## Overview

The UI Shell is the main application that:

- **Hosts plugin UIs** via Module Federation (Re.Pack)
- **Provides navigation** and routing between plugins
- **Manages shared state** via the State Bridge
- **Handles cross-plugin events** via the Event Bus
- **Supports all platforms**: Web, iOS, and Android

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      UI Shell (Host)                      │
├─────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐      │
│  │   Plugin    │  │   Plugin    │  │   Plugin    │      │
│  │   Loader    │  │   Router    │  │   Bridge    │      │
│  └─────────────┘  └─────────────┘  └─────────────┘      │
├─────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────┐    │
│  │              Module Federation                    │    │
│  │    (Dynamically loads remote plugin UIs)         │    │
│  └─────────────────────────────────────────────────┘    │
├─────────────────────────────────────────────────────────┤
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐    │
│  │ Plugin  │  │ Plugin  │  │ Plugin  │  │ Plugin  │    │
│  │ A UI    │  │ B UI    │  │ C UI    │  │ D UI    │    │
│  │(Remote) │  │(Remote) │  │(Remote) │  │(Remote) │    │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘    │
└─────────────────────────────────────────────────────────┘
```

## Quick Start

```bash
# Clone the repository
git clone https://github.com/Unified-Plugin-Framework/ui-shell.git
cd ui-shell

# Install dependencies
npm install

# Start development server
npm run start

# Run on specific platform
npm run ios
npm run android
npm run web
```

## Configuration

Configure the UI Shell via environment variables or `app.config.ts`:

```typescript
// app.config.ts
export default {
  // API Gateway URL
  gatewayUrl: process.env.GATEWAY_URL || 'http://localhost:8080',

  // Plugin Registry URL
  registryUrl: process.env.REGISTRY_URL || 'http://localhost:50052',

  // Module Federation settings
  federation: {
    // Shared dependencies
    shared: ['react', 'react-native', '@unified-plugin-framework/ui-sdk'],
  },
};
```

## Plugin Loading

The UI Shell automatically loads plugin UIs based on manifests from the Plugin Registry:

```typescript
// plugins are loaded dynamically
import { usePluginRegistry } from '@unified-plugin-framework/ui-sdk';

function App() {
  const { plugins, loading } = usePluginRegistry();

  if (loading) return <LoadingScreen />;

  return (
    <NavigationContainer>
      <Navigator>
        {plugins.map(plugin => (
          <Screen
            key={plugin.id}
            name={plugin.id}
            component={plugin.screens.main}
          />
        ))}
      </Navigator>
    </NavigationContainer>
  );
}
```

## Shared Dependencies

The following dependencies are shared across all plugins:

- `react`
- `react-native`
- `@react-navigation/native`
- `@unified-plugin-framework/ui-sdk`
- `@tanstack/react-query`

## Documentation

See the [UI Federation documentation](https://unified-plugin-framework.github.io/docs/architecture/ui-federation) for complete details.

## Related Repositories

- [@unified-plugin-framework/sdk](https://github.com/Unified-Plugin-Framework/sdk) - UI SDK package
- [plugin-template](https://github.com/Unified-Plugin-Framework/plugin-template) - Plugin template with UI
- [examples](https://github.com/Unified-Plugin-Framework/examples) - Example plugins

## License

MIT
