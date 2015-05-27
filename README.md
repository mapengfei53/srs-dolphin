# srs-dolphin

The dolphin is a MultipleProcess for [SRS][SRS].

## About

The dolphin is [the most simple MultipleProcess arch][ARCH] for RTMP/HTTP-FLV of SRS.

The [srs-sharp][SHARP] is a similar project, but srs-sharp is a HTTP FLV reverse proxy, its performance is great and is's ok for HTTP FLV stream. The bellow table is a comparation between srs-sharp and srs-dolphin:

|   Project   |   Goal        |   Protocol    |   Performance   |   Deploy    |
|   -------   |   -----       |   --------    |   -----------   |   -------   |
|   srs-sharp | HTTP-FLV      |   HTTP        |   10k, 300%CPU  |   Manual(*) |
| srs-dolphin | RTMP/HTTP-FLV |   TCP         |   -             |   Auto(*)   |

Remark:

1. **Manual** Deploy of srs-sharp: User should manually deply the SRS edge, then start srs-sharp to proxy the ports of SRS.
1. **Auto** Deploy of srs-dolphin: User only need to start srs-dophin, which will auto manage the SRS.

## Usage

** Step 1: ** Prepare SRS:

```
cd ~ && git clone https://github.com/simple-rtmp-server/srs.git &&
cd ~/srs/trunk && ./configure --with-ffmpeg && make && ./objs/srs -c conf/srs.conf
```

** Step 2: ** Clone the srs-dolphin:

```
cd ~ && git clone https://github.com/simple-rtmp-server/srs-dolphin.git
```

** Step 3: ** Build the srs-dolphin:

```
cd ~/srs-dolphin/trunk && make
```

** Step 4: ** Run dolphin

```
cd ~/srs-dolphin/trunk && 
./objs/srs_dolphin -p 19350 -w 4 -s 1936,1937,1938,1939 -b ../../srs/trunk/objs/srs -c conf/srs.conf
```

** Step 5: ** Publish stream

```
cd ~/srs/trunk &&
./objs/ffmpeg/bin/ffmpeg -re -i ./doc/source.200kbps.768x320.flv \
    -vcodec copy -acodec copy -f flv -y rtmp://127.0.0.1:1935/live/livestream
```

** Step 6: ** Play stream

```
Origin SRS stream: rtmp://127.0.0.1:1935/live/livestream
Edge Dolphin stream: rtmp://127.0.0.1:19350/live/livestream
```

Winlin 2015.5

[SRS]: https://github.com/simple-rtmp-server/srs
[ARCH]: https://github.com/simple-rtmp-server/srs/wiki/v3_CN_Architecture#multiple-processes-planb
[SHARP]: https://github.com/simple-rtmp-server/go-sharp
