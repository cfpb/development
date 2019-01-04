# Browser Testing with Sauce Labs

Use Sauce Labs to manually test your application in a range of browsers and virtual machines. Automated tests can also be run in multiple browsers with Sauce Labs using their API. For examples, check out their documentation for [running frontend JavaScript unit tests](https://wiki.saucelabs.com/display/DOCS/JavaScript+Unit+Testing).


## Sign up for saucelabs.com

1. Contact Catherine Farman (catherine.farman@cfpb.gov) or Marc Esher (marc.esher@cfpb.gov) for an invite to access Sauce Labs.
2. Check your email and follow the instructions for signup.

## Set up Sauce Connect to test URLs on local and dev servers

Once you create an account, you'll need to install "Sauce Connect" so you can actually access build through Sauce Labs.

1. Download the app here: https://wiki.saucelabs.com/display/DOCS/Basic+Sauce+Connect+Proxy+Setup#BasicSauceConnectProxySetup-SettingUpSauceConnect
1. Unzip the folder and copy the unzipped folder to your user Applications directory.
1. Open the unzipped folder in Terminal. The easiest way to do this is to open Terminal, then in Finder go to the folder and drag it to the Terminal dock icon.
1. Get your username and Access Key ready: click your name on the top right when logged in on SauceLabs.com and click "My account" in the menu. Your username is at the top of the page. Scroll down to find your access key and copy it.
1. Back in Terminal, run this command, using your credentials from SauceLabs.com:
    ```
    bin/sc -u YOUR_USERNAME -k YOUR_ACCESS_KEY
    ```

1. You might get a system popup window asking for administrator login- you can hit "Cancel" and things will still work.
1. When you see this message in the command line, you're good to go: "Sauce Connect is up, you may start your tests."
1. To disconnect, use the keyboard command `CTRL-C` in the Terminal window where Sauce Connect is running.



## Store your Sauce credentials for running Sauce Connect easily in the future

1. Go to Finder and navigate to your user directory.
1. Open the `.exports` file with your text editor. Can't find it? In Finder, try entering the following keyboard command to show hidden files:
    ```
    CMD + SHIFT + .
    ```


1. Add the following to your `.exports` file (with your credentials):
    ```
    export SAUCE_USERNAME=<sauce_username>
    export SAUCE_ACCESS_KEY=<sauce_access_key>
    ```


    Sauce Connect will automatically check for the existence of those variables when it starts up.


## Create an alias for running Sauce Connect easily in the future

1. Go to Finder and navigate to your user directory.
1. Open the `.aliases` file with your text editor. Can't find it? In Finder, try entering the following keyboard command to show hidden files:
    ```
    CMD + SHIFT + .
    ```

1. Add the following line of code to your `.aliases` file:
    ```
    alias saucy="~/Applications/sc-4.4.12-osx/bin/sc"
    ```

You may need to update the path from `~/Applications/sc-4.4.12-osx/` to match wherever your Sauce Connect app was downloaded, if you did not move it to your Applications folder, or if the version number you downloaded is different.
1. In a new Terminal window, test out your command:
    ```
    saucy
    ```
1. When you see this message in the command line, you're good to go: "Sauce Connect is up, you may start your tests."
1. You can edit the name of the alias ("saucy") to whatever you like by editing the `.aliases` file.


## Testing on Sauce Labs

1. From SauceLabs.com, click the "Live testing" tab.
1. Enter the URL you want to test.
1. Select your browser and device.
1. If you are testing a local or dev server URL (such as build), under "Sauce Connect proxy," click the dropdown and select your username from the list. If you are testing a public URL, you can skip this step.
1. Click "Start session."


