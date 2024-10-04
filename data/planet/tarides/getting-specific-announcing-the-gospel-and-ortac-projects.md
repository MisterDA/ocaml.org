---
title: 'Getting Specific: Announcing the Gospel and Ortac Projects'
description: "Part of the benefit of open-source development is the opportunity to
  collaborate on projects across traditional organisational boundaries\u2026"
url: https://tarides.com/blog/2024-09-03-getting-specific-announcing-the-gospel-and-ortac-projects
date: 2024-09-03T00:00:00-00:00
preview_image: https://tarides.com/static/c5f8545605bcadc615295dc02a4e63e0/99ec5/magnifying-glass.jpg
authors:
- Tarides
source:
---

<p>Part of the benefit of open-source development is the opportunity to collaborate on projects across traditional organisational boundaries, such as academia and industry. Tarides is part of a larger effort to develop a behavioural specification language and associated tooling for OCaml. The project creates an easy-to-use foundation for formal specifications, allowing users to include them in generated documentation and perform automated testing and verification. This important work is funded in part by <a href="https://anr.fr/en/">ANR</a>.</p>
<h2 style="position:relative;"><a href="https://tarides.com/feed.xml#the-gospel-project" aria-label="the gospel project permalink" class="anchor before"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewbox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>The Gospel Project</h2>
<p>The French National Research Agency (ANR) is a public institution in France that funds innovative research projects where public institutions collaborate with each other or the private sector.  OCaml was invented at the French National Institute for Research in Digital Science and Technology, <a href="https://www.inria.fr/en">INRIA</a>, and ANR is <a href="https://anr.fr/en/funded-projects-and-impact/funded-projects/project/funded/project/b2d9d3668f92a3b9fbbf7866072501ef-bcaf728f49/?tx_anrprojects_funded%5Bcontroller%5D=Funded&amp;cHash=abe71830301addcbf212c5a439e7fbbf">funding a research project</a> as a collaboration between <a href="https://www.inria.fr/fr">INRIA</a>, <a href="https://tarides.com">Tarides</a>, <a href="https://lmf.cnrs.fr">LMF UPSaclay</a>, and <a href="https://www.nomadic-labs.com">Nomadic Labs</a>. The goal of the project is to develop and improve the specification language <a href="https://github.com/ocaml-gospel/gospel">Gospel</a> alongside its tooling ecosystem and demonstrate its usefulness in different case studies.</p>
<h2 style="position:relative;"><a href="https://tarides.com/feed.xml#what-is-gospel" aria-label="what is gospel permalink" class="anchor before"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewbox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>What is Gospel?</h2>
<p>Gospel is a contract-based behavioural specification language that allows you to write specifications in the module interface you want to specify. As a specification language, it is a formal language, meaning its semantics are precisely defined (by means of translation into Separation Logic, see <a href="https://inria.hal.science/hal-02157484v2/document">this paper</a>).</p>
<p>By <em>behavioural</em> specification language, we mean a language that allows you to describe the expected functional behaviour of a function. Specifications don't reference resources such as CPU time or memory size, but only what the program does (so-called functional behaviour). Expected behaviour is expressed as a contract. The basic premise is that as long as the user of the library calls functions with arguments that respect the expressed preconditions, then the implementation of the library should behave per their description.</p>
<p>Per se, Gospel doesn't guarantee that your implementation respects its given specifications, it is simply a language that allows you to express precisely what your code <em>should</em> do. However, note that Gospel still comes with a type-checker. This type-checker lets you check that your specifications are well-formed and in sync with the interface. For example, if you add an extra argument to a function in your library, the Gospel type-checker will tell you if you forgot to update the specifications accordingly.</p>
<p>Gospel is a relatively new specification language and is bound to evolve, but it is already mature enough to specify a diverse set of libraries. It provides developers with a non-invasive and easy-to-use syntax to annotate their module interfaces with formal contracts describing type invariants, mutability, function pre- and postconditions, exceptions, etc.
For example, let's say you want to specify a fixed-size stack. The type declaration in the module interface would look like:</p>
<div class="gatsby-highlight" data-language="ocaml"><pre class="language-ocaml"><code class="language-ocaml"><span class="token keyword">type</span> a t
<span class="token comment">(*@ model capacity : integer
    mutable model contents : a sequence
    with s
    invariant Sequence.length s.contents &lt;= s.capacity *)</span></code></pre></div>
<p>You give two logical models to your datatype: an immutable one for the capacity of the stack and a mutable one for the content. Then, given a stack, <code>s</code>, you can formulate type invariants. Namely, the stack should not have more elements than capacity allows.
The specification for the <code>create</code> function would look like:</p>
<div class="gatsby-highlight" data-language="ocaml"><pre class="language-ocaml"><code class="language-ocaml"><span class="token keyword">val</span> create <span class="token punctuation">:</span> int <span class="token operator">-&gt;</span> <span class="token type-variable function">'a</span> t
<span class="token comment">(*@ s = create n
    requires n &gt; 0
    ensures s.capacity = n
    ensures s.contents = Sequence.empty *)</span></code></pre></div>
<p>The first line binds the arguments and the returned value to names so that we can talk about them. Then we express the precondition that the given argument should be strictly positive and the two postconditions that fill the logical models as expected.</p>
<p>Gospel is also a tool-agnostic specification language, meaning it doesn’t make any assumption about how and by which tools its specifications will be consumed. Some users use Gospel specifications to provide proofs of functional correctness for their implementations. For example, <a href="https://github.com/ocaml-gospel/cameleer">Cameleer</a> does so by leveraging the power of the <a href="https://www.why3.org/">Why3</a> deductive verification platform. At Tarides, with the <a href="https://github.com/ocaml-gospel/ortac">Ortac</a> project, we explore how to use Gospel specifications to do runtime assertion checking.</p>
<p>Gospel was initially developed by Cláudio Lourenço (<a href="https://www.lri.fr">LRI</a> post-doctorate) and is currently maintained by Jean-Christophe Filliâtre, <a href="https://github.com/shym">Samuel Hym</a>, <a href="https://github.com/n-osborne">Nicolas Osborne</a>, and Mário Pereira. <a href="https://github.com/pascutto">Clément Pascutto</a> also maintained Gospel for several years as part of his PhD work at Tarides and LRI.</p>
<h2 style="position:relative;"><a href="https://tarides.com/feed.xml#a-tool-for-gospel-ortac" aria-label="a tool for gospel ortac permalink" class="anchor before"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewbox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>A Tool for Gospel: Ortac</h2>
<p><a href="https://github.com/ocaml-gospel/ortac">Ortac</a> stands for OCaml RunTime Assertion Checking. Clément's PhD thesis initiated the Ortac project which has since grown into a greater cooperative effort. Samuel Hym and Nicolas Osborne currently maintain it. At its core, Ortac translates the computable subset of Gospel terms into OCaml code. In addition, it provides a plugin architecture to implement different uses of this translation. Translating Gospel terms into runnable OCaml code opens the possibility of checking an implementation against the interface specification at runtime.</p>
<p>Three plugins have been built upon this translation, plus a fourth one, which is slightly different. Let’s take a look:</p>
<ol>
<li>The Ortac/Wrapper plugin was developed during Clément's PhD. Given the Gospel specified interface of a module, this plugin generates a new module with the same interface, wrapping the initial implementation with runtime checks coming from Gospel specifications. When a Gospel specification is violated by the client for preconditions or by the initial implementation for postconditions, the wrapped version will output an error message providing the user with useful information, such as which Gospel clause they have violated. Users can then use the new wrapped module in place of the original one in their project, to, for example, aid in debugging efforts. This plugin is still considered experimental.</li>
<li>The Ortac/Monolith plugin, which is based on Ortac/Wrapper and is the product of an internship, was presented at <a href="https://inria.hal.science/hal-03328646">ICFP 2021</a>. Given the specified interface of a module, this plugin generates the <a href="https://gitlab.inria.fr/fpottier/monolith">Monolith</a> standalone program, testing the initial implementation against the wrapped one. The idea is that, in case the implementation doesn't respect the specification, the wrapped version will return a special Ortac error while the bare initial one won't. Monolith allows you to use fuzzing to test your library and provides a runnable scenario that demonstrates the unexpected behaviour. This plugin is also still considered experimental.</li>
<li>The Ortac/QCheck-STM plugin is based on Naomi Spargo's internship project. Given the specified interface of a module and some user-provided extra information, this plugin generates standalone <a href="https://github.com/ocaml-multicore/multicoretests">QCheck-STM</a> tests. In addition to avoiding having to write the QCheck-STM test by hand and as a recently added feature, in case of a test failure, the generated tests will inform you which part of the specification has been violated, give you a runnable scenario demonstrating the unexpected behaviour, and the expected returned value when the Gospel specifications allow to compute it. This plugin has been released.</li>
<li>The Ortac/Dune plugin is slightly different as it doesn't rely on a Gospel specification. Instead, it helps you by generating the Dune rules necessary to run another plugin. So far, only the command for Dune rules related to the Ortac/QCheck-STM plugin is available, as it is the only one that has been released. The command yields the Dune rules required to generate and run the tests.</li>
</ol>
<h2 style="position:relative;"><a href="https://tarides.com/feed.xml#future-steps" aria-label="future steps permalink" class="anchor before"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewbox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Future Steps</h2>
<p>Regarding Ortac, the 0.3.0 version of a set of Ortac packages, including the first Ortac/Dune release, has recently been <a href="https://discuss.ocaml.org/t/ann-ortac-0-3-0-dynamic-formal-verification-made-easy/14936">published</a>. Among other improvements and fixes, this release makes dynamic formal verification with Ortac very easy. The team is now investigating how to test more functions by lifting some limitations that come with a naive use of the QCheck-STM test framework, with an intern (Nikolaus Huber) working on this last topic.</p>
<p>With <a href="https://discuss.ocaml.org/t/ann-gospel-0-3-0/14480/2">Gospel 0.3.0 published</a>, the upcoming goals centre around continuing maintenance and development, using our engineering expertise to help our research partners bring new features to the Gospel language.</p>
<h2 style="position:relative;"><a href="https://tarides.com/feed.xml#until-next-time" aria-label="until next time permalink" class="anchor before"><svg aria-hidden="true" focusable="false" height="16" version="1.1" viewbox="0 0 16 16" width="16"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Until Next Time</h2>
<p>Do you want to try out <a href="https://github.com/ocaml-gospel/gospel">Gospel</a> and <a href="https://github.com/ocaml-gospel/ortac">Ortac</a>? Check out the documentation and report back on your experience! If you want to stay informed about our projects, follow us on <a href="https://twitter.com/tarides_">X</a> (formerly known as Twitter) and <a href="https://www.linkedin.com/company/tarides">LinkedIn</a> for all our latest updates. Are you interested in using Gospel for your own projects? <a href="https://tarides.com/contact/">Contact us</a>, and we will be happy to discuss the benefits of implementing specification languages in your workflow.</p>
<blockquote>
<p>Tarides champions open-source development. We create and maintain key features of the OCaml language in collaboration with the OCaml community. To learn more about how you can support our open-source work, discover our <a href="https://github.com/sponsors/tarides">page on GitHub</a>.</p>
</blockquote>
<blockquote>
<p>We are always happy to discuss commercial opportunities around OCaml. We provide core services, including training, tailor-made tools, and secure solutions. <a href="https://tarides.com/contact/">Contact us today</a> to learn more about how Tarides can help your teams realise their vision.</p>
</blockquote>