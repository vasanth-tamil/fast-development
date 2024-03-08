### react app deploy
**step 1 ( modify package.json )**
```text
"homepage": "./",
```
**step 2**
```text
npm run build
```
**step 3 ( copy all files )**
```
cp build/* server/node-app/
```