# Prot16 Generator

A nimble tool to automate the process of exporting the [prot16 colour schemes](https://protesilaos.com/schemes) into a variety of file formats. The output is used to style applications such as terminal emulators or Vim.

The generator consists of a bash script which parses the sets of variables of each scheme through templates and outputs the result to the terminal (stdout).

## Usage

Start by cloning the repo:

```sh
git clone https://github.com/protesilaos/prot16-generator.git --depth 1
```

Then enter the directory (all subsequent commands assume you are working within the `prot16-generator` directory):

```sh
cd prot16-generator
```

Run the script with the necessary arguments (see following section):

```sh
./prot16-generator [scheme] [template] [variant]
```

### Available CLI arguments

For the available schemes, run `ls schemes`. Similarly, run `ls templates`. The `variant` must be either `light` or `dark`.

## Examples

Print the `gaia` theme for `vim` in its `dark` variant to the terminal output:

```sh
./prot16-generator gaia vim dark
```

Directly create a new file containing the output and place in on the `~/Desktop`:

```sh
./prot16-generator gaia vim dark > ~/Desktop/gaia_dark.vim
```

## Applying the themes

Each application uses a different set of conventions. Below are some tried and tested examples that I have run on Arch Linux as well as Debian and Debian-based distros (Ubuntu and Linux Mint). Technically these should be distro-agnostic, though I cannot be certain of that (please report any issues).

### RXVT-Unicode (urxvt)

Urxvt saves colour values in either of two places. The most common use case is within the `~/.Xresources`. Since that file can contain all sorts of configurations, it is best to *append* the output of the prot16-generator rather than overwrite its contents.

As such, run the following command:

```sh
./prot16-generator gaia urxvt dark >> ~/.Xresources
```

Notice the use of `>>`. It is what appends the output to the file. If your `.Xresources` is empty, then just run the following instead (notice the single `>`):

```sh
./prot16-generator gaia urxvt dark > ~/.Xresources
```

The other approach to having colour values for urxvt is to source an `.Xcolors` file from within the `.Xresources`. The file can be located anywhere. For the purposes of this tutorial, it is assumed you have created a directory `~/.urxvt-themes/`:

```sh
# Create directory for urxvt Xcolors
mkdir ~/.urxvt-themes

# Generate the desired theme and place it in the new directory
./prot16-generator gaia urxvt dark > ~/.urxvt-themes/gaia_dark.Xcolors
```

Then source that file from within the `.Xresources` with the following line (note that comments in `.Xresources` start with a `!`, not an `#`):

```
#include </home/USERNAME/.urxvt-themes/gaia_dark.Xcolors>
```

Whatever method you use, do not forget to reload the configurations, with `xrdb -merge ~/.Xresources` (may need to close all terminals and re-open them).

### Xterm

Much like urxvt, xterm stores its configurations in `~/.Xresources`. Append the theme of your choice to the existing configs with the following:

```sh
./prot16-generator gaia xterm dark >> ~./Xresources
```

### Xfce4-terminal

The theme files can be located in either of two places:

- At `/usr/share/xfce4/terminal/colorschemes/` which makes them accessible to all users (requires root privileges).
- Or `~/.local/share/xfce4/terminal/colorschemes/` for use by the current user (directory path needs to be created if it does not already exist).

Choose whatever option suits your needs, modifying the following command accordingly:

```sh
# Create directory if it does not already exist (ONLY for the .local option)
mkdir -p ~/.local/share/xfce4/terminal/colorschemes/

# Generate the theme and place it in the created directory
./prot16-generator gaia xfce4-terminal dark > ~/.local/share/xfce4/terminal/colorschemes/gaia_dark.theme
```

The theme will then be available from the terminal's preferences panel. Specifically, open the Xfce4 Terminal, navigate to `Preferences` > `Colours`. The themes should be available in the `Presets` section.

### Vim

The theme files can be copied manually or installed as a bundle with a plugin.

The manual method requires you to copy the file to `~/.vim/colors/`. Run the following command:

```sh
# Create path to colors directory if it does not already exist
mkdir -p ~/.vim/colors/

# Generate theme and place it in the newly created directory
./prot16-generator gaia vim dark > ~/.vim/colors/gaia_dark.vim
```

As for the plugin, you can use your favourite plugin manager. With [vim-plug](https://github.com/junegunn/vim-plug) add the following line to your `.vimrc`:

```vim
Plug "protesilaos/prot16-vim"
```

Then execute the plugin manager's command to update the plugin files.

Once available, the theme is declared with the following options inside the `.vimrc`:

```vim
" Theme
syntax enable
colorscheme gaia_dark
```

## Roadmap (help is much appreciated)

- Refine the code.
- Improve documentation.
- Improve templates where possible.
- Produce templates for other popular apps (Termite, Kitty, Tilix, etc.).
- Packages for various GNU/Linux distros (if packages are an option, there needs to be a manpage and a more streamlined CLI experience, such as `prot16 --help` `prot16 [options]`).
- Anything else?

## Changelog

Refer to the [CHANGELOG.md](https://github.com/protesilaos/prot16-generator/blob/master/CHANGELOG.md)

## License

GNU General Public License Version 3. 

See [LICENSE](https://github.com/protesilaos/prot16-generator/blob/master/LICENSE)

## Miscellaneous

The Prot16 Generator replaces the [prot16-builder](https://github.com/protesilaos/prot16-builder)
