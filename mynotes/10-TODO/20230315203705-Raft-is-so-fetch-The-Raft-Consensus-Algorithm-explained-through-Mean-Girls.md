---
title: Raft is so fetch: The Raft Consensus Algorithm explained through "Mean Girls"
url: https://www.cockroachlabs.com/blog/raft-is-so-fetch/
clipped_at: 2023-03-15 20:37:05
category: default
tags: 
 - 无
---


# Raft is so fetch: The Raft Consensus Algorithm explained through "Mean Girls"

Raise your hand if you’ve ever been personally victimized by the Raft Consensus Algorithm.

 [![](assets/1678883825-83f2246c0dbd11851572d45ab5867d45.webp) ''](https://giphy.com/gifs/MeanGirls-mean-girls-raise-hand-victimized-emLv8IQBnhabCHgAwl?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

[](https://giphy.com/gifs/MeanGirls-mean-girls-raise-hand-victimized-emLv8IQBnhabCHgAwl?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

Related Gifs

[

Mean Girls

@meangirls

![](assets/1678883825-74e81e61ead1b338148163c89166576b.svg)

](https://giphy.com/MeanGirls?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

[

![mean girls raise hand GIF](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/filmeditor-mean-girls-movie-xT9KVwVraalcvmKBfW)[

![amanda seyfried raise hand GIF](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/filmeditor-mean-girls-movie-3o7aTucH1E8PTkEe7S)[

![So Fetch Mean Girls GIF by Coolidge Corner Theatre](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/thecoolidge-mean-girls-coolidge-corner-theatre-3pYdSakqn9sI0Ao0LN)[

![mean girls hand GIF](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/raising-arizona-eaIpScHwBsA5a)[

![Mean Girls Raise Your Hand GIF by Ava Davis](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/TheAvaDavis-mean-girls-indie-film-raise-your-hand-sOAiaiJp9fNxQE7089)[

![karen smith raises hand GIF](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/filmeditor-mean-girls-movie-xT9KVrWHdXNBoakSGY)

[](https://giphy.com/?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

Continue Watching on GIPHY

[via GIPHY](https://giphy.com/gifs/MeanGirls-mean-girls-raise-hand-victimized-emLv8IQBnhabCHgAwl)

Understanding Raft can be tough. In fact, I’ve seen conversations recently on Twitter in which technical leaders of infrastructure companies demonstrate a lack of understanding. Point being, you’re not alone.

## So, like, what is Raft?![](assets/1678883825-6ef661c28ca97e7f54c6ccc3b0e04d1b.svg)

**Raft is a consensus algorithm used in distributed systems to ensure that data is replicated safely and consistently.** That sentence alone can be confusing. Hopefully the analogy in this post can help people understand how it works. In honor of national _Mean Girls_ day (“on October 3rd he asked me what day it was”), I present the Raft Consensus Algorithm as explained through the movie _Mean Girls_. (For a great, more technical overview of Raft, we recommend [The Secret Lives of Data](http://thesecretlivesofdata.com/raft/)).

 [![](assets/1678883825-2d86bbb3e6c2231db2814e7973dd9766.webp) ''](https://giphy.com/gifs/mean-girls-movie-jV9m6dVescrC0?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.poliglota.org%2F)

[](https://giphy.com/gifs/mean-girls-movie-jV9m6dVescrC0?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.poliglota.org%2F)

Related Gifs

[](https://giphy.com/?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.poliglota.org%2F)

Continue Watching on GIPHY

[via GIPHY](https://giphy.com/gifs/mean-girls-movie-jV9m6dVescrC0)

Raft consensus can be explained using cliques in high school. In the beginning of the movie, Cady is a “home-schooled jungle freak” and thus is not a member of a clique. She is a lone piece of data with no replicas. If she were to be hit by a big yellow school bus, her thoughts on army pants and flip flops would die with her and would never trend. 

The Plastics however, are part of a cluster. If Regina is hit by a school bus, the information she had wouldn’t die with her, since she had already shared it with Karen and Gretchen. If someone were looking for the Burn Book, they could find it by asking one of the remaining two members, even while Regina was recovering in the hospital. If she hadn’t replicated that knowledge, nobody would ever be able to locate the book.

Every cluster of replicas needs to have a Raft leader, or a Queen Bee. Of course, this would be Regina George. Regina is the leader of the Plastics, a group comprised of Gretchen Wieners (her father invented Toaster Strudel and her hair is full of secrets) and Karen Smith (she’s not the brightest bulb and she has weather forecasting superpowers). Gretchen and Karen are the follower replicas. 

This dynamic is similar to Raft in that if there isn’t consensus among replicas, no action can be taken. I mean, you wouldn’t buy a skirt without asking your friends if it looks good on you first, right? Exactly, that’s why you need consensus, or the majority vote. If Regina is shopping and wants to buy a skirt, she can’t do so unless either Gretchen or Karen has signed off on the purchase.

Let’s say Regina tells Gretchen and Karen that on Wednesdays they wear pink. Gretchen eagerly approves first. Now that Regina has Gretchen’s confirmation, the majority of the Plastics (⅔)  are in favor of wearing pink on Wednesdays, and consensus has been reached. Now it’s official. 

 [![](assets/1678883825-49ca52128966fdb9d1bdebc13008f789.webp) ''](https://giphy.com/gifs/filmeditor-mean-girls-movie-3otPozZKy1ALqGLoVG?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

[](https://giphy.com/gifs/filmeditor-mean-girls-movie-3otPozZKy1ALqGLoVG?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

Related Gifs

[](https://giphy.com/?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

Continue Watching on GIPHY

[via GIPHY](https://giphy.com/gifs/filmeditor-mean-girls-movie-3otPozZKy1ALqGLoVG)

## Understanding Quorum in Raft![](assets/1678883825-6ef661c28ca97e7f54c6ccc3b0e04d1b.svg)

The high school environment of _Mean Girls_ is comprised of many different cliques. Typically these cliques each sit together at lunch, with no intermingling between tables. Let’s think of the space between tables as a deliberate schism between the Plastics and the “Art Freaks,” also known as “the Greatest People You Will Ever Meet.” Let’s make numbers easy and think of the Plastics as having 3 people and the Art Freaks as having 2 people, Damien and Janice.

Let’s say a client delivered a message to the Plastics at the same time as another client delivered a message to the Art Freaks. ‘4 for Glenn Coco’ was sent to the Plastics (through Regina, the [Raft leader](https://www.cockroachlabs.com/docs/v19.1/architecture/life-of-a-distributed-transaction.html#raft-leader)), and ‘0 for Gretchen Wieners’ was written to the Art Freaks (through Janice, the Raft leader). 

Since the Art Freaks are made up of only two people, Janice and Damien, they are not able to achieve a quorum, since the clique needs more than two members in order to resolve a tie when voting. Since they can’t achieve a quorum, the commit can’t even be made. However, because her clique has greater than two members (3), Regina was able to secure a majority and commit the change ‘4 for Glenn Coco.’

 [![](assets/1678883825-e70aa7a0f5bf981cdcf9c7a06f0a5389.webp) ''](https://giphy.com/gifs/filmeditor-mean-girls-movie-l2YWwAjBwZdjxIXBe?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

[](https://giphy.com/gifs/filmeditor-mean-girls-movie-l2YWwAjBwZdjxIXBe?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

Related Gifs

[](https://giphy.com/?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

Continue Watching on GIPHY

[via GIPHY](https://giphy.com/gifs/filmeditor-mean-girls-movie-l2YWwAjBwZdjxIXBe)

## Leader Election: Who Gets to Be the Raft Leader?![](assets/1678883825-6ef661c28ca97e7f54c6ccc3b0e04d1b.svg)

When Regina shows up to lunch wearing sweatpants _on a Monday_ she is dramatically booted from her role as leader of the Plastics. At given intervals, a leader must send out a sort of heartbeat to maintain their leadership status. This is their way of saying “hi, I’m still here.” Similarly, any deserving Queen Bee needs to send out cues of their dominance at regular intervals, and when Regina can no longer assert her status, she is no longer the Queen Bee.

The Plastics need a new Raft leader, obviously. Luckily, Cady Heron steps up as the candidate replica, and Gretchen and Karen each reply with their vote to ensure Cady is the new Queen Bee. Now, the Plastics can’t do anything without Cady’s direction first.

When Cady dresses in army pants and flip flops, she only needs one other member of the Plastics to agree that it’s cool to achieve a quorum (with 2 out of 3 votes), and now her style is accepted by all. The state of their high school has become “army pants and flip flops.”

 [![](assets/1678883825-31d1dadac4ed7ffee266a752231a85cb.webp) ''](https://giphy.com/gifs/filmeditor-mean-girls-movie-3otPoABrVegFBZzZIc?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

[](https://giphy.com/gifs/filmeditor-mean-girls-movie-3otPoABrVegFBZzZIc?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

Related Gifs

[](https://giphy.com/?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.cockroachlabs.com%2Fblog%2Fraft-is-so-fetch%2F)

Continue Watching on GIPHY

[via GIPHY](https://giphy.com/gifs/filmeditor-mean-girls-movie-3otPoABrVegFBZzZIc)

Since Cady is now the Queen Bee, or Raft leader, requests from the client will go to Cady first. Let’s say Damien is the client speaking to Cady when the entire class of junior girls is in the gymnasium. Damien shouts “she doesn’t even go here!” Because Cady is the Raft leader, she should be the one to receive this piece of information, and relay it to the rest of the Plastics. Once she has confirmation from at least one of the other Plastics, the information has been committed. 

Does she even go here? No, she just has a lot of feelings. Karen knows she doesn’t go here, Gretchen knows she doesn’t go here, Ms. Norbury knows she doesn’t go here, Janice Ian knows she doesn’t even go here. Everyone knows she doesn’t even go here.

 [![](assets/1678883825-bb988fd28a76c9bfd3635ad3c953122c.webp) ''](https://giphy.com/gifs/filmeditor-mean-girls-movie-xT9KVJZFvCYEWGKHGU?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=)

[](https://giphy.com/gifs/filmeditor-mean-girls-movie-xT9KVJZFvCYEWGKHGU?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=)

Related Gifs

[](https://giphy.com/?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=)

Continue Watching on GIPHY

[via GIPHY](https://giphy.com/gifs/filmeditor-mean-girls-movie-xT9KVJZFvCYEWGKHGU)

Hopefully this makes the Raft consensus algorithm more relatable. With this level of consistency in CockroachDB, the limit does not exist! How fetch is that?

 [![](assets/1678883825-0986007a7af4e3e9970ec3c0e5862b86.webp) ''](https://giphy.com/gifs/reactionseditor-3oKIPkMhoaMlhtarF6?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.kiss925.com%2F2017%2F10%2F03%2Fmean-girls-cast-asks-3-donations-mean-girls-day%2F)

[](https://giphy.com/gifs/reactionseditor-3oKIPkMhoaMlhtarF6?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.kiss925.com%2F2017%2F10%2F03%2Fmean-girls-cast-asks-3-donations-mean-girls-day%2F)

Related Gifs

[

![Fetch Mean Girls GIF](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/rachel-mcadams-lindsay-lohan-amanda-seyfried-SUgOYsXqmexxe)[

![fetch mean girls GIF](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/mean-girls-fetch-gretchen-weiners-G6ojXggFcXWCs)[

![so fetch mean girls GIF by Coolidge Corner Theatre](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/thecoolidge-mean-girls-coolidge-corner-theatre-69qoNrkAAlx4T9njnX)[

![so fetch mean girls GIF by Coolidge Corner Theatre](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/thecoolidge-mean-girls-coolidge-corner-theatre-1lwmbLpLSRGRQg4mqu)[

![fetch mean girls GIF](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/mean-girls-movie-fetch-Y2KuqRqdCsxYk)[

![so fetch mean girls GIF by Coolidge Corner Theatre](assets/1678883825-b9a31d3949b1882a09ed2f8508d538f3.gif)

](https://giphy.com/gifs/thecoolidge-mean-girls-coolidge-corner-theatre-yN4DpWZ6DppfeB99la)

[](https://giphy.com/?utm_source=iframe&utm_medium=embed&utm_campaign=Embeds&utm_term=https%3A%2F%2Fwww.kiss925.com%2F2017%2F10%2F03%2Fmean-girls-cast-asks-3-donations-mean-girls-day%2F)

Continue Watching on GIPHY

[via GIPHY](https://giphy.com/gifs/reactionseditor-3oKIPkMhoaMlhtarF6)