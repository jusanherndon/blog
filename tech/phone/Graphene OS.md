This is currently what I use on my Pixel 8a phone.

# GrapheneOs project

The [Graphene OS Projects](https://grapheneos.org/) is a fork of the [AOSP](https://source.android.com/) or Android Open Source project. Its main goal is to be a security focused fork of Android, while still trying to maintain compadibility with the operating system.

# Notable features

- [Sandboxed Google Play](https://grapheneos.org/usage#sandboxed-google-play)
- [Storage Scopes](https://grapheneos.org/usage#storage-access)
- [Contact Scopes](https://grapheneos.org/usage#contact-scopes)
- [Accessibility](https://grapheneos.org/usage#accessibility)
- [WiFi Privacy](https://grapheneos.org/usage#wifi-privacy)
- [Hardened Malloc](https://github.com/GrapheneOS/hardened_malloc)

And so much more. But I figured I would focus on these for now and then add more once I learn more about Graphene

# Why Choose GrapheneOS

I feel very overwhelmed by the current state of the internet and the world in general. Most companies are spying on you and selling location data, buyer preferences, and many other things that we are not aware of. But in spite of that, I would like to do whatever I can to get a little bit of privacy back into my life. Through using and giving feedback on this operating system is one way I can do that. 

# Sandboxed Google Play

Google Play services by default on Android has control over your whole phone on Android. And since it is a proprietary app owned by google, we have no idea what it does. Graphene basically puts it in a special isolated container, so it can limit the data collection it does.

# Storage scopes and Contact Scopes

These two are similar, so I am grouping them together. Basically you want to limit access that apps have and scopes allow you to not let a banking app have access to your contacts. Why would a banking app want to see who you know anyway?

# WiFi Privacy

Their are a lot of features locked up in this one, but I especially like them since I do QA testing on Routers. It would probably just be easier to list them

- Random MAC addresses' every time you connect to a WiFi network
- Applies fixes to IPv6 privacy addresses in the Linux Kernal
- Private network locations through a graphene hosted server

And many more features

# Hardened Malloc

I do not know much about C's implementation of memory addressing so that is why I left a link to the GitHub source code. They are way better at explaining it then I can right now. *Future Justin will give an attempt later*
