# Merckle-Y750-Final-Project
MG-CFA simulation exploring parameter recovery for invariance testing.

# Contents
## Code
**1. Model**

CE.2 <- "DD =~ DDrace + DDeconomic + DDreligion + DDpolitical
             ET =~ ETgoals + ETorganize + ETexample + ETdraftfb + ETfeedback
             SF =~ SFcareer + SFotherwork + SFdiscuss + SFperform
             SE =~ SEacademic + SElearnsup + SEdiverse + SEsocial + SEwellness + SEnonacad + SEactivities + SEevents
            CE=~DD + ET + SF + SE
            DDreligion ~~ DDpolitical
            ETdraftfb ~~ ETfeedback
            ETorganize ~~ ETdraftfb
            ETgoals ~~ ETorganize
            ETorganize ~~ ETexample
            SFdiscuss ~~ SFcareer
            SFdiscuss ~~ SFotherwork
            SFcareer ~~ SFotherwork
            SEacademic ~~ SEevents
            SEdiverse ~~ SEactivities
            SEacademic ~~ SElearnsup
            SEsocial ~~ SEwellness
            SEwellness ~~ SEnonacad
            SEsocial ~~ SEnonacad
            SEevents ~~ SEactivities
            SEsocial ~~ SEevents"
            
**1a. Base lavaan model**

CE.model<- cfa(CE.2, 
             data = NSSE, 
             missing = "ML")
             
**1a.1. Adding group level**

CE.2.model3<- cfa(CE.2, 
       data = NSSE, 
       group = "countrycol",
       missing = "ML")

**1b. Configural invariance model**

CE.2.model1<- cfa(CE.2, 
                   data = NSSE, 
                   group = "countrycol",
                   missing = "ML",
                   meanstructure = TRUE,
                   group.equal = c("loadings"))
                   
**1c. Metric Invariance model**

CE.2.model2a<- cfa(CE.2, 
                  data = NSSE, 
                  group = "countrycol",
                  missing = "ML",
                  meanstructure = TRUE,
                  group.equal = c("loadings", "intercepts"))

**1d. Scalar Invariance model**

CE.2.model3<- cfa(CE.2, 
                  data = NSSE, 
                  group = "countrycol",
                  missing = "ML",
                  meanstructure = TRUE,
                  group.equal = c("loadings", "intercepts", "means"))

**2. Code for any model summary/fit indices**

summary([model], fit.measures = TRUE)

fitmeasures([model], fit.measures = c("chisq", "df", "CFI", "TLI", "RMSEA"))

**3. Data simulations**

Output.1 <- sim(generate = lavaan, rawData = NSSE, nRep = 500, n = 275, model = CE.2.model1, group = "countrycol", lavaanfun = "cfa",  silent = FALSE, stopOnError = TRUE, seed = 47401)
summary(Output.1)

Output.2 <- sim(generate = lavaan, rawData = NSSE, nRep = 500, n = 275, model = CE.2.model2, group = "countrycol", lavaanfun = "cfa",  silent = FALSE, stopOnError = TRUE, seed = 47401)
summary(CE.2.model2a)

Output.2a <- sim(generate = lavaan, rawData = NSSE, nRep = 500, n = 275, model = CE.2.model2a, group = "countrycol", lavaanfun = "cfa",  silent = FALSE, stopOnError = TRUE, seed = 47401)
summary(CE.2.model2)

Output.3 <- sim(generate = lavaan, rawData = NSSE, nRep = 500, n = 275, model = CE.2.model3, group = "countrycol", lavaanfun = "cfa",  silent = FALSE, stopOnError = TRUE, seed = 47401)
summary(CE.2.model3)
summary(Output.3)



## Data

These data cannot be released into the public domain; however, I have included a results excel document that has many tabs of summary data and covariance matrices by group, so replication would be possible.


## Reflection
I have several broad areas that, as I reflect, I see areas of challenge and growth. The first area related to the exploration of my chosen topic, the second related to the exploration of a process, a last area related to expectations and reality. Our task was simple: create a project. We were given a very large canvas from which to paint; the outcome was an appropriate-to-graduate,-doctoral-level-coursework study on a topic of our choosing; the only small caveat was data needed to be in-hand early, so that brush strokes could be smoothed and shading and touch-ups could be corrected before the project 'dried' on December 7. For this project, I chose a simulation study; it was a first for me.

The first notion of exploring a topic seemed typical for graduate seminar; that is, most courses have the students choose a topic and write or create a project. A common difference in a methods course is that the professor's expertise may not be your topical field; she is only a check on the methodology. This is not to limit her knowledge of sub-disciplines within education, but instead to frame expectations about her advice and direction; the content area is left entirely to the student. There is an inherent call in this process for master painter to trust the student. Moreover, there is a call for the student to trust their learning, preparation, and all the coursework taken.

I chose an existing dataset to make my study and analysis applicable to broader projects (mainly, my dissertation), so in the category of topic exploration, I include data prep, cleaning, and quality. Data prep and cleaning are completed by my research center. This does not mean data are in a final state; at first, I used the entire dataset with over 850 variables. This was an obvious mistake to seasoned r users and I quickly learned to limit the set to a smaller set. I ran a couple simulations and knew that the time required to work on the set I wanted was too long, so I cut further. When time was in a real pinch, I cut all but my stated variables. Interestingly at a later point, I actually wished I has included some other student characteristics as controls, but did not have the time to rework my simulations.

The next issue to tackle is the notion in data cleaning related to missingness on a key variable. To explain missingness in my data, I offer a bit about my project. The data I have is a national survey of college students and I am interested in if there were testing between international and domestic students based on country of origin which are otherwise not related to the latent variables. The data have a country variable that I used to group the students.  Missingness on this grouping variable is at nearly 21% (as a grouping variable that is identity based, it is not one that can be imputed). This was not an inconsequential issue to address and I know of at least three 'causes' of this missingness. The first would be typical survey fatigue from any participant (domestic or international); did the student just get bored? The second would be if there were language/other barriers related to the survey; did the student not understand the language or did the concept not make sense? And the last, did the student understand the question but not want to disclose (or feel safe disclosing) their country of origin due to the political climate in the US.

Exploring a process can be messy. My experience so far is that data simulation lacks paint-by-number designs as each simulation is constructed from the base up. Some projects, where simulation has been employed frequently to answer research questions, examples would be common, while other projects may not have had many. This simulation was not a difficult task but intensive; there were a number of decisions made along the way, but it was the time for which I was ill prepared.
I used the r package simsem to simulate and analyze the data. simsem can call a lavaan object for analysis, so it does make simulation designs easier. It also can investigate real datasets as a input, so the user has an option of creating covariance matrices or pointing to an existing dataset. I chose the latter, but learned quickly that no instructions in the manual or in any published papers had syntax readily available to review with instructions on how to input real data.
One large shift in my project related to understanding the simsem package. My initial proposal was to conduct two or three analysis types (MG-CFA, DIF, and a type of hybrid called alignment). simsem as a package makes one of these methods very easy as it can call a model that analyzes CFAs during the simulation. For the other two components, I planned to used the simulated data and run DIF and Alignment on them. I was unable to figure out, in the time constraints, how to pull the simulated data from simsem to conduct the other analysis. Thus, with the length of time the MG-CFA took, I did not have time to analyze conduct DIF or alignment.

The last area that I will touch on I call expectations and reality. The opening day of class, our professor stated her goal was to teach us to self-direct and self-learn. Her statement was 'you're not always going to have a professor to work with or a class to meet with regularly, so how will you move forward?' While doctoral education is about self-learning, this was one of the first classes where the professor really meant it. This was both frustrating and rewarding. I answered more of my own questions by digging into literature… and then digging again, and reading a couple of sources. This is not to imply that I did not have plenty of questions along the way, but I developed an even clearer perspective on how to proceed when there is limited external help.

When all was said and done, I did simulate my data. I had to change course from my original plan as the process of simulating data took much more time than I anticipated. But, I completed a good project that I am happy with the results for a class project. I anticipate that I will move forward with completing the components of the initial proposal as I will use it as part of my dissertation (I plan on doing a three article disserataion). 

