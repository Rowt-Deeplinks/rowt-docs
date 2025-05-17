


## Use Rowt

If you want to get started fast, you can use Rowt's API service and connect your app with the SDK. 

1. [Create an account](https://console.rowt.app)

2. Create your first project

3. Copy your Project ID and API Key from the project configuration page

4. In your React Native or Expo app:
```bash
npm install rowt-sdk
```
[Full SDK Docs](/app-sdk/) - 
[view on npm](https://npmjs.com/rowt-sdk)

5. For mobile apps, follow the SDK Integration guide for your framework
- [Expo](/app-sdk/?id=expo-integration-guide)
- [React Native](/app-sdk/?id=react-native-cli-integration-guide)

6. Start creating links to your app's content


[Rowt's Pricing](https://console.rowt.app/pricing)

<br>
<br>
<br>

## Self-Host
### Alternatively, you can self-host on your own infrastructure for more control over your API's behavior and costs.
We're an open source project, and this serves as a free or low cost way for us to make deep linking simpler :)

[Self-Hosting a Rowt instance](/self-host/)

Self-hosting gives you the option to run your instance in single-tenant mode, quickly deploy with SQLite, or use your own Postgres database (Postgres recommended for most production uses)


<!-- # Console SDK

When deploying your own Rowt Server, the same API we use on the [Rowt Console](https://console.rowt.app) is accessible for anyone to use through our Console SDK. 

```bash
npm install rowt-console-sdk
```


```typescript
const sdk = new RowtConsole('https://your.server.url')

const user = await sdk.login('myserverusername', 'myserverpassword')

// Then you're free to use any method
// For example - Get all links and interactions for your project

const project = sdk.getProjectById('your-project-uuid')

```

[Complete Console SDK Docs](/console-sdk) -->


