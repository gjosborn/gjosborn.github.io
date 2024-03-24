---
layout: post
title:  "Check Out Projects"
date:   2023-03-07 23:54:36 -0500
categories: project
---

# Unstructured.io - Make Your Data LLM Ready
## Machine Learning in Production - Individual Assignment 3 - MLOps Tools
### Grayson Osborne

# Introduction
Anyone who has spent hours cleaning and preparing data for AI/ML applications knows the frustration of unstructured data, trying to turn it into something usable for training models. It's a time consuming process and according to one [survey](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/?sh=170a0c506f63), data scientists spend 60% of their time cleaning and organizing data, while most also found it to be the least enjoyable part of their job. [Unstructured](https://github.com/Unstructured-IO/unstructured) aims to free us (or at least limit) from the pain of spending hours manipulating and cleaning data into a usable form for our proposed application. Unstructued's core functionality includes partitioning, cleaning, extracting, staging, chunking, and embedding. The original use case of this package is preparing unstructued data for training of LLMs and Retrieval Augmented Generation.

In the context of our movie streaming and recommendation scenario, consider that we want to provid a chatbot to our users to help them determine which movie they want to watch. The process of training this chatbot starts with determining the data we want to include in the chatbot and transforming it into a usable format for training. In the below tutorial, I will show practical examples of Unstructued's [Core Functionality](https://unstructured-io.github.io/unstructured/core.html)

# Setup 

When installing Unstructured, you can specify which document parsing/processing capabilities you need in the pip command which can drastically cut down on the necessary dependencies. If, for example, you plan on using it to process word documents and PDFs, you could install the package with 
`
pip install unstructured[docx,pdf]
`
or install the full package with 
`
pip install unstructuredp[all-docs]
`

I created a Dockerfile that installed jupyterlab alongside unstructured to create a space I could experiment with the package. The Github repository for this experimentation can be found here. To run the Dockefile in the repository, execute
`
docker build -t jupyter-unstructured .
docker run -p 8888:8888 -v .:/notebook jupyter-unstructured
`
The run command exposes port 8888 for the use of jupyterlab and mounts the current directory into /notebook in the container so adding files to that directory on the host will also add them to /notebook in the container.

# Strengths and Limitations
## Strengths
- **Reduces Custom Data Cleaning Code** - This package can significantly reduce the amount of custom data cleaning code you need to write. Being able to parse a wide range of document types for the information pertinent to your use case will give you a much better starting point from which to start your project, effectively skipping most of the tedious custom data cleaning code.

- **Works on a Wide Range of Document Types** - Unstructured supports a wide range of document types including automatic detection and applying the correct parsing functions so you can feed the program a directory of data of varying types without worry of conflicting types causing errors in the program. 

## Limitations
- **Limited Documentation** - The documentation for their open source package provides enough information to get started but to tailor it to the needs of a specific use case, you would almost certainly need to reference the source code of the package to understand get your desired results. If you are not comfortable doing this, it will have a steep learning curve.

- **Lots of Dependencies** - Unstructured uses a lot of dependencies in the background to accomplish different data parsing tasks. Some of the depenencies are not natively compatible with Windows making Docker the preferred install method and further increasing the overhead.


# Core Functionality Examples

```python
from unstructured.partition.auto import partition
from unstructured.documents.elements import *
```

## Partition
Partitioning in unstructured allows us to extract content from unstructured documents. It divides the elements of the documents into elements like Title, ListItem, Text, and NarrativeText among others. For the sake of the application of training a chatbot for movie recommendations, after I partition the documents I will only be keeping the narrative text elements, which contain the contents of the review.

In our first example, we will be providing a URL to the partition function which will fetch and parse the returned html document. As you can see, providing only the URL was filtered by the website but adding a user-agent header allowed us to properly fetch the html document.


```python
url_elements = partition(url='https://sarahgvincentviews.com/movies/dune-part-one/')
for i in url_elements:
    print(i)
```

    Not Acceptable!
    An appropriate representation of the requested resource could not be found on this server. This error was generated by Mod_Security.



```python
# Fetch the URL with a custom user agent
url_elements = partition(url='https://sarahgvincentviews.com/movies/dune-part-one/', headers={'User-Agent': 'Mozilla/5.0 (Windows NT 10.4; Win64; x64; en-US) Gecko/20130401 Firefox/73.2'})

# Create a set of the unique types of elements in the returned HTML
url_types = set([type(i) for i in url_elements])

# Print the elements that are of type NarrativeText
print("Narrative Text Content: \n")
for i in [i for i in url_elements if isinstance(i, NarrativeText)]:
    print(i)

# Print the types of elements in the returned HTML
print("\nTypes of Elements in the HTML:\n")
for i in url_types:
    print(i)
```

    Narrative Text Content: 
    
    “Dune” or “Dune: Part One” (2021) is the second film adaptation of Frank Herbert’s 1965 novel series. In the future year of 10191, an Emperor governs the universe, and this universe’s most valuable resource is spice, which aids in interstellar travel, but it is only found on Arrakis, a desert planet. The Emperor determines which House (thank you, “Game of Thrones” for making these concepts more comprehensible) will occupy Arrakis and insure that spice can be harvested without interference from the indigenous people, the Fremen, who never consented to the Emperor’s rule. The brutal house of Harkonnen have occupied the land and grown wealthy as a result of their position, but the Emperor orders them to withdraw and gives the job to the House of Atreides, whose head is Duke Leto Atreides (Oscar Isaac) and whose home planet is Caladan. His consort, Lady Jessica (Rebecca Ferguson) is training their young, inexperienced son, Paul (Timothee Chamalet), in the ways of the Bene Gesserit, a group of women who possess physical and mental abilities, even though men do not exhibit these traits; however, Paul is different. He dreams of fragments of the future, is learning how to control minds and shows physical prowess that other men do not exhibit. Moving to Arrakis challenges the House of Atreides to the point of possible extinction, but also makes Paul’s dreams come true.
    Why would I, a person who had zero interest in Herbert’s novels and did not enjoy David Lynch’s version (through no fault of Kyle Maclachlan), commit to watching not one, but two or more Dune movies? Denis Villeneuve. I found most of his films prior to “Blade Runner 2049” (2017), which is gorgeous, to be riveting, and arresting visuals accompany surprising stories. Apparently Villeneuve’s lifelong dream was to adapt Herbert’s universe for the big screen so while I am happy that he, like Paul, is finally able to bring his fantasies to life, it is not always a good thing for him or everyone else. No slander on Villeneuve’s adaptation, which I prefer over Lynch’s take, but “Dune” is broad in its characters and narrative. It feels predictable, and there is zero investment or tension over well-known characters’ development. Villeneuve’s “Dune” is the cinematic equivalent of a meditation garden: gorgeous to watch with its characters’ polished skin, smooth stonelike spaceships, sand enveloped landscapes, sunlight and shadows playing portentously on golden, bronzed metallic surfaces or verdant landscapes contrasting its arid sister planet. It is important for visuals to tell the story, but the people feel like the supporting actors, not the driving force, as they float elegantly with their gravity defying suits or trembling shields. The more that a person’s face is covered, the less than the audience can relate to them or should be suspicious of them. It is like a tasteful “Star Wars” with superb production values, but lacking in the raw, untidy energy of Lucas’ epic.
    While Timothee Chalamet may be the headliner, “Dune” is an ensemble affair; however, Chalamet sets the dour tone as a brooding, wan Paul. At least cowriters Villeneuve, Jon Spaihts (who penned “Prometheus,” “Doctor Strange” and “The Mummy”) and Eric Roth (“Wolfen,” “Forrest Gump,” “The Insider,” “The Insider,” “Ali,” “Munich,” “A Star Is Born,” “Killers of the Flower Moon”) acknowledge that the man boy does not look like he can hold his own in a stiff wind, forget a physical confrontation. In one scene, Paul dodges and catches a minute assassination drone, a hunter-seeker. At the time, it seems plausible and deft, but as the film unfolds and more impressive warriors cannot withstand such an attack, it becomes retroactively impressive. Paul’s slight frame makes sense because if he displays feminine mental abilities, he should also possess feminine physical attributes. Paul’s sullen countenance is not chronic and breaks when he spends time with his weapons teachers and father’s faithful aides, Duncan Idaho (Jason Momoa, who is still gorgeous but mostly cleanshaven), an adventurous cheerful bro type, and Gurney Halleck (Josh Brolin), a man determined to teach Paul how to fight because the memory of war haunts him.
    Paul reverts to his original settings countenance when he is with his parents. The weight of his parents’ expectations, his predicament and his visions suppress his youthful buoyancy. In retrospect, Duke Leto Atreides (the scrumptious Oscar Issac sporting a lush, perfectly trimmed beard) feels like an inspiration for Ned Stark, and the only person on the chessboard who wakes up too late to the stakes of the pageant-filled game that he is playing and begins panicking long after it is too late to turn things around. The Duke is the kind of guy that others may not need to go to the trouble of assassinating since he engages in enough activities that could get him killed. He fancies himself above the Harkonnen because he is respectful of the Fremen and treats them like people, but he still wants to harvest the spice and make the Emperor proud so the only difference is a lack of self-preservation gene and good manners. His disembodied hand on Jessica’s neck, a gesture of love, gets framed as a feeble attempt at possession and control. His identity is irrelevant in the sequence, and he almost does not exist long before he arrives at Arrakis.
    The real star of “Dune” feels like Jessica. Ferguson is born to play supernaturally dubious women, and Jessica in Part One may edge out her performance as Rose the Hat in “Doctor Sleep” (2019) because of the range of her character’s journey through the course of the film. Jessica appears to be a privileged mother and significant other, but she gets the best lines during the Reverend Mother and Imperial Truthsayer Gaius Helen Mohiam (Charlotte Rampling, who is incapable of delivering a less than excellent performance despite the heavy veil) implementation of a test, “Fear is the mind killer.” It is like someone else delivering Hamlet’s “To be or not to be” speech. The creative choice suggests that she is being tested as much as Paul by refusing to save him. There is an interesting power struggle between Jessica and the rest of the Bene Gesserit regarding how to produce the Kwisatz Haderach through a breeding program. If any part of “Dune” feels reminiscent of Villeneuve’s previous films, it is the breeding program. It reprises themes from “Enemy” (2013) and “Blade Runner 2049.” All women in this universe, except for Fremen women, are double agents with their own agenda by belonging to this breeding cult which entwined itself into the framework of this problematic structure. While Paul’s existence and survival would be impossible without them, especially his mother, at some point, everyone views the women with more suspicious than declared enemies. By the end of Part One, Jessica bears no resemblance to the woman having breakfast with her son in the opening. She is a fierce, driven, and focused person intent on her mission and survival.
    Even though the plot points may be intentional, sardonic takes on such tropes as the noble savage and the white savior, they simultaneously validate such tropes in the overt storyline. It is kind of like the think pieces on “Saturday Night Live” where a problematic political figure goes on the show and makes fun of themselves. Showing that you know something is problematic is not the same as not being a problem. “Dune” is not doing the same work as Bong Joon-Ho’s “Snowpiercer” (2013) or “Parasite” (2019) where he takes a storyline where the audience expects one well-trod turn of events then subverts it by obliterating all expectations because the narrative is inherently flawed and needs to be flipped. Narratives like “Dune” understand what is wrong but cannot imagine how to obliterate it or what being right would look like. The story’s structure is kind of like Paul. He is aggrieved that his mom and her fellowship are treating him and others like puppets, but he rather likes being center stage whether as a duke’s son or the Fremen’s champion. It is hard to give up benefits of something that works for you. It also unironically purveys in tropes like the evil albino.
    Evil albino in chief is Baron Vladimir Harkonnen, whom Stellan Skarsgard plays and proves why he thrives in Lars von Trier films. He relishes being unrecognizable under mounds of practical effects and makeup. In gender bending casting, Sharon Duncan-Brewster plays Dr. Liet Kynes, a character with ambiguous allegiances, who gets an eleventh-hour trite backstory, but on the way to that moment, is mysterious and intriguing as Arrakis’ guardian. Javier Bardem plays Stilgar, a Fremen, and largely provides comedic relief.
    “Dune” is a stunning realization of a beloved sci-fi series, but I felt nothing. Jargon heavy and remote, Villeneuve’s proudest achievement feels like a step back in terms of being more than the equivalent of a purveyor of beautiful gowns. Once the occasion is over, it can be forgotten without any sense of lessons that can be extracted to preserve the heart and soul on harsh, realistic days of drudgery.
    Side note: loved Villeneuve’s allusion to famous paintings such as Casper David Friedrich’s “Wanderer above the Sea of Fog” or Jacques-Louis David’s “The Death of Marat.”
    Join my mailing list to get updates about recent reviews, upcoming speaking engagements, and film news.
    
    Types of Elements in the HTML:
    
    <class 'unstructured.documents.html.HTMLNarrativeText'>
    <class 'unstructured.documents.html.HTMLTitle'>
    <class 'unstructured.documents.html.HTMLListItem'>


In the following examples, I will partition other Dune movie reviews in different formats including image, PDF, and htm (already downloaded)


```python
pdf_filename = 'example-files/dune_review_pdf.pdf'
image_filename = 'example-files/dune_review_image.png'
html_filename = 'example-files/dune_review_html.htm'

pdf_elements = partition(filename=pdf_filename)
image_elements = partition(filename=image_filename)
html_elements = partition(filename=html_filename)

pdf_elements_narrative = [i for i in pdf_elements if isinstance(i, NarrativeText)]
image_elements_narrative = [i for i in image_elements if isinstance(i, NarrativeText)]
html_elements_narrative = [i for i in html_elements if isinstance(i, NarrativeText)]

print("\nPDF Narrative Text Elements: \n")
for i in pdf_elements_narrative:
    print(i)

print("\nImage Narrative Text Elements: \n")
for i in image_elements_narrative:
    print(i)

print("\nHTML Narrative Text Elements: \n")
for i in html_elements_narrative:
    print(i)
    
print("\n\nUnique Types across all three files: \n")
all_types = set([type(i) for i in pdf_elements + image_elements + html_elements])
for i in all_types:
    print(i)
```

    This function will be deprecated in a future release and `unstructured` will simply use the DEFAULT_MODEL from `unstructured_inference.model.base` to set default model name


    
    PDF Narrative Text Elements: 
    
    Directed by Denis Villeneuve, performances by Timothée Chalamet, Rebecca Ferguson, and Oscar Isaac, 2021. 156 mins. Reviewed by Fabio Bego
    Denis Villeneuve’s film Dune (2021) provides interesting insight on how
    notions of race, gender, and empire that are at the core of current post- colonial critique are being transferred into popular culture. Analyses of the short- and long-term consequences of colonialism in the contempo- rary world pervade public discourse in shows and documentaries for main- stream media, blockbuster movies, institutionally financed film festivals, and art exhibitions. From a political perspective it is possible to distinguish two broad approaches. On the one hand there is a critique from the left which is focused on the deconstruction of race and ethnicity. On the other hand, there is a critique from the far right that aims at restoring race and ethnic divisions and privileges which were presumably spoiled by “globalization” or “communism.” Without wanting to draw strict lines between the two political orientations of current postcolonial critique, in this review I argue that Dune squarely falls into the second category.
    I became interested in this film after reading some positive comments on Italian far right blogs. To understand the reasons why fascists appre- ciated this film, I take a look at Dune through the book Revolt Against the Modern World, by Italian racist and fascist philosopher Julius Evola. The book was originally published in 1934 but is still a major reference for fascists around the world.1 In order to highlight the analogies between the book and Dune’s anti-imperialist discourse, it is necessary to present the basic concepts of Evola’s ideas of empire. Evola thought that world history evolved around the dialectic between a supernatural order and a worldly or inferior order. He identified the supernatural order with the term “tra- dition.” Evola affirmed that traditional societies were organized according to races, caste systems and sexes. This form of organization allowed societies to live harmoniously, although not peacefully, according to their status. The decay of traditional values and hierarchies led to the advent of the inferior order. This is the reason why modern societies are dominated by greed and individualistic interests. Evola affirms that the supernatural
    Fabio Bego, “Film Review: Dune,” Black Camera: An International Film Journal 14, no. 2 (Spring 2023): 406–409, doi: 10.2979/blackcamera.14.2.21.
    order is ruled by men and has a masculine character whereas the inferior order has a feminine character.
    Drawing on this division, Evola defines two types of empires. The tra- ditional empire, where the emperor has authority because he is sacred, and modern imperialism where the ruler has lost his sacredness and can obtain authority only with violence. Evola associates the term imperialism to the expansion of Western European states in the modern era and affirms that Europeans have destroyed the traditional societies that they colonized. He also believed that modern slavery in North America has been much crueller than the slavery of the ancient societies. In his view, in Indian traditional society, slavery was inconceivable because the persons that belonged to the caste of workers accepted their social function and, differently from modern slaves and workers, they did not feel alienated from their work. He justifies the hierarchical divisions of traditional society by claiming that workers were free to belong to their caste as their rulers were to belong to theirs. Evola criti- cized modern nationalism that, in analogy to more recent Marxist-inspired theories, he considered invented traditions.
    One of the first similarities that emerge by the comparison between the two works is the division of society into castes. In Dune there is an aristocratic class represented by the emperor (that we do not see in this film), the duke of Atreides, his son and the main antagonist of the film, the baron. Then there are warriors who, in analogy to Evola’s imagination, are depicted as ascetic persons, devoted to fighting and to the rituality that surrounds it. There is a class of clerks that mediates the relations between the different rulers. And finally there is the “people,” which is represented by the mass from the planet Arrakis, the Fremen. The latter praise and chant the name of their prince. They recognize his sacredness and seem spiritually aware of the mission that the prince is about to undertake before he knows it.
    Dune reflects Evola’s thoughts on gender divisions. Evola considers men as depositaries of supernatural values and knowledge. Only men can be sacred rulers. In his view women acquired sacerdotal and political power only when Tradition decayed. Their religious practices cause the devirilisation of men and lead societies to embrace collectivist forms of organizations which end up in transferring power from the ruling elites to the lower popular classes. In Dune women are represented as owners of magical knowledge that disturb the rule of men. The main female character, the mother of the prince, is seen with suspicion by the duke who doubts her love for their son. She is also seen with suspicion by her son, the prince, who believes that she has turned him into a freak with her magic powers. Finally, she is seen with suspicion by the fierce barbarians, the Fremen, whose warrior lifestyle gains no benefit from an “aged” woman.
    The conflict between the Atreides and the Baron resembles the struggle between the masculine character of Tradition and feminine character of the infe- rior order. The Baron is not a traditional ruler. His actions do not follow a chiv- alrous bon-ton. The only tools that he has to increase his authority are deceit, blackmail and rude force. He’s in no way sacred, but he’s rather the representation of modern imperialism. In Evola’s world, the figure of the Baron reminds that of the greedy giants who in his opinion lived in the Age of Bronze. The baron has magic powers that relate him to the inferior order. He can expand his body in a snake-like shape. Evola associated snakes to feminine religious cults. This link clearly emerges when the character of Dr. Liet Kynes, a Fremen, is portrayed as a worshipper of the desert giant worms before being swallowed by it.
    The temporality of Dune is quite similar to the historical ages described by Evola. The film shows us a future happening eight thousand years from now. We would expect AI to have taken our place in many dangerous and heavy duties such as war, but this might not be the case if we took Evola’s thought seriously. Dune warriors, including the duke and the prince, prefer body to body combat by using swords, daggers, and spears. The fighting scenes are characterised by rituality and techniques that remind the orders of the knights of the Middle Ages and the Samurai that Evola considered quintessential representatives of Tradition. The story of Dune is not set in a random future. In analogy to Evola’s work, Dune’s conception of time is not evolutionary but revolutionary. The film seems to be set in the “Age of the Heroes,” which according to Evola, was dominated by a civilization that tried to cleanse Tradition from the influence of the inferior order. The purpose of the “heroes” was to restore the Age of Gold, that is the time when humans were godlike creatures and Tradition reigned in its purest form.
    The setting of the films provides metahistorical background for positive stereotypes about non-Western society that inform both Dune and Evola’s work. The Fremen are portrayed as an ethnically and racially mixed commu- nity. They have diverse accents and bring to mind various descriptions of noble savages that populate Western imagination. The relation between the Fremen and the prince is reminiscent of the relation between the followers of Islamic religion and the Prophet described by Evola. According to Evola, the Prophet founded a race of spirit which was made of several disparate ethnic and race groups that were united under the sacred principle of the Umma, the Islamic nation. References to “Islamic” religion are not particu- larly audible in the film as they are in the book. But the image of the Fremen as heroic warriors who are blindly devoted to Tradition persistently emerges throughout the film. The prince is represented as a hero who, in analogy to the Prophet, is destined to create a new race of spirit.
    Dune transposes into popular culture race conceptions that are similar to the ones conceived by Evola. The latter believed that race is not determined by
    blood but by spirit and rituality. The blood functions as a vehicle that transfers certain spiritual qualities from one generation to the other. Evola’s spiritual racism is not meant to overcome categories determined by other race theories, but to strengthen them on account of both “ethnic” (that is spiritual) and social grounds (rituality). He asserted that in traditional societies chiefs were not elected because they were connected to aristocracy by blood but after a ritual investiture. At times the ritual investiture demanded a physical duel. Drawing on Frazer’s Golden Bough, Evola reports the legend of the priest king of Nemi, who, in order to be recognized as a ruler, needed to defeat a fugitive slave that challenged him. In Dune the moment of the investiture is represented by the duel between the prince and a Fremen played by a black actor with what sounds like a West African accent. The scene should be read through a hypertextual perspective, that is by considering the history of modern slavery and its impact on the history of Europe and North America. The “accent” of the character and the fact that he lives marooned with the other Fremen in the desert, denotes his “fugitive” status, both in a spatial and in a temporal sense. The “slave,” in this case, has not merely broke free, but he is born “free.” His freedom is so radical that he dares to challenge a “white” aristocrat who has trespassed into his territory. However, such freedom is an illusory state, generated by modernity. If society was governed by Tradition, there would have been no slaves and no masters, but rulers and workers separated in different castes. The challenger is therefore destined to be killed in what can be considered a visual representation of the Hegelian master/slave struggle that is expected to produce always the same result. The ending of the film is a metaphor of the contemporary world. It reinstates the primacy of white aris- tocracy over narratives of post-racialized societies. This is in my view one of the main reasons why fascists liked this movie.
    Fabio Bego received a PhD in European and International Studies from the Roma Tre University in 2017. He works as a film programmer and independent researcher and is the curator of the Albanian film festi- val “Albania, si Gira.” His interests include historiography, postcolonial studies, and the far right.
    
    Image Narrative Text Elements: 
    
    Dune: Part Two @ NYT Critic’s Pick - Directed by Denis Villeneuve - Action, Adventure, Drama, Sci-Fi - PG-13 - 2h 46m
    ‘When you purchase a ticket for an independently reviewed fim through our site, we earn an effiate commission,
    Having gone big in “Dune,” his 2021 adaptation of Frank Herbert’s futuristic opus, the director Denis Villeneuve has gone bigger and more far out in the follow up. Set in the aftermath of the first movie, the sequel resumes the story boldly, delivering visions both phantasmagoric and familiar. Like Timothée Chalamet’s dashingly coifed hero — who steers monstrous sandworms over the desert like a charioteer — Villeneuve puts on a great show. The art of cinematic spectacle is alive and rocking in “Dune: Part Two,” and it’s a blast.
    It’s a surprisingly nimble moonshot, even with all its gloom and doom and brutality. Big-screen enterprises, particularly those adapted from books with a huge, fiercely loyal readership, often have a ponderousness built in to every image. In some, you can feel the enormous effort it takes as filmmakers try to turn reams of pages into moving images that have commensurate life, artistry and pop on the screen. Adaptations can be especially deadly when moviemakers are too precious with the source material; they’re torpedoed by fealty.
    “Dune” made it clear that Villeneuve isn’t that kind of textualist. As he did in the original, he has again taken plentiful liberties with Herbert’s behemoth (one hardcover edition runs 528 pages) to make “Part Two.” which he wrote with the returning Jon Spaihts.
    
    HTML Narrative Text Elements: 
    
    Now streaming on:
    Powered by
    JustWatch
    "It's like a dream," my friend from Hollywood was explaining. "It 
    doesn't make any sense, and the special effects are straight from the 
    dime store but if you give up trying to understand it, and just sit back
     and let it wash around in your mind, it's not bad." That was not 
    exactly a rave review for a movie that someone paid $40 million to make,
     but it put me into a receptive frame of mind for "Dune," the epic based
     on the novels by Frank Herbert. I was even willing to forgive the 
    special effects for not being great; after all, in an era when George 
    Lucas' "Star Wars" has turned movies into high tech, why not a film that
     looks like a throwback to Flash Gordon. It might be kind of fun.
    It took "Dune" about nine minutes to completely strip me of my 
    anticipation. This movie is a real mess, an incomprehensible, ugly, 
    unstructured, pointless excursion into the murkier realms of one of the 
    most confusing screenplays of all time. Even the color is no good; 
    everything is seen through a sort of dusty yellow filter, as if the film
     was left out in the sun too long. Yes, you might say, but the action 
    is, after all, on a desert planet where there isn't a drop of water, and
     there's sand everywhere. David Lean solved that problem in "Lawrence of Arabia," where he made the desert look beautiful and mysterious, not shabby and drab.
    The
     movie's plot will no doubt mean more to people who've read Herbert than
     to those who are walking in cold. It has to do with a young hero's 
    personal quest. He leads his people against an evil baron and tries to 
    destroy a galaxy-wide trade in spice, a drug produced on the desert 
    planet. Spice allows you to live indefinitely while you discover you 
    have less and less to think about. There are various theological 
    overtones, which are best left unexplored.
    The
     movie has so many characters, so many unexplained or incomplete 
    relationships, and so many parallel courses of action that it's 
    sometimes a toss-up whether we're watching a story, or just an assembly 
    of meditations on themes introduced by the novels (the movie is like a 
    dream).
    Occasionally a striking image will swim into view: 
    The alien brain floating in brine, for example, or our first glimpse of 
    the giant sand worms plowing through the desert. If the first look is 
    striking, however, the movie's special effects don't stand up to 
    scrutiny. The heads of the sand worms begin to look more and more as if 
    they came out of the same factory that produced Kermit the Frog (they 
    have the same mouths). An evil baron floats through the air on 
    trajectories all too obviously controlled by wires. The spaceships in 
    the movie are so shabby, so lacking in detail or dimension, that they 
    look almost like those student films where plastic models are shot 
    against a tablecloth.
    Nobody looks very happy in this movie. Actors stand around in 
    ridiculous costumes, mouthing dialogue that has little or no context. 
    They're not even given scenes that work on a self-contained basis; 
    portentious lines of pop profundity are allowed to hang in the air 
    unanswered, while additional characters arrive or leave on unexplained 
    errands. "Dune" looks like a project that was seriously out of control 
    from the start. Sets were constructed, actors were hired; no usable 
    screenplay was ever written; everybody faked it as long as they could. 
    Some shabby special effects were thrown into the pot, and the producers 
    crossed their fingers and hoped that everybody who has read the books 
    will want to see the movie. Not if the word gets out, they won't.
    Roger Ebert was the film critic of the Chicago Sun-Times from 
    1967 until his death in 2013. In 1975, he won the Pulitzer Prize for 
    distinguished criticism.
    comments powered by Disqus.
    Watch our YouTube channel
    
    
    Unique Types across all three files: 
    
    <class 'unstructured.documents.elements.Image'>
    <class 'unstructured.documents.elements.Text'>
    <class 'unstructured.documents.elements.Footer'>
    <class 'unstructured.documents.elements.ListItem'>
    <class 'unstructured.documents.html.HTMLNarrativeText'>
    <class 'unstructured.documents.html.HTMLTitle'>
    <class 'unstructured.documents.elements.Title'>
    <class 'unstructured.documents.elements.Header'>
    <class 'unstructured.documents.elements.NarrativeText'>


## Cleaning

Unstructured provides fucntions to clean the partitioned data including bullets, dashes, whitespace, unicode quotes, and even text translation. The below examples are the only apparent issue in from the examples above, grouping broken paragraphs and extra whitespace


```python
from unstructured.cleaners.core import group_broken_paragraphs, clean

sum_pdf_narrative = sum([len(i.text) for i in pdf_elements_narrative])
sum_image_narrative = sum([len(i.text) for i in image_elements_narrative])
sum_html_narrative = sum([len(i.text) for i in html_elements_narrative])

print("Sum of lengths pre-cleaning:")
print("PDF: ", sum_pdf_narrative)
print("Image: ", sum_image_narrative)
print("HTML: ", sum_html_narrative)

bp_cleaned_pdf_narrative = []
for i in pdf_elements_narrative:
    bp_cleaned_pdf_narrative.append(group_broken_paragraphs(i.text))
    
bp_cleaned_image_narrative = []
for i in image_elements_narrative:
    bp_cleaned_image_narrative.append(group_broken_paragraphs(i.text))
    
bp_cleaned_html_narrative = []
for i in html_elements_narrative:
    bp_cleaned_html_narrative.append(group_broken_paragraphs(i.text))

sum_bp_cleaned_pdf_narrative = sum([len(i) for i in bp_cleaned_pdf_narrative])
sum_bp_cleaned_image_narrative = sum([len(i) for i in bp_cleaned_image_narrative])
sum_bp_cleaned_html_narrative = sum([len(i) for i in bp_cleaned_html_narrative])

print("\nSum of lengths after broken paragraphs:")
print("PDF: ", sum_bp_cleaned_pdf_narrative)
print("Image: ", sum_bp_cleaned_image_narrative)
print("HTML: ", sum_bp_cleaned_html_narrative)
```

    Sum of lengths pre-cleaning:
    PDF:  11016
    Image:  1560
    HTML:  3864
    
    Sum of lengths after broken paragraphs:
    PDF:  11016
    Image:  1560
    HTML:  3764



```python
cleaned_pdf_narrative = [clean(i, bullets=True, extra_whitespace=True, dashes=True, trailing_punctuation=True, lowercase=True) for i in bp_cleaned_pdf_narrative]
cleaned_image_narrative = [clean(i, bullets=True, extra_whitespace=True, dashes=True, trailing_punctuation=True, lowercase=True) for i in bp_cleaned_image_narrative]
cleaned_html_narrative = [clean(i, bullets=True, extra_whitespace=True, dashes=True, trailing_punctuation=True, lowercase=True) for i in bp_cleaned_html_narrative]

sum_cleaned_pdf_narrative = sum([len(i) for i in cleaned_pdf_narrative])
sum_cleaned_image_narrative = sum([len(i) for i in cleaned_image_narrative])
sum_cleaned_html_narrative = sum([len(i) for i in cleaned_html_narrative])

print("\nSum of lengths after generic clean:")
print("PDF: ", sum_cleaned_pdf_narrative)
print("Image: ", sum_cleaned_image_narrative)
print("HTML: ", sum_cleaned_html_narrative)
```

    
    Sum of lengths after generic clean:
    PDF:  10991
    Image:  1548
    HTML:  3754


## Extracting 

The extracting functionality in unstructured provides the functionaility to extract features from objects such as datetime, emails, ip addresses, bulleted lists, and phone numbers. While the extraction functions provided in Unstructured are not directly applicable to the movie reviews I selected, I am showing the translate_text function provided by unstructured that uses the Helsinki NLP MT Models


```python
from unstructured.cleaners.translate import translate_text

translated_pdf_narrative = [translate_text(i, target_lang='fr') for i in cleaned_pdf_narrative] 
for i in translated_pdf_narrative:
    print(i)

```

    mise en scène de denis villeneuve, performances de timothée chalamet, rebecca ferguson, et oscar isaac, 2021. 156 minutes. reviewed by fabio bego
    denis villeneuve’s film dune (2021) provides interesting insight on how
    Les notions de race, de genre et d'empire qui sont au cœur de la critique postcoloniale actuelle sont transférées dans la culture populaire. des analyses des conséquences à court et à long terme du colonialisme dans le monde contemporain imprègnent le discours public dans les spectacles et les documentaires pour les principaux médias, les films blockbuster, les festivals de cinéma financés par les institutions et les expositions d'art. d'un point de vue politique il est possible de distinguer deux grandes approches. d'une part, il y a une critique de gauche qui se concentre sur la déconstruction de la race et de l'ethnicité. d'autre part, il y a une critique de l'extrême droite qui vise à restaurer la race et les divisions ethniques et les privilèges qui ont vraisemblablement été gâtés par la mondialisation.
    Je me suis intéressé à ce film après avoir lu quelques commentaires positifs sur les blogs italiens d'extrême droite. pour comprendre les raisons pour lesquelles les fascistes appréciaient ce film, je regarde à travers la révolte du livre contre le monde moderne, par le philosophe italien raciste et fasciste julius evola. le livre a été publié à l'origine en 1934 mais est toujours une référence majeure pour les fascistes autour du monde.1 afin de mettre en évidence les analogies entre le livre et le discours anti-impérialisme des dunes, il est nécessaire de présenter les concepts de base des idées d'empire d'evola. evola pensait que l'histoire du monde a évolué autour de la dialectique entre un ordre surnaturel et un ordre mondain ou inférieur. il a identifié l'ordre surnaturel avec le terme d'édition d'Evola. Evola a affirmé que les sociétés traditionnelles étaient organisées selon les races, les systèmes de castes et les sexes. cette forme d'organisation a permis aux sociétés de vivre harmonieusement, bien que non pacifiquement, selon leur statut. la décomposition des valeurs et des hiérarchies traditionnelles a conduit à l'avènement de l'ordre inférieur.
    fabio bego, critique de film: dune, doi: 10.2979/blackcamera.14.2.21
    l'ordre est gouverné par les hommes et a un caractère masculin tandis que l'ordre inférieur a un caractère féminin
    En s'appuyant sur cette division, evola définit deux types d'empires. l'empire traditionnel, où l'empereur a autorité parce qu'il est sacré, et l'impérialisme moderne où le souverain a perdu son caractère sacré et ne peut obtenir autorité qu'avec violence. evola associe le terme impérialisme à l'expansion des états européens occidentaux dans l'ère moderne et affirme que les Européens ont détruit les sociétés traditionnelles qu'ils ont colonisées. il croyait aussi que l'esclavage moderne en Amérique du Nord a été beaucoup plus cruel que l'esclavage des sociétés anciennes. à son avis, dans la société traditionnelle indienne, l'esclavage était inconcevable parce que les personnes qui appartenaient à la caste des travailleurs acceptaient leur fonction sociale et, différemment des esclaves et des travailleurs modernes, ils ne se sentaient pas aliénés de leur travail. il justifie les divisions hiérarchiques de la société traditionnelle en prétendant que les travailleurs étaient libres d'appartenir à leur caste comme leurs dirigeants. evola criti cised le nationalisme moderne qui, par analogie avec les théories plus récentes inspirées par les marxistes, il considérait que les traditions inventées étaient des traditions.
    l'une des premières similitudes qui émergent par la comparaison entre les deux œuvres est la division de la société en castes. en dune il y a une classe aristocratique représentée par l'empereur (que nous ne voyons pas dans ce film), le duc d'atréides, son fils et le principal antagoniste du film, le baron. alors il y a des guerriers qui, par analogie avec l'imagination d'evola, sont représentés comme des personnes ascétiques, consacrées à la lutte et au rituel qui l'entoure. il y a une classe de commis qui médiateur les relations entre les différents dirigeants. et enfin il y a le peuple,,, qui est représenté par la masse de la planète arrakis, les frémen. ces derniers louent et chantent le nom de leur prince. ils reconnaissent son caractère sacré et semblent spirituellement conscients de la mission que le prince est sur le point d'entreprendre avant de le connaître
    evola considère les hommes comme dépositaires de valeurs et de connaissances surnaturelles. seuls les hommes peuvent être des dirigeants sacrés. à son avis, les femmes n'ont acquis le pouvoir sacerdotal et politique que lorsque la tradition s'est dégradée. leurs pratiques religieuses provoquent la dévirilisation des hommes et conduisent les sociétés à embrasser des formes collectivistes d'organisations qui finissent par transférer le pouvoir des élites dirigeantes aux classes populaires inférieures. en dune, les femmes sont représentées comme propriétaires de connaissances magiques qui perturbent la règle des hommes. le personnage féminin principal, la mère du prince, est vu avec suspicion par le duc qui doute de son amour pour leur fils. elle est également vue avec suspicion par son fils, le prince, qui croit qu'elle l'a transformé en un monstre avec ses pouvoirs magiques. enfin, elle est vue avec suspicion par les barbares féroces, les frémen, dont le style de vie guerrier ne profite pas d'une femme âgée de....................................................................................................................................................................................................................................................................................................
    le conflit entre les atréides et le baron ressemble à la lutte entre le caractère masculin de la tradition et le caractère féminin de l'ordre infé rieur. le baron n'est pas un souverain traditionnel. ses actions ne suivent pas une tonne de bon alros chiv. les seuls outils qu'il doit augmenter son autorité sont la tromperie, le chantage et la force impolie. il n'est nullement sacré, mais il représente plutôt l'impérialisme moderne. dans le monde evola, la figure du baron rappelle que des géants avides qui, à son avis, vivaient dans l'âge du bronze. le baron a des pouvoirs magiques qui le relient à l'ordre inférieur. il peut étendre son corps en forme de serpent. evola associe serpents aux cultes religieux féminins. ce lien émerge clairement lorsque le caractère du dr. liet kynes, un frémen, est représenté comme un adorateur des vers géants du désert avant d'être avalé par elle.
    la temporalité de la dune est assez semblable aux âges historiques décrits par evola. le film nous montre un avenir qui se passe huit mille ans à partir de maintenant. nous nous attendrions à avoir pris notre place dans beaucoup de tâches dangereuses et lourdes telles que la guerre, mais cela pourrait ne pas être le cas si nous avons pris evolas pensée sérieusement. les guerriers de dune, y compris le duc et le prince, préfèrent le corps au combat corporel en utilisant des épées, des poignards, et des lances. les scènes de combat sont caractérisées par le rituel et les techniques qui rappellent les ordres des chevaliers du Moyen-Âge et des samouraïs que evola considéré comme des représentants quintessences de la tradition. l'histoire de la dune n'est pas mise dans un avenir aléatoire. en analogie avec le travail d'evola, la conception du temps est non pas évolutionnaire mais révolutionnaire. le film semble être mis dans le -age des héros,, qui selon evola, a été dominé par une civilisation qui a essayé de nettoyer la tradition de l'influence de l'ordre inférieur.
    La relation entre les frères et le prince rappelle la relation entre les adeptes de la religion islamique et le prophète décrit par evola. selon evola, le prophète a fondé une race d'esprit qui a été faite de plusieurs groupes ethniques et raciaux disparates qui étaient unis sous le principe sacré de l'umma, la nation islamique. les références à la religion islamique ne sont pas particu lièrement audibles dans le film comme ils sont dans le livre. mais l'image des frères comme guerriers héroïques qui sont aveuglément consacrés à la tradition apparaît constamment tout au long du film. le prince est représenté comme un héros qui, par analogie avec le prophète, est destiné à créer une nouvelle race d'esprit d'esprit.
    dune transpose dans la culture populaire les conceptions de la race qui sont similaires à celles conçues par evola.
    Il a affirmé que, dans les sociétés traditionnelles, les chefs n'étaient pas élus parce qu'ils étaient liés à l'aristocratie par le sang, mais après une investiture rituelle. parfois l'investigation rituelle a exigé un duel physique. dessinant sur frazers la branche d'or, evola rapporte la légende du prêtre roi de nemi, qui, pour être reconnu comme un souverain, a besoin de vaincre un esclave fugitif qui l'a défié. dans le dune le moment de l'investissement est représenté par le duel entre le prince et un frémen pas les raisons d'un acteur noir né avec ce qui semble être un accent africain, la scène doit être lue à travers une perspective hypertextuelle, c'est-à-dire en considérant l'histoire de l'esclavage moderne et son impact sur l'histoire de l'euro et du nord des sociétés d'américa.
    Fabio bego a reçu un doctorat en études européennes et internationales de l'université roma tre en 2017.Il travaille comme programmeur de films et chercheur indépendant et est le conservateur du film albanien festi val Albania, si gira.


## Chunking

Chunking allows us to split document elements into smaller parts for use cases like retrieval augmented generation. It is similar to the partitioning we performed above but combines elements from the partitioning to produce a sequence of CompositeElements, Table, or TableChunk elements. Since in our examples, we did not have any tables, it should just produce CompositeElements


```python
from unstructured.chunking.basic import chunk_elements

# The basic chunking strategy performed on the pdf elements during the partitioning process provides an identical outcome to chunk_elements function performed after partitioning.
chunk_elements_pdf = chunk_elements(pdf_elements)
print(len(chunk_elements_pdf))

partition_chunk_pdf = partition(filename=pdf_filename, chunking_strategy='basic')
print(len(partition_chunk_pdf))

# Showing that as predicted the chunks are made up entirely of CompositeElement obejcts
for i in set([type(i) for i in chunk_elements_pdf + partition_chunk_pdf]):
    print(i)

# By artificially limiting the max_characters parameter, the chunk_elements function will create more chunks
chunk_elements_pdf_small = chunk_elements(pdf_elements, max_characters=250)
print(len(chunk_elements_pdf_small))
    
```

    32
    32
    <class 'unstructured.documents.elements.CompositeElement'>
    58