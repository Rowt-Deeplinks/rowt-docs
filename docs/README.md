# What is Rowt?
Deep links are links created to open native apps from web URLs — an issue that (somehow) still doesn’t have a simple solution without costing you an inconvenient amount of money.

#### Building deep links yourself typically requires:
- **A hosted API** *– to handle link creation, redirection, and tracking*
- **A database** *– to store link metadata, analytics, expiration rules*
- **Unique ID generation** *- to give each deeplink a unique web link*
- **Native-level intent handling** *– to your app when a link is tapped*
- **Routing logic** *– to navigate to the right content when a link was used to open the app*
- **Security layers** *– to prevent spoofing, tampering, and waking up to runaway usage bills*
- **Social metadata support** *– so links preview correctly on platforms like Twitter and iMessage*
- **Analytics dashboards** *– to measure what’s working*
- **Cleanup chronjobs** *– to expire or remove outdated links*


It’s an absurd amount of moving parts for something as simple as sharing a link.

#### Rowt simplifies everything into a few focused components.
If you’re using our managed service, all you need is the App SDK — everything else is handled for you.
If you’re self-hosting to save costs or customize behavior, you get a modular, production-ready stack with:
- A clean, intuitive API
- Preconfigured database schema
- A prebuilt unique id generator for postgres
- Auto-generated metadata previews
- Built-in analytics and security
- Fallback links for your app store listings and website
- Easy-to-configure native integrations

Seriously, the hardest part about this is copy-pasting some native code into your ios and android builds. And you don't even have to think about that if you're using Expo.

Whether you use our managed service or host it yourself, Rowt gives you the tools to integrate reliable deep links in minutes — not weeks.

[Get started with our managed service](/get-started/)

[Self-host your own Rowt Infrastructure](/self-host/)




