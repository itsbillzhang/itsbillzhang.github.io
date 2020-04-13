JavaScript is required. Please enable it to continue.
Your browser lacks required capabilities. Please upgrade it or switch to
another to continue.
Loading…
,,,,,In this game, you assume the position of a judge. You will listen
to stories of the victim, the accused, and examine evidence. You will
then give out jail sentences, or pardon the accused if you believe they
are innocent. To get started, what is your last name again? \<\<textbox
"\$name" ""\>\> [[Confirm name|Welcome Screen]] \<\<audio "background"
loop play\>\>Welcome to the court-room, Judge \$name. Select your case:
[[Turner v. People|Rape Trial]] \<div\> June 2, 2016 People v. Turner
Santa Clara County Superior Court [[Continue|Brian Case Main Screen]]
\</div\> \<\<audio gavel play\>\> \<\<set \$viewedevidence = false\>\>
\<\<set \$viewedaccused = false\>\> \<\<set \$viewedvictim = false\>\>
\<\<set \$l1 = false, \$l2 = false, \$l3 = false\>\> \<\<set \$e1 =
false, \$e2 = false, \$e3 = false, \$e4 = false\>\> \<\<set \$innocent =
false\>\> \<\<cacheaudio "background" "media/ambientmusic.mp3"\>\>
\<\<cacheaudio "firstbvo" "media/p1brian.mp3"\>\> \<\<cacheaudio
"secondbvo" "media/p2brian.mp3"\>\> \<\<cacheaudio "thirdbvo"
"media/p3brian.mp3"\>\> \<\<cacheaudio "satern"
"media/saternmusic.mp3"\>\> \<\<cacheaudio "fuck"
"media/ksisound.mp3"\>\> \<\<cacheaudio "partysound"
"media/partysound.mp3"\>\> \<\<cacheaudio "nightsound"
"media/nightsound.mp3"\>\> \<\<cacheaudio "emilyvoice"
"media/p1victim.mp3"\>\> \<\<cacheaudio "gavel" "media/gavel.mp3"\>\>
\<\<cacheaudio "police" "media/police.mp3"\>\> Here are the stories you
can view. Once examined, an story cannot be revisited. \<\<if
\$viewedaccused === false\>\> [[Brian's Story]] \<\</if\>\> \<\<if
\$viewedevidence === false\>\> [[Evidence]] \<\</if\>\> \<\<if
\$viewedvictim === false\>\> [[Victim's Story]] \<\</if\>\> \<\<if
\$viewedvictim === true and \$viewedaccused === true and
\$viewedevidence === true\>\> Do you think that Brian Turner is guilty?
[[Yes|Judgement Screen]] [[No|not guilty]] \<\<audio "background" volume
1 fadeoverto 5 0 \>\> \<\</if\>\> \<\<audio background volume 1\>\>
\<\<audio background play\>\> \<\<audio "emilyvoice" stop\>\> Hi, my
name is Brock Turner, and I am not a rapist. I grew up in a wealthy part
of [[Dayton, Ohio]]. I attended Oakland High School, and won many titles
while being on the swim team. I currently still hold the fastest 500
meter time in Ohio. Here’s me after [[winning a state title]]. I won
many of those, and I was an All-American Athlete. I was accepted to
[[Stanford University]] on a swimming scholarship. \<\<if \$l1=== true,
\$l2 === true, \$l3 === true\>\> \<div\>[[Next]]\</div\> \<\</if\>\>
[[Female at a last week's party]] [[Blood Alcohol Levels]] [[DNA Tests]]
[[Witness]] \<\<if \$e1=== true, \$e2 === true, \$e3 === true, \$e4 ===
true\>\> \<div\>[[Main Screen|Brian Case Main Screen]]\</div\>
\<\</if\>\> \<\<set \$viewedevidence = true\>\>One day, I was at work,
scrolling through the news on my phone, and came across an article. In
it, I read and learned for the first time about how I was found
unconscious, with my hair disheveled, long necklace wrapped around my
neck, bra pulled out of my dress, dress pulled off over my shoulders and
pulled up above my waist, that I was butt naked all the way down to my
boots, legs spread apart, and had been penetrated by a foreign object by
someone I did not recognize. I kept reading. In the next paragraph, I
read something that I will never forgive; I read that according to him,
I liked it. I liked it. Again, I do not have words for these feelings.
My breasts had been groped, fingers had been jabbed inside me along with
pine needles and debris, my bare skin and head had been rubbing against
the ground behind a dumpster, while an erect freshman was humping my
half naked, unconscious body. But I don’t remember, so how do I prove I
didn’t like it? He has done irreversible damage to me and my family
during the trial and we have sat silently, listening to him shape the
evening. [[Phone]] [[I have finished listening to Emily|Brian Case Main
Screen]] \<\<set \$viewedvictim = true\>\> \<\<set \$voice = false\>\>
\<\<if \$voice === false\>\> \<\<audio "emilyvoice" volume 0 fadein\>\>
\<\</if\>\> \<\<audio background pause\>\>\<div\>[[back|Brian's
Story]]\</div\> \<div class="container"\> \<img src="media/dayton.jpg"
width= "400" height= "400"/\> \</div\> \<\<set \$l1 = true\>\>
\<div\>[[back|Brian's Story]]\</div\> \<div class="container"\> \<img
src="media/medal.jpg"/\> \</div\> \<\<set \$l2 =
true\>\>\<div\>[[back|Brian's Story]]\</div\> \<div class="container"\>
\<img src="media/stanford.jpg"/\> \</div\> \<\<set \$l3 = true\>\>One
night, January 17, 2015, I went to a party with some bros of mine.
There, I flirted with her. We drank beer together. We drank, and we
drank. In total, I drank 9 beers. I don’t know how much she drank, but
it had to be more. We also made out. She agreed to come back to my room.
\<div\> [[Next|my room]] \</div\> \<\<audio "partysound" volume 0.6
fadeoverto 20 0\>\> \<\<audio "background" volume 1 fadeoverto 3 0\>\>
\<\<audio "nightsound" volume 0 fadeoverto 20 0.6\>\> On the way back,
she were the one who slipped. I started kissing her. I also asked her if
she wanted me to finger her, which she agreed to. We then started dry
humping. At no time did it ever occur to me, or did it ever seem that
she were too drunk to know what we were doing. \<div\> [[Next|Cops]]
\</div\>2 guys came up and grabbed me. I then heard the police come. I
smiled, as I thought they were coming to save me. To my utter dismay, I
was taken to jail on accounts of rape. \<\<set \$viewedaccused =
true\>\> \<div\>[[Go back to main screen|Brian Case Main Screen]]
\</div\> \<\<audio "nightsound" volume 0.6 fadeoverto 11 0\>\> \<\<audio
"police" play\>\> [[Emily's Reaction]] [[Further Readings]] Reading from
Brian's Point of View has been paraphrased from recounts. Reading from
Emily's Point of View is taken verbatim from her statements. \<a
href="https://en.wikipedia.org/wiki/People\_v.\_Turner"\>Wikipedia
Article\</a\> \<a
href="https://www.apnews.com/962a8de554994637afce94a22afb78e9"\>Timeline
of Events\</a\> \<a
href="https://www.buzzfeednews.com/article/katiejmbaker/heres-the-powerful-letter-the-stanford-victim-read-to-her-ra"\>
Emily's full statement to Brock\</a\> \<a
href="https://heavy.com/news/2016/06/brock-turner-full-statement-letter-judge-aaron-persky-court-victim-rape-stanford-rapist/"\>
Brock's defense \</a\> [[Back to Emily's Reaction|Emily's Reaction]]
Thank you for going through my first project. \<a
href="https://www.youtube.com/watch?v=dzNvk80XY9s"\>Song: Saturn by
Sleeping at Last\</a\> On the news, you go through the comments on the
articles reporting the case. [[Read comments]] [[Put phone down|Victim's
Story]] \<\<set \$voice = true\>\> \<div class="container"\> \<img
src="media/phone.png" alt="Snow" style="width:70%;"\> \<div
class="centered"\>"She's not pretty enough to have been raped."\</div\>
\</div\> \<div\> [[Next comment|Comment2]] [[Back|Victim's Story]]
\</div\> \<div class="container"\> \<img src="media/phone.png"
alt="Snow" style="width:70%;"\> \<div class="centered"\>"Sad. I hope my
daughter never ends up like her."\</div\> \</div\> \<div\> [[Next
comment|Comment3]] [[Back|Victim's Story]] \</div\>\<div
class="container"\> \<img src="media/phone.png" alt="Snow"
style="width:70%;"\> \<div class="centered"\>"No rape occurred….he
copped a feel…just like some 9th grader on a date…this is a railroad job
by the lesbians." "I agree. Attempted rape? He was way
overcharged."\</div\> \</div\>\<div\> [[Next comment|Comment4]]
[[Back|Victim's Story]] \</div\>\<div class="container"\> \<img
src="media/phone.png" alt="Snow" style="width:70%;"\> \<div
class="centered"\> \<span class="bigtext"\>"As a victim (if I was her) I
would feel horrible waking up and being told I was found bottomless and
sexually violated. I would want to never lose coherency again, I would
want for this to never be mentioned. I would want privacy to work
through this. I would not want to shame someone and forever alter their
lives for events that I cannot remember!! That is what would haunt me."
\</span\> \</div\> \</div\> \<div\> [[Back|Victim's Story]]
\</div\>\<\<if \$innocent === false\>\> You're correct!\<\</if\>\> A
jury months prior found Brian guilty of 3 felony counts: \<ul\>
\<li\>Assault with intent to commit rape of an intoxicated or
unconscious person.\</li\> \<li\>Sexual penetration of an intoxicated
person. \</li\> \<li\> Sexual penetration of an unconscious person.
\</li\> \</ul\> The min. prison sentencing by California law is 24
months, while the max. is 120 months. Judge \$name, what is your
decision on the number of months Brian should get? \<\<textbox
"\$decision" ""\>\> [[Confirm sentencing|Results screen]] \<\<audio
satern volume 0 fadein\>\> \<\<audio satern time 143\>\> A female
Stanford student who lived in Brian’s dorm building introduces a friend
to him. The friend later tells police Turner “creeped her out” and was
“grabby” with her because he was placing his hands on her waist, stomach
and thigh while they were dancing. The friend says she didn’t invite
Turner to dance with her nor did she seek his physical attention. She
tells police she left the dance floor to get away from Turner.
\<div\>[[Back|Evidence]]\</div\> \<\<set \$e2 = true\>\>Turner's blood
alcohol content was estimated to have been 0.171% at 1 a.m. Emily's
blood alcohol concentration was measured in a hospital several hours
after the assault, and doctors estimated her intoxication level at 1
a.m., to have been around 0.22%. For reference, in Canada, an alcohol
content of 0.08% is illegal to drive under.
\<div\>[[Back|Evidence]]\</div\> \<\<set \$e1 = true\>\>Emily's DNA was
found under the fingernails of Brian's left and right hands and on a
portion of his right finger. The test did not show when the DNA was
deposited and could not tell if it was blood, but resembled blood.
\<div\>[[Back|Evidence]]\</div\> \<\<set \$e3 = true\>\>"I was cycling
by the fraternity with my friend Arndt on the Standford Campus at around
1:00 a.m that night when we saw the assult. We surprised the man from
behind the dumpster, who was on top of an unconscious woman. I asked him
"What the fuck are you doing? She's unconscious." The man then fled. I
tripped him, and he was clearly smiling. Arndt went to determine if she
was alive." - Peter Lars Jonsson, Stanford graduate student.
\<div\>[[Back|Evidence]]\</div\> \<\<set \$e4 = true\>\> Brian Turner
was sentenced 6 months in jail, but was able to leave after 3 after
"good behaviour." That is \<\<set \$decision -= 3\>\> \$decision months
less than your intended sentencing. [[I thought you said the min. was 24
months|Judge info]] \<\<audio gavel time 0 play\>\> People v. Turner was
a real case that carried along great controversy in 2015. Under
California law, judges are able to give below the mininum if they wish
to factor in a defendant's lack of criminal history and the effect of
incarceration would have on the defendant. Judge Aaron Persky was a
Stanford grad, and a former Stanford lacrosse team captain. In reponse
to questions about his leniency, he responded how anything more would
have “a severe impact on him.” The California Commission on Judicial
Performance cleared Persky of any wrongdoing in the Turner sentence.
[[Final Thoughts]] [[Further Readings]] ... \<span class="smalltext"\>
Excerpts taken from \<a
href="https://www.glamour.com/story/women-of-the-year-emily-doe"\>Glamour\</a\>
\</span\> From the beginning, I was told I was a best case scenario. I
had forensic evidence, sober unbiased witnesses, police at the scene. I
had everything, and I was still told it was not a slam dunk. I thought,
if this is what having it good looks like, what other hells are
survivors living? I’m barely getting through this but I am being told
I’m the lucky one, some sort of VIP. It was like being checked into a
hotel room for a year with stained sheets, rancid water, and a bucket
with an attendant saying, No this is great! Most rooms don’t even have a
bucket. After the trial I was relieved thinking the hardest part was
over, and all that was left was the sentencing. I was excited to finally
be given a chance to read my statement and declare, I am here. I am not
that floppy thing you found behind the garbage, speaking melted words. I
am here, I can stand upright, I can speak clearly, I’ve been listening
and am painfully aware of all the hurt you’ve been trying to justify. I
yelled half of my statement. So when it was quickly announced that he’d
be receiving six months, I was struck silent. Immediately I felt
embarrassed for trying, for being led to believe I had any influence.
The violation of my body and my being added up to a few months out of
his summer. The judge would release him back to his life.I began to
panic; I thought, this can’t be the best case scenario. If this case was
meant to set the bar, the bar had been set on the floor. In the very
beginning of it all in 2015, one comment managed to lodge harmfully
inside me: Sad. I hope my daughter never ends up like her. I absorbed
that statement. Ends up. As if we end somewhere, as if what was done to
me marked the completion of my story. Instead of being a role model to
be looked up to, I was a sad example to learn from, a story that caused
you to shield your daughter’s eyes and shake your heads with pity. But
when my letter was published, no one turned away. No one said I’d rather
not look, it’s too much, or too sad. Everyone pushed through the hard
parts, saw me fully to the end, and embraced every feeling. If you think
the answer is that women need to be more sober, more civil, more
upright, that girls must be better at exercising fear, must wear more
layers with eyes open wider, we will go nowhere. When Judge Aaron Persky
mutes the word justice, when Brock Turner serves one month for every
felony, we go nowhere. When we all make it a priority to avoid harming
or violating another human being, and when we hold accountable those who
do, when the campaign to recall this judge declares that survivors
deserve better, then we are going somewhere. So now to the one who said,
I hope my daughter never ends up like her, I am learning to say, I hope
you end up like me, meaning, I hope you end up like me strong. I hope
you end up like me proud of who I’m becoming. I hope you don’t “end up,”
I hope you keep going. And I hope you grow up knowing that the world
will no longer stand for this. Victims are not victims, not some
fragile, sorrowful aftermath. Victims are survivors, and survivors are
going to be doing a hell of a lot more than surviving. - Emily ...
[[Further Readings]] \<div\>If you are on mobile, please switch to
Desktop by clicking the link at the very bottom of the page first. Make
sure sound is on.\</div\> \<span class="bigtext"\> \<div\> [[Click Here
to Start|Start Screen]] \</div\> \</span\> \<span class="smalltext"\>
\<div\> Version 1.1 \</div\> \</span\> \<div id="DesktopMobileFooter"
class='h\_center'\> \<hr/\> \<div class="MobileToggle"
style="display:none;"\> View \<a id="DesktopSite" href="\#"
\>Desktop\</a\> | \<strong\>Mobile\</strong\> Site \</div\>\<div
class="MobileToggle"\> View \<strong\>Desktop\</strong\> | \<a
id="MobileSite" href="\#" \>Mobile\</a\> Site \</div\> \</div\>
\<\<script\>\> \$(document).one(':passagerender', function (ev) {
\$(ev.content).find(".MobileToggle a").click(function () { var viewport
= (\$(this).attr('id') === "MobileSite") ? 'width=device-width,
initial-scale=1.0' : 'width=1200';
\$("meta[name=viewport]").attr('content', viewport);
\$(".MobileToggle").toggle(); return false; }); });
\<\</script\>\>That's [[incorrect|Judgement Screen]] \<\<set \$innocent
= true\>\>
