---
layout: post
title:  "The Road to a Prototype. 6. Strings and rant."
date:   2017-05-12
tags: ['article', 'prototyping', 'roguelike', 'sdl', 'strings', 'data']
author: "Hugo Viala"
---

Welcome to *The Road to a Protoype*, a blog series in which I weekly describe my attempt at prototyping a small game all by myself. (But today is kind of special, you will see.)

# A special episode

I did not spend much time working on my prototype this week. Instead, I felt more like working on another small project of mine I did not get the chance to talk about yet. Therefore, it is my pleasure to introduce to you *Gardakan*. *Gardakan* comes from my desire to learn more about networking. Most of what I know - which is still very little - mostly comes from the excellent blog by [Glenn Fiedler](https://twitter.com/gafferongames) called [Gaffer on Games](http://gafferongames.com/). But how could you pretend you really know what is at stake considering TCP, UDP, IP, HTTP, SSH, and all the other protocols that are out there if you did not go for it and implement basic functionallity of a networking engine.

This is the reason behind *Gardakan*. I decided, like in *The Road East*, to use as little libraries as possible without shooting myself in the foot and that is why the only dependency of *Gardakan* is the SDL_Net library. The reasons behind this choice are kind of obvious but, just to be sure, I am going to explain them right now.

1. Cross-platform. I work on Linux and Windows and I do not want to get to platform specific level when I can already get a low-level library doing it for me.
2. I use SDL libraries a lot, so I already have a codebase ready to integrate them along the way.
3. SDL_Net is still a very low-level libraries. I am not cheating. Basically, what you get from SDL_Net is connecting / disconnecting sockets, sending and receiving a TCP/UDP packet, and that's it. From there, my goal is to make a basic engine able to keep track of the state, generate a log, and implement higher-level protocols myself.

As of yet, the project goes quite slowly since it is not my top-priority right now, but as a first landmark, I am going for the implementation of a basic IRC bot. The steps involved in this are the following :

1. Establishing a (secure) connection with an IRC server.
2. Parsing the TCP packet received.
3. Interpret it as an IRC message and update the state accordingly.
4. React to the received IRC message from the server by sending a TPC / IRC message.

Strangely, a step that had me cringing the most as of yet was the second one. Parsing was one of those things that I always avoid to do, and it appeared to me that the reason that it was that way was because of a simple reason : the C string type.

# Why I hate C strings

I was never confortable with the string concept in C. C++ gets away by using a class wraping most of its usage, the infamous std::string. But even that have, I think, a lot of issues.

C, on the other hand, considers that a string is *an array of chars ended by the terminator character* (\0). This is a definition that I respect, but I personally have always hard time with it.

First, you actually have two ways to define a string in code.

{% highlight c++ %}
// NOTE(hugo) : First way
char* Str0 = "Hello, sailor!"; 

// NOTE(hugo) : Second way. You can fill this later with a sprintf-like fuction
char Str1[SIZE_OF_THE_BUFFER_YOU_WANT]; 
{% endhighlight %}

Since, in C, an array is actually a pointer to the first element (and with a given size), the second definition is a sub-case of the first. The problem is the way they induce memory management. The second way clearly allocates a buffer of a given size on the stack. As for making a case for the first way, well I confess I never really understoof where and how the memory is allocated, and whenever I try to understand I forget about it within a few days. 

Even though you could tell me that I should go ahead and learn that stuff for good, I claim that this system is not at all easy and obvious to use, and *that should definitively be the case for a type as simple as strings*. Finally, note that even the size you want to allocate is not at all obvious. Indeed, if you want to create the string "Hello", which is 5 characters long, then you should allocate and array of size 6 because you would have to include the terminator character (\0) to make C understand that this is the end of the string. One could claim that you should just add one whenever you allocate. I claim that this is error-prone, at least in my case.

Last but not least, something I always *hated* about strings in C are function names that perform on them. Maybe it is because I am not a native English speaker but still, for something as fundamental as the string type of your language, I think you should make sure that the name are understadable as first glance and explicit. Why *strtok* and not *ConsumeToken* ? Why *strchr* and not *FindFirstOccurenceOfChar* ?

And even when I see the prototype of *strcat* which is a function that simply concatenate two strings together :
{% highlight c++ %}
char *strncat(char *dest, const char *src, size_t n);
{% endhighlight %}

I can't help but wonder : "Ok, I give the memory index of where *dest* starts, same for *src*. *dest* supposedly have memory allocated just for its initial size, but it will get bigger after this function. Where does the new memory come from ? What happen with the memory of *src* ? Should I be responsible for freeing it ?"

And this is how memory leaks happen[^1].

# I was not the only one fed up

Ok, so you might guess what happens next, right ?

Right ?

I wrote my own small string library. Nothing too fancy, but I could not stand it anymore. I was at first inspired by [Jonathan Blow](https://twitter.com/jonathan_blow) which changed the string logic in his upcoming language *Jai*[^2]. But after making my little experiment, I discovered that other people had similar idea.

Indeed, there are for example the [Simple Dynamic String](https://github.com/antirez/sds) which, quoting the website : 

> SDS is a string library for C designed to augment the limited libc string handling functionalities by adding heap allocated strings that are: simpler to use, binary safe, computationally more efficient, but yet... compatible with normal C string functions.

As another example, you could stumble upon [The Better String Library](http://bstring.sourceforge.net/), a name that I find very funny because I am not the only one to think that C string are not the best. Again, taken from the website :

> The Better String Library is an abstraction of a string data type which is superior to the C library char buffer string type, or C++'s std::string. Among the features achieved are: substantial mitigation of buffer overflow/overrun problems and other failures that result from erroneous usage of the common C string library functions, *significantly simplified string manipulation* [...]

So yeah, the very fact that those libraries exist make my previous points valid. Unfortunately, I did not have the chance yet to test these libs and see if they really compensate for how bad the C string type was, but if I get the chance that could be a funny experience.


# My implementation

Nothing very fancy will happen in here. I already explained what my problems were with the default C string type and the solutions against them should be clear.

First of all, a string -- which I called a rvtn_string for obvious trademarks reasons -- does *NOT* end with a terminator character (\0). Instead, the memory allocated for it is *exactly* the size of the string you have. So if I want to have the string "Hello", then, in memory, 5 chars will be allocated for that. It does mean that I also need to store directly the length of a string in order for the operators to know when to stop looking up in memory.

So here it is ladies and gentleman, the rvtn_string type :

{% highlight c++ %}
struct rvtn_string
{
	u32 Size;
	char* Data;
};
{% endhighlight %}

And storing the size of a string is not even harmful because now you can know the length in constant time whereas it took linear time before.

All string have to be allocated and, it is known in advance, *it is up to the user to release them when their use have ended*.

The great thing is, with that representation, re-implementing most C string operator were very very simple. For example, the function to concatenate two strings is simply[^3] :

{% highlight c++ %}
rvtn_string ConcatString(rvtn_string A, rvtn_string B, memory_arena* Arena = 0)
{
	rvtn_string Result = CreateEmptyString(A.Size + B.Size, Arena);

	for(u32 CharIndex = 0; CharIndex < A.Size; ++CharIndex)
	{
		Result.Data[CharIndex] = A.Data[CharIndex];
	}
	for(u32 CharIndex = 0; CharIndex < B.Size; ++CharIndex)
	{
		Result.Data[A.Size + CharIndex] = B.Data[CharIndex];
	}

	return(Result);
}
{% endhighlight %}

Simple isn't it ? And even simpler to use : you have to give two rvtn_string that you have to free afterwards (and you know that this is your responsability because it always is) and you get a new fresh one. No \0-terminated string, no not-knowing-where-your-memory-went, not more weird name.

# Conclusion

Of course, this is an early stage of the string API but, for my current use -- which is TCP packet parsing -- I was quite happy with what I got. Code got smaller -- which is always good -- and clearer to me. I did not hit performance issue (even though performance was not really at stake) and, even more imporant *I know what I am doing with my memory*. 

Now I can code happily, knowing what I am doing.

---
[^1]: Oh... and do not get me started about *const* strings...
[^2]: Many details about the Jai language can be found in various streaming session from Jon available [on his youtube channel](https://www.youtube.com/user/jblow888).
[^3]: The memory_arena type is something shamelessly taken from Casey Muratori's Handmade Hero that is a basic memory allocator / manager.
