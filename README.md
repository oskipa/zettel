# Minimalistic Vim Zettel System

This is a minimalistic [zettelkasten](https://zettelkasten.de/posts/overview/) system to be used with Vim. Zettelkasten is a method to manage hyperlinked idea notes and bibliography.

The rakefile has tasks for creating template zettel cards, creating ctags for them, which allows for hyperlinking in Vim, and creates an index file.

To see available tasks with examples on how to use it, run 

   $rake help

## Setup

You may have to create an empty `tags` and `index` file if these are not created automatically.

The repo comes with a `.tool-versions` file, in case you use `asdf`. If you don't, you can install Ruby 3 and maybe have to install `rake` if it is not available automatically.

## How to use it
Once you have saved the records, run `$rake update` to update tags and index. You should be able to navigate from one file to another by putting the cursor on the timestamp code and use `ctrl-[` in command mode.

# Small software

This is an example of small software. It is meant to be personally made, easy to change, and adapted to personal use.

Feel free to use it, change it, or get ideas to create your own.
