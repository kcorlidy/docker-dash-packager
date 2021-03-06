# Description
A Docker container for an open source MPEG DASH packager featuring

 - Single period MPEG DASH for VOD and Live
 - Multi period MPEG DASH for VOD and Live

The MPEG DASH packager is built on:

 - Ubuntu 
 - Apache2 (https://httpd.apache.org)
 - ffmpeg (https://ffmpeg.org)
 - Bento4 (https://www.bento4.com)
 - hls2dash (https://pypi.python.org/pypi/hls2dash)
 - nodejs-express (for the admin UI)

|      | Input           | Output                  | Supported |
| ---- | --------------- | ----------------------- | --------- |
| Live | HLS             | Single Period MPEG-DASH | Yes       |
| Live | HLS EXT-CUE     | Multi Period MPEG-DASH  | Yes       |
| VOD  | MP4             | Single Period MPEG-DASH | Planned   |
| VOD  | MP4 w cue sheet | Multi Period MPEG-DASH  | Planned   |

# Deploying DASH Packager

## Running on localhost

Start DASH packager listening on port 3000:

    docker run -d -p 3000:80 --restart=always --name packager eyevinntechnology/packager:0.1.2

Verify it is up and running by entering http://localhost:3000/ in your web browser and you would see something like this:

![](screenshot-packager.png)

### Testing with encoder
Now configure an encoder to post HLS to:

    http://localhost:3000/ingest/event/

### Testing without encoder
   
If you don't have an encoder you can test the packager with the [hls-relay](https://github.com/Eyevinn/hls-relay) script. First install it:

	pip install hlsrelay
	
Then run it:

	hls-relay http://example.com/event/master.m3u8 http://localhost:3000/ingest/event/

### Playing
And you would then access the HLS here

    http://localhost:3000/live/event/master.m3u8

which should be playable in a Safari web browser.

The MPEG DASH stream is found here

    http://localhost:3000/live/event.mpd/manifest.mpd

and a multi period MPEG DASH variant is found here
    
    http://localhost:3000/live/event.mpd/multi.mpd

You can also see the available streams in the admin page, in this case on http://localhost:3000/admin/. Default username is "admin" and password is "eyevinntechnology"

![](screenshot-admin.png)

To stop the packager you run:

    docker stop packager && docker rm -v registry packager

## Using Docker Compose

You could also use a Docker compose config:

```
packager:
  restart: always
  image: eyevinntechnology/packager:0.1.2
  ports:
    - 3000:80 
  volumes:
    - /tmp/data:/data
```

And simply start it by running:

    docker-compose up -d

Shut down the packager by running:

    docker-compose down

# License

Licensed under The MIT License (MIT)

Copyright (c) 2016 Eyevinn Technology

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
