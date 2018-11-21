Custom editor using Twitter.Bootstrap.Mvc4
==========================================

Twitter.Bootstrap has tons of extensions that are available to anyone using  ``Twitter.Bootstrap.Mvc4`` [#tbm]_.
As an example, here are the steps to add a date picker [#datepicker]_ to your Asp.Net Mvc4 app 
using ``Twitter.Bootstrap.Mvc4``.

.. more::

Create an empty Mvc4 web project and let's start from the sample that comes with ``Twitter.Bootstrap.Mvc4``::

    Install-Package twitter.bootstrap.mvc4.sample
    
Download the date picker [#datepicker_download]_ and include .js and .css files in your project.
Add them to the ``BootstrapBundleConfig``::

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.Add(new ScriptBundle("~/js").Include(
                // ...
                , "~/Scripts/bootstrap-datepicker.js"
                ));

            bundles.Add(new StyleBundle("~/content/css").Include(
                // ...
                , "~/Content/datepicker.css"
                ));
        }

Create an editor template named in ``~/Views/Shared/EditorTemplates`` named ``Date.cshtml``::

    @model DateTime
    @Html.TextBox("", Model, new {@class = "datepicker"})

Make sure to add the ``datepicker`` class.
The file should be named ``Date.cshtml`` and not ``DateTime.cshtml``, 
because the `HomeInputModel.StartDate` property is decorated with the data annotation ``[DataType(DataType.Date)]``.

Finally add the Javascript code that hooks the datepicker text boxes::

    $(document).ready(function() {
        $('.datepicker').datepicker();
    })

Done.


.. rubric:: Footnotes

.. [#tbm] http://nuget.org/packages/twitter.bootstrap.mvc4
.. [#datepicker] http://www.eyecon.ro/bootstrap-datepicker/
.. [#datepicker_download] http://www.eyecon.ro/bootstrap-datepicker/datepicker.zip

.. author:: default
.. categories:: none
.. tags:: none
.. comments::
