---
layout: post
title: Randomizing American Football
subtitle: This Can't Be Worse Than The Vikes
gh-repo: ACBlock/ACBlock.github.io
gh-badge: [star, fork, follow]
tags: [American Football, random, offensive plays]
comments: true
---

It turns out that I lied and the first real post on this site is going to be something I thought about while watching the first Minnesota Vikings game of the season this afternoon. After watching the lackluster offensive performance of the team, I got to thinking "You could give these guys random plays and things would end up exactly the same".

And then I got to thinking: What is a random play? Is it a play taken at random from Mike Zimmer's playbook? Or is it a play that has all blocking assignments and routes randomized in ways that may or may not make any sense. I decided the second option was a more interesting question and I've decided I want to program a random play generator. I don't have it up an running yet, and a prototype will likwly not be ready until next weekend, but I do want to talk through my intial thoughts for the process and what I envision for the end state of this project.

**American Football, Briefly**

For those reading who do not necessarily know much about football but want to read anyway, a brief primer should be given. There are two types of American Football plays: Passes and Runs. On a pass, the quarter back gets the ball from his center and all player not blocking defenders run "routes". The quarterback will attempt to throw the ball to one of these players. On a run, the quarterback will either give the ball to a running back or run the ball himself. Easy peasy.

This is not difficult on the surface, but each play has a lot of moving parts. Sometimes running backs and tight ends block, sometimes they run routes. The offensive line will always block, but the actual block can get complicated and there are different types of block, despite just seeming like a bunch of dudes mashing themselves into one another. This problem is not terribly hard, but it does have a lot of moving pieces. Let's begin.

**The Players on the Field**

At any one time, there will be 22 players on the field, 11 of them offensive. 7 of them must be on the line of scrimmage (a line that stretches from the location of the ball to the sidelines). The five offensive lineman will always be on the line and of the remaing players, only 4 of the may be off the line of scrimmage. Let's look at the non-lineman players.

### Quarterback
The quarterback will either pass or handoff the ball. He is the most important player on the field. There is only one on the field at any one time (usually, these cases will not be handled because they are fairly rare).

### Running Backs
Running backs primarily run the ball, but may receive a pass or pass block. There will be between 0 and 3 of them on the field at once.

### Wide Receivers
Wide Receivers primarily catch passes, but may run with the ball. This is, in general, rarer than a running back catching a pass. Wide Recievers will almost never pass block, as their talents are better served actually catching passes. Anywhere betwween 0 and 5 Wide Receivers will be on the field at once, but there are usually at least 2.

### Tight Ends
Tight Ends are a hybrid between an offensive lineman and Wide Receiver. They will either block or run a route. A Tight End running the ball is exceptionally rare. There will be between 0 and 3 of them on the field at once.

The number of RB and TE can be summarized by a two letter code. The first number refers to the number of RB on the field, and the second refers to the number of TE on the field. For instance, 11 personnel refers to 1 RB and 1 TE. 32 personnel refers to 3 RB and 2 TE. The number of RB and TE on the field at once is 5, either 23 or 32 personnel. This is due to the maximum number of players on the field, which is 11. 6 RB and TE plus 5 OL plus the QB is 12 players, so 33 personnel and above is illegal.

**The Algorithm**

Now that we know a few things about football and the positions, let's talk about how to make a random play. I'll only discuss the passing plays today and talk about running plays later this week.

First, we need to determine what personnel we'll be using. Some personnel are easier to run out of and some are easier to pass out of, but this is a random play. We could very well pass out of 32 personnel. It's legal, just not a great idea. SO first, we determine the number of RB we have for this play. For this, we simply generate a random number between 0 and 3, inclusive. Then we determine the number of TE. 75% of the time, we'll again generate a number between 0 and 3, inclusive. However, 33 personnel is illegal. So if we have 3 RB, then the number of TE is a randomly generated number between 0 and 2, inclusive. The number of WR is then 11 - (6 + #RB + #TE). 

Once we have our personnel, we can determine blocking. On a passing play, the OL will block as will the RBs and TEs from time to time. I do not know roughly how often the RBs and TEs are kept in to block. For this first draft, I will assume they block 50% of the time. This isn't accurate, but I'll keep looking to find this info. OL blocking is complicated, and it isn't something I understand perfectly. For this draft, I will simply say that the OL is blocking and can add in more complexity as I learn more.

Once we know who is blocking, we can assign routes. Each WR will receive a route, as will the TEs and RBs who are not blocking. There are 9 basic routes that a WR will run, seen here.

![Route Tree]"assets/img/football-route-tree.jpg"

I will assign one of these at random to each WR. TEs that are not block will also receive one of these routes. RBs will receive different routes, as they cannot run the full route tree from the backfield. They will be assigned only the flat, slant, and wheel route. The wheel route is not shown on the diagram, but bascially just means that the RB starts by running an out from the backfield and the turns upfield, as in a go route.

And that's all we need to design a random passing play. Here's the full algorithm:

- Determine personnel. # of RB, then # of TE, then # of WR
- Determine blockers. All OL will block, and each RB and TE has a 50% chance to block. No WR will pass block
- Determine routes. Assign a random route to each WR and non-blocking TE. RB recevive a smaller subset of routes.

There is, of course, more complexity that can be added to the routess and blocking, but this is a good place to start. I hope to get my running play post up soon and a 1st draft of the generator by next weekend. Work will determine how likely that is.  

My ultimate goal for this project is to be able to generate an entire random playbook and even generate the playbook and name of the play. For now, I will be content to create text outlining responsibilities on each play. This should be fun project.

Thanks for reading, 

Andy 








