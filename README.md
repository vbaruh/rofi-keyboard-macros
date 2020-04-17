
rofi-keyboard-macros is a bash script which provides selection of keyboard macro,
which will be executed with 'xdotool type'.

# how to

The script uses a directory as a repo for keyboard macros and helper list scripts.
This is script's *macro directory*.

The macro directory is within rofi's config directory, which means:
 - $XDG_USER_CONFIG_DIR/rofi/macros
or
 - $HOME/.config/rofi/macros


The script looks for two type of files in the macro directory.

1. .macro files

A macro file should has .macro extension.
rofi-keyboard-macros initially show list of all macro files.

Each line  a .macro file is a keyboard macro which will be typed with
'xdotool type' command.

A macro supports two type of place holders.

The first one is <<input>> placeholder.
If your macro contains this placeholder rofi-keyboard-macros will run a rofi
to take your input.
For example one can define a macro 'docker kill <<input>>'. The <<input>>
placeholder will be replaced with what has been entered.

The second placeholder is the list placeholder. A list placeholder refers to
a .list file.
For example '<<list_docker_ps>>' is a list placeholder, because it matches the
pattern '<<list_(.*)>>. The regex group match is the name of a list.
In this example the name is docker_ps, which points to docker_ps.list file.

So the .list files are scripts which return a list of options which will be
supplied to another 'rofid -dmenu' call.

Example macro with list placeholder is 'docker logs -f <<list_docker_ps>>'.
So if the docker_ps.list script executes 'docker ps --format '{{ .Names }}''
then the 'rofi -dmenu' call will show list of running container names.
