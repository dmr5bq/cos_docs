.. _testing:

================================
Testing Standards and Guidelines
================================

This document contains information on testing practices used in developing the OSF, as well as how to specifically test the OSF itself.

.. note::

    The below examples are in Python, but the concepts apply to testing in any language.



General Testing Guidelines
--------------------------

- Use long, descriptive names in methods and functions. This often removes the need for doctrings in test methods. This also makes it easier to locate tests that fail.
- Tests should be isolated. Don't interact with a real database or network. Use a separate test database that gets torn down after running or use mock objects.
- Prefer `factories <https://github.com/rbarrois/factory_boy>`_ to `fixtures <https://en.wikipedia.org/wiki/Test_fixture>`_.
- Never let incomplete tests pass, or else you run the risk of forgetting about them. Instead, add a placeholder like ``assert False, "TODO: finish me"``. 
- If you are writing a stub for a test that will be written in the future, use the :meth:`@unittest.skip` decorator.
- Strive for 100% code coverage, but do not obsess over a score that won't reach perfection.
- When testing the contents of a dictionary, test the keys individually, i.e.,

.. code-block:: python

    # Yes
    assert_equal(result['foo'], 42)
    assert_equal(result['bar'], 24)

    # No
    assert_equal(result, {'foo': 42, 'bar': 24})

Unit Tests
----------

- Focus on one unit of functionality, i.e., one method or function.
- Strive for fast tests, but a slow-running test is better than not having a test.
- It often makes sense to have one ``unittest.TestCase`` class for a single class or model. 

.. code-block:: python

    import unittest
    import factories

    class PersonTest(unittest.TestCase):
        def setUp(self):
            self.person = factories.PersonFactory()

        def test_has_age_in_dog_years(self):
             assert self.person.dog_years == self.person.age / 7

Functional Tests
----------------

Functional tests are higher level tests that are closer to how an end-user would interact with your application. They are typically used for web and GUI applications.

- Write tests as scenarios. ``TestCase`` and test method names should read like a scenario description.
- Use comments to write out stories, *before writing the test code*.

.. code-block:: python

    class TestAUser(unittest.TestCase):
        def test_can_write_a_blog_post(self):
            # Goes to the her dashboard
            ...
            # Clicks "New Post"
            ...
            # Fills out the post form
            ...
            # Clicks "Submit"
            ...
            # Can see the new post
            ...

Notice how the testcase and test method read together like "Test A User can write a blog post".

.. seealso::

	Look at the :doc:`/resources` page for testing libraries and frameworks useful in developing for the OSF.

.. todo:: 
	This page will also need the Testing the OSF page content