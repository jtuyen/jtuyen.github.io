+++
title = "TotalRecall: Recall is worst than I thought"
date = 2024-06-06
[taxonomies]
tags = ["microsoft", "privacy", "AI", "infostealer"]
+++

[TotalRecall - a 'privacy nightmare'?](https://github.com/xaitax/TotalRecall)

> Windows Recall stores everything locally in an unencrypted SQLite database, and the screenshots are simply saved in a folder on your PC. Here’s where you can find them: `C:\Users\$USER\AppData\Local\CoreAIPlatform.00\UKP\{GUID}`. The images are all stored in the following subfolder `.\ImageStore\`

> TotalRecall copies the databases and screenshots and then parses the database for potentially interesting artifacts. You can define dates to limit the extraction as well as search for strings (that were extracted via Recall OCR) of interest. There is no rocket science behind all this. It's very basic SQLite parsing.

> During testing this with an off the shelf infostealer, I used Microsoft Defender for Endpoint — which detected the off the shelve infostealer — but by the time the automated remediation kicked in (which took over ten minutes) my Recall data was already long gone.

> In enterprise, you have to turn off Recall as it is enabled by default.

Well this is worst than I [thought](https://johntuyen.com/wiretap/microsoft-recall/), unencrypted SQLite and raw images stored in AppData, Microsoft didn't even bother with DPAPI. It's like they rolled out from alpha and didn't even GAF about security.

I like the blurb about infostealer. It's like a smash and grab of data and who gives AF about your endpoint security, data has been exfiltrated already.

Enterprise editions of Windows has Recall enabled by default 🤦 Enough of this shit show, let's see how they will fix this in Windows 15 😏

EDIT: just to clarify, I'm not hating on the idea that Recall or AI is useless, I think it's a useful piece of tech that will change the way you interface with your computer and offload tasks to Edge compute. Whether this is bad or good is up for the user to decide, there are choices. My gripe is how the data is being contained and handled.