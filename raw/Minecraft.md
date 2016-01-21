title: Minecraft
category: Gaming
---

### Add custom logo to Minecraft Server

Resize the image to 64x64 and convert it to .png format, then save it as `server-icon.png` to the Minecraft root directory.

### Text format in Server MOTD

Use unicode instead of `&` in server.properties to get formatting codes working.

```
\u00A70 - black
\u00A71 - dark blue
\u00A72 - dark green
\u00A73 - dark aqua
\u00A74 - dark red
\u00A75 - dark purple
\u00A76 - gold
\u00A77 - gray
\u00A78 - dark gray
\u00A79 - indigo
\u00A7a - green
\u00A7b - aqua
\u00A7c - red
\u00A7d - pink
\u00A7e - yellow
\u00A7f - white
\u00A7k - obfuscated
\u00A7l - bold
\u00A7m - strikethrough
\u00A7n - underline
\u00A7o - italic
\u00A7r - reset
```

### Get all player skins from whitelist

```javascript
'use strict';

/*
* Script to get all player skins from your server whitelist.
* $ npm install request
* $ mkdir -p ./skins/Alex ./skins/Steve
* $ node getskins.js
**/

var fs = require('fs');
var request = require('request');

var players = JSON.parse(fs.readFileSync('./whitelist.json', 'utf8'));

players.forEach(function(p) {
	var uuid = p.uuid.replace(/-/g, '');
	console.log('Request uuid ' + p.uuid + ' for player ' + p.name);
	request('https://sessionserver.mojang.com/session/minecraft/profile/' + uuid, function (error, response, body) {
		if (!error && response.statusCode == 200) {
			var data = JSON.parse(body);
			data.properties.forEach(function(t) {
				if (t.name === 'textures') {
					var texture = JSON.parse(new Buffer(t.value, 'base64').toString('ascii'));
					if (texture.textures.SKIN) {
						var skinUrl = texture.textures.SKIN.url;
						// if skin is Alex model
						if (texture.textures.SKIN.metadata && texture.textures.SKIN.metadata.model === 'slim') {
							request(skinUrl)
							  	.on('error', function(err) {
							  		console.error('Error requesting skin for player ' + data.name + ': ' + err)
							  	})
							  	.pipe(fs.createWriteStream('./skins/Alex/' + data.name + '.png'));
						} else {
							request(skinUrl)
								.on('error', function(err) {
							  		console.error('Error requesting skin for player ' + data.name + ': ' + err)
							  	})
								.pipe(fs.createWriteStream('./skins/Steve/' + data.name + '.png'));
						}
						console.log('Skin for player ' + data.name + ' saved.');
					} else {
						console.log('Skin for player ' + data.name + ' Not Found.');
					}
				}
			});
		}
	});
});

```

References:

1. <http://www.spigotmc.org/threads/how-to-add-a-server-icon-to-your-server-1-7-10.6564/>
2. <http://www.minecraftforum.net/forums/support/server-support/tutorials-and-faqs/1940468-how-to-add-colour-to-your-server-motd>
3. <http://wiki.vg/Mojang_API>

