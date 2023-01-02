
# VST Plugin Security- Why you, the Digital Musician, should care

VSTs (Virtual Studio Technology) are practically synonymous with the world of audio plug-ins at this point in history. Developed by Steinberg, and premiering alongside their flagship Cubase DAW in 1996, VSTs were the first audio plugin framework to become widely accepted and supported by digital audio workstations. They allow users to add virtual instruments, effects, and other audio processing tools to their DAWs, enabling a more customizable and versatile production process. Today, VSTs are used by musicians, sound designers, and audio engineers across a variety of genres and industries, making them an essential tool for modern audio production.




{% comment %} 
[VSTs (Virtual Studio Technology)]() at this point of history are practically synonymous with the world of audio plug-ins. And for good reason: it was the first audio plugin framework to become widely accepted and supported by [digital audio workstations (DAWs)]() in the music production industry. VSTs allow users to add virtual instruments, effects, and other audio processing tools to their DAWs, allowing for a more customizable and versatile production process. Today, VSTs are used by musicians, sound designers, and audio engineers across a variety of genres and industries, making them an essential tool for modern audio production.

developed by Steinberg, and premiering alongside their flagship [Cubase]() DAW in 1996, VSTs took the music industry by 
VSTs went on to become the first
{% endcomment %} 


## The Public (non)understanding of VST plugins as potential attack vectors

So you, the musician whose been using this tech almost daily for the last decade, might be asking yourself, *"Why should I care?"*.

Well to answer that, let's take a look at what can happen when a bad actor **does** get a malicious VST on your computer.


VST plugins are capable of running arbitrary code, just like any other program you run on your computer. While this might seem obvious many of you, what's important to note is that, ***unlike literally every other modern 3rd party plugin framework, there are absolutely no safeguards in place to protect you, the musician, from having a plugin perform malicious actions on your computer.*** 

This is especially awkward, given that, nearly all plugins (outside of miscellaneous telemetry and activation/anti-piracy functionality) don't really need access to more of your system resources, like your internet, filesystem, and unrelated programs, than just a place to read inputs from the DAW, quickly do some calculations, and then spit out some corresponding output-- whether they are sound, MIDI, or something else entirely. Yet, because of the decades-old framework that, despite having two major revisions since it's releases and today (1/1/2023), has largely stayed consistent in how it is loaded into VST hosts.

While this might seem obvious many of you, what's important to note is that, ***unlike literally every other modern 3rd party plugin framework, there are absolutely no safeguards in place to protect you, the musician, from having a plugin perform malicious actions on your computer.*** 

.


## But to prove the point, let's take a look at two examples that showcase two different ways an attacker could harm you to their benefit by convincing you open their VST

1. Hijack System Resources: 
2. Ransomware: Encrypt and/ or delete important files on your computer, then try to coerce you to pay the attacker significant amounts in order to recover your data.

Don't believe me when I say that VSTs are able to do this? Feel free to explore some pre-built proof-of-concepts, ready to download from my GitHub, at https://github.com/SpiritSeal/MaliciousVST.

> Note: For obvious reasons, don't open these Proof of Concept VSTs directly on a computer you actually use for your production tasks. Treat this like any other malware, and only mess with it in a safe environment with an abundance of caution,

## What's not so scary is that this attack vector exists so much as the wider Music Production community is either completely oblivious or apathetic to it

The fact that this attack exists by itself is one thing

There have been no real mentions or discussions of the potential for the VST framework to be used as an attack vector, [outside of a few obscure forum posts that are closer temporally to the initial release of the framework in the late '90s than our current year](). Only very recently have security researchers and bloggers been revisiting this long-standing weakness, but even then, it was easier to independently investigate and discover these issues myself than to [finally discover someone else who shared the same braincell as me after several dozens of hours of trying to find ANY recent mention of this issue](). But hopefully, as this issue gets talked about more on the internet, these types of posts discussing the security aspect of music technology will become more common in the music community, and will enable advancements in this area as plugin framework manufacturers and independent developers take notice and act upon these findings.


## Security Policy Recommendations

### Not all of the policy recommendations below will fully explained in technical detail, see [What's next?](#What's next?)

- Disable autoscan new VST plugins offered in DAWs like REAPER and Elements
	- In this context, "scanning" plugins is truly a misnomer. More accurate would be to say something like, "load plugin, and start running it's code until it tells me it's metadata, because, of course, lightweight information intended to be stored and accessed separately to the main content should absolutely only be accessible by first blindly trusting and running said content" 10/10 design choices. 
	- Side Tangent: Now that we've mentioned VST plugin metadata, I could prob go on a blog-length rant about how absurdity of the verificationless, trivially forgeable plugin metadata fields, including mildly critical information, such as the plugin manufacturer, but I'll save that for one of the future posts.
- Be careful of what plugins you download and where you download them from
	- Let's go ahead and briefly address the elephant in the room: VST piracy. Ignoring the ethical reasons to avoid engaging in this type of behavior, one important piece of information to recognize is that the time and effort it takes an independent 3rd party to create a crack for modern VST is extremely significant, as the process of reverse engineering in general is rarely trivial. As such, in exchange for dedicating resources to this task, these crackers often are expecting some sort of significant return, typically a path to infected hundreds or thousands of [zombie](https://en.wikipedia.org/wiki/Zombie_(computing)) computers that can be hijacked for everything from [cryptomining](https://en.wikipedia.org/wiki/Cryptojacking) to [DDoSing](https://en.wikipedia.org/wiki/Denial-of-service_attack).
	- Always try to download/install VSTs directly from a trusted source, like a manufacturer's website, a secure first-party installer, or an authorized seller like [PluginBotique](https://www.pluginboutique.com/)
	- For smaller VST-creators, if the plugins are open-source and still fairly popular, that is often (not always) a good sign that the plugins will be safe to use.
	- For smaller VST-vendors without any sort of independent security verification, carefully consider how necessary it is for you to install the plugin before you even download the file. While this obviously really sucks for smaller creators trying to get off the ground, with the current state and (lack of) security measures in place protecting users from malicious VST plugins, it may just be safer to skip out, at for the short-run.

## Takeaways (tl;dr)

At the present, until some major framework-level change occurs with regards to how VST plugins are loaded into hosts, you should be extremely cautious with whatever

Other relatively recent [security bloggers](https://blog.infosecnoodle.com/posts/vst-malware/) have shown that antiviruses are not very effective at detecting whether VST plugins are safe or contain malware. 

.
## What's next?

This post is supposed to be a quick introduction for this ongoing issue for nontechnical people: namely, the digital musicians, producers, and hobbyists who use this technology on a daily basis and are most vulnerable to being exploited by a malicious VST plugin. In the future, I'll be creating a few









Sources:
http://vstreview.net/the-brief-history-of-vst-plugins/
