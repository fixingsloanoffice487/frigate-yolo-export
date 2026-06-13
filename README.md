<div align="center">

# frigate-yolo-export

**One command → a deploy-ready YOLO detector for [Frigate](https://frigate.video).**

Export a pretrained-COCO YOLO model to ONNX, validated and saved locally then print the
exact Frigate config.

— for OpenVINO only.

![License](https://img.shields.io/badge/license-MIT-blue)
![Python](https://img.shields.io/badge/python-3.9%2B-blue)
![Frigate](https://img.shields.io/badge/frigate-yolo--generic-0a7)
![Models](https://img.shields.io/badge/models-YOLO26%20%7C%20YOLO11%20%7C%20YOLOv8-555)

</div>

---

> [!WARNING]
> Some YOLO models are AGPLv3. Personal use only.

# Single code Install

```console
$ uv run build_frigate_yolo.py
```

Frigate's default `ssdlite_mobilenet` detector is weak. A modern YOLO model is a
big accuracy upgrade and runs comfortably on an Intel iGPU via OpenVINO — this
tool builds one and hands you the config, with **no guesswork**.

## Features

- **One command, any model** — `yolo26`, `yolo11n/s/m`, `yolov8*`, or your own
  `.pt`. NMS-free models (YOLO26 / YOLOv10) are handled automatically.
- **Any resolution** — square (`640`) or non-square (`640x480`) to match 4:3
  cameras with no letterbox waste.
- **Validated before deploy** — runs real inference on a sample image and
  confirms the decode, so you never ship a broken model.
- **Config + deploy steps printed** — paste-ready `model:` block and numbered
  install instructions for HA add-on and Docker.
- **Zero setup** — dependencies declared inline ([PEP 723](https://peps.python.org/pep-0723/));
  just `uv run`.

## Quick start

```bash
# zero-setup with uv (deps auto-installed)
uv run build_frigate_yolo.py

# or with pip
pip install -r dependencies.txt
python build_frigate_yolo.py
```

Run with no flags and it **prompts** for model size and input resolution:

```text
Which model? (Intel iGPU via OpenVINO)
  1) yolo11n   fastest, lightest — busy GPU / many cameras
  2) yolo11s   balanced — recommended  (default)
  3) yolo11m   most accurate, heaviest
  4) yolo26s   newest architecture — NMS-free head handled automatically
  5) yolov8n   classic lightweight — broad hardware support
  6) yolov8s   classic balanced — broad hardware support
Which input size?
  1) 320       lightest
  2) 640       accurate, square (letterboxes 4:3 cameras)  (default)
  3) 640x480   accurate, matches 4:3 cameras (no letterbox)
```

Or go non-interactive and copy straight into Frigate's model cache:

```bash
python build_frigate_yolo.py --model yolo11s --imgsz 640x480 \
    --copy-to /path/to/frigate/model_cache
```

## Options

| Flag | Default | Description |
|---|---|---|
| `--model` | prompt | Ultralytics name (`yolo26s`, `yolo11n/s/m/l/x`, `yolov8n…`) or a `.pt` path |
| `--imgsz` | prompt | `640` (square) or `WIDTHxHEIGHT` like `640x480`. Both dims must be ÷32 |
| `--copy-to` | — | Also copy the ONNX into this `model_cache` dir (skips the manual move) |
| `--opset` | `12` | ONNX opset |
| `--output-dir` | `models` | Where the ONNX is written |
| `--no-validate` | off | Skip the local inference check |

## Deploy to Frigate

1. Copy `models/<name>.onnx` into Frigate's `model_cache/` (or use `--copy-to`).
   On the HA add-on it's `addon_configs/<frigate>/model_cache/`, which is
   `/config/model_cache/` inside the container.
2. Paste the printed `model:` block into your config, replacing the old one.
   The labelmap is Frigate's built-in `/labelmap/coco-80.txt`.
3. Restart Frigate → check **System → Logs** (clean load) and
   **System → Metrics → Detectors** (inference speed).

> [!TIP]
> **One detector.** Adding a second OpenVINO detector on the same GPU makes
> inference *slower* (compute contention), not faster. For more headroom, use a
> smaller model or lower resolution instead.

> [!IMPORTANT]
> **ONNX input shape is static.** You can't change resolution by editing
> `width/height` in Frigate — re-run this tool at the new size.

## Notes

- **Picking a model or hardware?** See [COMPARISON.md](COMPARISON.md) for a
  YOLO26 vs YOLO11 vs YOLOv8 breakdown across OpenVINO, Nvidia, Apple Silicon,
  and edge accelerators.
- **Square vs non-square** — a square model letterboxes 4:3 frames; a `640x480`
  model matches the aspect with no waste. Both work with `yolo-generic`.
- **YOLO11 isn't in Frigate's docs** (they list up to YOLOv9), but its output
  head matches, so `yolo-generic` accepts it — and the validation step confirms
  the decode before you deploy.
- **NMS-free models (YOLO26, YOLOv10)** export `[1, max_det, 6]` end-to-end
  detections by default, which `yolo-generic` can't read. The tool auto-disables
  the end-to-end head so they emit the standard `[1, 84, N]` grid instead —
  giving you the newest models in Frigate. Confirmed on live Frigate: `yolo26s`
  at `640×480` runs at **28 ms** on an Intel iGPU (OpenVINO).

<details>
<summary><b>YOLOv9 (official-repo alternative)</b></summary>

Frigate officially documents YOLOv9 via the WongKinYiu repo export, which this
tool does not wrap (it needs a third-party repo clone + patch):

```bash
git clone https://github.com/WongKinYiu/yolov9 && cd yolov9
# patch models/experimental.py: torch.load(..., weights_only=False)
pip install -r requirements.txt onnx onnxruntime onnx-simplifier onnxscript
# download yolov9-<size>-converted.pt from the repo's v0.1 release, then:
python export.py --weights yolov9-s.pt --imgsz 640 --simplify --include onnx
```

Use the same Frigate config block (`model_type: yolo-generic`, matching
`width/height`, `/labelmap/coco-80.txt`).

</details>

## License

[MIT](LICENSE)
