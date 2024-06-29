# Radio Player Icecast/Shoutcast with REST API Now Playing and PWA Support

This PHP project displays information about songs playing on Icecast/Shoutcast radio streams, including history, album art, lyrics, and Progressive Web App (PWA) support. The project offers flexibility to be used with either the local API or the web API, allowing you to choose the option that best suits your needs. Additionally, it is possible to rename the index.php file to index.html to use the project exclusively with the web API.

## Demo Screenshots

![Demo Screenshot](https://i.imgur.com/oULEMgZ.jpeg)

## Features:

* Displays current song, artist, and album art.
* History of played songs.
* Cover art of the current song
* Fetches song lyrics using Vagalume API ([Vagalume API](https://api.vagalume.com.br/docs/))
* Responsive design for mobile devices.
* Now available as a Progressive Web App (PWA) for enhanced user experience!
* Support for multiple radio stations

## Requirements

* PHP 7.0 or higher
* Web server (e.g., Apache, Nginx)
* Enabled PHP extensions: `file_get_contents`, `json`, `stream_context_create`
* Internet access to fetch song metadata and album covers


## Installation

1. Clone the repository or download the files.

```bash
git clone https://github.com/jailsonsb2/RadioplayerAPI.git
```

2. Make sure you have a web server configured to serve PHP files. If you are using WAMP, place the files in the `www` folder.

3. Navigate to the project directory.


```bash
cd RadioplayerAPI
```

4. Make sure write permissions are set for the directory where the history will be saved.

5. Edit the <script> in the index.php

```javascript
<script>
            window.streams = {
                timeRefresh: 5000,
                stations: [
                    {
                        name: "Jailson Web Rádio",
                        hash: "jailson",
                        description: "Música sem parar",
                        logo: "assets/jailson_logo.png",
                        album:
                            "assets/jailson_cover.png",
                        cover:
                            "assets/jailson_cover.png",
                        api: "get_stream_title.php?url=https://stream.zeno.fm/yn65fsaurfhvv",
                        stream_url: "https://stream.zeno.fm/yn65fsaurfhvv",
                        tv_url: "https://eu1.servers10.com:2020/VideoPlayer/8106?autoplay=1",
                        server: "spotify",
                        program: {
                            time: "00:00",
                            name: "Jailson Web Radio",
                            description: "AO VIVO // ON AIR",
                        },
                        social: {
                            facebook: "https://facebook.com/",
                            twitter: "https://twitter.com/",
                            instagram: "https://www.instagram.com//",
                        },
                        apps: {
                            android: "#",
                            ios: "#",
                        },
                    },
                    {
                        name: "BENDICIÓN STEREO",
                        hash: "bendicion",
                        description: "Bendecidos para bendecir!",
                        logo: "assets/default.png",
                        album:
                            "assets/cover.png",
                        cover:
                            "assets/cover.png",
                        api: "get_stream_title.php?url=https://sv2.globalhostlive.com/proxy/bendistereo/stream2",
                        stream_url: "https://sv2.globalhostlive.com/proxy/bendistereo/stream2",
                        tv_url: "https://eu1.servers10.com:2020/VideoPlayer/8106?autoplay=1",
                        server: "spotify",
                        program: {
                            time: "11:00",
                            name: "Bendición Stereo",
                            description: "EN VIVO // ON AIR",
                        },
                        social: {
                            facebook: "https://facebook.com/BendicionStereo",
                            twitter: "https://twitter.com/BendiStereo",
                            instagram: "https://www.instagram.com/BendiStereo/",
                        },
                        apps: {
                            android: "#",
                            ios: "#",
                        },
                    },                    
                ],
            };
        </script>

 ```


## Configuration

Edit the `get_stream_title.php` file to add the allowed URLs of the radio streams.


```php
$allowedUrls = [
    'https://stream.zeno.fm/yn65fsaurfhvv',
    'https://sv2.globalhostlive.com/proxy/bendistereo/stream2',
    // Adicione outras URLs permitidas aqui
];
```

## Usage

The API can be called with the desired stream URL. For example:

```
http://yoursite/get_stream_title.php?url=https://stream.zeno.fm/yn65fsaurfhvv
```

The response will be a JSON in the following format:

```json
{
  "songtitle": "Ton Molinari - Eu Não Estou Só",
  "artist": "Ton Molinari",
  "song": "Eu Não Estou Só",
  "source": "https://stream.zeno.fm/yn65fsaurfhvv",
  "artwork": "https://exemplo.com/capa-album.jpg",
  "song_history": [
    {
      "song": {
        "title": "Música Anterior 1",
        "artist": "Artista Anterior 1"
      }
    },
    {
      "song": {
        "title": "Música Anterior 2",
        "artist": "Artista Anterior 2"
      }
    }
  ]
}
```

### Parameters

- `url`: The radio stream URL. Must be in the list of allowed URLs.
- `interval` (optional): Metadata reading interval (in bytes). Default is `19200`.

## Architecture

### Main Functions

- **getMp3StreamTitle($streamingUrl, $interval)**: Reads the metadata from the radio stream and returns the title of the current song.
- **extractArtistAndSong($title)**: Extracts the artist name and song title from the stream metadata.
- **getAlbumArt($artist, $song)**: Searches for the album cover on iTunes.
- **getHistoryFileName($url, $ignoreFirst)**: Generates the history file name based on the stream URL.
- **updateHistory($url, $artist, $song)**: Updates the history of played songs for a given stream.

## Error Handling

The API returns clear error messages in case of failures, for example:


```json
{
  "error": "Invalid URL"
}
```

ou

```json
{
  "error": "Failed to retrieve stream title"
}
```

## CORS Permissions

The CORS header is configured to allow all origins. This can be adjusted as needed:


```php
header('Access-Control-Allow-Origin: *');
```

## Contributing

1. Fork the project.
2. Create a branch for your feature (`git checkout -b feature/new-feature`).
3. Commit your changes (`git commit -am 'Add new feature'`).
4. Push to the branch (`git push origin feature/new-feature`).
5. Create a new Pull Request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

