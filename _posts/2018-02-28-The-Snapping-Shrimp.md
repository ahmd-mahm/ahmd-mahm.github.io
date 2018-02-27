---
mathjax: true
---

# Before getting started

- All technical figures/files on this page are for public use, so share away!

- To cite this article in a technical paper, use either the Bibtex entry:


> @article{ahmed:2017,
author = {A. Mahmood},
title = {Snapping shrimp noise: motivation and modeling},
year = {2017},
url = {https:}
}

# The purpose of this article

In recent years, a significant chunk of work has been devoted to understanding snapping shrimp noise and its impact on underwater acoustic systems. It has indeed been established that conventional (Gaussian) noise models and techniques are extremely ineffective in modeling and mitigating snapping shrimp noise. The Acoustic Research Laboratory (ARL) at the National University of Singapore (NUS) and several other research groups have offered much in this regard. Yet with the constant onslaught of new research, it is important that the past be put in perspective. New works need to be analyzed and interpreted within previously established results. Have we been progressive, i.e., converted previous wrongs to rights? Or is it the other way around? In any case, a frank article of this sort has long been due. The article is directed not only towards new researchers in the underwater acoustics community, but offers a tutorial to anyone curious enough to know what snapping shrimp noise is, how prevalent it might be and how they have been modeled in the literature.

The first thing I would like to talk about is the motivation behind addressing snapping shrimp noise. Though the oceanic community is well-aware of the phenomenon, the global scale of this is known only to a few. Therefore, to most, its true impact may not be truly understood. The interested reader is directed to [Part 1: The marvelous snapping shrimp!](#P1) in this regard. This section also includes snippets of sound files recorded in snapping shrimp waters and sum fun videos too.

The oceanic research community is relatively young but has seen substantial growth since the early 90s. A quick search for the term 'Underwater Acoustics' in [Web of Science](https://webofknowledge.com/) gives us the following hits per year:


<!--![alt text][ArticlesUWAcoustics]
[ArticlesUWAcoustics]: /images/11.png "Annual hits for underwater acoustics"-->


![alt text][ArticlesUWAcoustics]

[ArticlesUWAcoustics]: /images/11.png "Annual hits for 'underwater acoustics'"

This explosive interest is indeed welcome, as it pushes research frontiers forward at rates never fathomed before. However, it brings along with it certain unfortunate caveats. In reference to snapping shrimp noise, I have found a certain disconnect between findings reported in the literature and some of the works that have come over the last 2-3 years. There is a knack of employing various statistical noise models for snapping shrimp noise which are imported from other branches of research. Though such 'models' may be based on substantiated claims there, they are not supported by recorded snapping shrimp data whatsoever! It is somewhat unfortunate that such works have not paid any heed to research spanning over decades, that has painstakingly investigated the snapping shrimp noise process, slowly building statistical models to where they stand today. I may be incorrect, but I think the underlying reason that has contributed to this trend is that researchers new to the oceanic community bring with them a thought heavily defined by works in their areas of expertise. Some are oblivious to the fact that underwater acoustic signal processing has evolved much on its own. Consequently, it is probable that they think themselves to be introducing novel concepts to the area. This path needs to be corrected and such myths need to be put down. To do so, this article offers a tutorial-like approach and analyzes recorded snapping shrimp noise data in [Part 2: Analyzing snapping shrimp data](#P2). Moreover, models substantiated in the literature are also briefly introduced in [Part 3: Modeling snapping shrimp noise](#P3) . The highlight amongst all is the αSGN(m) model. We end up by providing code that generates random variates and is open to all who need to use it.

# <a name="P1"></a> Part 1:The marvelous snapping shrimp!

## A quick intro.

The snapping shrimp (family Alpheidae) is an eccentric family of crustaceans. Immediately recognizable due to its asymmetric front pincers, these shrimp colonize coastal waters throughout the world. The larger of its two pincers is able to produce snaps (large surges in acoustic pressure) which are strong enough to stun or even kill small prey. It was initially hypothesized that the sound was an outcome of the two halves of the pincer physically colliding into each other. However, this has long been proven false and the phenomenon is attributed to imploding bubbles that are formed from the rapid closure of the pincer, or in other words, cavitation.

The shrimp thrive in their natural habitat and it is common to see the seabed/shallow water coral littered with the noisy crustaceans. It is hypothesized that the snapping shrimp use their snaps not only to hunt prey and ward of unwanted predators, but also to communicate with each other. Diving in snapping shrimp infested waters offers a unique experience as one can literally hear constant crackling in the background. A clip recorded in Singapore waters is given below:

<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/331067083&color=%23ff5500&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>

Amazing right?! For the curious, the audio is sampled at 60 kHz and represents the unadulterated soundscape within the frequency interval (bandwidth) 0 kHz - 25 kHz, thus spanning the entire human audible range. One also notices a constant hum in the background. This is anthropogenic (man-made) noise, another persistent (and rather unfortunate) characteristic of Singapore waters. The sound can be cleaned up by mitigating frequency components below 3 kHz. Behold, the audible snapping shrimp noise component in all its glory:

<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/331066895&color=%23ff5500&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe>

As Yoda would say, 'Noisy indeed are Singapore waters...' smile But is this phenomenon just limited to Singapore and a few other places? We find out next!

## Impact on underwater acoustic systems

Though apparent why they have garnered much attention from marine biologists and environmentalists, the snapping shrimp is also of great interest to researchers in the field of underwater acoustic communications and signal processing. The primary reason for this is that even in the clearest of waters, acoustics (sound) is the only known means of underwater (wireless) communication if a range of several tens of meters is required. As snapping shrimp are arguably the loudest animals in the ocean, they will most definitely impact acoustic systems nearby. More specifically, the snaps produced are essentially broadband signals and are observed to be the dominant noise source within the frequencies 2kHz - 250 kHz. Further still, it has been suggested (I have seen this first hand as well) that the spectra goes way beyond 250 kHz. Acoustic systems typically operate into the tens of kHz. Consequently, snapping shrimp noise poses a unique problem to such systems operating in the vicinity of a populace.

Loud or not, the snapping shrimp would not have been worth this much coverage if they existed only in a few specific locations. It turns out that this is not the case. The yellow dots in the figure below highlight geographic locations where snapping shrimp specimens (Alpheidae) have been discovered. The data was populated from OBIS, a huge international effort that hosts thousands of databases consisting of oceanic biogeographic data. Though some of the datasets may hold some erroneous data, the overall gist is clear: The spread is huge! Of Hawaii, the east and west coasts of the US, Mexico, Brazil, Europe (even as north as the UK), Turkey, Egypt, South Africa, the Indian sub-continent, China, the Korean peninsula, Japan, south-east Asia, Australia and even New Zealand. The global (coastal) presence of the snapping shrimp, along with their loudness, highlights the scale of the problem they pose! Awesome, right? I love it!

![alt text][SSworld]
[SSworld]: ../images/Screenshot_Corrected.png "Snapping shrimp: from space!"

On another note, terms like "warm shallow waters" and "shallow tropical waters" have been typically associated with the habitat of the snapping shrimp. Though "warm" is a flexible word and its usage in this sense is correct, "tropical" is incorrect. The latter specifically implies the zone approximately within the latitudes 23.5°N and 23.5°S. This is highlighted by the shaded region in the figure below. Clearly, snapping shrimp live well beyond the tropics. In fact, if the data is to be believed, they populate the sub-tropics and even some temperate regions too.

![alt text][SSworldTemp]
[SSworldTemp]: ../images/Screenshot_Tropical_Corrected.png "Snapping shrimp: from space - 2!"

## Fun facts!

- A new species, the Pink Floyd snapping shrimp, was recently discovered (published in 2017) of the pacific coast of Panama. A colorful specimen indeed, you find much more about them here.
- There are some beautiful videos out there that highlight the physics behind a snapping shrimp snap. The following is a slo-mo video capture of a snapping shrimp closing its pincer (courtesy BBCEarth)

<iframe width="560" height="315" src="https://www.youtube.com/embed/QXK2G2AzMTU?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

- Snapping shrimp are omnipresent in Singapore waters. A short clip by the History Channel Asia highlights some fun facts about these marvelous creatures. Expertise provided by our very own [Mandar Chitre](https://arl.nus.edu.sg/twiki6/bin/view/ARL/MandarChitre). Enjoy smile

<iframe width="560" height="315" src="https://www.youtube.com/embed/Y9UdkcsgNy4?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

# <a name="P2"></a>Part 2: Analyzing snapping shrimp noise data

Source level of a snapping shrimp snap


In acoustics, the definition of 'source level' is standard. It essentially depends on the pressure $$ p $$ measured at a distance $$d$$ from the acoustic source. Source levels are evaluated in decibels ($$\text{dB}$$) with respect to a reference pressure $$p_0$$. In underwater acoustics, the references are typically set to $$p_0=1\mu\text{Pa}$$ and $$d=1\text{m}$$. Consequently, the source level is evaluated by $$ \begin{align} x=10\log_{10}\big(\frac{p}{p_0}\big)=10\log_{10}p. \end{align} $$ To avoid any ambiguity, the source level is expressed in the shorthand form $$x~\text{dB re}~p_0\mu\text{Pa}~\text{at}~d\text{m}$$ or specifically for standard readings in the underwater case, $$x~\text{dB re}~1\mu\text{Pa}~\text{at}~1\text{m}$$ . Do note that $$d$$ is an important parameter and source levels reported without it are useless quantities as the same source may be deafening nearby but can be essentially reduced to a whisper by moving arbitrarily away from it.


Now that the necessary definitions are out of the way, I must highlight that peak-to-peak source levels of a single snapping shrimp snap have been recorded to be as high as $$180~\text{dB re}~1\mu\text{Pa}~\text{at}~1\text{m}$$. In fact, the recently discovered Pink Floyd snapping shrimp is recorded to be even higher! Not feeling the loudness yet? Well, let's make a comparison:


For the curious reader, a good summary of (air) acoustic terminology/measurements is given here and includes a list of source levels (measured in %$\text{dB}$%) of several sources. Of interest is the source level of a jet engine, quoted as $$140~\text{dB re}~20\mu\text{Pa}~\text{at}~50\text{m}$$ (or $$153~\text{dB re}~1\mu\text{Pa}~\text{at}~50\text{m}$$). This signifies an astonishing result: The peak-to-peak source level of a snap is much louder than a jet engine 50m away!! It really is a blessing that each individual snap lasts only for a few hundred picoseconds, else diving in such an environment without ear protection would have resulted in ruptured ear drums! With this in mind, I am sure the previously introduced audio files can be revisited with more appreciation.

# <a name="P3"></a> Part 3: Modeling snapping shrimp noise
