# Jitsi Meet API library
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

One if the first things we need to do is make sure that we use the `@jitsi/js-utils` npm package instead of the `js-utils`. This package is already used in some newer versions, but older versions do not use it. The main downside of the old `js-utils` library is the sporadic use of Flow. Which requires you to setup Flow in your own project. To do this run:

```
npm uninstall js-utils
npm install @jitsi/js-utils
```

After which you can replace all imports in the project that used `js-utils` with `@jitsi/js-utils`.

The final steps is updating the `package.json` so that it reflects the correct name and version of the package. Furthermore the `postinstall` command in the `package.json` requires webpack to be in the project, which is also not needed in our case.

Let's start with updating the name and version of the package, by replacing all existing values with these new values:

```
"name": "@q42/lib-jitsi-meet",
"version": "2.0.XXXX",
"description": "Borrel fork for accessing Jitsi server side deployments",
"repository": {
    "type": "git",
    "url": "https://github.com/Q42/Borrel.JitsiMeet.Lib"
}
```

Again, replace the X's with the correct version number so that it is clear which version is published. Now let's replace the `postinstall` command with `build` so that it can be called still.

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
