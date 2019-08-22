# jbs

jbs is an audio library for LÖVE that simplifies various aspects of audio handling, including tagging and playing multiple instances of a sound.

It is based on the [`ripple` library by tesselode](https://github.com/tesselode/ripple). As of this writing (2019-08-22 Thursday), the code is almost entirely theirs. I'm renaming to differentiate it as I add additional functionality; in case it ends up being useful to someone else in the future, I want to avoid confusion. Still, all credit should go to tesselode for the initial creation of the library.

## Installation

To use jbs, place jbs.lua in your project, and then add this code to your main.lua:

```lua
jbs = require 'jbs' -- if your jbs.lua is in the root directory
jbs = require 'path.to.jbs' -- if it's in subfolders
```

## Usage

### Loading sounds

```lua
local sound = jbs.newSound(source, options)
```

Use `jbs.newSound` to load a sound.
- `source` - the Source or SoundData to use for the sound. Sources can be created using [`love.audio.newSource`](https://love2d.org/wiki/love.audio.newSource), and SoundData can be created using [`love.sound.newSoundData`](https://love2d.org/wiki/love.sound.newSoundData).
- `options` (optional) - a table of additional options to apply to the sound. The table can have the following keys:
    - `volume` (optional) - the volume of the sound, from 0 to 1. Defaults to 1.
    - `tags` (optional) - a list of tags to apply to the sound (see below for more information on tags).

### Playing sounds

```lua
sound:play(options)
```

Plays a sound. `options` is a table with the following keys, all of which are optional:
- `volume` - sets the volume of this particular occurrence of the sound relative to the sound's main volume, from 0 to 1. Defaults to 1.
- `pitch` - the pitch of this particular occurrence of the sound, in terms of a multiple of the default playback speed. Defaults to 1.

### Tagging sounds

To create a tag, use `jbs.newTag`:

```lua
local tag = jbs.newTag()
```

You can then add tags to sounds or remove them from sounds using `sound.tag` and `sound.untag`:

```lua
sound:tag(tag1)
sound:untag(tag2)
```

If you don't want to add or remove tags individually, you can also specify an entire list of tags using `sound.setTags`:

```lua
sound:setTags {tag1, tag2, ...}
```

You can get a list of the tags a sound is currently tagged with using `sound.getTags`:

```lua
local tags = sound:getTags()
```

You can pause, resume, or stop all of the currently playing sounds with a tag using `tag:pause()`, `tag:resume()`, and `tag:stop()`.

### Adjusting volume levels

You can adjust the volume of individual sounds by setting the `volume` property of a sound. You can also adjust the volume of a tag by setting its `volume` property. The overall volume level of a sound is its own volume multiplied by the volume of each of its tags. This allows you to easily set the volume of categories of sounds. For example, you could have separate "music" and "sfx" tags, and you can adjust the volume of all music tracks and sound effects at once by setting the volume of their respective tags.

### Looping sounds

You can set whether a sound should loop or not using `sound.setLooping`:

```lua
sound:setLooping(true) -- enables looping
sound:setLooping(false) -- disables looping
```

### Using audio effects

You can set a sound to use an effect defined by [`love.audio.setEffect`](https://love2d.org/wiki/love.audio.setEffect) using `sound.setEffect`:

```lua
sound:setEffect(name, filtersettings)
```

- `name` - the name of the effect to use.
- `filtersettings` (optional) - filter settings to apply to the sound (see [`Source:setEffect`](https://love2d.org/wiki/Source:setEffect)).

You can disable an effect by passing `false` as the second argument.

You can also set effects on tags using `tag.setEffect`:

```lua
tag:setEffect(name, filtersettings)
```

Tag effects will be applied to every sound with that tag.

## Contributing

This library is still in early development, so feel free to report bugs, make pull requests, or just make suggestions about the code or design of the library. To run the test project, run `lovec .` in the jbs base directory.
