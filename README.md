# volume_limiter

The files in this repo are compiled to binary and you need to use ***Script Editor*** on MacOS to open them

I have the raw javascript at the bottom of this readme

Edited script from [Lucas Andión ](https://medium.com/@andion) and [Ranhiru Jude Cooray](https://medium.com/@ranhiru)

Used Ranhiro's [blog post](https://medium.com/@ranhiru/limiting-your-macs-headphone-volume-13671b64e569) for the headphone specific code [Andión's](https://medium.com/trabe/limiting-your-macs-volume-in-2019-f314e20408ab) for the base code


You need to install [switchaudio](https://formulae.brew.sh/formula/switchaudio-osx) first, I use home-brew.

```bash
> brew install switchaudio-osx
```

```this.app.doShellScript``` in ```get audioSource()``` runs a shell script to switch the audio source using your path to the package, and your chosen audio source.

The default path is ```pathToSwitchAudioSource = "opt/homebrew/cellar/switchaudio-osx/1.1.0/SwitchAudioSource"``` since I used brew install to install the package

And the default audio source ```yourAudioSource = "External Headphones"```

If you want to see your available options just run the following command in your terminal

```bash
> switchaudiosource -a
```


Last steps:

Go to Script in the menu bar and you'll see "Compile" and "Run Application". Select Compile, and then select Run Application. You should see a notification in the top right that says

Limiting your 🎧  volume to <your volume>% to protect your ears

When you save, make sure to set the File Format to Application

And in the Options, check Stay Open After Run Handler so it runs in the background


```javascript
// brew install switchaudio-osx

class Volimiter {
  constructor(appName, maxVolume) {
    this.app = Application.currentApplication();
    this.app.includeStandardAdditions = true;
    this.appName = appName;
    this.maxVolume = maxVolume;
  }
  
 

  get currentVolume() {
    const { outputVolume } = this.app.getVolumeSettings();
    return outputVolume;
  }

// after install, change pathToSwitchAudioSource to your path to switchaudiosource

pathToSwitchAudioSource = "opt/homebrew/cellar/switchaudio-osx/1.1.0/SwitchAudioSource"

/** run switchaudiosource -a in terminal to find current device, and edit the yourAudioSource variable to your choice
*/

yourAudioSource = "External Headphones"

  get audioSource() {
    return this.app.doShellScript('${pathToSwitchAudioSource} -s ${audioSource}');
    ;
  }


// failsafe in case you ran the wrong script
  limitVolume() {
    if (!this.audioSource) {
      return;
    }

/**
resets your volume to below the limit, if you exceed your limit. You can see on the interface for macOS that the volume squares won't fill the bar all the way if you limited the maxVolume to below 100%
*/
    if (this.currentVolume > this.maxVolume) {
      this.app.beep();
      this.app.setVolume(null, { outputVolume: this.maxVolume });
    }
  }

// shows notification in top right when you launch the program
  showNotification() {
    this.app.displayNotification("", {
      withTitle: this.appName,
      subtitle: `Limiting your 🎧 volume to ${
        this.maxVolume
      }% to protect your ears`
    });
  }
}




// set the maxVolume for Volimeter to your perferred percentage, I chose 1 for 1%
const PurrfectVolume = new Volimiter("Purrfect volume 😸", 1);
PurrfectVolume.showNotification();

function idle() {
  PurrfectVolume.limitVolume();
  return 0.5;

}


```
