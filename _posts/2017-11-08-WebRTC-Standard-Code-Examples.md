---
title: "WebRTC Standard Code Examples"
tags: [webrtc]
date: 2017-11-08 01:00:00
---

## Prerequisite
JavaScript(ES6) /w adapter.js
  - [webrtc-adapter](https://github.com/webrtchacks/adapter)

## Sender
```javascript
const constraints = {
    audio: true,
    video: true
};
const pcOptions = {
    iceServers: [
        {urls: ['stun:stun.l.google.com:19302', ...]},
        {urls: 'turn:turn.*.com', username: "mack", credential: "1234"}
    ]
};
  
const signaling = new SignalingChannel();
const pc = new RTCPeerConnection(pcOptions);
  
// retain media resources
const stream = await navigator.mediaDevices.getUserMedia(constraints);
stream.getTracks().forEach((track) => pc.addTrack(track, stream));
  
// videoTag.srcObject = stream;  // if show it in a self-view
  
// send any ice candidates to the other peer
pc.onicecandidate = ({candidate}) => signaling.send({candidate});
  
// let the "negotiationneeded" event trigger offer generation
pc.onnegotiationneeded = async () => {
    try {
        await pc.setLocalDescription(await pc.createOffer());
  
        // send the offer to the other peer
        signaling.send({desc: pc.localDescription});
    } catch (err) {
        console.error(err);
    }
};
  
// handle signaling channel messages
signaling.onmessage = async ({desc, candidate}) => {
    try {
        if (desc) {
            if (desc.type == 'answer') {
                await pc.setRemoteDescription(desc);
            } else {
                console.error('Illegal messages.');
            }
        } else if (candidate) {
            await pc.addIceCandidate(candidate);
        }
    } catch (err) {
        console.error(err);
    }
};
```

## Receiver
```javascript
const pcOptions = {
    iceServers: [
        {urls: ['stun:stun.l.google.com:19302', ...]},
        {urls: 'turn:turn.*.com', username: "mack", credential: "1234"}
    ]
};
 
const videoTag = document.querySelector('#videoTag');
const signaling = new SignalingChannel();
const pc = new RTCPeerConnection(pcOptions);
 
// send any ice candidates to the other peer
pc.onicecandidate = ({candidate}) => signaling.send({candidate});
 
// once media for a remote track arrives, show it in the remote video element
pc.ontrack = (event) => {
    // don't set srcObject again if it is already set.
    if (!videoTag.srcObject) {
        videoTag.srcObject = event.streams[0];
    }
};
 
// handle signaling channel messages
signaling.onmessage = async ({desc, candidate}) => {
    try {
        if (desc) {
            // if we get an offer, we need to reply with an answer
            if (desc.type == 'offer') {
                await pc.setRemoteDescription(desc);
                await pc.setLocalDescription(await pc.createAnswer());
  
                signaling.send({desc: pc.localDescription});
            } else {
                console.error('Illegal messages.');
            }
        } else if (candidate) {
            await pc.addIceCandidate(candidate);
        }
    } catch (err) {
        console.error(err);
    }
};
```