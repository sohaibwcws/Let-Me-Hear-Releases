# Third-party notices

Let Me Hear bundles the following components during `make app`. Their license texts are
installed into the app bundle at `Contents/Resources/Licenses` and are also kept in the
repository under `ThirdParty/Licenses`.

| Component | Source | License |
|---|---|---|
| DPDFNet `dpdfnet2_48khz_hr.onnx` | https://github.com/ceva-ip/DPDFNet | Apache-2.0 |
| sherpa-onnx 1.13.8 native runtime and vendored C header | https://github.com/k2-fsa/sherpa-onnx/tree/v1.13.8 | Apache-2.0 |
| ONNX Runtime 1.28.2, supplied with sherpa-onnx | https://github.com/microsoft/onnxruntime/tree/v1.28.2 | MIT, plus bundled third-party notices |

The model is the streaming export published in sherpa-onnx's `speech-enhancement-models`
release. Both the model and the runtime archive are pinned by SHA-256 in
`Scripts/prepare-model.sh`, which verifies each download before installing it. The
unmodified upstream C API header and its license live in `Sources/CDPDFNet/vendor`.

Modifications to the Apache-2.0 components, as required by section 4(b) of that license:

- `libsherpa-onnx-c-api.dylib` is rewritten with `install_name_tool` so its ONNX Runtime
  dependency resolves through `@loader_path` instead of `@rpath`, and it is re-signed
  with this project's Developer ID identity so library validation accepts it inside the
  hardened-runtime app bundle.
- The DPDFNet model is redistributed as a pre-exported ONNX graph, not in its original
  training form, and the surrounding inference code is an independent implementation.

## Apple frameworks

Let Me Hear uses Apple system frameworks supplied by macOS and Xcode: SwiftUI, AppKit,
AVFoundation, AudioToolbox, CoreAudio, CoreAudio's AudioServerPlugIn interface,
CoreFoundation, and CoreMedia.

Unlike the MIT-licensed project this app is derived from, Let Me Hear **does** enable
Apple's voice-processing IO mode (`kAudioUnitSubType_VoiceProcessingIO`) so that acoustic
echo cancellation runs against the system output reference before neural speech
enhancement is applied. Apple's echo canceller and the neural model are used together,
in that order.
