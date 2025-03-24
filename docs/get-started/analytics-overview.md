# Analytics

Rowt analytics are very customizable, but the core of every interaction is the link.

## Link Properties

Every Link contains:

| Property               | Description                                      |
|------------------------|--------------------------------------------------|
| `id`                  | Unique identifier for each link.                 |
| `url`                 | The destination URL of the link.                |
| `title?`              | Title for link previews (optional).             |
| `description?`        | Description for link previews (optional).        |
| `image_url?`          | Image URL for link previews (optional).          |
| `fallback_url_override?` | Optional override of the project's default fallback links. |
| `additional_metadata?` | JSON of any custom additional metadata associated with the link. <br> ex: {"campaign": "instagram_valentines"}  |
| `properties`          | Properties associated with the link.            |
| `lifetime_clicks`     | Number of times the link has been clicked.       |
| `created_at`          | Timestamp when the link was created.            |



| Property       | Description                                      |
|----------------|--------------------------------------------------|
| `id`           | Unique UUID identifier for the interaction.|
| `link`           | Information about the link they clicked.|
| `url`          | The destination URL of the link. |
| `timestamp`   | Timestamp when the link was created.            |
| `referer`  | Referring location where the link was clicked.       |
| `country`         | The country of the clicking user.|
| `device`       | The device the user clicks from (iphone, desktop). |
| `os`       | The OS of the user (iOS, Android, Mac OS X). |