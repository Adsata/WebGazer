# Requirements Analysis

WebGazer is an in-browser tool that predicts the user's gaze by analyzing the webcam's video feed. It detects face features using a deep learning model and predicts the gaze by combining multiple features and analyses. Modern hardware makes it possible so do all this on the client's computer so that no personal data gets transmitted to any servers and no video streams are saved.

## The following requirements must be met:
- WebGazer must be able to read the video stream of the webcam using the [MediaDevices API](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices)
- WebGazer must [tee the stream](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream/tee), creating one stream for predictions and one for the user to see
- WebGazer must give live (_very_ short delay) feedback if the users face can be detected
- WebGazer must not block the user from efficiently interacting with the UI
- Predictions must be in real time (possibly using [WebWorkers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API) speed up both UI and analyis)
- Predictions must be published as a [ReadableStream](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream/ReadableStream) that can be read by other code whenever new predictions are available ([Example](https://mdn.github.io/dom-examples/streams/simple-random-stream/))
- Predictions must contain
  - x and y coordinates in pixels (relative to the viewport)
  - timestamp in milliseconds since WebGazer started recording
  - (optional) error message why predictions failed, e.g. no face detected, no eyes detected, no pupils detected, connection to media failed
- Eyes must be detected
  - error message in prediction if no eyes found
- Pupils must be detected
  - error message in prediction if no pupils found
- Gaze must be detected
- Expose function to detect if eyes are too close to the video sides (distance as parameter) to prevent a user unintentionally getting out of sight
- Expose properties
  - timestamp when WebGazer began recording (set when `begin()` is called, `null` before)
- WebGazer must be able to recalibrate while running, `useCalibration()` with the following parameters. Changing while running must be possible
  - `[]` don't recalibrate
  - `['click']` recalibrate on clicks
  - `['move']` recalibrate on cursor movement
  - `['click', 'move']` recalibrate on clicks and cursor movement
- Libraries for regession and face detection must be exchangable

## General goals:
- keeping WebGazer and its dependencies up-to-date
- creating and API suitable for most use cases
- making WebGazer easy to integrate into modern frameworks like React
- making predictions more reliable as they are now
- providing an extensive documentation
