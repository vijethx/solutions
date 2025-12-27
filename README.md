# Solutions

A repository of all the solutions I found (figured) out while solving a problem

## git

1. Meaning of the GitHub message: push declined due to email privacy restrictions

   ![alt text](image.png)

   [StackOverflow](https://stackoverflow.com/a/44099011)

## linux (Pop!\_OS 22.04)

(these problems & their respective solutions were discovered in Pop!\_OS 22.04. these may or may not work for other distros or versions)

1. `initramfs-tools` error while installing anything on Pop!\_OS

   found this [solution on ask Ubuntu](https://askubuntu.com/questions/1136480/initramfs-error-when-installing-updating), but didn't work for me

   I had to do a fresh install of Pop!\_OS from the recovery mode, because the Refresh option in the settings was also not responding

   to get to `recovery mode` on Pop!\_OS 22.04, turn off the machine, turn on and start hitting the spacebar till you find a boot menu, navigate to Pop!\_OS recovery mode

2. google chrome does not open after changing machine name

   weird one, took me a while to figure out. currently reverted back to the old machine name fixes the issue

3. enabling per monitor fractional scaling

   Pop!\_OS ships with `xorg` by default which has problems implementing per monitor fractional scaling

   simplest solution is to switch to `wayland`, reboot your machine and then its pretty much straight forward after that

   to check if you are using `wayland` or `xorg`, run the following command in the terminal

   ```bash
   $ echo $XDG_SESSION_TYPE

   # If it returns wayland, you're using Wayland.
   # If it returns x11, you're using Xorg.
   ```

   Enabling Wayland

   In the terminal, run this command to edit the GDM configuration file

   ```bash
   $ sudo nano /etc/gdm3/custom.conf

   # you can use any text editor - vim, nano, gedit, code etc.,
   ```

   find this line

   ```ini
   WaylandEnable=false
   ```

   Change it to

   ```ini
   #WaylandEnable=false
   ```

   Save and Exit. Then reboot the system.

   At login, before entering your password, click the Gear Icon at the bottom right

   Select `Pop!\_OS on Wayland" and Log In

   Finally, go to Settings -> Displays and adjust the monitor resolutions

4. setting keyboard shortcuts for Night Light

   Turn on Night Light `gsettings set org.gnome.settings-daemon.plugins.color night-light-enabled true`

   Turn off Night Light `gsettings set org.gnome.settings-daemon.plugins.color night-light-enabled false`

   Go to Settings -> Keyboard -> Keyboard Shortcuts -> Custom Shortcuts

   Add two new shortcuts namely `Night Light ON` and `Night Light OFF`, use the above commands and set any shortcuts you like. I've set it to `Alt + N` to turn on and `Alt + M` to turn off.

## linux (Fedora Workstation 43)

(these problems & their respective solutions were discovered in Fedora Workstation 42. these may or may not work for other distros or versions)

1. flatpak warning "wrong layer checksum" (_Fedora Workstation 42_)

   indicates that the downloaded data for a Flatpak application or runtime does not match the expected checksum. This usually means the downloaded files are corrupted or incomplete, preventing Flatpak from verifying their integrity and ensuring a safe installation or update.

   Troubleshooting Steps:

   - Retry the Update/Installation: The simplest solution is often to try updating or installing the Flatpak again. Network issues might be temporary.
     ```bash
     flatpak update
     ```
   - Repair Flatpak: Use the flatpak repair command to check for and attempt to fix corrupted files in your Flatpak installation.
     ```bash
     sudo flatpak repair --system
     ```

2. installing Waydroid on Fedora 43

   try only if the official [instructions](https://docs.waydro.id/usage/install-on-desktops#fedora) do not work

   Install Waydroid from the official package repository

   ```bash
   sudo dnf update # just a precautionary measure
   sudo dnf install curl ca-certificates # if not already installed
   sudo dnf install waydroid
   ```

   Download System and Vendor Images from Sourceforge

   [System Image](https://sourceforge.net/projects/waydroid/files/images/system/lineage/waydroid_x86_64/)

   [Vendor Image](https://sourceforge.net/projects/waydroid/files/images/vendor/waydroid_x86_64/)

   Extract these images and keep ready

   Now, open the terminal/console in the directory where the image files are stored.

   Create these two directories, which Waydroid will read during initialization

   ```bash
   sudo mkdir /etc/waydroid-extra
   sudo mkdir /etc/waydroid-extra/images
   ```

   Move (or Copy) the two image files to the newly created directory

   ```bash
   sudo mv system.img vendor.img /etc/waydroid-extra/images

   # Verify if the files are moved (optional)
   ls /etc/waydroid-extra/images
   ```

   Finally, run the setup command and Fedora will create the Android container and prepare the environment

   ```bash
   sudo waydroid init -f
   ```

   Close the terminal once the initialization is done. Open Waydroid from the Applications menu.

## web

1. 'React' must be in scope while using JSX

   Came across this error when I was building a Todo App with ViteJS w/ React and added an ESLint configuration. The solution was to either add a couple rules to the `eslint.config.js` file or use `import React from 'react'` in all the files.

   ```js
   // eslint.config.js

   import js from "@eslint/js";
   import globals from "globals";
   import reactHooks from "eslint-plugin-react-hooks";
   import reactRefresh from "eslint-plugin-react-refresh";

   export default [
   	{ ignores: ["dist"] },
   	{
   		files: ["**/*.{js,jsx}"],
   		languageOptions: {
   			ecmaVersion: 2020,
   			globals: globals.browser,
   			parserOptions: {
   				ecmaVersion: "latest",
   				ecmaFeatures: { jsx: true },
   				sourceType: "module",
   			},
   		},
   		plugins: {
   			"react-hooks": reactHooks,
   			"react-refresh": reactRefresh,
   		},
   		rules: {
   			...js.configs.recommended.rules,
   			...reactHooks.configs.recommended.rules,
   			"no-unused-vars": ["error", { varsIgnorePattern: "^[A-Z_]" }],
   			"react-refresh/only-export-components": [
   				"warn",
   				{ allowConstantExport: true },
   			],

   			// new rules

   			"react/react-in-jsx-scope": "off", // No need to import React in React 19+
   			"react/jsx-filename-extension": [1, { extensions: [".js", ".jsx"] }],
   		},
   	},
   ];
   ```

2. set prettier as the default formatter for everything in vscode

   Open settings by clicking the cog in the bottom left of the vs code side bar and selecting settings from the menu, or by hitting `Ctrl + ,`

   At the top right of the settings pane, hit the open file icon (if you hover, the tooltip will read 'Open Settings (JSON)'

   Add the following line to the settings json:

   `"editor.defaultFormatter": "esbenp.prettier-vscode"`

3. html images not rendering

   change image path name from `/path/to/image.png` to `path/to/image.png` i.e., do not add a slash before the path
