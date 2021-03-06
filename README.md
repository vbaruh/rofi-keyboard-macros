# what

rofi-keyboard-macros is a bash script which provides selection of keyboard macro,
which will be executed with 'xdotool type'.

# how to

The script uses a directory as a repo for keyboard macros and helper list scripts.
This is script's *macro directory*.

## configuration

The script is configured via environment variables.

### ROFI_KM_MACRO_DIR

ROFI_KM_MACRO_DIR could be set to a directory with macro and list files.

If the variable is not initialized, then the script will default to a directory
under rofi's config directory, which means:
 - $XDG_USER_CONFIG_DIR/rofi/macros
or
 - $HOME/.config/rofi/macros

### ROFI_KM_MODE

Defines rofi-keyboard-macros mode, see below for more details.

### ROFI_KM_DMENU_CMD

Defines how to launch rofi in dmenu mode. The default value ('rofi -i -dmenu')
starts rofi in case-insenstive dmenu mode, assuming that rofi is on the $PATH.


## macro files

The script looks for two type of files in the macro directory - macro and
list files.

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
a .2step.list or a .list file.

For example '<<list_docker_ps>>' is a list placeholder, because it matches the
regex pattern '<<list_(.*)>>. The regex group match is the name of a list.
In this example the name is docker_ps, which is a reference to a file named
docker_ps.list under the macro directory.

### .list files
The .list files are scripts which should echo a list of options which will be
supplied to another 'rofid -dmenu' call. The placeholder will be replaced with
the selected option.

Example macro with list placeholder is 'docker logs -f <<list_docker_ps>>'.
So if the docker_ps.list script executes 'docker ps --format '{{ .Names }}''
then the 'rofi -dmenu' call will show list of running container names.

### .2step.list files

A 2step list used in two steps to get a value to replace the list
placeholder.

In the first step it will be executed without parameters
and it should return a list for selection. Items (lines) in this
list could be a lot more descriptive, they could be rows from a table.

In the second step the script will be executed with the selected by
the user line. The script should parse this line and return a string
which will replace the placeholder.

## modes

rofi-keyboard-macros supports two modes - CASCADE and FLAT.

In CASCADE mode macro selection happens in two steps - first a macro group
is selected and in the second step a macro is selected.

In FLAT mode macro selection happens in single step from a flattened macro list.
In this list each item is in format "<macro group name>|<macro>".
