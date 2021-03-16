# sdp-transform
[![pub package](https://img.shields.io/pub/v/sdp_transform.svg)](https://pub.dartlang.org/packages/sdp_transform) [![Gitter](https://badges.gitter.im/flutter-webrtc/Lobby.svg)](https://gitter.im/flutter-webrtc/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

Dart version of the [sdp-transform](https://github.com/clux/sdp-transform) JavaScript library.

dart-sdp-transform is a simple parser and writer of SDP. Defines internal grammar based on RFC4566 - SDP, RFC5245 - ICE, and many more.

## Usage

```dart

import 'dart:io';
import 'dart:convert';
import 'package:sdp_transform/sdp_transform.dart';

main() {
  new File('./test/ssrc.sdp').readAsString().then((String contents) {
    print('original sdp => ' + contents);
    var session = parse(contents);
    print('convert to json => ' + json.encode(session));

    var params = parseParams(session['media'][1]['fmtp'][0]['config']);
    print('params => ' + params.toString());

    var payloads = parsePayloads(session['media'][1]["payloads"]);
    print('payloads => ' + payloads.toString());

    String imageAttributesStr = "[x=1280,y=720] [x=320,y=180]";
    var imageAttributes = parseImageAttributes(imageAttributesStr);
    print('imageAttributes => ' + imageAttributes.toString());

    // // a=simulcast:send 1,~4;2;3 recv c
    String simulcastAttributesStr = "1,~4;2;3";
    var simulcastAttributes = parseSimulcastStreamList(simulcastAttributesStr);
    print('simulcastAttributes => ' + simulcastAttributes.toString());

    var sdp = write(session, null);
    print('convert to sdp => ' + sdp);
  });
}

```
