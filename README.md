# Arc

Arc is a manager for your secrets made of `arcd`, a RESTful API server written in Go which exposes read and write primitives for **encrypted records** on a sqlite database file.

![arcd](https://i.imgur.com/swC00gX.png)

And `arc`, the client application implemented in html5 and javascript, which runs in every html5 enabled browser and  it is served by `arcd` itself.

![multikey](https://pbs.twimg.com/media/DQN8W1KWsAEP6bd.jpg:large)

Records are generated, encrypted and decrypted **client side only** (Arc relies on CryptoJS for its AES encryption and the PRNG) by `arc`, which offers an intuitive management system equipped with UI widgets including:

- Simple text inputs.
- Simple text areas.
- Custom file attachments (**files are encrypted client side** before being uploaded as binary records).
- A markdown editor area with preview and full screen mode.
- A password field with **password strength estimation** and a **random password generator**. 

Elements can be created (with optional expiration dates), arranged and edited using `arc` and are stored on `arcd` safely.

||||
| ------------- | -- |-------------- |
| ![ui mix 1](https://i.imgur.com/KCn4RGw.png)  |  | ![ui mix 2](https://i.imgur.com/nxqmRqY.png) |
| A client side encrypted record set to expire and self delete with a markdown area and a password widget. |  |  Markdown and various attached files. |

#### TL;DR 

The idea is to use *the Arc* as a single manager for your passwords, encrypted notes, files and `-all the secret things here-` while hosting `arcd` yourself on some spare hardware like a Raspberry Pi (a very portable setup with a `Zero W` model, or an USB Armory) or a real dedicated server and accessing `arc` from every device with a modern browser.

<p align="center">
    <img src="https://i.imgur.com/h5cpCeN.png" alt="Encrypt all the things!"/>
</p>

## Usage

Download, install dependencies and compile the `arcd` server component:

    git clone https://github.com/evilsocket/arc
    cd arc/arcd
    make vendor_get
    make

Now copy the `sample_config.json` file to a new `config.json` file, customize it and run the `arc` web application:

    ./arcd -config config.json -app ../arc

Browse `http://localhost:8080/` and login with the credentials you specified in the `config.json` file.

**Note**

The first time the `arcd` server executes the `arc` web application. it will automatically import some example stores from the `arc/seeds.json` seed file (encryption key is `vault`).

## Configuration

You will find a `sample_config.json` file inside the `arcd` folder of the project, this is the example configuration you need to customize the first time.

```json
{
    "address": "127.0.0.1",
    "port": 8080,
    "username": "arc",
    "password": "arc",
    "database": "~/arc.db",
    "token_duration": 60,
    "scheduler": {
        "enabled": true,
        "period": 10
    },
    "tls": {
        "enabled": false,
        "pem": "/some/file.pem",
        "key": "/some/file.key"
    }
}
```

It is necessary to change only the `username` and `password` access parameters of Arc, while the others can be left to their default values.

| Configuration | Description |
| ------------- | ------------- |
| address | IP address to bind the `arcd` server to. |
| port | TCP to bind the `arcd` server to. |
| username | API access username. |
| password | API access passwrd. |
| database | SQLite database file path. |
| token\_duration | Validity in minutes of a JWT API token after it's being generated. |
| scheduler.enabled | Enable the scheduling and pruning of expired records. |
| scheduler.period | Delay in seconds between one period and another of the scheduler. |
| tls.enabled | Run `arcd` on HTTPS. |
| tls.pem | HTTPS certificate. |
| tls.key | HTTPS private key. |

## Export and import stores.

You can export stores and their encrypted records to a JSON file:

    ./arcd -config config.json -output ~/backup.json -export

Or export only one store by its numeric id:

    ./arcd -config config.json -output ~/arc_store_1.json -export -store 1 

Such export files can be later imported with:

    ./arcd -config config.json -import ~/backup.json

## Bugs

Before opening an issue, please make sure it is not already part of [a known bug](https://github.com/evilsocket/arc/issues?q=is%3Aopen+is%3Aissue+label%3Abug).

## License

Arc was made with ♥  by [Simone Margaritelli](https://www.evilsocket.net/) and it is released under the GPL 3 license.

