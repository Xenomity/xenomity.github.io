---
title: "RTP Payload Type"
tags: [webrtc, rtp]
date: 2017-11-11 01:00:00
---

SDP 전문에서 협상 가능한 미디어 코덱에 대한 식별자로 RTP Payload Type 값을 사용한다. IANA 표준 규약은 고정 미디어 프로파일(0~95번), 동적 미디어 프로파일(96~127번)으로 분류하여 정의하고 있으며, 후자의 경우 중복되지 않는 미디어 프로파일 번호를 할당하고 협상할 수 있어야 한다.

*e.g., SDP*
```
// opus/48000/2 -> rtp payload type 111, dynamic range
a=rtpmap:111 opus/48000/2
a=rtcp-fb:111 transport-cc
a=fmtp:111 minptime=10;useinbandfec=1
// ISAC/16000 -> rtp payload type 103, dynamic range
a=rtpmap:103 ISAC/16000
// G7222/8000 -> rtp payload type 9, static range
a=rtpmap:9 G722/8000
// PCMU/8000 -> rtp payload type 0, static range
a=rtpmap:0 PCMU/8000
```

표 1. RTP Profile (출처: Wikipedia)

|Payload type (PT)|Name|Type|No. of channels|Clock rate (Hz)[note 1]|Frame size (ms)|Default packet size (ms)|Description|References|
|---|---|---|---|---|---|---|---|---|
|0|PCMU|audio|1|8000|any|20|ITU-T G.711 PCM µ-Law audio 64 kbit/s|RFC 3551|
|1|reserved (previously FS-1016 CELP)|audio|1|8000|||reserved, previously FS-1016 CELP audio 4.8 kbit/s|RFC 3551, previously RFC 1890|
|2|reserved (previously G721 or G726-32)|audio|1|8000|||reserved, previously ITU-T G.721 ADPCM audio 32 kbit/s or ITU-T G.726 audio 32 kbit/s|RFC 3551, previously RFC 1890|
|3|GSM|audio|1|8000|20|20|European GSM Full Rate audio 13 kbit/s (GSM 06.10)|RFC 3551|
|4|G723|audio|1|8000|30|30|ITU-T G.723.1 audio|RFC 3551|
|5|DVI4|audio|1|8000|any|20|IMA ADPCM audio 32 kbit/s|RFC 3551|
|6|DVI4|audio|1|16000|any|20|IMA ADPCM audio 64 kbit/s|RFC 3551|
|7|LPC|audio|1|8000|any|20|Experimental Linear Predictive Coding audio 5.6 kbit/s|RFC 3551|
|8|PCMA|audio|1|8000|any|20|ITU-T G.711 PCM A-Law audio 64 kbit/s|RFC 3551|
|9|G722|audio|1|8000[note 2]|any|20|ITU-T G.722 audio 64 kbit/s|RFC 3551 - Page 14|
|10|L16|audio|2|44100|any|20|Linear PCM 16-bit Stereo audio 1411.2 kbit/s,[2][3][4] uncompressed|RFC 3551, Page 27|
|11|L16|audio|1|44100|any|20|Linear PCM 16-bit audio 705.6 kbit/s, uncompressed|RFC 3551, Page 27|
|12|QCELP|audio|1|8000|20|20|Qualcomm Code Excited Linear Prediction|RFC 2658, RFC 3551|
|13|CN|audio|1|8000|||Comfort noise. Payload type used with audio codecs that do not support comfort noise as part of the codec itself such as G.711, G.722.1, G.722, G.726, G.727, G.728, GSM 06.10, Siren, and RTAudio.|RFC 3389|
|14|MPA|audio|1, 2|90000|8–72||MPEG-1 or MPEG-2 audio only|RFC 3551, RFC 2250|
|15|G728|audio|1|8000|2.5|20|ITU-T G.728 audio 16 kbit/s|RFC 3551|
|16|DVI4|audio|1|11025|any|20|IMA ADPCM audio 44.1 kbit/s|RFC 3551|
|17|DVI4|audio|1|22050|any|20|IMA ADPCM audio 88.2 kbit/s|RFC 3551|
|18|G729|audio|1|8000|10|20|ITU-T G.729 and G.729a audio 8 kbit/s; Annex B is implied unless the annexb=no parameter is used|RFC 3551, Page 20, RFC 3555, Page 15|
|19|reserved (previously CN)|audio|||||reserved, previously comfort noise|RFC 3551|
|25|CELB|video||90000|||Sun CellB video[5]|RFC 2029|
|26|JPEG|video||90000|||JPEG video|RFC 2435|
|28|nv|video||90000|||Xerox PARC's Network Video (nv)[6]|RFC 3551, Page 32|
|31|H261|video||90000|||ITU-T H.261 video|RFC 4587|
|32|MPV|video||90000|||MPEG-1 and MPEG-2 video|RFC 2250|
|33|MP2T|audio/video||90000|||MPEG-2 transport stream|RFC 2250|
|34|H263|video||90000|||H.263 video, first version (1996)|RFC 3551, RFC 2190|
|72–76|reserved||||||reserved because RTCP packet types 200–204 would otherwise be indistinguishable from RTP payload types 72–76 with the marker bit set|RFC 3550, RFC 3551|
|dynamic|H263-1998|video||90000|||H.263 video, second version (1998)|RFC 3551, RFC 4629, RFC 2190|
|dynamic|H263-2000|video||90000|||H.263 video, third version (2000)|RFC 4629|
|dynamic (or profile)|H264 AVC|video||90000|||H.264 video (MPEG-4 Part 10)|RFC 6184, previously RFC 3984|
|dynamic (or profile)|H264 SVC|video||90000|||H.264 video|RFC 6190|
|dynamic (or profile)|H265|video||90000|||H.265 video (HEVC)|RFC 7798|
|dynamic (or profile)|theora|video||90000|||Theora video|draft-barbato-avt-rtp-theora|
|dynamic|iLBC|audio|1|8000|20, 30|20, 30|Internet low Bitrate Codec 13.33 or 15.2 kbit/s|RFC 3952|
|dynamic|PCMA-WB|audio|1|16000|5||ITU-T G.711.1 A-law|RFC 5391|
|dynamic|PCMU-WB|audio|1|16000|5||ITU-T G.711.1 µ-law|RFC 5391|
|dynamic|G718|audio||32000 (placeholder)|20||ITU-T G.718|draft-ietf-payload-rtp-g718|
|dynamic|G719|audio|(various)|48000|20||ITU-T G.719|RFC 5404|
|dynamic|G7221|audio||16000, 32000|20||ITU-T G.722.1 and G.722.1 Annex C|RFC 5577|
|dynamic|G726-16|audio|1|8000|any|20|ITU-T G.726 audio 16 kbit/s|RFC 3551|
|dynamic|G726-24|audio|1|8000|any|20|ITU-T G.726 audio 24 kbit/s|RFC 3551|
|dynamic|G726-32|audio|1|8000|any|20|ITU-T G.726 audio 32 kbit/s|RFC 3551|
|dynamic|G726-40|audio|1|8000|any|20|ITU-T G.726 audio 40 kbit/s|RFC 3551|
|dynamic|G729D|audio|1|8000|10|20|ITU-T G.729 Annex D|RFC 3551|
|dynamic|G729E|audio|1|8000|10|20|ITU-T G.729 Annex E|RFC 3551|
|dynamic|G7291|audio||16000|20||ITU-T G.729.1|RFC 4749|
|dynamic|GSM-EFR|audio|1|8000|20|20|ITU-T GSM-EFR (GSM 06.60)|RFC 3551|
|dynamic|GSM-HR-08|audio|1|8000|20||ITU-T GSM-HR (GSM 06.20)|RFC 5993|
|dynamic (or profile)|AMR|audio|(various)|8000|20||Adaptive Multi-Rate audio|RFC 4867|
|dynamic (or profile)|AMR-WB|audio|(various)|16000|20||Adaptive Multi-Rate Wideband audio (ITU-T G.722.2)|RFC 4867|
|dynamic (or profile)|AMR-WB+|audio|1, 2 or omit|72000|13.3–40||Extended Adaptive Multi Rate – WideBand audio|RFC 4352|
|dynamic (or profile)|vorbis|audio|(various)|(various)|||Vorbis audio|RFC 5215|
|dynamic (or profile)|opus|audio|1, 2|48000[note 3]|2.5–60|20|Opus audio|RFC 7587|
|dynamic (or profile)|speex|audio|1|8000, 16000, 32000|20||Speex audio|RFC 5574|
|dynamic|mpa-robust|audio|1, 2|90000|24–72||Loss-Tolerant MP3 audio|RFC 5219 (previously RFC 3119)|
|dynamic (or profile)|MP4A-LATM|audio||90000 or others|||MPEG-4 Audio|RFC 6416 (previously RFC 3016)|
|dynamic (or profile)|MP4V-ES|video||90000 or others|||MPEG-4 Visual|RFC 6416 (previously RFC 3016)|
|dynamic (or profile)|mpeg4-generic|audio/video||90000 or other|||MPEG-4 Elementary Streams|RFC 3640|
|dynamic|VP8|video||90000|||VP8 video|RFC 7741|
|dynamic|VP9|video||90000|||VP9 video|draft-ietf-payload-vp9|
|dynamic|L8|audio|(various)|(various)|any|20|Linear PCM 8-bit audio with 128 offset|RFC 3551 Section 4.5.10 and Table 5|
|dynamic|DAT12|audio|(various)|(various)|any|20 (by analogy with L16)|IEC 61119 12-bit nonlinear audio|RFC 3190 Section 3|
|dynamic|L16|audio|(various)|(various)|any|20|Linear PCM 16-bit audio|RFC 3551 Section 4.5.11, RFC 2586|
|dynamic|L20|audio|(various)|(various)|any|20 (by analogy with L16)|Linear PCM 20-bit audio|RFC 3190 Section 4|
|dynamic|L24|audio|(various)|(various)|any|20 (by analogy with L16)|Linear PCM 24-bit audio|RFC 3190 Section 4|
|dynamic|raw|video||90000|||Uncompressed Video|RFC 4175|
|dynamic|ac3|audio|(various)|32000, 44100, 48000|||Dolby AC-3 audio|RFC 4184|
|dynamic|eac3|audio|(various)|32000, 44100, 48000|||Enhanced AC-3 audio|RFC 4598|
|dynamic|t140|text||1000|||Text over IP|RFC 4103|
|dynamic|EVRCEVRC0EVRC1|audio||8000|||EVRC audio|RFC 4788|
|dynamic|EVRCBEVRCB0EVRCB1|audio||8000|||EVRC-B audio|RFC 4788|
|dynamic|EVRCWBEVRCWB0EVRCWB1|audio||16000|||EVRC-WB audio|RFC 5188|
|dynamic|jpeg2000|video||90000|||JPEG 2000 video|RFC 5371|
|dynamic|UEMCLIP|audio||8000, 16000|||UEMCLIP audio|RFC 5686|
|dynamic|ATRAC3|audio||44100|||ATRAC3 audio|RFC 5584|
|dynamic|ATRAC-X|audio||44100, 48000|||ATRAC3+ audio|RFC 5584|
|dynamic|ATRAC-ADVANCED-LOSSLESS|audio||(various)|||ATRAC Advanced Lossless audio|RFC 5584|
|dynamic|DV|video||90000|||DV video|RFC 3189|
|dynamic|BT656|video|||||ITU-R BT.656 video|RFC 3555|
|dynamic|BMPEG|video|||||Bundled MPEG-2 video|RFC 2343|
|dynamic|SMPTE292M|video|||||SMPTE 292M video|RFC 3497|
|dynamic|RED|audio|||||Redundant Audio Data|RFC 2198|
|dynamic|VDVI|audio|||||Variable-rate DVI4 audio|RFC 3551|
|dynamic|MP1S|video|||||MPEG-1 Systems Streams video|RFC 2250|
|dynamic|MP2P|video|||||MPEG-2 Program Streams video|RFC 2250|
|dynamic|tone|audio||8000 (default)|||tone|RFC 4733|
|dynamic|telephone-event|audio||8000 (default)|||DTMF tone|RFC 4733|
|dynamic|aptx|audio|2 – 6|(equal to sampling rate)|4000 ÷ sample rate|4[note 4]|aptX audio|RFC 7310|


## References
- [Wikipedia RTP Profile](https://en.wikipedia.org/wiki/RTP_audio_video_profile)
