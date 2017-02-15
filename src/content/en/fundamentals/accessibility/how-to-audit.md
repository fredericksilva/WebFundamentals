project_path: /web/_project.yaml
book_path: /web/fundamentals/_book.yaml
description: Why you should do an accessibility audit and how to go about it.


{# wf_updated_on: 2017-02-07 #}
{# wf_published_on: 2017-02-07 #}

# How To Do an Accessibility Audit {: .page-title }

{% include "web/_shared/contributors/robdodson.html" %}

<div>
  <div class="video-wrapper">
    <iframe class="devsite-embedded-youtube-video" data-video-id="cOmehxAU_4s"
            data-autohide="1" data-showinfo="0" frameborder="0" allowfullscreen>
    </iframe>
  </div>
When creating a web site or application, it’s important to consider that not all
users see or experience the world in the same way. Because humans are diverse,
and have a range of abilities, there may be some users who require additional
help to access the content on a page. It’s important to consider the perspective
of these users when designing and developing your site, and to routinely audit
your site to test that it is flexible enough to cater to the needs of all users.
This post covers the why and how of doing an accessibility audit to ensure that
everyone can access your content.
</div>

## Why should you do an accessibility audit?

### In many countries it's required by law
In countries like the United States and the U.K., disability rights are
protected by law. For example, in the U.S., the Americans with Disabilities Act
(ADA) forbids discrimination of users “on the basis of disability in the full
and equal enjoyment of the goods, services, facilities, privileges, advantages
or accommodations of any place of public accommodation.”
([source](https://www.law.cornell.edu/uscode/text/42/12182))

Similarly, the U.S. has section 508 of the Rehabilitation Act which applies to
all federal agencies and those receiving federal funding. Section 508 requires
"agencies give disabled employees and members of the public access to
information that is comparable to access available to others."
([source](https://www.section508.gov/content/learn/laws-and-policies))

Ensuring that accessibility is ingrained in your process from the beginning can
help to avoid potential legal issues down the line.

### You could be missing out on customers
The World Health Organization [estimates that about 15% of the population, or
about 1 billion people have some form of
disability](http://www.who.int/mediacentre/factsheets/fs352/en/). This number
can be difficult to quantify because not everyone self-identifies as having a
disability. For example, as we age our eyesight tends to degrade, but many folks
who wear corrective lenses wouldn’t label themselves as having a disability.
Still, these users benefit from accessibility techniques like ensuring proper
color contrast for text and images, appropriate font sizes, and text zooming. If
a site is not accessible to a user, they’ll likely take their business
elsewhere.

### Good accessibility === Good UX
In general, focusing on accessibility forces you to consider your product from
different user perspectives. You may find yourself asking questions like, "Can
we make the keyboard navigation more efficient? Can we improve the contrast of
this body copy? Should we move these buttons and labels closer together?"
Invariably this leads to a product that is easier for all users to interact
with.

Now that you understand why it’s so important to audit for accessibility, the
next step is to begin reviewing your site. It may seem daunting at first, so to
help you scope the work ahead, this guide will break down the steps into
relevant topic areas.

## Start with the keyboard
<img src="imgs/ic_keyboard_black_24px.svg" class="attempt-right" alt="" width="120"/>
For users who either cannot or choose not to use a mouse, keyboard navigation is
their primary means of reaching everything on screen. This audience includes
users with motor impairments, such as Repetitive Stress Injury (RSI) or
paralysis, as well as screen reader users. The goal of a keyboard audit is to
ensure that the site has a logical tab order and easily discernable focus
styles.

Start by tabbing through your site. Ideally the order in which elements are
focused should follow the DOM order. If you’re unsure which elements should
receive focus,see [Focus Fundamentals](/web/fundamentals/accessibility/focus/)
for a refresher. The general rule of thumb is that any control a user can
interact with or provide input to should be focusable and should display a focus
indicator (e.g., a focus ring).

### Key points

- All focusable elements should have a focus indicator of some kind. It’s a very
  common practice to disable focus styles without providing an alternative by
  using `outline: none` in CSS, but this is an anti-pattern. If a keyboard user
  can’t see what’s focused, they have no way of interacting with the page.

- No controls should have a `tabindex` > 0. These controls will jump ahead of
  everything else in the tab order, regardless of their position in the DOM.
  This can be confusing for screen reader users as they tend to navigate the DOM
  in a linear fashion.

- Custom interactive controls should be focusable. If you use JavaScript to turn
  a `<div>` into a fancy dropdown, it will not automatically be inserted into
  the tab order. To make a custom control focusable, give it a `tabindex=”0”`.

- Non-interactive content (e.g., headings) should not be focusable. Sometimes
  developers add a `tabindex` to headings because they think they’re important.
  This is also an anti-pattern because it makes keyboard users who can see the
  screen less efficient. For screen reader users, the screen reader will already
  announce these headings, so there’s no need to make them focusable.

- If new content is added to the page, make sure that the user’s focus is
  directed to that content so they can take action on it. See [Managing Focus at
  the Page
  Level](/web/fundamentals/accessibility/focus/using-tabindex#managing_focus_at_the_page_level)
  for examples.

- At no point should focus get trapped. Watch out for autocomplete widgets,
  where keyboard focus may get stuck. The only time focus should be trapped is
  in specific situations, such as displaying a modal, when you don't want the
  user interacting with the rest of the page. See the guide on [Modals and
  Keyboard
  Traps](/web/fundamentals/accessibility/focus/using-tabindex#modals_and_keyboard_traps)
  for an example.

### Just because something is focusable doesn’t mean it’s usable
If you’ve built a custom control then a user should be able to reach _all_ of
its functionality using only the keyboard. See [Managing Focus In
Components](/web/fundamentals/accessibility/focus/using-tabindex#managing_focus_in_components)
for techniques on ensuring good keyboard access.

### Don’t forget offscreen content
Many sites have offscreen content that is present in the DOM but not visible,
for example, links inside a responsive drawer menu or a button inside a modal
window that has yet to be displayed. Leaving these elements in the DOM can lead
to a confusing keyboarding experience, especially for screen readers which will
announce the offscreen content as if it’s part of the page. See [Handling
Offscreen
Content](/web/fundamentals/accessibility/focus/dom-order-matters#offscreen_content)
for tips on how to deal with these elements.

## Try it with a screen reader
<img src="imgs/ic_speaker_notes_black_24px.svg" class="attempt-right" alt="" width="100"/>
After checking for general keyboard support, the next step is to ensure the page
has proper semantics and there are no obstructions to screen reader navigation.
If you’re unfamiliar with how semantic markup gets interpreted by assistive
technology, see the [Introduction to
Semantics](/web/fundamentals/accessibility/semantics-builtin/)
for a refresher.

### Key points

- A screen reader should be able to navigate to all content on the page. Make
  sure no sections of the site are hidden or blocked from screen reader access.

- All images should have proper `alt` text. The exception to this rule is when
  images are primarily for presentation purposes and are not essential pieces of
  content. To signify that an image should be skipped by a screen reader you
  should set the value of the `alt` attribute to an empty string, e.g.,
  `alt=””`.

- The flow of information should make sense. Because screen readers navigate the
  page in DOM order, if you’ve used CSS to visually reposition elements they may
  be announced in a nonsensical sequence. If you need something to appear
  earlier in the page, try to physically move it earlier in the DOM.

- Ensure that all controls have a label. For custom controls this may require
  the use of `aria-label` or `aria-labelledby`. See [ARIA Labels and
  Relationships](/web/fundamentals/accessibility/semantics-aria/aria-labels-and-relationships)
  for examples.

- Ensure that all custom controls have an appropriate `role` and any required
  ARIA attributes that confer their state. For example, a custom checkbox will
  need a `role=”checkbox”` and `aria-checked=”true|false”` to properly convey
  its state. See the [Introduction to
  ARIA](/web/fundamentals/accessibility/semantics-aria/)
  for a general overview of how ARIA can provide missing semantics for custom
  controls.

- If content _should_ be hidden from a screen reader, for instance, if it’s
  offscreen or just presentational, make sure that content is set to
  `aria-hidden=”true”`. Take a look at the guide on [Hiding
  content](/web/fundamentals/accessibility/semantics-aria/hiding-and-updating-content#aria-hidden)
  for further explanation.

### Familiarity with even one screen reader goes a long way
Though it might seem daunting to learn a screen reader, they’re actually pretty
easy to pick up. In general, most developers can get by with just a few simple
key commands.

If you’re on a Mac check out [this video on using
VoiceOver](https://www.youtube.com/watch?v=5R-6WvAihms&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&index=6),
the screen reader that comes with Mac OS. If you’re on a PC check out [this
video on using
NVDA](https://www.youtube.com/watch?v=Jao3s_CwdRU&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&index=4),
a donation supported, open source screen reader for Windows.

### aria-hidden does not prevent keyboard focus
It’s important to understand that ARIA can only affect the _semantics_ of an
element; it has no effect on the _behavior_ of the element. So while you can
make an element hidden to screen readers with `aria-hidden=”true”`, that does
not change the focus behavior for that element. For offscreen interactive
content you will often need to combine `aria-hidden=”true”` and `tabindex=”-1”`
to make sure it’s truly removed from the keyboard flow. The proposed [inert
attribute](https://github.com/WICG/inert) aims to make this easier by combining
the behavior of both attributes.

## Take advantage of headings and landmarks
<img src="imgs/ic_map_black_24px.svg" class="attempt-right" alt="" width="100"/>
Headings and landmark elements imbue your page with semantic structure, and
greatly increase the navigating efficiency of assistive technology users. Many
screen reader users report when they first land on an unfamiliar page they
typically try to [navigate by
headings](http://www.heydonworks.com/article/responses-to-the-screen-reader-strategy-survey).
Similarly, screen readers also offer the ability to jump to important landmarks
like `<main>` and `<nav>`. For these reasons it’s important to consider how the
structure of your page can be used to guide the user’s experience.

### Key points

- Make proper use of `h1-h6` hierarchy. Think of headings as tools to create an
  outline for your page. Don’t rely on the built-in styling of headings;
  instead, consider all headings as if they were the same size and use the
  semantically appropriate level for primary, secondary, and tertiary content.
  Then use CSS to make the headings match your design.

- Use landmark elements and roles so users can bypass repetitive content. Many
  assistive technologies provide shortcuts to jump to specific parts of the
  page, such as those defined by `<main>` or `<nav>` elements. These elements
  have implicit landmark roles. You can also use the ARIA `role` attribute to
  explicitly define regions on the page, e.g., `<div role=”search”>`. See [the
  guide on headings and
  landmarks](/web/fundamentals/accessibility/semantics-builtin/navigating-content)
  for more examples.

- Avoid `role=”application”` unless you know what you’re doing. The
  `application` landmark role will tell assistive technology to disable its
  shortcuts and pass through all key presses to the page. This means that the
  keys screen reader users typically use to move around the page will no longer
  work, and you will need to implement _all_ keyboard handling yourself.

### Quickly audit headings and landmarks with a screen reader
Screen readers like VoiceOver and NVDA provide a context menu for skipping to
important regions on the page. If you’re doing an accessibility audit, you can
use these menus to get a quick overview of the page and determine if heading
levels are appropriate and which landmarks are in use. To learn more check out
these instructional videos on the basics of
[VoiceOver](https://www.youtube.com/watch?v=5R-6WvAihms&index=6&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g)
and
[NVDA](https://www.youtube.com/watch?v=Jao3s_CwdRU&list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g&index=4).

## Automate the process
<img src="imgs/ic_build_black_24px.svg" class="attempt-right" alt="" width="100"/>
Manually testing a site for accessibility can be tedious and error prone.
Eventually you’ll want to automate the process as much as possible. This can be
done through the use of browser extensions, and command line accessibility test
suites.

### Key points

- Does the page pass all the tests from either the
  [aXe](https://chrome.google.com/webstore/detail/axe/lhdoppojpmngadmnindnejefpokejbdd)
  or
  [WAVE](https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh)
  browser extensions? These extensions are a useful addition to any manual test
  process as they can quickly pick up on subtle mistakes like failing contrast
  ratios and missing ARIA attributes. If you prefer to do things from the
  command line, [axe-cli](https://github.com/dequelabs/axe-cli) provides the
  same functionality as the browser extension, but can be easily run from your
  terminal.

- To avoid regressions, especially in a continuous integration environment,
  incorporate a library like [axe-core](https://github.com/dequelabs/axe-core)
  into your automated test suite. axe-core is the same engine that powers the
  aXe chrome extension, but in an easy-to-run command line utility.

- If you’re using a framework or library, does it provide its own accessibility
  tools? Some examples include
  [protractor-accessibility-plugin](https://github.com/angular/protractor-accessibility-plugin/)
  for Angular, and
  [a11ysuite](https://github.com/Polymer/web-component-tester#a11ysuite) for
  Polymer and Web Components. Take advantage of these tools whenever possible to
  avoid reinventing the wheel.

### If you’re building a Progressive Web App consider giving Lighthouse a shot
<img src="imgs/lighthouse.png" class="attempt-right" alt="" />
Lighthouse is a tool to help measure the performance of your progressive web
app. It also uses the axe-core library to power a set of accessibility tests. If
you’re already using Lighthouse, keep an eye out for failing accessibility tests
in your report. Fixing these will help improve the overall user experience of
your site.

## Wrapping Up
Making accessibility audits a regular part of your team process, and doing these
checks early and often, ensures that all users will be able to access the
content of your site. Remember, good accessibility equals good UX!

### Additional Resources

- [Web Accessibility by Google](https://bit.ly/web-a11y)
- [Accessibility Fundamentals](/web/fundamentals/accessibility/)
- [A11ycasts](https://www.youtube.com/playlist?list=PLNYkxOF6rcICWx0C9LVWWVqvHlYJyqw7g)


