---
title: Linux PVR – Part 2
author: drrandom

date: 2007-06-10T13:17:08+00:00
url: /2007/06/10/linux-pvr-part-2/
tags:
  - Uncategorized

---
I've been doing a lot of poking around the [MythTV ](1) and [Knoppmyth ](2) sites recently trying to figure out what is going to work best in my current situation.  Since I've got cable, and not satellite, it would seem that it is possible to get all of the non-encrypted content from my provider, as long as I have a capture card with QAM support.  These cards seem to be HD cards overall (at least I haven't found one that is supported that is not HD), which makes sense given the QAM signal will be a digital signal.  It sounds like getting the channel mapping put together for the QAM content can be somewhat tedious depending on how well Myth can auto-discover your channels (which it would seem is not that good), so I know there is some work involved in getting the digital channels.  For analog, any of the supported analog capture cards would work, and it seems that my wife and I tend to watch more of the analog stations than the digital (though, in my current line-up all of the &#8220;expanded&#8221; cable stations (Discovery, Comedy Central, TBS, etc) have both an analog station and a digital station...pretty strange.  I also know that I want the ability to watch and record at the same time, which means at least two tuners.  Here is the trick, though, since I need different tuners for digital vs analog, how many tuners do I really need?  And how many PCI slots do I want to eat up in the quest for multiple record?

So here is what I've decided.  Since I'm mostly concerned with analog, I went with a [Hauppauge ](3) WinTV PVR-500, which is a dual-tuner analog PCI card, and which seems to be supported just fine in myth (looks like two PVR-150 cards from the sound of it).  That by itself takes care of my analog needs.  As for the digital, I haven't decided for sure yet, but I'm leaning towards the [DVICO FusionHDTV RT Lite, ](4) which seems to have good support in Myth.  It also has the advantage of being one of the few HD cards with a hardware encoder.

I should take a step back here and explain this.  You have two choices when watching or recording TV on a PC.  You can have your machine take care of encoding and decoding the MPEG4 video that is your TV show, or you can let the capture card do it.  If you let the capture card do it, then your machine is going to not be nearly as busy as it would otherwise.  This presents a couple advantages: 1.  You can get away with less horse-power on the machine, which means parts will be cheaper (spend the cash on storage if you can). 2. You're machine will consume less power (I'm talking wattage here), and so therefore you are less likely to have a machine in your living room that sounds like a Lear jet taking off when your trying to watch all of your recorded episodes of &#8220;Eating bugs with the Stars&#8221;, or whatever the latest reality fad is.  

The PVR-500 contains a hardware encoder, so if I can get the same thing on the digital side, I'll be in good shape.  While I'm on the subject of power, I'm also seriously looking at an AMD Turion based system.  This is the AMD mobile processor, so it is designed with a low-power footprint in mind.  It also seems that there are several motherboards out there which will support it, and some additional components which will help keep it cool.  The only other major sources of noise are the power-supply, and the hard-drive.  Cooler-Master makes some nice quite power supplies, so I will probably check there first.  As for hard drives, I haven't really started looking yet.  If I can find noise specs on them, then I may use that as a criteria when deciding which to buy, but my primary concern is how many GB can I get for my money.

The other components have been moved to the end of the decision-tree.  I like the Sliverstone cases I mentioned in my last post, but I decided that I should get the components together first, since I didn't really want to end up with a nice-looking case where I couldn't fit all of my goodies.  I also need to decide whether or not to go with a DVD writer in the case....could be handy, but then I can also grab stuff off the network, so is it really needed (I've got a CD writer lying around, which may be my stand-in for a while)?

Overall this is going to be a fun project, and lets face it, how often do you get a geek project that your wife is behind 100%?

 [1]: http://www.mythtv.org
 [2]: http://www.mysettopbox.tv/knoppmyth.html
 [3]: http://www.hauppauge.com
 [4]: http://www.fusionhdtv.co.kr/eng/Products/RTLite.aspx