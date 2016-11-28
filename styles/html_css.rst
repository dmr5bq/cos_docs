=============================
HTML and CSS Style Guidelines
============================= 

HTML Style Guidelines
*********************

- Follow `mdo's Code Guide <http://mdo.github.io/code-guide/>`_, with one exception: Use *four spaces* for indentation instead of two spaces.
- Use ``.lowercase-and-dashes`` for class names and ``#camelCase`` for IDs.
- Add a comment marking the end of large blocks where clarity may be a problem. Use ``<!-- end class-name -->``
- When closing a nested ``div``, make sure to include a ``<!-- end <name> -->`` comment at the end for clarity.

.. code-block:: html

    <div class="container-fluid">
        <div class="row">
        	<div class="col-md-4">
        		<div class="inner-element">
        			...
        		</div> <!-- end inner-element-->
        	</div><!-- end col-md-4 -->
        	<div class="col-md-8">
        		...
        	</div><!-- end col-md-8 -->
        </div><!-- end row -->
        <div class="row">
        	... 
        </div><!-- end row -->
        ...
    </div><!-- end container-fluid -->

CSS Style Guidelines
********************

- Avoid inline CSS. 
- Use CSS classes for maintainability and reusability.
- Use separate stylesheets whenever possible, including a link in the ``<head>`` section of the HTML document.
