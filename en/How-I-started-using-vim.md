# How I Started Using Vim and Why the Hell I'm Doing It

Vim, or rather Neovim, are of course mainly known for the meme (or joke) that "something is as hard as getting out of vim". Hahaha, so funny, I'm pretty sure even people who don't use vim know :q!. Because once in a while it just opens somewhere, you need to edit that one stupid config option and then you want to get out. So you kill the entire session and open a new terminal.

## What drives you to this, do you want to be a cool hacker or what?

Who wouldn't want to! But that's not the main reason. Look, I use IntelliJ IDEA for work. I swear by it, it's a great tool, the Community version is good enough, Ultimate is worth every penny. It's packed with functionality and with additional plugins it's just a comprehensive supertool.

BUT! Since I don't primarily develop in Java anymore (my primary language is Go), I don't really need most of those things for living. Maven/Gradle, Spring and all the syntax helpers for config files and navigation through annotations, and definitely a bunch of other stuff - I'm throwing in the trash. So much so that I don't even remember where to find them in which menu or what hotkey they have mapped.

The main thing I want is code navigation - find me where a function is called, jump from a function call to its definition, do a search or search and replace of some string within the project, run tests, run build, open terminal, maybe do some git operation. That's currently the main thing for me. Oh and syntax highlighting and autocomplete while typing, but that's kind of automatic.

Plus, what I got used to doing was opening IDEA in fullscreen, in zen mode - just code with minimal menus and stuff around. Ideally two/three files side by side. It works, but when I found out that switching between these files requires some obscure hotkey that conflicts with some system one in macOS, I kind of lost patience.

## Wouldn't it be worth it to just learn to use IDEA properly?

Like, when I add up the time I spend learning vim, it's actually not that bad of an idea. But I'll admit I was a bit swayed by the vision of not needing a mouse at all when programming and doing everything through the keyboard. Because I already try to do as many tasks as possible without a mouse and this is another step.

At the same time, the entry barrier to vim is pretty high though. I knew the basics from school, but it wasn't enough for any reasonable work. Plus I really have no idea how to set it all up to meet my requirements, what plugins to use and so on. Ricing vim configuration is a vice I don't want to have, I don't want to sink that much time into it.

So I reached for **Lazyvim**.

## Ah, so you swapped one bloatware for another, do I understand correctly?

Yes and no. Or like this - sure it's pretty loaded (neo)vim that carries a lot with it - plugins for almost everything you can think of, nerdtree sidebar, buffer tabs, it even has integrated lazygit (which is btw an awesome feature). But at the same time it loads those plugins only when they're needed. You have frontend open? So we'll let the golang plugin sleep, here's the one for typescript!

And also that configuration, where you can really interactively enable plugins you want to install, they download and you're rolling. Awesome.

## Yeah, but the learning curve will be even steeper, right? When you have to learn lazyvim commands and bindings on top of basic vim...

That's something that made me give up on this whole thing once before. Working through the combination of neovim + lazygit + individual plugin documentation was endless pain. Like really, for example `<leader> (which is spacebar) + t` was supposed to open the test menu, but nothing happened. The fact that you have to configure neotest with the appropriate integration for the given language is something you find in three different places.

But here's where AI comes into play and an area where it's really good and useful.

## But AI bullshits a lot...

Yeah, that's true. It also threw me some shortcut that was actually different - but here you can confront it right away - you just try it and when it doesn't work, it either finds it differently or well, you'll probably have to dive into those docs. But so far I've always managed to solve it without long searching.

## So how do you actually learn to work with it?

I just took code and asked AI how to do a given programming step in lazyvim. How to open a file, how to switch between windows and buffers, how to run tests, how to find function calls. And it gladly answered. It even offered me multiple ways to achieve a given goal and I picked the one that suits me best.
When it didn't work, I asked differently or rephrased it. If everything worked, I wrote it down in **my personal cheat sheet**.
And so I'm building my own cheat sheet and I need that AI less and less.

## Hmm, and is it useful to you?

Even with the little I know so far, I feel faster at navigating code. Text operations still give me some trouble, same as remembering whether I need to switch between buffers `(shift + hjkl)` or windows `(ctrl + hjkl)` or even panels in Ghostty `(command + hjkl)`.

But the whole thing is much more responsive than IDEA, which sometimes just crunches and thinks in the background for ages. Here everything is fast as diarrhea and eats a tenth of the RAM. Lots of people will definitely appreciate that lazygit integration. That's probably the most comfortable way to use git from the keyboard. True, I don't appreciate it that much because I already have aliases for what I use often (even for conventional commits), but I'll use it for some _squash_ or _rebase_ too.

## So you'll be one of those annoying people who'll recommend (neo)vim to everyone everywhere?

No, really not. I see it as solving some comfort problems with writing code that I have. Like I can really imagine lots of reasons not to go into it (using Windows, specific gui tools for development, maybe some problematic support for certain languages and platforms), but I'm pretty determined.

We'll see how long it lasts for me. :)

VK, 30.7.2025
