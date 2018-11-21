Blogging in reSt on GitHub
==========================

I started hosting my blog on GitHub. Why?

There are some requirements that I found hard to meet
using a standard blogging engine:

 * I want to locally edit my blog, using a text editor.
   (And, by the way, all online blog-editors stink.)
 * I want to store my blog "source files" in plain(ish) text, 
   with only little markup:
   	* I can put plain text under source control. 
	  (This also gives me an easy backup facility.)
	* I like writing in plain text - very little distractions.
	* I can easily transform it to multiple output formats.
   I'd prefer restructured text (reSt), because I'm familiar with it.
 * I would like to keep record of my published posts.
   Which posts were online at what moment in time?
 * I would like to have some freedom in styling the site.
   (Even though I'm not very good at it.)
 
Since I'm already familiar with `Sphinx <http://sphinx.pocoo.org>`_, 
I decided to write my blog in reSt,
run it through Sphinx to produce a static html site
and then push the output to my 
`user page <http://pages.github.com/>`_ 
on GitHub.

You are looking at the result.

Because the personal page is backed by a git repo,
I have full history of my published posts.
The source files are stored in reSt and are put under source control [#hg]_.
This is a private repository - I have to put my drafts somewhere, right.

There are some thing I'd like to add to this blog:

 * An automatically created atom feed.
 * Apply categories or labels to blog posts.
 * Enable readers to leave comments.
 
Only the atom feed is a priority to me. 
I'll investigate if there's a Sphinx extension for that.

.. rubric:: Footnotes

.. [#hg] In fact I use use a Mercurial repo for the source files, 
         in combination with a private online repository at Bibucket. Mercurial is a nice DVCS too! And I like Bitbucket almost as much as I do GitHub.
 
 