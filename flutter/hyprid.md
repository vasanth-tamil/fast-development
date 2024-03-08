### Hyprid App

**Plugins**

```dart
webview_flutter: ^4.7.0
connectivity_plus: ^5.0.2
```

**code**

```dart
import 'package:connectivity_plus/connectivity_plus.dart';
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';

class InitScreen extends StatefulWidget {
  const InitScreen({Key? key}) : super(key: key);

  @override
  State<InitScreen> createState() => _InitScreenState();
}

class _InitScreenState extends State<InitScreen> {
  bool loading = false;
  bool isConnected = false;
  bool isError = false;
  late WebViewController controller;

  @override
  void initState() {
    initWebController();
    checkConnectivity(); // Check initial connectivity status
    Connectivity().onConnectivityChanged.listen((result) {
      setState(() {
        isConnected = (result != ConnectivityResult.none);
      });
      controller.reload();
    });
    super.initState();
  }

  // check internet connectivity
  Future<void> checkConnectivity() async {
    var connectivityResult = await Connectivity().checkConnectivity();
    setState(() {
      isConnected = (connectivityResult != ConnectivityResult.none);
    });
  }

  void initWebController() {
    controller = WebViewController()
      ..setJavaScriptMode(JavaScriptMode.unrestricted)
      ..setBackgroundColor(const Color(0x00000000))
      ..setNavigationDelegate(
        NavigationDelegate(
          onProgress: (int progress) {},
          onPageStarted: (String url) {
            setState(() {
              loading = true;
            });
          },
          onPageFinished: (String url) {
            setState(() {
              loading = false;
            });
          },
          onWebResourceError: (WebResourceError error) {
            setState(() {
              isError = true;
            });
          },
          onNavigationRequest: (NavigationRequest request) {
            return NavigationDecision.navigate;
          },
        ),
      )
      ..loadRequest(
          Uri.parse('https://apartment.projectfun.website/dashboard'));
  }

  @override
  Widget build(BuildContext context) {
    return PopScope(
      canPop: false,
      onPopInvoked: (e) {
        controller.goBack();
      },
      child: RefreshIndicator(
        onRefresh: () async {
          controller.reload();
        },
        child: SafeArea(
          child: Scaffold(
            body: isConnected
                ? loading
                    ? const Center(
                        child: CircularProgressIndicator(),
                      )
                    : isError
                        ? Center(
                            child: Column(
                              mainAxisAlignment: MainAxisAlignment.center,
                              children: [
                                const Icon(
                                  Icons.wifi_off_rounded,
                                  size: 40,
                                ),
                                const SizedBox(height: 15),
                                const Text(
                                  "No Internet Connection",
                                  style: TextStyle(
                                    fontSize: 20,
                                    fontWeight: FontWeight.bold,
                                  ),
                                ),
                                const Text(
                                  "Please check your internet connection and try again.",
                                ),
                                TextButton(
                                  onPressed: () async {
                                    isError = false;
                                    await controller.reload();
                                  },
                                  child: const Row(
                                    mainAxisAlignment: MainAxisAlignment.center,
                                    children: [
                                      Icon(Icons.refresh),
                                      Text("Reload"),
                                    ],
                                  ),
                                )
                              ],
                            ),
                          )
                        : WebViewWidget(
                            controller: controller,
                          )
                : Center(
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      crossAxisAlignment: CrossAxisAlignment.center,
                      children: [
                        const Icon(
                          Icons.wifi_off_rounded,
                          size: 40,
                        ),
                        const SizedBox(height: 15),
                        const Text(
                          "No Internet Connection",
                          style: TextStyle(
                            fontSize: 20,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        const Text(
                          "Please check your internet connection and try again.",
                        ),
                        TextButton(
                          onPressed: () async {
                            isError = false;
                            await controller.reload();
                          },
                          child: const Row(
                            mainAxisAlignment: MainAxisAlignment.center,
                            children: [
                              Icon(Icons.refresh),
                              Text("Reload"),
                            ],
                          ),
                        )
                      ],
                    ),
                  ),
          ),
        ),
      ),
    );
  }
}

```
