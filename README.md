This Flutter application demonstrates a smooth integration of the DocuSeal document signing form into any Flutter application.

## Getting Started

Generate Android and iOS directory

```bash
flutter create .
```

Install dependencies:

```bash
flutter pub get
```

Run the command and choose Android or iOS emulator

```bash
flutter run
```

### Example Code

```dart
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';

void main() {
  runApp(const DocuSealDemoApp());
}

class DocuSealDemoApp extends StatelessWidget {
  const DocuSealDemoApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'DocuSeal Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      home: const WebviewPage(),
    );
  }
}

class WebviewPage extends StatefulWidget {
  const WebviewPage({Key? key}) : super(key: key);

  @override
  State<WebviewPage> createState() => _WebviewPageState();
}

class _WebviewPageState extends State<WebviewPage> {
  WebViewController? _controller;

  @override
  void initState() {
    String html = '''
      <html>
        <head>
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <script src="https://cdn.docuseal.co/js/form.js"></script>
        </head>
        <body>
          <docuseal-form
            id="docusealForm"
            data-src="https://docuseal.co/d/LEVGR9rhZYf86M"
            data-email="signer@example.com">
          </docuseal-form>
        </body>
      </html>
      ''';

    super.initState();
    _controller = WebViewController()
      ..setJavaScriptMode(JavaScriptMode.unrestricted)
      ..setBackgroundColor(const Color(0x00000000))
      ..setNavigationDelegate(
        NavigationDelegate(
          onNavigationRequest: (NavigationRequest request) {
            return NavigationDecision.navigate;
          },
        ),
      )
      ..loadHtmlString(html);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: const Text("DocuSeal Demo"),
          actions: const [],
        ),
        body: WebViewWidget(controller: _controller!));
  }
}
```
