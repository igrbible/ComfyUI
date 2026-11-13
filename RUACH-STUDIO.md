# Ruach Studio's derivative of ComfyUI

This branch, `ruach-studio`, is [Ruach Studio](https://github.com/igrbible/Ruach_Studio)'s derivative of [Comfy-Org/ComfyUI](https://github.com/Comfy-Org/ComfyUI). It holds the change we made to ComfyUI's code so that it can load our grown YuE2 decoder, published as it is. We hide nothing and are not ashamed of it: what we take from upstream, and what we change in it, is here for anyone to read, use or send back. The other branches of this fork are upstream's, untouched.

## Base

Upstream commit `c5751c8` («Remove directml support. (#17711)», master of 11.11.2026), the master our patch applies to cleanly.

## What it changes

| commit | HERESY | change |
|---|---|---|
| `audio VAE: load a grown YuE2 decoder (YuE2-VAE-RS x3)…` | 2280, 2282, 2329 | `comfy/ldm/audio/autoencoder.py`: `DecoderBlock(extra_units=0)` and `OobleckDecoder(pre_units=0, extra_units=0)` add residual units after Standard's three in each upsampling block and at the latent rate before the first one; zero by default, so Standard and every other Oobleck VAE build as before. `comfy/sd.py`: the YuE2 branch counts the units in the state dict, builds the x3 shape when there are more than Standard's, and pins float32 for it. Without it, stock ComfyUI loads x3 into Standard's shape under `strict=False` and drops 210 tensors without an error. |

This is the change offered in [issue #17697](https://github.com/Comfy-Org/ComfyUI/issues/17697); the branch is there to be the head of its pull request. HERESY numbers are entries of Ruach Studio's numbered patch log (`docs/HERESY.md` in the Studio). The decoders, the integration kit and its [programmers' guide](https://huggingface.co/goldhub/YuE2-RS-Finetunes/blob/main/docs/VAE-RS-x3-integration.md) are published at [goldhub/YuE2-RS-Finetunes, VAE-RS/](https://huggingface.co/goldhub/YuE2-RS-Finetunes/tree/main/VAE-RS).

## Licence

ComfyUI stays under its own licence, the GNU General Public License v3.0 (`LICENSE`), and this branch as a whole is distributed under it. Our change is part of the YuE2-VAE-RS integration kit (`comfyui-native.patch`) and is licensed, as the kit is, under the Apache License 2.0, which is compatible with GPL-3.0, with this notice:

> YuE2-VAE-RS integration code. Copyright 2026 Viktor Zhuromskyy, Ruach Studio. https://github.com/igrbible/Ruach_Studio. Under section 4 (d) of the License, any redistribution of this code or of a work derived from it must include this notice, and must credit "Ruach Studio and Viktor Zhuromskyy" in its NOTICE file, in its documentation, or where such third-party notices normally appear.

The YuE2-VAE-RS decoder weights are not in this repository; they are adaptations of m-a-p's YuE2-Vae and are licensed under CC BY-NC 4.0, as m-a-p's YuE2-3B and YuE2-Vae. Ruach Studio itself is AGPL-3.0-or-later.
