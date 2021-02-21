# Public beta test. Use in production at your own risk!
## tgcalls - a python binding for tgcalls (c++ lib by Telegram);
## pytgcalls - library connecting python binding for tgcalls and Pyrogram.

### Notes

**Only for Linux systems on x86_64 platform!**

**If the sound does not start playing after entering the chat - turn on and 
turn off your microphone!**

### Features

- Python solution
- Join to voice chats
- Payout from file
- Output (recording) to file
- Change files at runtime
- Stop payout/output
- Multiply chat (CPU load)

### TODO

- Payout and output by bytes from Python
- Incoming and Outgoing calls (already there and working, but not in beta)
- Video calls (video from a file etc)
- Additional things for working with ffmpeg
- Convenient callbacks and methods
- Mac OS builds
- Maybe ARM support
- Windows instruction how to build (maybe)

### Audio file formats

RAW files are now used. You will have to convert to this format yourself 
using ffmpeg. This procedure may become easier in the future.

From mp3 to raw (to play in voice chat):
```
ffmpeg -i input.mp3 -f s16le -ac 2 -ar 48000 -acodec pcm_s16le input.raw
```

From raw to mp3 (files with recordings):
```
ffmpeg -f s16le -ac 2 -ar 48000 -acodec pcm_s16le -i output.raw clear_output.mp3

```

### Installing

**-pre argument is necessary!** 

```
pip install --pre pytgcalls
```

### How to test

No docs, but example!

```python
import os
import asyncio

import pytgcalls
import pyrogram

# EDIT VALUES!
API_HASH = None
API_ID = None
CHAT_ID = '@tgcallschat'
INPUT_FILENAME = 'input.raw'
OUTPUT_FILENAME = 'output.raw'


async def main(client):
    await client.start()
    while not client.is_connected:
        await asyncio.sleep(1)

    # you can pass init filenames in the constructor
    group_call = pytgcalls.GroupCall(client, INPUT_FILENAME, OUTPUT_FILENAME)
    await group_call.start(CHAT_ID)

    # to change audio file you can do this:
    # group_call.input_filename = 'input2.raw'

    # to change output file:
    # group_call.output_filename = 'output2.raw'

    # to restart play from start:
    # group_call.restart_playout()

    # to stop play:
    # group_call.stop_playout()

    # same with output (recording)
    # .restart_recording, .stop_output

    # to send speaking status you can use this:
    # group_call.send_speaking_group_call_action()

    # to mute yourself:
    # group_call.set_is_mute(True)

    await pyrogram.idle()


if __name__ == '__main__':
    pyro_client = pyrogram.Client(
        os.environ.get('SESSION_NAME', 'pytgcalls'),
        api_hash=os.environ.get('API_HASH', API_HASH),
        api_id=os.environ.get('API_ID', API_ID)
    )

    loop = asyncio.get_event_loop()
    loop.run_until_complete(main(pyro_client))

```

## Issues, bugs, questions

I am aware of problems with the start of playout on login. I know about 
a non-working sound analyzer (does not display how the bot speaks). 
I know that debug logs are enabled. Everything else follow the links below.

- Create issue on GitHub: https://github.com/MarshalX/tgcalls-beta/issues
- Ask for help on Telegram chat https://t.me/tgcallschat
- Read news on Telegram channel: https://t.me/tgcallslib

## License

You may copy, distribute and modify the software provided that modifications 
are described and licensed for free under [LGPL-3](https://www.gnu.org/licenses/lgpl-3.0.html). 
Derivatives works (including modifications or anything statically 
linked to the library) can only be redistributed under LGPL-3, but 
applications that use the library don't have to be.
