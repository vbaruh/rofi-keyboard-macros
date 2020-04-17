# what

rofi-keyboard-macros is a bash script which provides selection of keyboard macro,
which will be executed with 'xdotool type'.

# how to

The script uses a directory as a repo for keyboard macros and helper list scripts.
This is script's *macro directory*.

The macro directory is within rofi's config directory, which means:
 - $XDG_USER_CONFIG_DIR/rofi/macros
or
 - $HOME/.config/rofi/macros


The script looks for two type of files in the macro directory - macro and
list files.

## macro files

A macro file should has .macro extension.
A macro file represents a group of macros.

When started, rofi-keyboard-macros shows list of all macro files,
so one can select a group from which to pick a kebyoard macro.

Each line in a .macro file is a keyboard macro.

Selected kebyoard marro will typed with 'xdotool type' command.

A macro supports two type of place holders which will be populated with
a follow up user interaction.

The first one is '&lt;&lt;input&gt;&gt;' placeholder.

If a macro contains this placeholder rofi-keyboard-macros will run a rofi
to take your input.

For example one can define a macro 'docker kill &lt;&lt;input&gt;&gt;'. The '&lt;&lt;input&gt;&gt;'
placeholder will be replaced with what has been entered.

## list files

The second placeholder is the list placeholder. A list placeholder refers to
a .list file.

For example '<<list_docker_ps>>' is a list placeholder, because it matches the
regex pattern '<<list_(.*)>>. The regex group match is the name of a list.
In this example the name is docker_ps, which is a reference to a file named
docker_ps.list under the macro directory.

The .list files are scripts which should echo a list of options which will be
supplied to another 'rofid -dmenu' call. The placeholder will be replaced with
the selected option.

Example macro with list placeholder is 'docker logs -f <<list_docker_ps>>'.
So if the docker_ps.list script executes 'docker ps --format '{{ .Names }}''
then the 'rofi -dmenu' call will show list of running container names.
