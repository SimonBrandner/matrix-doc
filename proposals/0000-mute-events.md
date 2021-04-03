# MSC0000: VoIP mute events

During VoIP calls it is common for a user to mute their microphone or webcam for
a short period. This is usually done by disabling the corresponding tracks. The
problem is that there is no way to know what is the muted state on the
opponent's side.

It is important that this is possible because of the following reasons:

+ Clients may wish to inform the user of the opponent's mute state.
+ If a user mutes their camera, their opponent just sees a black screen, but the
  opponent's client may wish to show a placeholder instead (e.g. avatar, display
  name).

## Proposal

This MSC proposes adding a new call event `m.call.mute` which has the common
VoIP fields as specified in
[MSC2746](https://github.com/matrix-org/matrix-doc/pull/2746) (`version`,
`call_id`, `party_id`) and a `mute_info` object which contains the following
fields:

+ `stream_id` - the `id` of the stream about which this event informs
+ `audio_muted`
+ `video_muted`

### Rules

+ A client sends this event when the users changes their mute state (i.e. mutes or
unmutes their microphone or camera).
+ Clients should persist this information even in the event of a stream
replacement as defined in
[MSC3077](https://github.com/matrix-org/matrix-doc/pull/3077).
+ When deciding how to display the current mute state clients should give priority
  to missing tracks - if there is no video track the client should act as if the
  video was muted.
+ Clients should assume that any new stream has all tracks enabled.

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

Clients could remove the audio or video track temporarily. This would work but
would require a lot renegotiation which wouldn't be very efficient.

## Potential issues

None that I can think of.

## Security considerations

None that I can think of.

## Unstable prefix

|Release      |Development                     |
|-------------|--------------------------------|
|`m.call.mute`|`org.matrix.msc0000.call.mute`  |
