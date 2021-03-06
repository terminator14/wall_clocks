Known Bugs::

	If the script is run manually more often than once a minute, it will use the pre-generated image that it had just created, so the clock will show one minute ahead
		No matter how many times you run it, it should only be a maximum of 1 minute ahead - the problem doesn't get compounded
		It resolves itself after a minute - once it catches up, it'll resync
		If I want to fix it, I see two solutions:
			One: 
				Only run script if the argument --cron is passed to it, and add it to cron
					This will prevent people from running it manually
			Two:
				I should have a way to verify that a pre-gen.png file is actually set at the right time before using it
					The only way I can think of is png metadata
						Basically, when I create a pre-gen.png file, label it with the hour, minute, day, and month in metadata
						Then before I load it, figure out the current hour, minute day and month, and compare it to the pre_gen.png file

To Do::

	Test with other clocks from http://www.vladstudio.com/wallpaperclocks/browse.php

	Test with other OS's
		Will almost definately not work because they don't have mate, and the gsettings line I use to set the wallpaper uses mate in its path
			Easily fixed by using "gsettings set org.gnome.desktop.background picture-uri file://$PATH_TO_FILE" instead

	Add a bit of code to rc.local to update the script on boot so that it doesn't take an entire minute (worst case) to update the clock

	I thought about making it update once a second (using "sleep 1" instead of cron) but this introduces several problems:
		Cron self synchronizes, because it always runs every minute on the minute. sleep 1 will keep getting more and more offset as the time it takes for the script to run adds up
		Every time there's a wallpaper update, there's a tiny disturbance on the desktop. It's like a half a second of lag, that doesn't really effect anything
			Updating the wallpaper every second, with a half second of lag each time means 50% of the time, the desktop will be lagging

	Can I just have the desktop play a video, and turn the images into a video stream? That would probably take a lot more resources, but might be more smooth as there is no transition
