# KirkDawson.zsh-theme

A ZSH theme optimized for people who use:

- Solarized
- Git
- Unicode-compatible fonts and terminals (I use iTerm2 + Menlo)

For Mac users, I highly recommend iTerm 2 + Solarized Dark

# Compatibility

**NOTE:** In all likelihood, you will need to install a [Powerline-patched font](https://github.com/Lokaltog/powerline-fonts) for this theme to render correctly.

To test if your terminal and font support it, check that all the necessary characters are supported by copying the following command to your terminal: `echo "\ue0b0 \u00b1 \ue0a0 \u27a6 \u2718 \u26a1 \u2699"`. The result should look like this:

![Character Example](characters.png)

## What does it show?

- If the previous command failed (✘)
- User @ Hostname (if user is not DEFAULT_USER, which can then be set in your profile)
- Git status
  - Branch () or detached head (➦)
  - Current branch / SHA1 in detached head state
  - Dirty working directory (±, color change)
- Working directory
- Elevated (root) privileges (⚡)

![Screenshot](screenshot.png)

## Customize your prompt view

By default prompt has these segments: `prompt_status`, `prompt_context`, `prompt_virtualenv`, `prompt_dir`, `prompt_git`, `prompt_end` in that particular order.

If you want to add, change the order or remove some segments of the prompt, you can use array environment variable named `KirkDawson_PROMPT_SEGMENTS`.

Examples:
- Show all segments of the prompt with indices:
```
echo "${(F)KirkDawson_PROMPT_SEGMENTS[@]}" | cat -n
```
- Add the new segment of the prompt to the beginning:
```
KirkDawson_PROMPT_SEGMENTS=("prompt_git" "${KirkDawson_PROMPT_SEGMENTS[@]}")
```
- Add the new segment of the prompt to the end:
```
KirkDawson_PROMPT_SEGMENTS+="prompt_end"
```
- Insert the new segment of the prompt = `PROMPT_SEGMENT_NAME` on the particular position = `PROMPT_SEGMENT_POSITION`:
```
PROMPT_SEGMENT_POSITION=5 PROMPT_SEGMENT_NAME="prompt_end";\
KirkDawson_PROMPT_SEGMENTS=("${KirkDawson_PROMPT_SEGMENTS[@]:0:$PROMPT_SEGMENT_POSITION-1}" "$PROMPT_SEGMENT_NAME" "${KirkDawson_PROMPT_SEGMENTS[@]:$PROMPT_SEGMENT_POSITION-1}");\
unset PROMPT_SEGMENT_POSITION PROMPT_SEGMENT_NAME
```
- Swap segments 4th and 5th:
```
SWAP_SEGMENTS=(4 5);\
TMP_VAR="$KirkDawson_PROMPT_SEGMENTS[$SWAP_SEGMENTS[1]]"; KirkDawson_PROMPT_SEGMENTS[$SWAP_SEGMENTS[1]]="$KirkDawson_PROMPT_SEGMENTS[$SWAP_SEGMENTS[2]]"; KirkDawson_PROMPT_SEGMENTS[$SWAP_SEGMENTS[2]]="$TMP_VAR"
unset SWAP_SEGMENTS TMP_VAR
```
- Remove the 5th segment:
```
KirkDawson_PROMPT_SEGMENTS[5]=
```

A small demo of the dummy custom prompt segment, which has been created with help of the built-in `prompt_segment()` function from KirkDawson theme:
```
# prompt_segment() - Takes two arguments, background and foreground.
# Both can be omitted, rendering default background/foreground.

customize_KirkDawson() {
  prompt_segment 'red' '' ' ⚙ ⚡⚡⚡ ⚙  '
}
```
![Customization demo](https://github.com/apodkutin/KirkDawson-zsh-theme/raw/customize-prompt/KirkDawson_customization.gif)

## Future Work

I don't want to clutter it up too much, but I am toying with the idea of adding RVM (ruby version) and n (node.js version) display.

It's currently hideously slow, especially inside a git repo. I guess it's not overly so for comparable themes, but it bugs me, and I'd love to hear ideas about how to improve the performance.

Would be nice for the code to be a bit more sane and re-usable. Something to easily append a section with a given FG/BG, and add the correct opening and closing.

Also the dependency on a powerline-patched font is regrettable, but there's really no way to get that effect without it. Ideally there would be a way to check for compatibility, or maybe even fall back to one of the similar unicode glyphs.
