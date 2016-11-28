==============================
Code Reviews for Contributions
==============================

This document contains the guidelines for our code review process, through which all OSF contributions must go before they are worked into the OSF software.

General Guidelines
******************

- Ask for clarification on vague or ambiguous code review comments, i.e., "I didn't understand this comment. Can you clarify?"
- Talk in person, when possible, if there are too many "I didn't understand" comments.

Having Your Code Reviewed
*************************

Before sending a pull request for code review, make sure you have met the :doc:`pull request guidelines <pull_requests>`.

- It may be difficult not to perceive code review as personal criticism, but, keep in mind, it is a review of the code, not the person. 
- Code reviews are supposed to be a helpful learning experience and should be treated as such.
- After addressing all comments from a review, reach out to your reviewer on Flowdock for the next pass.
- If there is a style guideline that affects your pull request and you believe the guideline itself is incorrect, post an issue or send a pull request to the `COSDev <https://github.com/CenterForOpenScience/COSDev>`_ repo rather than discussing it in the pull request itself. Make sure to reference the problematic guidelines directly.

Reviewing Code
**************

- Make sure you understand the purpose of the code being reviewed.
- Checkout the branch being reviewed, and manually test the intended behavior.
- In your comments, keep in mind the fact that what you're saying can easily be perceived as personal criticism and adjust your tone accordingly; stick to constructive and objective comments.
- After doing a pass of code review, add a comment with an emoji attached to signify that your pass is complete and ready to be processed.
- Style fixes should refer to the style guides, when possible.

Example style comment:

.. code-block:: markdown

    > Use parentheses for line continuation.

    From http://cosdev.readthedocs.org/en/latest/style_guides/python.html:

.. note::
	When possible, include a link to COS documentation (or outside documentation, as applicable) to back up claims of style violations or best practices violations. This practice aids in keeping the tone of comments objective and hopefully mitigates any feelings of criticism in the recipient. *(See the example comment above)*
