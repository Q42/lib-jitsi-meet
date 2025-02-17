# Borrel Jitsi Meet API library
You can use Jitsi Meet API to create Jitsi Meet video conferences with a custom GUI.

## Picking the right version
It is important to pick the correct version of the API library that fits your Jitsi Meet Server. Your Jitsi Meet Server already comes with the appropriate minified version of javascript to be used in the client.
This version is also the version number we want to use. To find it you can type:

```
dpkg -l | grep jitsi
```

Find the line called `jitsi-meet-web` and the appriopriate version number next to it. It should look something like this: `1.0.XXXX-1`. The numbers marked with the X's is the number you want to use as the Jitsi Meet API library.

## Checking out the correct version tag
Now that we know the version number, it is important to check out the correct version number and be able to publish it to NPM.

You can checkout the correct version using the following command:

```
git checkout tags/jitsi-meet_XXXX -b version/XXXX
```

Ofcourse replace the X's with the correct version number.

## Making the package ready for publishing
The default Jitsi Meet API library is developed to be used directly in the Jitsi Meet Server environment. This means that it is not always easy to re-use in other projects because of certain requirements. It is quite easy to work around these requirements because the Jitsi development team is also working hard on cutting down these dependancies.

The only step required to do so is updating the `package.json` so that it reflects the correct name and version of the package. Furthermore the `postinstall` command in the `package.json` requires webpack to be in the project, which is also not needed in our case.

Let's start with updating the name and version of the package, by replacing all existing values with these new values:

```
"name": "@mibo/lib-jitsi-meet",
"version": "2.0.XXXX",
"description": "Mibo fork for accessing Jitsi server side deployments",
"repository": {
    "type": "git",
    "url": "https://github.com/getmibo/Mibo.JitsiMeet.Lib"
}
```

Again, replace the X's with the correct version number so that it is clear which version is published. Now let's replace the `postinstall` command with `build` so that it can be called still.

### Custom patch: Screensharing

Go to ScreenObtainer.js around line 222 and replace
```
    let video = {};
```

with 
```
let video = {
            width: 1280,
            height: 720,
            frameRate: {
                min: 5,
                max: 24
            }
        };
```

And comment out lines below where the video.height and video.width is set to 99999

```
 if (browser.isChromiumBased()) {
            // Allow users to seamlessly switch which tab they are sharing without having to select the tab again.
            browser.isVersionGreaterThan(106) && (video.surfaceSwitching = 'include');

            // Set bogus resolution constraints to work around
            // https://bugs.chromium.org/p/chromium/issues/detail?id=1056311 for low fps screenshare. Capturing SS at
            // very high resolutions restricts the framerate. Therefore, skip this hack when capture fps > 5 fps.
            // if (!(desktopSharingFrameRate?.max > SS_DEFAULT_FRAME_RATE)) {
            //     video.height = 99999;
            //     video.width = 99999;
            // }
        }
```

it hardcodes screensharing to 720p, jitsi is a config hell and this is the best option until they allow you to do from config (they do crazy config filtering for desktop)

⚠️ Beware the receiving end might also constrain the incoming stream by calling `setReceiverConstraints`. If we up the screenshare video, we also should check what that constraint is set to.

## Publishing package
After doing all of the above there is only one more step to publish the package to NPM and use it in the project. Run the following commands to make sure we're publishing the right version and it's not broken:

```
npm install
npm run build
```

⚠️ If it is not building, update to the latest npm temporarily with `npm install -g npm`, build and then install npm v6 again to run our own code.

Finally we can publish the package:

```
npm publish
```
