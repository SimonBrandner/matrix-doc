# MSC0000: VoIP mute events

During VoIP calls it is a common thing for a user to mute their microphone or
webcam for a short period of time. This is usually done by disabling the
corresponding tracks. The problem is that there is no way for the opponent to
know that the person on the other side has muted their microphone. The opponent
may wish to know this simply to show that the a participant is muted. Also, if
the user mutes their camera, the opponent just sees a black screen, but the
opponent client may wish to show their avatar instead.

## Proposal

This MSC proposes adding a new call event `m.call.mute` which has the common
VoIP fields as specified in
[MSC2746](https://github.com/matrix-org/matrix-doc/pull/2746) (`version`,
`call_id`, `party_id`) and `mute_info` object which contains the following
fields:

+ `stream_id`
+ `audio_muted`
+ `video_muted`

A client sends this event when the users changes their mute state (i.e. mutes or
unmutes their microphone or camera).

Clients should persist this information even in the event of a stream
replacement as defined in
[MSC3077](https://github.com/matrix-org/matrix-doc/pull/3077).

Clients should assume that any new stream has both audio and video enabled.

TODO: These two points are a little contradictory.

### Example

```JSON
{
    "type": "m.call.mute",
    "room_id": "!roomId",
    "content": {
        "call_id": "1414213562373095",
        "party_id": "1732050807568877",
        "mute_info": {
            "streamId": "271828182845",
            "audioMuted:": false,
            "videoMuted": true,
        },
        "version": "1",
    },
}
```

## Alternatives

Clients could remove the audio/video track temporarily. This would theoretically
work but would require a lot renegotiation which would make inefficient.

## Potential issues

None that I can think of.

## Security considerations

None that I can think of.

## Unstable prefix

|Release      |Development                     |
|-------------|--------------------------------|
|`m.call.mute`|`org.matrix.msc0000.call.mute`  |
