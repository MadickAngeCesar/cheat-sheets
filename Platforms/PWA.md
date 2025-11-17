# Progressive Web Apps (PWA) Cheat Sheet

## Table of Contents
- [What is PWA](#what-is-pwa)
- [Core Concepts](#core-concepts)
- [HTML/Vanilla JavaScript PWA](#htmlvanilla-javascript-pwa)
- [React PWA](#react-pwa)
- [Next.js PWA](#nextjs-pwa)
- [Desktop PWA Features](#desktop-pwa-features)
- [Mobile PWA Features](#mobile-pwa-features)
- [Testing & Debugging](#testing--debugging)

---

## What is PWA

Progressive Web Apps are web applications that use modern web capabilities to deliver app-like experiences to users on both desktop and mobile devices.

### Key Benefits
- **Installable**: Can be installed on home screen/desktop
- **Offline-capable**: Works without internet connection
- **Fast**: Quick load times with caching
- **Responsive**: Works on any device
- **Secure**: Served via HTTPS
- **Engaging**: Push notifications and background sync

### Core Requirements
1. HTTPS (or localhost for development)
2. Web App Manifest
3. Service Worker
4. Responsive design

---

## Core Concepts

### Service Worker Lifecycle
```javascript
// States: installing → installed → activating → activated → redundant
self.addEventListener('install', event => {
  console.log('Service Worker installing');
});

self.addEventListener('activate', event => {
  console.log('Service Worker activated');
});

self.addEventListener('fetch', event => {
  console.log('Fetching:', event.request.url);
});
```

### Caching Strategies

#### 1. Cache First (Offline-first)
```javascript
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request)
      .then(response => response || fetch(event.request))
  );
});
```

#### 2. Network First
```javascript
self.addEventListener('fetch', event => {
  event.respondWith(
    fetch(event.request)
      .catch(() => caches.match(event.request))
  );
});
```

#### 3. Stale While Revalidate
```javascript
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.open('dynamic-cache').then(cache => {
      return cache.match(event.request).then(response => {
        const fetchPromise = fetch(event.request).then(networkResponse => {
          cache.put(event.request, networkResponse.clone());
          return networkResponse;
        });
        return response || fetchPromise;
      });
    })
  );
});
```

#### 4. Cache Only
```javascript
self.addEventListener('fetch', event => {
  event.respondWith(caches.match(event.request));
});
```

#### 5. Network Only
```javascript
self.addEventListener('fetch', event => {
  event.respondWith(fetch(event.request));
});
```

---

## HTML/Vanilla JavaScript PWA

### 1. Create Web App Manifest (`manifest.json`)
```json
{
  "name": "My PWA Application",
  "short_name": "My PWA",
  "description": "A Progressive Web App",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "orientation": "portrait-primary",
  "icons": [
    {
      "src": "/icons/icon-72x72.png",
      "sizes": "72x72",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icons/icon-96x96.png",
      "sizes": "96x96",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icons/icon-384x384.png",
      "sizes": "384x384",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any maskable"
    }
  ],
  "screenshots": [
    {
      "src": "/screenshots/desktop.png",
      "sizes": "1280x720",
      "type": "image/png",
      "form_factor": "wide"
    },
    {
      "src": "/screenshots/mobile.png",
      "sizes": "750x1334",
      "type": "image/png",
      "form_factor": "narrow"
    }
  ],
  "categories": ["productivity", "utilities"],
  "shortcuts": [
    {
      "name": "Open Dashboard",
      "short_name": "Dashboard",
      "description": "Open the dashboard",
      "url": "/dashboard",
      "icons": [{ "src": "/icons/dashboard.png", "sizes": "96x96" }]
    }
  ]
}
```

### 2. Link Manifest in HTML
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My PWA</title>
  
  <!-- PWA Manifest -->
  <link rel="manifest" href="/manifest.json">
  
  <!-- Theme Color -->
  <meta name="theme-color" content="#000000">
  
  <!-- iOS Support -->
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black">
  <meta name="apple-mobile-web-app-title" content="My PWA">
  <link rel="apple-touch-icon" href="/icons/icon-152x152.png">
  
  <!-- Favicon -->
  <link rel="icon" type="image/png" href="/icons/icon-192x192.png">
</head>
<body>
  <h1>My PWA Application</h1>
  <button id="installBtn" style="display:none;">Install App</button>
  
  <script src="/app.js"></script>
</body>
</html>
```

### 3. Register Service Worker (`app.js`)
```javascript
// Check if service workers are supported
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/service-worker.js')
      .then(registration => {
        console.log('Service Worker registered:', registration);
      })
      .catch(error => {
        console.log('Service Worker registration failed:', error);
      });
  });
}

// Install prompt
let deferredPrompt;
const installBtn = document.getElementById('installBtn');

window.addEventListener('beforeinstallprompt', (e) => {
  e.preventDefault();
  deferredPrompt = e;
  installBtn.style.display = 'block';
});

installBtn.addEventListener('click', async () => {
  if (deferredPrompt) {
    deferredPrompt.prompt();
    const { outcome } = await deferredPrompt.userChoice;
    console.log(`User response: ${outcome}`);
    deferredPrompt = null;
    installBtn.style.display = 'none';
  }
});

// Track installation
window.addEventListener('appinstalled', () => {
  console.log('PWA installed successfully');
});
```

### 4. Create Service Worker (`service-worker.js`)
```javascript
const CACHE_NAME = 'my-pwa-cache-v1';
const urlsToCache = [
  '/',
  '/index.html',
  '/app.js',
  '/styles.css',
  '/icons/icon-192x192.png',
  '/icons/icon-512x512.png'
];

// Install event - cache resources
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => {
        console.log('Opened cache');
        return cache.addAll(urlsToCache);
      })
  );
  self.skipWaiting(); // Activate immediately
});

// Activate event - clean old caches
self.addEventListener('activate', event => {
  event.waitUntil(
    caches.keys().then(cacheNames => {
      return Promise.all(
        cacheNames.map(cacheName => {
          if (cacheName !== CACHE_NAME) {
            console.log('Deleting old cache:', cacheName);
            return caches.delete(cacheName);
          }
        })
      );
    })
  );
  return self.clients.claim();
});

// Fetch event - serve from cache, fallback to network
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request)
      .then(response => {
        // Cache hit - return response
        if (response) {
          return response;
        }
        
        // Clone the request
        const fetchRequest = event.request.clone();
        
        return fetch(fetchRequest).then(response => {
          // Check if valid response
          if (!response || response.status !== 200 || response.type !== 'basic') {
            return response;
          }
          
          // Clone the response
          const responseToCache = response.clone();
          
          caches.open(CACHE_NAME)
            .then(cache => {
              cache.put(event.request, responseToCache);
            });
          
          return response;
        });
      })
  );
});

// Background Sync
self.addEventListener('sync', event => {
  if (event.tag === 'sync-data') {
    event.waitUntil(syncData());
  }
});

async function syncData() {
  // Sync logic here
  console.log('Syncing data...');
}

// Push Notifications
self.addEventListener('push', event => {
  const data = event.data ? event.data.json() : {};
  const title = data.title || 'Notification';
  const options = {
    body: data.body || 'You have a new notification',
    icon: '/icons/icon-192x192.png',
    badge: '/icons/icon-72x72.png',
    vibrate: [200, 100, 200],
    data: data.url || '/'
  };
  
  event.waitUntil(
    self.registration.showNotification(title, options)
  );
});

// Notification Click
self.addEventListener('notificationclick', event => {
  event.notification.close();
  event.waitUntil(
    clients.openWindow(event.notification.data)
  );
});
```

### 5. Push Notifications Setup
```javascript
// Request notification permission
async function requestNotificationPermission() {
  const permission = await Notification.requestPermission();
  if (permission === 'granted') {
    console.log('Notification permission granted');
    subscribeUserToPush();
  }
}

// Subscribe to push notifications
async function subscribeUserToPush() {
  const registration = await navigator.serviceWorker.ready;
  const subscription = await registration.pushManager.subscribe({
    userVisibleOnly: true,
    applicationServerKey: urlBase64ToUint8Array('YOUR_PUBLIC_VAPID_KEY')
  });
  
  // Send subscription to server
  await fetch('/api/subscribe', {
    method: 'POST',
    body: JSON.stringify(subscription),
    headers: {
      'Content-Type': 'application/json'
    }
  });
}

function urlBase64ToUint8Array(base64String) {
  const padding = '='.repeat((4 - base64String.length % 4) % 4);
  const base64 = (base64String + padding)
    .replace(/\-/g, '+')
    .replace(/_/g, '/');
  
  const rawData = window.atob(base64);
  const outputArray = new Uint8Array(rawData.length);
  
  for (let i = 0; i < rawData.length; ++i) {
    outputArray[i] = rawData.charCodeAt(i);
  }
  return outputArray;
}
```

---

## React PWA

### 1. Create React App with PWA Template
```bash
# Create new React app with PWA template
npx create-react-app my-pwa --template cra-template-pwa

# Or for TypeScript
npx create-react-app my-pwa --template cra-template-pwa-typescript
```

### 2. Enable Service Worker (`src/index.js`)
```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import * as serviceWorkerRegistration from './serviceWorkerRegistration';
import reportWebVitals from './reportWebVitals';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// Register service worker
serviceWorkerRegistration.register({
  onSuccess: registration => {
    console.log('Service Worker registered successfully:', registration);
  },
  onUpdate: registration => {
    console.log('New content available; please refresh.');
    // Show update notification to user
    if (window.confirm('New version available! Refresh to update?')) {
      window.location.reload();
    }
  }
});

reportWebVitals();
```

### 3. Custom Manifest (`public/manifest.json`)
```json
{
  "short_name": "React PWA",
  "name": "My React Progressive Web App",
  "description": "A React PWA with offline capabilities",
  "icons": [
    {
      "src": "favicon.ico",
      "sizes": "64x64 32x32 24x24 16x16",
      "type": "image/x-icon"
    },
    {
      "src": "logo192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "logo512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": ".",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff"
}
```

### 4. Install Button Component
```javascript
// src/components/InstallPWA.jsx
import { useState, useEffect } from 'react';

function InstallPWA() {
  const [supportsPWA, setSupportsPWA] = useState(false);
  const [promptInstall, setPromptInstall] = useState(null);

  useEffect(() => {
    const handler = (e) => {
      e.preventDefault();
      setSupportsPWA(true);
      setPromptInstall(e);
    };
    
    window.addEventListener('beforeinstallprompt', handler);

    return () => window.removeEventListener('beforeinstallprompt', handler);
  }, []);

  const handleInstallClick = async () => {
    if (!promptInstall) return;
    
    promptInstall.prompt();
    const { outcome } = await promptInstall.userChoice;
    
    if (outcome === 'accepted') {
      console.log('User accepted the install prompt');
    }
    
    setPromptInstall(null);
    setSupportsPWA(false);
  };

  if (!supportsPWA) return null;

  return (
    <button onClick={handleInstallClick} className="install-button">
      Install App
    </button>
  );
}

export default InstallPWA;
```

### 5. Custom Service Worker with Workbox
```bash
# Install dependencies
npm install workbox-webpack-plugin workbox-precaching workbox-routing workbox-strategies
```

```javascript
// src/service-worker.js
import { clientsClaim } from 'workbox-core';
import { ExpirationPlugin } from 'workbox-expiration';
import { precacheAndRoute, createHandlerBoundToURL } from 'workbox-precaching';
import { registerRoute } from 'workbox-routing';
import { StaleWhileRevalidate, CacheFirst, NetworkFirst } from 'workbox-strategies';

clientsClaim();

// Precache all of the assets generated by your build process
precacheAndRoute(self.__WB_MANIFEST);

// Set up App Shell-style routing
const fileExtensionRegexp = new RegExp('/[^/?]+\\.[^/]+$');
registerRoute(
  ({ request, url }) => {
    if (request.mode !== 'navigate') return false;
    if (url.pathname.startsWith('/_')) return false;
    if (url.pathname.match(fileExtensionRegexp)) return false;
    return true;
  },
  createHandlerBoundToURL(process.env.PUBLIC_URL + '/index.html')
);

// Cache images with Cache First strategy
registerRoute(
  ({ request }) => request.destination === 'image',
  new CacheFirst({
    cacheName: 'images',
    plugins: [
      new ExpirationPlugin({
        maxEntries: 60,
        maxAgeSeconds: 30 * 24 * 60 * 60, // 30 Days
      }),
    ],
  })
);

// Cache CSS and JS with Stale While Revalidate
registerRoute(
  ({ request }) =>
    request.destination === 'style' || request.destination === 'script',
  new StaleWhileRevalidate({
    cacheName: 'static-resources',
  })
);

// Cache API calls with Network First
registerRoute(
  ({ url }) => url.pathname.startsWith('/api/'),
  new NetworkFirst({
    cacheName: 'api-cache',
    plugins: [
      new ExpirationPlugin({
        maxEntries: 50,
        maxAgeSeconds: 5 * 60, // 5 minutes
      }),
    ],
  })
);

// Listen to messages from the client
self.addEventListener('message', (event) => {
  if (event.data && event.data.type === 'SKIP_WAITING') {
    self.skipWaiting();
  }
});
```

### 6. Offline Detection Hook
```javascript
// src/hooks/useOnlineStatus.js
import { useState, useEffect } from 'react';

function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);

  useEffect(() => {
    const handleOnline = () => setIsOnline(true);
    const handleOffline = () => setIsOnline(false);

    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);

    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);

  return isOnline;
}

export default useOnlineStatus;

// Usage
function App() {
  const isOnline = useOnlineStatus();

  return (
    <div>
      {!isOnline && (
        <div className="offline-banner">
          You are currently offline. Some features may be limited.
        </div>
      )}
      {/* Rest of app */}
    </div>
  );
}
```

### 7. Push Notifications Hook
```javascript
// src/hooks/usePushNotifications.js
import { useState, useEffect } from 'react';

function usePushNotifications() {
  const [permission, setPermission] = useState(Notification.permission);
  const [subscription, setSubscription] = useState(null);

  useEffect(() => {
    if ('serviceWorker' in navigator && 'PushManager' in window) {
      navigator.serviceWorker.ready.then(registration => {
        registration.pushManager.getSubscription().then(sub => {
          setSubscription(sub);
        });
      });
    }
  }, []);

  const requestPermission = async () => {
    const result = await Notification.requestPermission();
    setPermission(result);
    return result;
  };

  const subscribe = async (vapidPublicKey) => {
    if (permission !== 'granted') {
      const result = await requestPermission();
      if (result !== 'granted') return null;
    }

    const registration = await navigator.serviceWorker.ready;
    const sub = await registration.pushManager.subscribe({
      userVisibleOnly: true,
      applicationServerKey: urlBase64ToUint8Array(vapidPublicKey)
    });

    setSubscription(sub);
    return sub;
  };

  const unsubscribe = async () => {
    if (subscription) {
      await subscription.unsubscribe();
      setSubscription(null);
    }
  };

  return { permission, subscription, requestPermission, subscribe, unsubscribe };
}

function urlBase64ToUint8Array(base64String) {
  const padding = '='.repeat((4 - base64String.length % 4) % 4);
  const base64 = (base64String + padding).replace(/\-/g, '+').replace(/_/g, '/');
  const rawData = window.atob(base64);
  const outputArray = new Uint8Array(rawData.length);
  for (let i = 0; i < rawData.length; ++i) {
    outputArray[i] = rawData.charCodeAt(i);
  }
  return outputArray;
}

export default usePushNotifications;
```

---

## Next.js PWA

### 1. Install next-pwa
```bash
npm install next-pwa
```

### 2. Configure next.config.js
```javascript
const withPWA = require('next-pwa')({
  dest: 'public',
  register: true,
  skipWaiting: true,
  disable: process.env.NODE_ENV === 'development',
  runtimeCaching: [
    {
      urlPattern: /^https:\/\/fonts\.(?:gstatic)\.com\/.*/i,
      handler: 'CacheFirst',
      options: {
        cacheName: 'google-fonts-webfonts',
        expiration: {
          maxEntries: 4,
          maxAgeSeconds: 365 * 24 * 60 * 60 // 365 days
        }
      }
    },
    {
      urlPattern: /^https:\/\/fonts\.(?:googleapis)\.com\/.*/i,
      handler: 'StaleWhileRevalidate',
      options: {
        cacheName: 'google-fonts-stylesheets',
        expiration: {
          maxEntries: 4,
          maxAgeSeconds: 7 * 24 * 60 * 60 // 7 days
        }
      }
    },
    {
      urlPattern: /\.(?:eot|otf|ttc|ttf|woff|woff2|font.css)$/i,
      handler: 'StaleWhileRevalidate',
      options: {
        cacheName: 'static-font-assets',
        expiration: {
          maxEntries: 4,
          maxAgeSeconds: 7 * 24 * 60 * 60 // 7 days
        }
      }
    },
    {
      urlPattern: /\.(?:jpg|jpeg|gif|png|svg|ico|webp)$/i,
      handler: 'StaleWhileRevalidate',
      options: {
        cacheName: 'static-image-assets',
        expiration: {
          maxEntries: 64,
          maxAgeSeconds: 24 * 60 * 60 // 24 hours
        }
      }
    },
    {
      urlPattern: /\/_next\/image\?url=.+$/i,
      handler: 'StaleWhileRevalidate',
      options: {
        cacheName: 'next-image',
        expiration: {
          maxEntries: 64,
          maxAgeSeconds: 24 * 60 * 60 // 24 hours
        }
      }
    },
    {
      urlPattern: /\.(?:mp3|wav|ogg)$/i,
      handler: 'CacheFirst',
      options: {
        rangeRequests: true,
        cacheName: 'static-audio-assets',
        expiration: {
          maxEntries: 32,
          maxAgeSeconds: 24 * 60 * 60 // 24 hours
        }
      }
    },
    {
      urlPattern: /\.(?:mp4)$/i,
      handler: 'CacheFirst',
      options: {
        rangeRequests: true,
        cacheName: 'static-video-assets',
        expiration: {
          maxEntries: 32,
          maxAgeSeconds: 24 * 60 * 60 // 24 hours
        }
      }
    },
    {
      urlPattern: /\.(?:js)$/i,
      handler: 'StaleWhileRevalidate',
      options: {
        cacheName: 'static-js-assets',
        expiration: {
          maxEntries: 32,
          maxAgeSeconds: 24 * 60 * 60 // 24 hours
        }
      }
    },
    {
      urlPattern: /\.(?:css|less)$/i,
      handler: 'StaleWhileRevalidate',
      options: {
        cacheName: 'static-style-assets',
        expiration: {
          maxEntries: 32,
          maxAgeSeconds: 24 * 60 * 60 // 24 hours
        }
      }
    },
    {
      urlPattern: /\/_next\/data\/.+\/.+\.json$/i,
      handler: 'StaleWhileRevalidate',
      options: {
        cacheName: 'next-data',
        expiration: {
          maxEntries: 32,
          maxAgeSeconds: 24 * 60 * 60 // 24 hours
        }
      }
    },
    {
      urlPattern: /\/api\/.*/i,
      handler: 'NetworkFirst',
      method: 'GET',
      options: {
        cacheName: 'apis',
        expiration: {
          maxEntries: 16,
          maxAgeSeconds: 24 * 60 * 60 // 24 hours
        },
        networkTimeoutSeconds: 10
      }
    },
    {
      urlPattern: /.*/i,
      handler: 'NetworkFirst',
      options: {
        cacheName: 'others',
        expiration: {
          maxEntries: 32,
          maxAgeSeconds: 24 * 60 * 60 // 24 hours
        },
        networkTimeoutSeconds: 10
      }
    }
  ]
});

/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
};

module.exports = withPWA(nextConfig);
```

### 3. Create Manifest (`public/manifest.json`)
```json
{
  "name": "Next.js PWA",
  "short_name": "NextPWA",
  "description": "A Next.js Progressive Web Application",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "icons": [
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any maskable"
    }
  ]
}
```

### 4. Update _document.js
```javascript
// pages/_document.js
import { Html, Head, Main, NextScript } from 'next/document';

export default function Document() {
  return (
    <Html lang="en">
      <Head>
        <link rel="manifest" href="/manifest.json" />
        <link rel="icon" href="/favicon.ico" />
        <link rel="apple-touch-icon" href="/icons/icon-192x192.png" />
        <meta name="theme-color" content="#000000" />
        <meta name="description" content="Next.js PWA Application" />
        <meta name="apple-mobile-web-app-capable" content="yes" />
        <meta name="apple-mobile-web-app-status-bar-style" content="black" />
        <meta name="apple-mobile-web-app-title" content="NextPWA" />
      </Head>
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}
```

### 5. Custom App Component with Install Prompt
```javascript
// pages/_app.js
import { useEffect, useState } from 'react';
import '../styles/globals.css';

function MyApp({ Component, pageProps }) {
  const [installPrompt, setInstallPrompt] = useState(null);
  const [showInstallBanner, setShowInstallBanner] = useState(false);

  useEffect(() => {
    const handler = (e) => {
      e.preventDefault();
      setInstallPrompt(e);
      setShowInstallBanner(true);
    };

    window.addEventListener('beforeinstallprompt', handler);

    return () => window.removeEventListener('beforeinstallprompt', handler);
  }, []);

  const handleInstall = async () => {
    if (!installPrompt) return;

    installPrompt.prompt();
    const { outcome } = await installPrompt.userChoice;

    if (outcome === 'accepted') {
      console.log('PWA installed');
    }

    setInstallPrompt(null);
    setShowInstallBanner(false);
  };

  return (
    <>
      {showInstallBanner && (
        <div className="install-banner">
          <p>Install this app for a better experience!</p>
          <button onClick={handleInstall}>Install</button>
          <button onClick={() => setShowInstallBanner(false)}>Dismiss</button>
        </div>
      )}
      <Component {...pageProps} />
    </>
  );
}

export default MyApp;
```

### 6. Offline Page
```javascript
// pages/offline.js
export default function Offline() {
  return (
    <div style={{ padding: '2rem', textAlign: 'center' }}>
      <h1>You are currently offline</h1>
      <p>Please check your internet connection and try again.</p>
    </div>
  );
}
```

### 7. Advanced PWA Features Component
```javascript
// components/PWAFeatures.jsx
import { useEffect, useState } from 'react';

export default function PWAFeatures() {
  const [isOnline, setIsOnline] = useState(true);
  const [isPWA, setIsPWA] = useState(false);
  const [platform, setPlatform] = useState('browser');

  useEffect(() => {
    // Check if running as PWA
    const isStandalone = window.matchMedia('(display-mode: standalone)').matches;
    setIsPWA(isStandalone);

    // Detect platform
    const userAgent = navigator.userAgent.toLowerCase();
    if (/iphone|ipad|ipod/.test(userAgent)) {
      setPlatform('iOS');
    } else if (/android/.test(userAgent)) {
      setPlatform('Android');
    } else if (/win/.test(userAgent)) {
      setPlatform('Windows');
    } else if (/mac/.test(userAgent)) {
      setPlatform('macOS');
    } else if (/linux/.test(userAgent)) {
      setPlatform('Linux');
    }

    // Online/offline detection
    const handleOnline = () => setIsOnline(true);
    const handleOffline = () => setIsOnline(false);

    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);

    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);

  return (
    <div className="pwa-info">
      <p>Status: {isOnline ? '🟢 Online' : '🔴 Offline'}</p>
      <p>Mode: {isPWA ? '📱 PWA' : '🌐 Browser'}</p>
      <p>Platform: {platform}</p>
    </div>
  );
}
```

### 8. API Route with Caching Headers
```javascript
// pages/api/data.js
export default function handler(req, res) {
  // Set cache control headers
  res.setHeader('Cache-Control', 's-maxage=60, stale-while-revalidate');
  
  res.status(200).json({
    data: 'Your data here',
    timestamp: new Date().toISOString()
  });
}
```

---

## Desktop PWA Features

### Window Controls Overlay (Chromium-based browsers)
```json
// manifest.json
{
  "display_override": ["window-controls-overlay"],
  "theme_color": "#2196F3"
}
```

```css
/* styles.css */
/* Title bar drag region */
.title-bar {
  app-region: drag;
  -webkit-app-region: drag;
  height: 32px;
}

/* Interactive elements in title bar */
.title-bar button {
  app-region: no-drag;
  -webkit-app-region: no-drag;
}

/* Handle window controls overlay */
@media (display-mode: window-controls-overlay) {
  .title-bar {
    position: fixed;
    top: env(titlebar-area-y, 0);
    height: env(titlebar-area-height, 32px);
    width: env(titlebar-area-width, 100%);
    left: env(titlebar-area-x, 0);
  }
}
```

### File Handling
```json
// manifest.json
{
  "file_handlers": [
    {
      "action": "/open-file",
      "accept": {
        "text/plain": [".txt"],
        "application/json": [".json"],
        "image/png": [".png"],
        "image/jpeg": [".jpg", ".jpeg"]
      }
    }
  ]
}
```

```javascript
// Handle file opening
if ('launchQueue' in window) {
  window.launchQueue.setConsumer((launchParams) => {
    if (launchParams.files && launchParams.files.length) {
      launchParams.files.forEach(async (fileHandle) => {
        const file = await fileHandle.getFile();
        // Process the file
        console.log('Opened file:', file.name);
      });
    }
  });
}
```

### Protocol Handling
```json
// manifest.json
{
  "protocol_handlers": [
    {
      "protocol": "web+myapp",
      "url": "/handle-protocol?url=%s"
    }
  ]
}
```

### Share Target (Receive Shares)
```json
// manifest.json
{
  "share_target": {
    "action": "/share",
    "method": "POST",
    "enctype": "multipart/form-data",
    "params": {
      "title": "title",
      "text": "text",
      "url": "url",
      "files": [
        {
          "name": "media",
          "accept": ["image/*", "video/*"]
        }
      ]
    }
  }
}
```

### Keyboard Shortcuts
```javascript
// Add keyboard shortcuts
document.addEventListener('keydown', (e) => {
  // Ctrl/Cmd + K
  if ((e.ctrlKey || e.metaKey) && e.key === 'k') {
    e.preventDefault();
    openCommandPalette();
  }
  
  // Ctrl/Cmd + N
  if ((e.ctrlKey || e.metaKey) && e.key === 'n') {
    e.preventDefault();
    createNewDocument();
  }
});
```

### Badge API (Desktop)
```javascript
// Set badge count
if ('setAppBadge' in navigator) {
  navigator.setAppBadge(5); // Show badge with number
  // navigator.setAppBadge(); // Show badge without number
  // navigator.clearAppBadge(); // Clear badge
}
```

---

## Mobile PWA Features

### Pull to Refresh (Disable)
```css
/* Disable pull-to-refresh on mobile */
body {
  overscroll-behavior-y: contain;
}
```

### Add to Home Screen Prompt
```javascript
// Mobile-specific install prompt
let installPrompt;

window.addEventListener('beforeinstallprompt', (e) => {
  e.preventDefault();
  installPrompt = e;
  
  // Show custom install UI
  showMobileInstallButton();
});

function showMobileInstallButton() {
  const installBtn = document.getElementById('mobile-install');
  installBtn.style.display = 'block';
  
  installBtn.addEventListener('click', async () => {
    if (!installPrompt) return;
    
    installPrompt.prompt();
    const { outcome } = await installPrompt.userChoice;
    
    console.log(`Install outcome: ${outcome}`);
    installPrompt = null;
    installBtn.style.display = 'none';
  });
}
```

### iOS Specific Meta Tags
```html
<!-- iOS specific -->
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="My PWA">
<link rel="apple-touch-icon" href="/icons/apple-touch-icon.png">
<link rel="apple-touch-startup-image" href="/splash.png">

<!-- iOS splash screens for different devices -->
<link rel="apple-touch-startup-image" 
      media="(device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3)"
      href="/splash/iphone-x.png">
<link rel="apple-touch-startup-image" 
      media="(device-width: 390px) and (device-height: 844px) and (-webkit-device-pixel-ratio: 3)"
      href="/splash/iphone-12.png">
```

### Web Share API (Mobile)
```javascript
// Share content from your app
async function shareContent() {
  if (navigator.share) {
    try {
      await navigator.share({
        title: 'Check this out!',
        text: 'Amazing content to share',
        url: window.location.href
      });
      console.log('Content shared successfully');
    } catch (err) {
      console.log('Error sharing:', err);
    }
  } else {
    // Fallback for browsers without Web Share API
    copyToClipboard(window.location.href);
  }
}

// Share files (images, videos, etc.)
async function shareFile(file) {
  if (navigator.canShare && navigator.canShare({ files: [file] })) {
    try {
      await navigator.share({
        files: [file],
        title: 'Shared File',
        text: 'Check out this file'
      });
    } catch (err) {
      console.log('Error sharing file:', err);
    }
  }
}
```

### Touch Gestures
```javascript
// Handle touch gestures
let touchStartX = 0;
let touchEndX = 0;

function handleGesture() {
  if (touchEndX < touchStartX - 50) {
    // Swipe left
    console.log('Swiped left');
  }
  if (touchEndX > touchStartX + 50) {
    // Swipe right
    console.log('Swiped right');
  }
}

document.addEventListener('touchstart', e => {
  touchStartX = e.changedTouches[0].screenX;
});

document.addEventListener('touchend', e => {
  touchEndX = e.changedTouches[0].screenX;
  handleGesture();
});
```

### Vibration API
```javascript
// Vibrate device
if ('vibrate' in navigator) {
  // Vibrate for 200ms
  navigator.vibrate(200);
  
  // Pattern: vibrate 100ms, pause 50ms, vibrate 100ms
  navigator.vibrate([100, 50, 100]);
  
  // Stop vibration
  navigator.vibrate(0);
}

// Haptic feedback on button click
button.addEventListener('click', () => {
  if ('vibrate' in navigator) {
    navigator.vibrate(10); // Short feedback
  }
});
```

### Screen Orientation API
```javascript
// Lock screen orientation
async function lockOrientation() {
  try {
    await screen.orientation.lock('portrait');
    console.log('Orientation locked to portrait');
  } catch (err) {
    console.log('Orientation lock failed:', err);
  }
}

// Unlock orientation
function unlockOrientation() {
  screen.orientation.unlock();
}

// Listen for orientation changes
screen.orientation.addEventListener('change', () => {
  console.log(`Orientation: ${screen.orientation.type}`);
});
```

### Wake Lock API (Keep screen on)
```javascript
// Keep screen awake
let wakeLock = null;

async function requestWakeLock() {
  try {
    wakeLock = await navigator.wakeLock.request('screen');
    console.log('Wake lock acquired');
    
    wakeLock.addEventListener('release', () => {
      console.log('Wake lock released');
    });
  } catch (err) {
    console.log('Wake lock error:', err);
  }
}

async function releaseWakeLock() {
  if (wakeLock) {
    await wakeLock.release();
    wakeLock = null;
  }
}

// Re-acquire wake lock when page becomes visible
document.addEventListener('visibilitychange', async () => {
  if (document.visibilityState === 'visible' && wakeLock === null) {
    await requestWakeLock();
  }
});
```

### Contact Picker API
```javascript
// Pick contacts (Chrome Android)
async function pickContact() {
  if ('contacts' in navigator && 'ContactsManager' in window) {
    try {
      const props = ['name', 'email', 'tel'];
      const contacts = await navigator.contacts.select(props, { multiple: false });
      
      if (contacts.length > 0) {
        console.log('Selected contact:', contacts[0]);
      }
    } catch (err) {
      console.log('Contact picker error:', err);
    }
  }
}
```

---

## Testing & Debugging

### Chrome DevTools
```javascript
// Check PWA status in Chrome DevTools
// 1. Open DevTools (F12)
// 2. Go to Application tab
// 3. Check:
//    - Manifest: View manifest.json details
//    - Service Workers: Status, update, unregister
//    - Storage: Cache, IndexedDB, Local Storage
//    - Clear site data: Remove all caches and storage
```

### Lighthouse PWA Audit
```bash
# Install Lighthouse CLI
npm install -g lighthouse

# Run PWA audit
lighthouse https://your-site.com --view

# Run specific PWA checks
lighthouse https://your-site.com --only-categories=pwa --view
```

### Testing Service Worker Updates
```javascript
// Force service worker update
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.getRegistrations().then(registrations => {
    registrations.forEach(registration => {
      registration.update();
    });
  });
}

// Skip waiting and activate new service worker immediately
self.addEventListener('message', event => {
  if (event.data.action === 'skipWaiting') {
    self.skipWaiting();
  }
});

// Client-side: Send skip waiting message
navigator.serviceWorker.controller.postMessage({ action: 'skipWaiting' });
```

### Debug Mode
```javascript
// Enable verbose service worker logging
self.addEventListener('install', event => {
  console.log('[SW] Installing:', event);
});

self.addEventListener('activate', event => {
  console.log('[SW] Activating:', event);
});

self.addEventListener('fetch', event => {
  console.log('[SW] Fetching:', event.request.url);
});
```

### PWA Checklist
- [ ] HTTPS enabled
- [ ] Valid manifest.json
- [ ] Icons (192x192, 512x512)
- [ ] Service worker registered
- [ ] Offline page/functionality
- [ ] Installable (passes A2HS criteria)
- [ ] Responsive design
- [ ] Fast loading (< 3s)
- [ ] Cross-browser tested
- [ ] Meta tags for iOS
- [ ] Theme color defined
- [ ] Display mode configured
- [ ] Start URL configured
- [ ] Cache strategy implemented

### Testing Tools
```bash
# Test on real devices
# 1. Expose local server on network
npx serve -l 3000

# 2. Find local IP
ipconfig  # Windows
ifconfig  # Mac/Linux

# 3. Access from mobile: http://YOUR_IP:3000

# Use ngrok for external testing
npx ngrok http 3000

# PWA Builder - Test and package PWA
# Visit: https://www.pwabuilder.com/
```

### Common Issues & Solutions

**Service Worker not updating:**
```javascript
// Solution: Update cache name
const CACHE_NAME = 'my-cache-v2'; // Increment version
```

**iOS not showing install prompt:**
```
// iOS doesn't support beforeinstallprompt
// Users must manually "Add to Home Screen" from Safari menu
// Provide instructions for iOS users
```

**Mixed content errors:**
```
// Ensure all resources use HTTPS
// Check console for mixed content warnings
```

**Cache not clearing:**
```javascript
// Clear all caches
caches.keys().then(keys => {
  keys.forEach(key => caches.delete(key));
});
```

### Performance Monitoring
```javascript
// Measure cache performance
const startTime = performance.now();
caches.match(request).then(response => {
  const endTime = performance.now();
  console.log(`Cache lookup took ${endTime - startTime}ms`);
});

// Track service worker events
let swUpdateTime = null;
navigator.serviceWorker.addEventListener('controllerchange', () => {
  swUpdateTime = performance.now();
  console.log('Service worker updated at:', swUpdateTime);
});
```

---

## Additional Resources

### Documentation
- [MDN PWA Guide](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)
- [web.dev PWA](https://web.dev/progressive-web-apps/)
- [Google Workbox](https://developers.google.com/web/tools/workbox)
- [PWA Builder](https://www.pwabuilder.com/)

### Tools
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [Workbox](https://developers.google.com/web/tools/workbox)
- [PWA Asset Generator](https://github.com/elegantapp/pwa-asset-generator)
- [Maskable.app](https://maskable.app/) - Icon generator

### Icon Sizes Required
- **Favicon**: 16x16, 32x32, 48x48
- **Android**: 192x192 (minimum), 512x512 (recommended)
- **iOS**: 120x120, 152x152, 167x167, 180x180
- **Windows**: 70x70, 150x150, 310x310
- **Maskable**: 192x192, 512x512 (with safe zone)

---

**Best Practices:**
1. Always test on real devices (iOS, Android, Desktop)
2. Implement proper error handling for offline scenarios
3. Use cache versioning to manage updates
4. Provide fallback for unsupported features
5. Monitor performance with analytics
6. Keep service worker file size small
7. Test install/uninstall flows
8. Handle app updates gracefully
9. Optimize for both mobile and desktop experiences
10. Follow platform-specific guidelines (iOS, Android, Windows)
