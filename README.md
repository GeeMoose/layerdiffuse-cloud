# ComfyUI-layerdiffuse
ComfyUI implementation of https://github.com/layerdiffusion/LayerDiffuse.

## Installation
Download the repository and unpack into the custom_nodes folder in the ComfyUI installation directory.

Or clone via GIT, starting from ComfyUI installation directory:
```bash
cd custom_nodes
git clone git@github.com:huchenlei/ComfyUI-layerdiffuse.git
```

Run `pip install -r requirements.txt` to install python dependencies. You might experience version conflict on diffusers if you have other extensions
that depends on other versions of diffusers. In this case, it is recommended to setup separate Python venvs.

## Workflows
### [Generate foreground](https://github.com/huchenlei/ComfyUI-layerdiffuse/blob/main/examples/layer_diffusion_fg_example_rgba.json)
![rgba](https://github.com/huchenlei/ComfyUI-layerdiffuse/assets/20929282/5e6085e5-d997-4a0a-b589-257d65eb1eb2)

### [Generate foreground (RGB + alpha)](https://github.com/huchenlei/ComfyUI-layerdiffuse/blob/main/examples/layer_diffusion_fg_example.json)
If you want more control of getting RGB image and alpha channel mask separately, you can use this workflow.
![readme1](https://github.com/huchenlei/ComfyUI-layerdiffuse/assets/20929282/4825b81c-7089-4806-bce7-777229421707)

### [Blending (FG/BG)](https://github.com/huchenlei/ComfyUI-layerdiffuse/blob/main/examples/layer_diffusion_cond_example.json)
Blending given FG
![fg_cond](https://github.com/huchenlei/ComfyUI-layerdiffuse/assets/20929282/7f7dee80-6e57-4570-b304-d1f7e5dc3aad)

Blending given BG
![bg_cond](https://github.com/huchenlei/ComfyUI-layerdiffuse/assets/20929282/e3a79218-6123-453b-a54b-2f338db1c12d)

### [Extract FG from Blended + BG](https://github.com/huchenlei/ComfyUI-layerdiffuse/blob/main/examples/layer_diffusion_diff_fg.json)
![diff_bg](https://github.com/huchenlei/ComfyUI-layerdiffuse/assets/20929282/45c7207d-72ff-4fb0-9c91-687040781837)

### [Extract BG from Blended + FG](https://github.com/huchenlei/ComfyUI-layerdiffuse/blob/main/examples/layer_diffusion_diff_bg.json)
[Forge impl's sanity check](https://github.com/layerdiffuse/sd-forge-layerdiffuse#sanity-check) sets `Stop at` to 0.5 to get better quality BG.
This workflow might be inferior comparing to other object removal workflows.
![diff_fg](https://github.com/huchenlei/ComfyUI-layerdiffuse/assets/20929282/05a10add-68b0-473a-acee-5853e4720322)

### [Extract BG from Blended + FG (Stop at 0.5)](https://github.com/huchenlei/ComfyUI-layerdiffuse/blob/main/examples/layer_diffusion_diff_bg_stop_at.json)
In [SD Forge impl](https://github.com/layerdiffuse/sd-forge-layerdiffuse), there is a `stop at` param that determines when
layer diffuse should stop in the denosing process. In the background, what this param does is unapply the LoRA and c_concat cond after a certain step
threshold. This is hard/risky to implement directly in ComfyUI as it requires manually load a model that has every changes except the layer diffusion
change applied. A workaround in ComfyUI is to have another img2img pass on the layer diffuse result to simulate the effect of `stop at` param.
![diff_fg_stop_at](https://github.com/huchenlei/ComfyUI-layerdiffuse/assets/20929282/e383c9d3-2d47-40c2-b764-b0bd48243ee8)


### [Generate FG from BG combined](https://github.com/huchenlei/ComfyUI-layerdiffuse/blob/main/examples/layer_diffusion_cond_fg_all.json)
Combines previous workflows to generate blended and FG given BG. We found that there are some color variations in the extracted FG. Need to confirm
with layer diffusion authors on whether this is expected.
![fg_all](https://github.com/huchenlei/ComfyUI-layerdiffuse/assets/20929282/f4c18585-961a-473a-a616-aa3776bacd41)

## Note
- Currently only SDXL is supported. See https://github.com/layerdiffuse/sd-forge-layerdiffuse#model-notes for more details.

## TODO
- [x] Foreground conditioning
- [x] Background conditioning
- [x] Blended + foreground => background
- [x] Blended + background => foreground
