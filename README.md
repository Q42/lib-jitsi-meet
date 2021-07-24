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

Go to ScreenObtainer.js around line 225 and replace
```
      const constraints = {
            video,
```

with 
```
      const constraints = {
            video: { width: 1280, height: 720 },
```

it hardcodes screensharing to 720p, jitsi is a config hell and this is the best option until they allow you to do from config (they do crazy config filtering for desktop)

## Publishing package
After doing all of the above there is only one more step to publish the package to NPM and use it in the project. Run the following commands to make sure we're publishing the right version and it's not broken:

```
npm install
npm run build
```

Finally we can publish the package:

```
npm publish
```
