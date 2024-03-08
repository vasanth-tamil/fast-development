### Flutter Deployment

**step 1**

```text
flutter build web
```

**step 2 ( edit build/web/index.html )**

```
<base href="/flutter-web">
```

**step 3**

```
cp build/web/* server/flutter-web/
```

**step 4**
