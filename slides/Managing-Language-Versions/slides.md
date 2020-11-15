---
paginate: true
theme: custom-theme
size: 16:9
title: Developing in Multiple Languages
_class: prose
---
<!-- _class: lead -->
<!--
Hi! I'm Mike!!

I'm going to talk about making it easier to install & switch between languages.
-->

# Developing in Multiple Languages

Lets install & use multiple languages!

---
<!-- _class: lead -->
<!--
I like to mess around with different languages, especially on Exercism channel.

It's fun to install a new language one weekend & play about!

But out the box, installing languages is kind of tricky.
-->

# Where does this come from?

I like to try lots of languages, but I also don't want to break my work machine.

---
<!-- _class: lead -->
<!--
Question for the crowd!

(Wait for responses)

I do Ruby...I still don't know the _right way_ to install it 🤫

I messed up installing languages, and had Software Update breaking everything.
-->

# Question

How do does everyone install their favourite programming language right now?

---
<!-- _class: lead -->
<!--

I started using this tool a few months ago & really liked it.

asdf: It's a bit like homebrew, if anyone has used that.

You can install a bunch of languages, but it was also designed to make switching between language versions easier.

E.g. You want to run the latest Ruby on one project, but on another project it's running an older version. It just kind of handles it.
-->

# I like asdf

It can be used to install Node.js, Python, Ruby, Elixir, Elm - Pretty much everything!

https://asdf-vm.com/

---
<!--
It has a really super install page, where you can put in your setup & it'll give you tailored install instructions.

I liked that! Super awesome!
-->

# Installing asdf

<center class="center-contents">
  <img src="images/asdf-vm-setup.png" width="900px" />
</center>

---
<!--
Once you have asdf setup, 

It's approachable to get a new language setup.
-->

# Installing a Language

```bash{0}
# Setup Ruby
$ asdf plugin add ruby

# Install Latest Ruby Version:
$ asdf install ruby 2.7.2
$ asdf global ruby 2.7.2

# Did it work?
$ ruby -v
ruby 2.7.2p137 (2020-10-01 revision 5445e04352) [x86_64-darwin19]
```

---
<!--
The "plugin add" line is saying "give me the resources to install ruby".

Behind the scenes it has a neat plugin architecture which lets anyone create a way to install plugins.

So if someone was to release something new you could get access to it pretty fast.
-->

# Installing a Language

```bash{2}
# Setup Ruby
$ asdf plugin add ruby

# Install Latest Ruby Version:
$ asdf install ruby 2.7.2
$ asdf global ruby 2.7.2

# Did it work?
$ ruby -v
ruby 2.7.2p137 (2020-10-01 revision 5445e04352) [x86_64-darwin19]
```

---
<!--
Then the next two commands are:

- Lets install this version of ruby
- Lets use this version of ruby globally (The default version), so when nothing else is set.
-->

# Installing a Language

```bash{5,6}
# Setup Ruby
$ asdf plugin add ruby

# Install Latest Ruby Version:
$ asdf install ruby 2.7.2
$ asdf global ruby 2.7.2

# Did it work?
$ ruby -v
ruby 2.7.2p137 (2020-10-01 revision 5445e04352) [x86_64-darwin19]
```

---
<!--
Then once that's done, you can start running ruby commands.
-->

# Installing a Language

```bash{9,10}
# Setup Ruby
$ asdf plugin add ruby

# Install Latest Ruby Version:
$ asdf install ruby 2.7.2
$ asdf global ruby 2.7.2

# Did it work?
$ ruby -v
ruby 2.7.2p137 (2020-10-01 revision 5445e04352) [x86_64-darwin19]
```

---
<!--
To install python, it's similar

I think that's really nice!
-->

# Installing a Language

```bash{0}
# Setup Python
$ asdf plugin add python

# Install Latest Python Version:
$ asdf install python 3.9.0
$ asdf global python 3.9.0

# Did it work?
$ python --version
Python 3.9.0
```

---
<!-- _class: lead -->
<!--
Pretty often you'll probably have to run multiple versions of a tool. Here is how you do it!
-->

# Different versions per project

What if you have different projects running different versions of languages?

---

<!--
I mentioned it's good for managing multiple versions of languages.

Within a projects folder you can run a command which pretty much says:

"Everything under this folder, run this version of this language"

And it'll create a `.tool-versions` to keep track of what you chose, everything under it will use what's defined in the file.

Assuming you've got that version installed, it'll just work.
-->

# Different versions per project

```bash{3,4}
$ cd ~/Old_Project

$ asdf local ruby 2.7.2
$ asdf local python 3.9.0

$ cat .tool-versions
ruby 2.7.2
python 3.9.0
```

---

<!--
If you don't have it right installed you can run the "asdf install" command, and it'll make sure you've got the right version installed.

This is super hand for if you're working in a team & that `.tool-version` file is in version control.
-->

# Different versions per project

```bash{5}
$ git clone git@github.com:MikeRogers0/Old_Project.git
$ cd ~/Old_Project

# Make sure we have the right versions installed
$ asdf install
python 3.9.0 is already installed
ruby 2.7.2 is already installed

$ python --version
Python 3.9.0
```

---
<!-- _class: lead -->
<!--
You made it!
-->

# That's all
