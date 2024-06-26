+++
title = "OSCP Experience"
date = 2019-05-24
[taxonomies]
tags = ["personal"]
+++

![cover](oscp-cover.png)

OSCP is widely regarded as a difficult certificate to achieve and I understand why people would see it that way. There are not many certificates that requires passing a 24-hour hands-on exam. I've spent around 300+ hours in the past 3 months preparing for this exam and managed to [pass](https://www.youracclaim.com/badges/1a8cddb7-4bb5-4cf9-acce-f6053f08f494) on my first attempt with 80/100 points. Was it easy? Absolutely not. Don't be scared though, I'm sharing my ways to optimize your time and experience when taking OSCP.

### Skills and resources

Coding experience is not really required but will help with reading exploits easier. I would recommend doing a quick python tutorial on codeacademy and get familiar with syntax and loops. Knowing a little bit of bash would help as well but not required.

If you don't have a strong IT background, read Georgia Weidman's book on penetration testing. It's a classic on what will entail when working through the OSCP labs and exam. Another recommendation is go through INE's OSCP course, it will give you an insight on what the OSCP course material is like and can be a good complementary material for the actual course.

### Building confidence

Try doing some of vulnerable machines listed on [vulnhub](http://www.vulnhub.com). I would advise doing some of the easier machines like Kioptrix and Mr Robot. Try attempting these machines with best of your abilities. If you get stuck, make a guess of where the vulnerability possibly be found and read some of the walkthrough guides as hints until eventually you reach root access. I know a lot of people who would speed through these walkthroughs or use them as crutches, don't do this. The point of reading walkthrough guides is seeing if you are on the right direction _after_ spending countless of hours on a single problem. You need to train your brain to think like a hacker, a lot of hard work, discipline and time is required to think like one. Don't sell yourself short.

Lastly, know that it's not so much about the techniques you need to learn, it's all about learning the methodology that offsec is trying to teach. Coming from an system administration background did help a bit during my OSCP studies but honestly, it's all about paying attention to what you are reading and "trying harder" to find relevant information to help you steer in the right direction.

### "I don't have time to study" 

Studying for the OSCP is not something you do with minimal effort. Depending on how much experience you have in IT, you probably will have to spend 2-3 hours every night and 4-6 on the weekends for 1-3 months depending on the lab time you bought. The number of hours you spend on studying and preparing for the OSCP will reveal itself during the exam. Don't try to cram or speed through the labs. I've seen people finish all of the lab machines and still fail the exam.

In addition, once the OSCP lab time expires, you may want to continue sharpening your skills using penetration platforms such as vulnhub or hackthebox. If you do spend time on hackthebox, I would recommend watching ippsec on youtube to see how an experienced penetration tester approaches with problem solving. Take time to write down notes as his methodology will give you insights about your own flawed methodology.

### Maximizing lab time

**The labs are there for you to explore** and soak in the hard work of your successes and failures. From what I've seen so far, the average person that passes the exam has rooted about 30 lab machines. I found this to be true as well for myself as I only had purchased 60 days lab time. There are outliers who only rooted 10 machines and still manage to pass on the first attempt. What I'm trying to tell you is, how many machines you rooted will not matter. It's about what you have learned during the labs and how your thought processes have changed the way you think when approaching problems. As simple as that sounds, it won't be easy and will require plenty of hard work, dedication and frustration.

**Practice taking notes** while working on the labs. Your notes will be clunky at first because either you copy and paste things that aren't useful, or not taking down notes at all when it's needed. Taking notes will give you a structure of how you think, what you have done so far, and if you get stuck, you can review your notes and see what the overall machine looks like and may give you an idea on approaching the problem differently.

If you get stuck on a machine after working on it for a hours, it can be helpful to **take a peek for hints** on the offsec forums. Just so you know, the hints given are cryptic and sometimes confusing which makes the situation more confusing. If you don't know what the hint is about, write it down on your notes and work on the problem later. Sometimes your brain just needs a break and focus on something else.

**Don't become too reliant on hints** and before you take a peek, have a guess of what you think the vulnerability could be and if there is anything you can do to confirm your hypothesis. For instance, you got a PoC that does RCE, can you perform a simple experiment that confirms the RCE such as a ping back? How would you confirm if you have received ICMP packets? Breaking down the steps into managable pieces is immensely helpful because you will run into plenty of troubleshooting of PoC code that you don't have full understanding of.

**Creating your own checklist** shows your current understanding of what your methodology and knowledge looks like. I would advise updating this checklist as you move along in the labs. By the end the labs, you will see that your checklist is like a mindmap of what you did and learned. This checklist will be helpful during the exam to keep you sane in the enumeration process.

**Create your own cheatsheet** so you can quickly refer frequently used commands such as nmap. A quick Google search will show many OSCP cheatsheets available but you should make your own because it will retain in your memory better. I personally used Dokuwiki to store my cheatsheet and works wonderfully. The great part about Dokuwiki is it saves pages as plain text. I can easily export the document and convert to markdown if needed. If you use a text editor like vim, you can easily edit the pages all in command line if that's your thing. Lastly, you don't need to run a web server to host dokuwiki, you can run the docker image on your local machine.

Finally, **don't use kernel exploits to root machines**. There is only 1 machine on the labs that are designed to be kernel exploited. As I've said before, rooting all lab machines isn't the objective. Taking the time to learn the pentesting methodology and understanding your own flaws is the biggest take away from the labs. Discovering your flaws will save time from falling into rabbit holes. You will get better at not falling into rabbit holes over time as you will see the repeating pattern for failures.

### Exam tips

**Manage your time is #1 priority**. It's a 24-hour exam but in reality, it's only 16-17 workable hours due to sleep/naps. No sleep = death. There are 5 machines in the exam. 2x 25pts, 2x 20pts, 1x 10pts. The BoF (25pts) should take about 2 hours and the 10 pointer should take another hour. Roughly, you have about 13-14 hours to work on the other 3 machines so that's about 4 hours per machine. Keep track of how much time you are spending per machine. If you get stuck for awhile, move on to the next machine and come back to it later.

**If you have a computer with beefy specs, use it**. If you don't, consider stopping or removing unnecessary running background services. During the exam, you are required by offsec's proctor to run screen sharing and web cam software, it can be quite a memory hog and CPU intensive. My 2015 Macbook Pro with 8GB specs running Kali VM was barely chugging along. My fan and CPU was running at 80-90% utilization for the entire exam. I had a bit of lag in between mouse clicks and typing. Not the greatest experience but it can be way better if you had a performance machine to work with. Lastly, I've seen people use screen recording software during the exam, I heard it's allowed but not officially. It would be useful to record your screen to capture screenshots that you may have missed.

**Use a tool to automate your enumeration process**. I used [AutoRecon](https://github.com/Tib3rius/AutoRecon) and it was great for the labs and helpful in the exam as it will run scans in the background while you are working on a machine.

**Exercise, snacks, water, and bathroom breaks**. Do I need to say more about this? Take care of yourself. A 30-minute run or exercise before the exam would be helpful for the brain.

**You managed to get enough points and passed?** Great! Have a short celebration as it's only half the battle, get ready to be even more scared. I hope you taken enough screenshots required to write your report or it's a big fat zero. Did you follow your methodology and checklist? Yes? Well you don't need to be scared, you learned the ways of being a penetration tester. Congrats! Writing the report can be scary because there are many little details required for the report. I've read stories about how someone failed their exam because they submitted the report with an incorrect filename and/or format even when he/she scored 100%. Don't let this happen to you, read the report requirements in detail and many times as you need.

**Enjoy your exam**. I know that sounds ridiculous because exams are suppose to be stressful right? The way I see it is, if you spent a good chunk of time studying OSCP, your knowledge and skills should have improved immensely since you've started. Passing and failing the exam doesn't matter, as long as you care about improving yourself everyday. Even if you failed, you are still better than yesterday. There will always be another chance of taking the exam, but the time loss from not improving yourself is lost forever.

### Life after OSCP

I didn't stop at OSCP, neither should you. OSCP is considered to be a beginners pentesting certification believe it or not. It's only a part of what infosec is really about. Attacking a network is one piece of the puzzle, defending is another. I'm currently learning about the defensive side as the job market has a need for penetration testers that specializes for both sides. 

While OSCP is great to have, what it lacks is real world and up to date lab machines. Although, I would think offsec would disagree with my statement because they wanted to teach the fundamentals and methodology rather than focusing on the technology piece. In the real world, you need to learn about protecting Active Directory and cloud services. This is what I'm currently focusing on and it's not easy.

Finding work in infosec is difficult if you don't have much experience in infosec even though you have plenty of IT experience. I'm trying to break into infosec and I haven't had much luck. It's not easy in this slow job market even if you have OSCP. Although, my job search is getting better lately after I creating this website to display my work and experiences.

Best of luck with OSCP and keep on learning!