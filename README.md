# volume_limiter

Edited script from Lucas Andión 

https://medium.com/@andion

And Ranhiru Jude Cooray 

https://medium.com/@ranhiru

Used blog post for the headphone specific code

https://medium.com/@ranhiru/limiting-your-macs-headphone-volume-13671b64e569

And for the base code

https://medium.com/trabe/limiting-your-macs-volume-in-2019-f314e20408ab

You need to install switchaudio first, I use home-brew.

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
