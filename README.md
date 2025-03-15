# Solutions

A repository of all the solutions I found (figured) out while solving a problem

## git

1. Meaning of the GitHub message: push declined due to email privacy restrictions

   ![alt text](image.png)

   [StackOverflow](https://stackoverflow.com/a/44099011)

## linux

(all my problems and solutions are/were for Pop!\_OS 22.04, may or may not work for other distros or versions)

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

## web

1. 'React' must be in scope while using JSX

   Came across this error when I was building a Todo App with ViteJS w/ React and added an ESLint configuration. The solution was to either add a couple rules to the `eslint.config.js` file or use `import React from 'react' in all the files.

   ```js
   // eslint.config.js

   import globals from "globals";
   import pluginJs from "@eslint/js";
   import pluginReact from "eslint-plugin-react";

   export default [
   {
      files: ["**/*.{js,mjs,cjs,jsx}"],
      languageOptions: {
         globals: globals.browser,
         ecmaVersion: "latest",
         sourceType: "module"
      },
      rules: {
         "react/react-in-jsx-scope": "off", // No need to import React in React 19+
         "react/jsx-filename-extension": [1, { extensions: [".js", ".jsx"] }]
      },
      extends: [
         pluginJs.configs.recommended,
         pluginReact.configs.recommended
      ]
   }
   ];

   ```