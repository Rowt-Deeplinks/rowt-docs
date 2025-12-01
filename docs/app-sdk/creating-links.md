# Creating Links

##### You can create links with the App SDK that redirect to a given deeplink or web link and track the interactions.

<br>

# Basics
> Let's generate a simple link that redirects to some web url.

```typescript
import {RowtLink} from 'rowt-sdk'

const config = {
    serverUrl: 'https://rowt.app', // or your self-hosted URL
    apiKey: 'your-api-key',
    projectId: 'your-project-id'
}

const linkOptions = {
    url: 'https://example.com'
}

const myLink: RowtLink = new RowtLink(config, linkOptions)

const shortlink: string = await myLink.createLink()

console.log(shortlink)

```

This should return something along the lines of `https://rowt.app/oIRlPWSd9r6D`.

By default, link previews will show the metadata at the destination.

<br>
<br>

# Generating Custom Metadata

> Let's write our own preview metadata for a link.

```typescript
...
const linkOptions = {
    url: 'https://example.com'
    title: 'Example Site'
    description: 'This link and metadata were generated with Rowt'
    imageUrl: 'https://picsum.photos/300/157'
}
...
```
When sent on social media (or chat apps that generate previews, etc), it should look something like this:


<hr>
<img src="_media/rowtPreviewImage.svg" alt="Example Site" title="Example Site" width="20%">

**Example Site**  
This link and metadata were generated with Rowt
<hr>

> [!NOTE]
> This is especially important for generating deep links.  
> If you want the preview to show the content from your app, you must overwrite the data. Otherwise, it will attempt to load metadata from your fallback URL.

In the case of web links, the original link's preview metadata will be used by default unless overwritten.

<br>
<br>

# Custom Shortcodes

> By default, Rowt generates a random 12-character shortcode for each link. You can optionally provide your own custom shortcode for memorable, branded links.

```typescript
...
const linkOptions = {
    url: 'https://example.com',
    customShortcode: 'promo2024' // 1-12 characters, alphanumeric, hyphens, and underscores
}
...
```

This will create a link like `https://rowt.app/promo2024` instead of `https://rowt.app/oIRlPWSd9r6D`.

**Requirements:**
- Must be 1-12 characters long
- Can only contain letters (a-z, A-Z), numbers (0-9), hyphens (-), and underscores (_)
- Must be unique across your entire server
- If the shortcode is already taken, the API will return a 400 error

> [!TIP]
> Custom shortcodes are great for marketing campaigns, QR codes, or any link you want to be easy to remember and type.

#
