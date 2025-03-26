# Sandboss bypass with Terminal
PoC presented at SOCON-2025 demonstrating the ability to bypass Office documents sandbox using Terminal Preferences

This project presents another alternative to sandbox bypass under the Microsoft Office context. As previously presented at SOCON-2025, this macro prompts the Folder selection for the user. This is performed until the user accepts the selection of the current folder (selected at "/"). By opening the "/" directory, Office applications will grant the ability to write to any location in the system, with the exception of 

`$HOME/Library/LaunchAgents`

`$HOME/Library/Application Scripts`

as defined in the entitlements.

Any file created under the Office application context will receive the quarantine attribute, making it not possible to execute applications. 

# The trick

It is possible to open applications using the "open" command in Office Macros. Not only that but you can also open a new window of a running application using "-n" argument. When an application is started with "open" it will run under its own context, which means that any scripts configured to launch when they are initialized are launched they will not be sandboxed.

And then we have the article published at https://theevilbit.github.io/beyond/beyond_0020/. The article mentions "If we are not sandboxed, we can write to this file, and whenever a user starts Terminal our command will be executed.", but by abusing the dialog technique we can obtain write privileges to ~/Library/Preferences/com.apple.Terminal.plist. 

![image](https://github.com/user-attachments/assets/9bb70f79-d622-49de-b954-0e4d8e9afb04)

Anything created under the sandbox context will receive the quarantine attribute. However, _plist_ are still interpreted even if they have the attribute, thus, allowing the technique in here.

## Steps

1. Follow the instructions here to create your bash command in Terminal Preferences: https://theevilbit.github.io/beyond/beyond_0020/
2. Base64 encode ~/Library/Preferences/com.apple.Terminal.plist
3. Host it in your remote server
4. Update the macro with the endpoint where the base64 payload is hosted.
