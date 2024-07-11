---
description: Learn a little about Microsoft's feature Windows Recall
---

# Windows Recall

## What is Windows Recall?

Windows Recall allows you to search and re-engage with past content on your PC using an explorable timeline. By describing a memory, Recall retrieves the exact moment you saw it, whether it's a photo, link, or message. It takes snapshots of your screen every five seconds, storing and analysing them locally. This enables natural language searches for both text and images. For instance, if you need to remember the name of a Korean restaurant mentioned by a friend, Recall can find and display the relevant text and visuals, taking you back to the original context. This was The use of frequent snapshots to remember what you were just doing every once in a while.

<figure><img src="../.gitbook/assets/Pasted image 20240711232012.png" alt=""><figcaption><p>Image of windows recall</p></figcaption></figure>

## System requirements for Recall

Your PC needs the following minimum system requirements for Recall:

* A Copilot+ PC
* 16 GB RAM
* 8 logical processors
* 256 GB storage capacity
* To enable Recall, you’ll need at least 50 GB of storage space free Saving screenshots automatically pauses once the device has less than 25 GB of storage space

## What is the problem with this?

First of, you can clearly note the problem with the privacy breach in this implementation, everything done on your machine can effectively be retraced visually using this implementation.

### So since it had something to do with data, surely the security for the data is encrypted right?

Wrong, it was found that the images/screenshots were saved as jpg files, despite looking like gibberish hashes that seem encrypted, adding a simple `.jpg` was all it took to view the files nicely.

### What can you do with Windows Recall from an attacker's perspective?

For starters, we can view text in the snapshots, meaning any clear credentials used in logins that could be potentially get recorded. These information are clearer than your browsing history, it literally shows what you had done during your time using the feature.

The screenshots are stored locally, and developers at Microsoft claim that it is a rather safe since outsiders will need to physically access you machine and know your password in order to access the screenshots. However, physical access may not be necessary as people have developed tools to extract these screenshots, such as Total Recall tool created by @xaitax. The Total Recall tool extracts all of Windows Recall images out for the attacker.

The index based feature allows users to easily search for information among screenshots using actual image to text matching, meaning that just typing words into windows recall search bar, you can find information related to the users activities, such as Facebook, YouTube, and more private sites (you know what I mean).

Currently patches had been rolled out to make it such that the feature would no longer be enabled by default.

## Questions

* How is the data from the snapshots stored and protected on the local machine?
* Are the snapshots encrypted to prevent unauthorized access?
* What measures are in place to ensure that sensitive information within the snapshots is not exposed?
* If I were to make a policy that our company shall have this service installed on all our machines, what steps should be taken to secure it?

References

* https://github.com/xaitax/TotalRecall
* https://support.microsoft.com/en-us/windows/retrace-your-steps-with-recall-aa03f8a0-a78b-4b3e-b0a1-2eb8ac48701c
* https://krebsonsecurity.com/2024/06/patch-tuesday-june-2024-recall-edition/
